---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703980"
---
# <a name="classes"></a>类

类是一种数据结构，它可以包含数据成员（常量和字段）、函数成员（方法、属性、事件、索引器、运算符、实例构造函数、析构函数和静态构造函数）和嵌套类型。 类类型支持继承，这是一个派生类可以扩展和专用化基类的机制。

## <a name="class-declarations"></a>类声明

*Class_declaration*是声明新类的*type_declaration* （[类型声明](namespaces.md#type-declarations)）。

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

*Class_declaration*包含一组可选的*特性*（[特性](attributes.md)），后跟一组可选的*class_modifier*s （[类修饰符](classes.md#class-modifiers)），后跟一个可选的 `partial` 修饰符 `class`，然后是一个可选的修饰符，后面跟一个可选的修饰符 *，然后是*一个可选的*type_parameter_list* （[类型参数](classes.md#type-parameters)），后跟一个*可选的 class_base 规范（* [类基本规范](classes.md#class-base-specification)），后跟通过一组可选的*type_parameter_constraints_clause*s （[类型参数约束](classes.md#type-parameter-constraints)），后跟一个*class_body* （[类体](classes.md#class-body)），后面可以跟一个分号。

类声明无法提供*type_parameter_constraints_clause*s，除非它还提供了*type_parameter_list*。

提供*type_parameter_list*的类声明是***泛型类声明***。 此外，嵌套在泛型类声明或泛型结构声明中的任何类本身都是一个泛型类声明，因为必须提供包含类型的类型参数才能创建构造类型。

### <a name="class-modifiers"></a>类修饰符

*Class_declaration*可以选择性地包含一系列类修饰符：

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

同一修饰符在类声明中多次出现会出现编译时错误。

在嵌套类上允许 `new` 修饰符。 它指定类按[新修饰符](classes.md#the-new-modifier)中所述隐藏同名的继承成员。 `new` 修饰符出现在不是嵌套类声明的类声明中是编译时错误。

`public`、`protected`、`internal`和 `private` 修饰符控制类的可访问性。 根据出现类声明的上下文，可能不允许使用其中某些修饰符（[声明的可访问性](basic-concepts.md#declared-accessibility)）。

以下各节将讨论 `abstract`、`sealed` 和 `static` 修饰符。

#### <a name="abstract-classes"></a>抽象类

`abstract` 修饰符用于指示某个类不完整，并且它将仅用作一个基类。 抽象类与非抽象类在以下方面有所不同：

*  抽象类不能直接实例化，而是在抽象类上使用 `new` 运算符的编译时错误。 尽管可以具有编译时类型为抽象的变量和值，但这类变量和值必须 `null` 或包含对从抽象类型派生的非抽象类的实例的引用。
*  允许（但不要求）抽象类包含抽象成员。
*  抽象类不能是密封的。

如果非抽象类是从抽象类派生的，则非抽象类必须包含所有继承的抽象成员的实际实现，从而重写这些抽象成员。 示例中
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
抽象类 `A` 引入了 `F`抽象方法。 类 `B` 引入了附加方法 `G`，但由于该方法不提供 `F`的实现，因此 `B` 还必须声明为抽象。 类 `C` 重写 `F` 并提供实际实现。 由于 `C`中没有抽象成员，因此允许（但不要求） `C` 为非抽象。

#### <a name="sealed-classes"></a>密封类

`sealed` 修饰符用于阻止从类派生。 如果将密封类指定为其他类的基类，则会发生编译时错误。

密封类不能也是抽象类。

`sealed` 修饰符主要用于防止意外派生，但它还支持某些运行时优化。 特别是，由于知道密封类绝不会有任何派生类，因此可以将密封类实例上的虚函数成员调用转换为非虚拟调用。

#### <a name="static-classes"></a>静态类

`static` 修饰符用于标记声明为***静态类***的类。 静态类不能被实例化，不能用作类型并且只能包含静态成员。 只有静态类可以包含扩展方法的声明（[扩展方法](classes.md#extension-methods)）。

静态类声明受到下列限制：

*  静态类不能包含 `sealed` 或 `abstract` 修饰符。 但请注意，由于无法对静态类进行实例化或派生，因此它的行为就像它既是密封的又是抽象的。
*  静态类不能包含*class_base*规范（[类基规范](classes.md#class-base-specification)），并且无法显式指定基类或实现的接口列表。 静态类隐式继承自类型 `object`。
*  静态类只能包含静态成员（[静态成员和实例成员](classes.md#static-and-instance-members)）。 请注意，常量和嵌套类型归类为静态成员。
*  静态类不能包含具有 `protected` 或声明可访问性 `protected internal` 的成员。

这是一种编译时错误，违反其中的任何限制。

静态类没有实例构造函数。 不能在静态类中声明实例构造函数，也不会为静态类提供默认的实例构造函数（[默认构造函数](classes.md#default-constructors)）。

静态类的成员不是自动静态的，成员声明必须显式包含 `static` 修饰符（常量和嵌套类型除外）。 当类嵌套在静态外部类中时，嵌套类不是静态类，除非它显式包含 `static` 修饰符。

__引用静态类类型__

如果有*namespace_or_type_name* （[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)），则允许引用静态类

*  *Namespace_or_type_name*是窗体 `T.I`*namespace_or_type_name*的 `T`，或
*  *Namespace_or_type_name*是窗体 `typeof(T)`的*typeof_expression* （[参数列表](expressions.md#argument-lists)1）中的 `T`。

如果*primary_expression* （[函数成员](expressions.md#function-members)），则允许引用静态类

*  *Primary_expression*是窗体 `E.I`的*member_access*中的 `E` （对[动态重载决策进行编译时检查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。

在其他任何上下文中，引用静态类会导致编译时错误。 例如，将静态类用作基类、成员的构成类型（[嵌套类型](classes.md#nested-types)）、泛型类型参数或类型参数约束时，将会出现错误。 同样，静态类不能用于数组类型、指针类型、`new` 表达式、强制转换表达式、`is` 表达式、`as` 表达式、`sizeof` 表达式或默认值表达式。

### <a name="partial-modifier"></a>Partial 修饰符

`partial` 修饰符用于指示此*class_declaration*为分部类型声明。 具有相同名称的多个分部类型声明在封闭命名空间或类型声明中组合在一起，并遵循在[部分类型](classes.md#partial-types)中指定的规则组成一个类型声明。

如果在不同的上下文中生成或维护这些段，则通过单独的程序文本段来声明的类的声明会很有用。 例如，类声明的一部分可以是计算机生成的，而另一个是手动编写的。 这两者的文本分离可防止更新中的一个发生更新。

### <a name="type-parameters"></a>类型参数

类型参数是一个简单标识符，它表示为创建构造类型而提供的类型参数的占位符。 类型参数是将稍后提供的类型的正式占位符。 与此相反，类型参数（[类型参数](types.md#type-arguments)）是在创建构造类型时替换类型参数的实际类型。

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

类声明中的每个类型参数在该类的声明空间（[声明](basic-concepts.md#declarations)）中定义一个名称。 因此，它不能与另一个类型参数或该类中声明的成员同名。 类型参数不能与类型本身具有相同的名称。

### <a name="class-base-specification"></a>类基规范

类声明可以包含*class_base*规范，该规范定义类的直接基类和类直接实现的接口（[接口](interfaces.md)）。

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

类声明中指定的基类可以是构造类类型（[构造类型](types.md#constructed-types)）。 基类本身不能是类型参数，但它可以涉及范围内的类型参数。

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>基类

*Class_type*包含在*class_base*中时，它将指定所声明的类的直接基类。 如果类声明没有*class_base*，或者*class_base*只列出接口类型，则假定直接基类是 `object`的。 类从其直接基类继承成员，如[继承](classes.md#inheritance)中所述。

示例中
```csharp
class A {}

class B: A {}
```
类 `A` 称为 `B`的直接基类，`B` 称为从 `A`派生。 由于 `A` 未显式指定直接基类，因此它的直接基类被隐式 `object`。

对于构造类类型，如果在泛型类声明中指定了基类，则会通过将基类声明中的每个*type_parameter*替换为构造的类型的相应*type_argument*来获取构造类型的基类。 给定泛型类声明
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
构造类型的基类 `G<int>` 将 `B<string,int[]>`。

类类型的直接基类必须至少具有与类类型本身相同的可访问性（[可访问域](basic-concepts.md#accessibility-domains)）。 例如，`public` 类派生自 `private` 或 `internal` 类的编译时错误。

类类型的直接基类不得为以下类型之一： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。 此外，泛型类声明不能将 `System.Attribute` 用作直接或间接的基类。

确定类 `B`的直接基类规范 `A` 的含义时，`B` 的直接基类暂时假设为 `object`。 这可以确保基类规范的含义无法以递归方式依赖于自身。 示例：
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
存在错误，因为在基类规范中 `A<C.B>` `C` 的直接基类被视为 `object`，因此（由[命名空间和类型名称](basic-concepts.md#namespace-and-type-names)的规则） `C` 不被视为具有成员 `B`。

类类型的基类是直接基类及其基类。 换言之，基类集是直接基类关系的传递闭包。 参考上面的示例，`B` 的基类 `A` 并 `object`。 示例中
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
`D<int>` 的基类 `C<int[]>`、`B<IComparable<int[]>>`、`A`和 `object`。

除了类 `object`，每个类类型都有一个直接基类。 `object` 类没有直接基类，是其他所有类的最终基类。

当类 `B` 从类 `A`派生时，`A` 依赖于 `B`，则会发生编译时错误。 类***直接依赖于***其直接基类（如果有），并且***直接依赖于***直接嵌套它的类（如果有）。 根据此定义，类所依赖的完整类集是***直接依赖***关系的反身和可传递闭包。

示例
```csharp
class A: A {}
```
是错误的，因为类依赖于自身。 同样，示例
```csharp
class A: B {}
class B: C {}
class C: A {}
```
出现错误，因为类循环依赖于自身。 最后，示例
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
导致编译时错误，这是因为 `A` 依赖于 `B.C` （其直接基类），这取决于 `B` （它的直接封闭类），后者循环依赖于 `A`。

请注意，类不依赖于嵌套在其中的类。 示例中
```csharp
class A
{
    class B: A {}
}
```
`B` 依赖于 `A` （因为 `A` 既是它的直接基类，也是它的直接封闭类），但 `A` 不依赖于 `B` （因为 `B` 既不是基类，也不是 `A`的封闭类）。 因此，该示例是有效的。

不能从 `sealed` 类派生。 示例中
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
类 `B` 出错，因为它尝试从 `sealed` 类 `A`派生。

#### <a name="interface-implementations"></a>接口实现

*Class_base*规范可能包含一系列接口类型，在这种情况下，类可以直接实现给定的接口类型。 [接口实现中进一步](interfaces.md#interface-implementations)讨论了接口实现。

### <a name="type-parameter-constraints"></a>类型参数约束

泛型类型和方法声明可以选择通过包含*type_parameter_constraints_clause*来指定类型参数约束。

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

每个*type_parameter_constraints_clause*都包含标记 `where`，后跟类型参数的名称，后跟一个冒号和该类型参数的约束列表。 每个类型参数最多只能有一个 `where` 子句，`where` 子句可以按任意顺序列出。 与 `get` 并 `set` 属性访问器中的标记一样，`where` 标记不是关键字。

`where` 子句中给定的约束列表可以包括以下任何组件（按以下顺序）：单个主约束、一个或多个辅助约束以及构造函数约束 `new()`。

主约束可以是类类型或***引用类型约束***`class` 或 `struct`***值类型约束***。 辅助约束可以是*type_parameter*或*interface_type*。

引用类型约束指定用于类型参数的类型参数必须是引用类型。 已知为引用类型的所有类类型、接口类型、委托类型、数组类型和类型参数均满足此约束。

值类型约束指定用于类型参数的类型参数必须是不可以为 null 的值类型。 所有不可以为 null 的结构类型、枚举类型和具有值类型约束的类型参数均满足此约束。 请注意，尽管归类为值类型，但可以为 null 的类型（[可为 null 的类型](types.md#nullable-types)）不满足值类型约束。 具有值类型约束的类型形参也不能具有*constructor_constraint*。

指针类型永远不允许为类型参数，并且不被视为满足引用类型或值类型约束。

如果约束是类类型、接口类型或类型参数，则该类型指定用于该类型参数的每个类型参数都必须支持的最小 "基类型"。 当使用构造类型或泛型方法时，将在编译时对照类型参数上的约束检查类型参数。 提供的类型参数必须满足[满足约束](types.md#satisfying-constraints)中所述的条件。

*Class_type*约束必须满足以下规则：

*  类型必须是类类型。
*  类型不得 `sealed`。
*  类型不得为以下类型之一： `System.Array`、`System.Delegate`、`System.Enum`或 `System.ValueType`。
*  类型不得 `object`。 由于所有类型都派生自 `object`，因此如果允许，此类约束将不起作用。
*  给定类型参数最多只能有一个约束为类类型。

指定为*interface_type*约束的类型必须满足以下规则：

*  类型必须是接口类型。
*  在给定的 `where` 子句中不能多次指定一个类型。

在任一情况下，约束都可以涉及关联的类型或方法声明的任何类型参数作为构造类型的一部分，并且可以涉及所声明的类型。

指定为类型参数约束的任何类或接口类型必须至少具有与声明的泛型类型或方法相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

指定为*type_parameter*约束的类型必须满足以下规则：

*  类型必须为类型参数。
*  在给定的 `where` 子句中不能多次指定一个类型。

此外，类型参数的依赖项关系图中必须没有循环，其中依赖项是由定义的传递关系：

*  如果类型参数 `T` 用作类型参数的约束 `S` 则 `S`***依赖于***`T`。
*  如果类型参数 `S` 依赖于类型参数 `T` 并且 `T` 依赖于类型 `U` 参数，则 `S`***取决于***`U`。

在给定此关系的情况下，如果类型参数依赖于自身（直接或间接），则会发生编译时错误。

所有约束必须在依赖类型参数之间保持一致。 如果类型参数 `S` 依赖于类型参数 `T` 则：

*  `T` 不得具有值类型约束。 否则，`T` 将被有效密封，因此 `S` 强制与 `T`的类型相同，从而无需两个类型参数。
*  如果 `S` 具有值类型约束，则 `T` 不能具有*class_type*约束。
*  如果 `S` 具有*class_type*约束 `A` 并且 `T` 具有*class_type*约束，则必须存在从 `B` 到 `A` 或从 `B` 到 `B` 的隐式引用转换。`A`
*  如果 `S` 还依赖于类型参数 `U` 并且 `U` 具有*class_type*约束 `A` 并且 `T` 具有*class_type*的约束，则必须存在从 `B` 到 `A` 或从 `B` 到 `B` 的隐式引用转换。`A`

`S` 具有值类型约束，并且 `T` 具有引用类型约束，则有效。 有效地限制 `T` `System.Object`、`System.ValueType`、`System.Enum`和任何接口类型的类型。

如果类型参数的 `where` 子句包含构造函数约束（具有 `new()`格式），则可以使用 `new` 运算符来创建类型的实例（[对象创建表达式](expressions.md#object-creation-expressions)）。 用于具有构造函数约束的类型参数的任何类型参数都必须具有一个公共的无参数构造函数（此构造函数对于任何值类型都是隐式的），或是具有值类型约束或构造函数约束的类型参数（有关详细信息，请参阅[类型参数约束](classes.md#type-parameter-constraints)）。

下面是约束的示例：
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

下面的示例出错，因为它在类型参数的依赖项关系图中导致循环：
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

下面的示例演示了其他无效情况：
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

类型参数 `T` 的***有效基类***定义如下：

*  如果 `T` 没有主约束或类型参数约束，则其有效基类将 `object`。
*  如果 `T` 具有值类型约束，则其有效基类将 `System.ValueType`。
*  如果 `T` *class_type*约束 `C` 但没有*type_parameter*约束，则它的有效基类是 `C`的。
*  如果 `T` 没有*class_type*约束，但具有一个或多个*type_parameter*约束，则它的有效基类是其*type_parameter*约束的一组有效基类中包含程度最高的类型（[提升转换运算符](conversions.md#lifted-conversion-operators)）。 一致性规则确保存在这种包含最多的类型。
*  如果 `T` 同时具有*class_type*约束和一个或多个*type_parameter*约束，则它的有效基类是包含 `T` 的*class_type*约束和其*type_parameter*约束的有效基类的集中包含程度最高的类型（[提升转换运算符](conversions.md#lifted-conversion-operators)）。 一致性规则确保存在这种包含最多的类型。
*  如果 `T` 具有引用类型约束但没有*class_type*约束，则它的有效基类是 `object`的。

对于这些规则，如果 T 的约束 `V` 是*value_type*，请改用*class_type*的最特定类型 `V` 的基类型。 此操作永远不会在显式给定的约束中发生，但当通过重写方法声明或接口方法的显式实现隐式继承泛型方法时，可能会出现这种情况。

这些规则确保有效的基类始终是*class_type*。

类型参数 `T` 的***有效接口集***定义如下：

*  如果 `T` 没有*secondary_constraints*，则其有效接口集为空。
*  如果 `T` 具有*interface_type*约束但没有*type_parameter*约束，则其有效接口集为其*interface_type*约束的集合。
*  如果 `T` 没有*interface_type*约束但具有*type_parameter*约束，则其有效接口集是其*type_parameter*约束的有效接口集的并集。
*  如果 `T` 同时具有*interface_type*约束和*type_parameter*约束，则其有效接口集是其一组*interface_type*约束和其*type_parameter*约束的有效接口集的并集。

如果类型参数具有引用类型约束，或者其有效的基类不是 `object` 或 `System.ValueType`，则已知该类型参数是***引用类型***。

受约束的类型参数类型的值可用于访问约束隐含的实例成员。 示例中
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
可以在 `x` 上直接调用 `IPrintable` 方法，因为 `T` 被限制为始终实现 `IPrintable`。

### <a name="class-body"></a>类体

类的*class_body*定义该类的成员。

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>分部类型

类型声明可跨多个***分部类型声明***拆分。 类型声明是根据此部分中的规则从其部分构造的，因此在程序的编译时和运行时处理的其余部分，将它视为单个声明。

如果*class_declaration*、 *struct_declaration*或*interface_declaration*表示包含 `partial` 修饰符的分部类型声明，则为。 `partial` 不是关键字，并且仅在以下情况下充当修饰符： `class`、`struct` 或 `interface` 在类型声明中，或者在方法声明中的类型 `void` 之前出现。 在其他上下文中，可以将其用作常规标识符。

分部类型声明的每个部分都必须包含一个 `partial` 修饰符。 它必须具有相同的名称，并在与其他部分相同的命名空间或类型声明中声明。 `partial` 修饰符指示类型声明的其他部分可能存在于其他位置，但不要求存在此类附加部分;它对于具有单个声明的类型是有效的，以包含 `partial` 修饰符。

分部类型的所有部分都必须一起编译，以便可以在编译时将这些部分合并为单个类型声明。 具体而言，分部类型不允许扩展已编译的类型。

嵌套类型可以在多个部分中通过使用 `partial` 修饰符来声明。 通常，包含类型是使用 `partial` 进行声明的，并且嵌套类型的每个部分都是在包含类型的不同部分中声明的。

委托或枚举声明中不允许使用 `partial` 修饰符。

### <a name="attributes"></a>Attributes

分部类型的属性是通过将每个部件的属性按未指定顺序组合来确定的。 如果特性放置在多个部件上，则等效于在类型上多次指定属性。 例如，这两个部分：

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
等效于如下所示的声明：
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

类型参数上的属性以类似的方式进行组合。

### <a name="modifiers"></a>修饰符

当分部类型声明包括辅助功能规范（`public`、`protected`、`internal`和 `private` 修饰符）时，它必须与包括辅助功能规范的所有其他部分协商。 如果分部类型的任何部分都不包括辅助功能规范，则会为该类型提供适当的默认可访问性（已[声明的可访问](basic-concepts.md#declared-accessibility)性）。

如果嵌套类型的一个或多个分部声明包含 `new` 修饰符，则在嵌套类型隐藏继承成员（[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)）时不会报告警告。

如果某个类的一个或多个分部声明包含 `abstract` 修饰符，则将此类视为抽象类（[抽象类](classes.md#abstract-classes)）。 否则，类被视为非抽象类。

如果某个类的一个或多个分部声明包含 `sealed` 修饰符，则将此类视为 sealed （[密封类](classes.md#sealed-classes)）。 否则，类被视为未密封。

请注意，类不能既是抽象的又是密封的。

当对分部类型声明使用 `unsafe` 修饰符时，只有该特定部分被视为不安全的上下文（[不安全](unsafe-code.md#unsafe-contexts)上下文）。

### <a name="type-parameters-and-constraints"></a>类型参数和约束

如果泛型类型在多个部分中声明，则每个部分都必须声明类型参数。 每个部分必须具有相同数量的类型参数，并且每个类型参数的名称必须相同。

当部分泛型类型声明包含约束（`where` 子句）时，约束必须与包含约束的所有其他部分一致。 具体而言，包括约束的每个部分都必须具有对同一组类型参数的约束，并且对于每个类型参数，主要、次要和构造函数约束的集必须是等效的。 如果两组约束包含相同的成员，则它们是等效的。 如果部分泛型类型的任何部分都不指定类型参数约束，则类型参数被视为不受约束。

示例
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
是正确的，因为包含约束的部分（前两个）为同一组类型参数有效地指定了相同的一组主、辅助和构造函数约束。

### <a name="base-class"></a>基类

当分部类声明包含基类规范时，它必须与包含基类规范的所有其他部分协商。 如果分部类的任何部分都不包含基类规范，则基类成为 `System.Object` （[基类](classes.md#base-classes)）。

### <a name="base-interfaces"></a>基接口

在多个部分中声明的类型的基接口集是在每个部件上指定的基本接口的联合。 特定基接口的每个部分只能命名一次，但允许多个部件命名相同的基接口。 任何给定基接口的成员只能有一个实现。

示例中
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
类 `C` 的基接口集是 `IA`、`IB`和 `IC`。

通常，每个部分都提供对该部件声明的接口的实现;但这并不是必需的。 部件可以为在不同的部分声明的接口提供实现：
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Members

除了分部方法（[分部方法](classes.md#partial-methods)）以外，在多个部分中声明的类型的成员集只是每个部分中声明的成员集的并集。 类型声明的所有部分的主体共享同一声明空间（[声明](basic-concepts.md#declarations)），并且每个成员（[范围](basic-concepts.md#scopes)）的作用域都扩展到所有部分的主体。 任何成员的可访问域始终包括封闭类型的所有部分;在一个部分中声明的 `private` 成员可从另一个部件自由地访问。 在该类型的多个部分中声明同一成员是编译时错误，除非该成员是带有 `partial` 修饰符的类型。

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

类型中成员的顺序对C#代码的排序很少，但在与其他语言和环境交互时可能很重要。 在这些情况下，在多个部分中声明的类型中的成员顺序是不确定的。

### <a name="partial-methods"></a>分部方法

分部方法可在类型声明的一部分中定义，并在另一个中实现。 实现是可选的;如果没有部分实现分部方法，则将从部分组合生成的类型声明中删除分部方法声明和对它的所有调用。

分部方法不能定义访问修饰符，而是隐式 `private`。 它们的返回类型必须是 `void`的，并且其参数不能具有 `out` 修饰符。 仅当标识符 `partial` 在 `void` 类型之前出现时，才会被识别为方法声明中的特殊关键字;否则，可将其用作常规标识符。 分部方法不能显式实现接口方法。

存在两种类型的分部方法声明：如果方法声明的主体为分号，则声明声明为***定义分部方法声明***。 如果将正文作为*块*提供，则声明声明为***实现分部方法声明***。 在类型声明的各个部分中，只能有一个用给定的签名定义分部方法声明，只能有一个实现具有给定签名的分部方法声明。 如果给定了实现分部方法声明，则必须存在相应的定义分部方法声明，并且声明必须与下面指定的声明匹配：

* 声明必须具有相同的修饰符（尽管不必按相同顺序）、方法名称、类型参数的数目和参数的数目。
* 声明中的相应参数必须具有相同的修饰符（尽管不必按相同顺序）和相同的类型（在类型参数名称中取模差异）。
* 声明中的相应类型参数必须具有相同的约束（在类型参数名称中取模不同）。

实现分部方法声明可以与相应的定义分部方法声明出现在同一部分中。

只有定义分部方法参与重载决策。 因此，无论是否给定了实现声明，调用表达式都可以解析为分部方法的调用。 由于分部方法始终返回 `void`，因此此类调用表达式将始终为 expression 语句。 此外，由于分部方法是隐式 `private`的，因此，此类语句将始终出现在声明分部方法的类型声明的某个部分中。

如果分部类型声明的任何部分都不包含给定分部方法的实现声明，则从组合类型声明中只会删除调用它的任何表达式语句。 因此，调用表达式（包括任何构成表达式）在运行时不起作用。 分部方法本身也会被移除，并且不是组合类型声明的成员。

如果给定分部方法存在实现声明，则保留分部方法的调用。 分部方法提供与实现分部方法声明类似的方法声明，但以下情况除外：

* 不包含 `partial` 修饰符
* 生成的方法声明中的特性是定义的组合特性和实现分部方法声明的未指定顺序。 不会删除重复项。
* 生成的方法声明的参数上的属性为定义的相应参数的组合特性，并按未指定的顺序进行实现分部方法声明。 不会删除重复项。

如果为分部方法 M 提供定义声明，而不是实现声明，则以下限制适用：

* 创建委托到方法（[委托创建表达式](expressions.md#delegate-creation-expressions)）时出现编译时错误。
* 在转换为表达式树类型的匿名函数内引用 `M` 是编译时错误（[对表达式树类型的匿名函数转换的计算](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)）。
* 作为 `M` 调用的一部分出现的表达式不影响明确的赋值状态（[明确赋值](variables.md#definite-assignment)），这可能会导致编译时错误。
* `M` 不能是应用程序的入口点（[应用程序启动](basic-concepts.md#application-startup)）。

分部方法有助于允许类型声明的一部分自定义另一个部件的行为，例如，由工具生成的部分。 请考虑以下分部类声明：
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

如果此类是在没有任何其他部分的情况下编译的，则将删除定义分部方法声明及其调用，并且生成的组合类声明将等效于以下内容：
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

假定另外提供了一个部分，后者提供了分部方法的实现声明：
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

然后，生成的组合类声明将等效于以下内容：
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>名称绑定

尽管可扩展类型的每个部分都必须在同一个命名空间内声明，但这些部分通常是在不同的命名空间声明中编写的。 因此，每个部件可能都有不同的 `using` 指令（[使用指令](namespaces.md#using-directives)）。 解释一个部分中的简单名称（[类型推理](expressions.md#type-inference)）时，仅考虑包含该部分的命名空间声明的 `using` 指令。 这可能导致同一标识符在不同的部分中具有不同的含义：
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>类成员

类的成员由其*class_member_declaration*引入的成员和从直接基类继承的成员组成。

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

类类型的成员分为以下几个类别：

*  常量，表示与类（[常量](classes.md#constants)）关联的常量值。
*  字段，它们是类（[字段](classes.md#fields)）的变量。
*  方法，可实现类（[方法](classes.md#methods)）可以执行的计算和操作。
*  属性，定义命名特性以及与读取和写入这些特性相关联的操作。[属性](classes.md#properties)
*  事件，定义可由类生成的通知（[事件](classes.md#events)）。
*  索引器，允许以相同方式（语法为）将类的实例索引为数组（[索引器](classes.md#indexers)）。
*  运算符，用于定义可应用于类的实例（[运算符](classes.md#operators)）的表达式运算符。
*  实例构造函数，用于实现初始化类的实例所需的操作（[实例构造函数](classes.md#instance-constructors)）
*  析构函数，用于实现在永久丢弃类的实例之前要执行的操作（[析构函数](classes.md#destructors)）。
*  静态构造函数，用于实现初始化类本身所需的操作（[静态构造函数](classes.md#static-constructors)）。
*  类型，表示类的本地类型（[嵌套类型](classes.md#nested-types)）。

可以包含可执行代码的成员统称为类类型的*函数成员*。 类类型的函数成员是类类型的方法、属性、事件、索引器、运算符、实例构造函数、析构函数和静态构造函数。

*Class_declaration*创建新的声明空间（[声明](basic-concepts.md#declarations)），并且*class_declaration*立即包含的*class_member_declaration*会将新成员引入此声明空间。 以下规则适用于*class_member_declaration*：

*  实例构造函数、析构函数和静态构造函数必须具有与直接封闭类相同的名称。 所有其他成员的名称必须不同于立即封闭的类的名称。
*  常数、字段、属性、事件或类型的名称必须不同于同一个类中声明的所有其他成员的名称。
*  方法的名称必须不同于同一个类中声明的所有其他非方法的名称。 此外，方法的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)）必须不同于同一个类中声明的所有其他方法的签名，同一类中声明的两个方法的签名可能不只是 `ref` 和 `out`不同。
*  实例构造函数的签名必须不同于同一个类中声明的所有其他实例构造函数的签名，同一类中声明的两个构造函数的签名可能不只是 `ref` 和 `out`不同。
*  索引器的签名必须与同一类中声明的所有其他索引器的签名不同。
*  运算符的签名必须与同一类中声明的所有其他运算符的签名不同。

类类型的继承成员（[继承](classes.md#inheritance)）不属于类的声明空间。 因此，允许派生类声明与继承成员具有相同名称或签名的成员（实际上隐藏了继承成员）。

### <a name="the-instance-type"></a>实例类型

每个类声明都具有关联的绑定类型（[绑定类型和未绑定类型](types.md#bound-and-unbound-types)）和***实例类型***。 对于泛型类声明，实例类型通过从类型声明创建构造类型（[构造类型](types.md#constructed-types)）来形成，其中每个提供的类型参数都是对应的类型参数。 由于实例类型使用类型参数，因此只能在类型参数位于范围内时使用。也就是说，在类声明中。 实例类型是在类声明中编写的代码的 `this` 的类型。 对于非泛型类，实例类型只是声明的类。 下面显示了多个类声明及其实例类型： 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>构造类型的成员

构造类型的非继承成员是通过将成员声明中的每个*type_parameter*替换为构造类型的对应*type_argument*来获取的。 替换过程基于类型声明的语义含义，而不只是文本替换。

例如，给定泛型类声明
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
构造类型 `Gen<int[],IComparable<string>>` 具有以下成员：
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

泛型类声明中的成员 `a` 的类型 `Gen` 为 "二维数组" `T`，因此以上构造类型中的成员 `a` 类型为 "一维数组的二维数组，即 `int`" 或 "`int[,][]`"。

在实例函数成员中，`this` 的类型是包含声明的实例类型（[实例类型](classes.md#the-instance-type)）。

泛型类的所有成员都可以直接或作为构造类型的一部分，使用任何封闭类中的类型参数。 当在运行时使用特定的封闭式构造类型（[开放和闭合类型](types.md#open-and-closed-types)）时，会将类型形参的每个使用替换为提供给构造类型的实际类型实参。 例如：
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>继承

类***继承***其直接基类类型的成员。 继承意味着类隐式包含其直接基类类型的所有成员，基类的实例构造函数、析构函数和静态构造函数除外。 继承的一些重要方面包括：

*  继承是可传递的。 如果 `C` 派生自 `B`，并且 `B` 派生自 `A`，则 `C` 将继承在 `B` 中声明的成员以及在 `A`中声明的成员。
*  派生类扩展其直接基类。 派生类可以其继承的类添加新成员，但无法删除继承成员的定义。
*  实例构造函数、析构函数和静态构造函数不能继承，但所有其他成员均为，而不考虑它们的声明可访问性（[成员访问](basic-concepts.md#member-access)）。 但是，根据其声明的可访问性，继承成员可能无法在派生类中访问。
*  派生类可以通过使用相同的名称或签名声明新成员，***隐藏***（[通过继承](basic-concepts.md#hiding-through-inheritance)隐藏）继承成员。 但请注意，隐藏继承成员不会删除该成员，只是使该成员直接通过派生类无法访问。
*  类的实例包含在类及其基类中声明的所有实例字段的集合，以及从派生类类型到其任何基类类型的隐式转换（[隐式引用转换](conversions.md#implicit-reference-conversions)）。 因此，可以将对某个派生类的实例的引用视为对其任何基类的实例的引用。
*  类可以声明虚拟方法、属性和索引器，派生类可以重写这些函数成员的实现。 这使类能够显示多态行为，其中函数成员调用执行的操作取决于通过其调用该函数成员的实例的运行时类型。

构造类类型的继承成员是直接基类类型（[基类](classes.md#base-classes)）的成员，可通过使用构造类型的类型参数替换*class_base*规范中每个对应类型参数的匹配项。 这些成员又通过替换成员声明中的每个*type_parameter*来转换*class_base*规范的相应*type_argument* 。

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

在上面的示例中，构造类型 `D<int>` 具有一个非继承成员 `public int G(string s)` 通过将类型参数 `int` 替换为类型参数 `T`来获得。 `D<int>` 还具有类声明 `B`的继承成员。 此继承成员是通过以下方式确定的：通过在基类规范 `B<T[]>`中替换 `T` `int` 来确定 `D<int>` 的基类类型 `B<int[]>`。 然后，作为 `B`的类型参数，`int[]` 替换 `public U F(long index)`中的 `U`，从而生成继承成员 `public int[] F(long index)`。

### <a name="the-new-modifier"></a>New 修饰符

允许*class_member_declaration*声明与继承成员具有相同名称或签名的成员。 出现这种情况时，会说派生类成员***隐藏***基类成员。 隐藏继承成员不被视为错误，但会导致编译器发出警告。 若要禁止显示该警告，派生类成员的声明可以包含一个 `new` 修饰符，以指示该派生成员用于隐藏基成员。 本主题将在[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)中进一步进行讨论。

如果在不隐藏继承成员的声明中包括了 `new` 修饰符，则会发出对该结果的警告。 删除 `new` 修饰符后，就会禁止显示此警告。

### <a name="access-modifiers"></a>访问修饰符

*Class_member_declaration*可以具有五种可能类型的声明的可访问性（已[声明的可访问性](basic-concepts.md#declared-accessibility)）之一： `public`、`protected internal`、`protected`、`internal`或 `private`。 除了 `protected internal` 组合以外，它是一种编译时错误，用于指定多个访问修饰符。 如果*class_member_declaration*不包含任何访问修饰符，则采用 `private`。

### <a name="constituent-types"></a>构成类型

在成员的声明中使用的类型称为成员的构成类型。 可能的构成类型为常量、字段、属性、事件或索引器的类型、方法或运算符的返回类型，以及方法、索引器、运算符或实例构造函数的参数类型。 成员的构成类型必须至少与该成员本身具有相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

### <a name="static-and-instance-members"></a>静态成员和实例成员

类的成员为***静态成员***或***实例成员***。 一般来说，将静态成员视为属于类类型和属于对象的实例成员（类类型的实例）非常有用。

当字段、方法、属性、事件、运算符或构造函数声明包含 `static` 修饰符时，它将声明一个静态成员。 此外，常数或类型声明隐式声明静态成员。 静态成员具有以下特征：

*  在 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用静态成员 `M` 时，`E` 必须表示包含 `M`的类型。 `E` 表示实例，则会发生编译时错误。
*  静态字段仅标识给定封闭式类类型的所有实例共享的一个存储位置。 无论给定封闭式类类型创建了多少个实例，都只有一个静态字段副本。
*  静态函数成员（方法、属性、事件、运算符或构造函数）对特定实例不起作用，并且在此类函数成员中引用 `this` 是编译时错误。

当字段、方法、属性、事件、索引器、构造函数或析构函数声明不包含 `static` 修饰符时，它将声明一个实例成员。 （实例成员有时称为非静态成员。）实例成员具有以下特征：

*  在 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用实例成员 `M` 时，`E` 必须表示包含 `M`的类型的实例。 `E` 表示类型，则是绑定时错误。
*  类的每个实例都包含该类的所有实例字段的单独集。
*  实例函数成员（方法、属性、索引器、实例构造函数或析构函数）对类的给定实例进行操作，可以将此实例作为 `this` （[此访问](expressions.md#this-access)）进行访问。

下面的示例演示用于访问静态成员和实例成员的规则：
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

`F` 方法表明，在实例函数成员中，可使用*simple_name* （[简单名称](expressions.md#simple-names)）来访问实例成员和静态成员。 `G` 方法表明，在静态函数成员中，通过*simple_name*访问实例成员是编译时错误。 `Main` 方法表明，在*member_access* （[成员访问](expressions.md#member-access)）中，必须通过实例访问实例成员，并且必须通过类型访问静态成员。

### <a name="nested-types"></a>嵌套类型

在类或结构声明中声明的类型称为***嵌套类型***。 在编译单元或命名空间内声明的类型称为***非嵌套类型***。

示例中
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
类 `B` 是嵌套类型，因为它是在类 `A`中声明的，而类 `A` 是一个非嵌套类型，因为它是在编译单元中声明的。

#### <a name="fully-qualified-name"></a>完全限定的名称

嵌套类型的完全限定名（[完全限定](basic-concepts.md#fully-qualified-names)名）为 `S.N`，其中 `S` 是声明类型 `N` 的类型的完全限定名称。

#### <a name="declared-accessibility"></a>声明的可访问性

非嵌套类型可以具有 `public` 或 `internal` 声明的可访问性，并且默认情况下 `internal` 声明为可访问性。 嵌套类型也可以具有这些形式的声明的可访问性，还可以包含一个或多个声明的可访问性的其他形式，具体取决于包含类型是否为类或结构：

*  在类中声明的嵌套类型可以具有五种形式的声明的可访问性（`public`、`protected internal`、`protected`、`internal`或 `private`），与其他类成员一样，默认为 `private` 声明的可访问性。
*  在结构中声明的嵌套类型可以有三种形式的声明的可访问性（`public`、`internal`或 `private`），与其他结构成员一样，默认情况下 `private` 声明的可访问性。

示例
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
声明一个私有嵌套类 `Node`。

#### <a name="hiding"></a>遮盖

嵌套类型可以隐藏基成员（[命名隐藏](basic-concepts.md#name-hiding)）。 在嵌套类型声明中允许使用 `new` 修饰符，以便可以显式表达隐藏。 示例
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
显示隐藏 `Base`中定义的方法 `M` 的嵌套类 `M`。

#### <a name="this-access"></a>此访问

嵌套类型及其包含类型与*this_access* （[此访问](expressions.md#this-access)）没有特殊关系。 具体来说，不能使用嵌套类型中 `this` 引用包含类型的实例成员。 如果嵌套类型需要访问其包含类型的实例成员，则可以通过为包含类型的实例提供作为嵌套类型的构造函数参数的 `this` 来提供访问权限。 下面的示例
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
显示此方法。 实例 `C` 创建 `Nested` 的实例，并将其自己的 `this` 传递到 `Nested`的构造函数，以提供对 `C`实例成员的后续访问。

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>访问包含类型的私有和受保护成员

嵌套类型可以访问其包含类型可以访问的所有成员，包括包含类型的成员，这些成员具有 `private` 和 `protected` 声明可访问性。 示例
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
显示 `C` 包含嵌套类 `Nested`的类。 在 `Nested`中，方法 `G` 调用 `C`中定义 `F` 静态方法，`F` 具有私有声明的可访问性。

嵌套类型还可以访问在其包含类型的基类型中定义的受保护成员。 示例中
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
嵌套类 `Derived.Nested` 通过调用 `Derived`的实例来访问 `Derived`的 `Base`基类中定义的受保护的方法 `F`。

#### <a name="nested-types-in-generic-classes"></a>泛型类中的嵌套类型

泛型类声明可以包含嵌套类型声明。 可以在嵌套类型中使用封闭类的类型参数。 嵌套类型声明可以包含其他仅适用于嵌套类型的类型参数。

泛型类声明中包含的每个类型声明都是隐式的泛型类型声明。 写入嵌套在泛型类型中的类型的引用时，必须将包含构造类型（包括其类型参数）命名为。 但是，从外部类中可以使用嵌套类型而无需进行限定;构造嵌套类型时，可以隐式使用外部类的实例类型。 下面的示例演示了三种不同的方法来引用从 `Inner`创建的构造类型;前两个等效项：
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

尽管编程样式不正确，但嵌套类型中的类型参数可以隐藏在外部类型中声明的成员或类型参数：
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>保留的成员名称

为了便于基础C#运行时实现，对于作为属性、事件或索引器的每个源成员声明，实现必须根据成员声明的类型、名称和类型保留两个方法签名。 如果程序声明的成员的签名与这些保留签名之一匹配，则这是编译时错误，即使基础运行时实现不使用这些保留。

保留名称不会引入声明，因此它们不参与成员查找。 但是，声明的关联的保留方法签名确实参与了继承（[继承](classes.md#inheritance)），并且可使用 `new` 修饰符（[新修饰符](classes.md#the-new-modifier)）隐藏。

保留这些名称有三个用途：

*  如果为，则允许基础实现使用普通标识符作为方法名称，以获取或设置对C#语言功能的访问权限。
*  允许其他语言使用普通标识符作为方法名称进行互操作，以获取或设置对C#语言功能的访问权限。
*  为了帮助确保由一个符合的编译器接受的源接受另一个，通过使保留成员名称的细节在所有C#实现中保持一致。

析构函数的声明（[析构函数](classes.md#destructors)）还会导致保留签名（[为析构函数保留的成员名称](classes.md#member-names-reserved-for-destructors)）。

#### <a name="member-names-reserved-for-properties"></a>为属性保留的成员名称

对于类型 `P` 的属性[（](classes.md#properties)属性`T`），将保留以下签名：

```csharp
T get_P();
void set_P(T value);
```

即使属性是只读的或只写的，都保留两个签名。

示例中
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
类 `A` 定义只读属性 `P`，从而保留 `get_P` 和 `set_P` 方法的签名。 类 `B` 从 `A` 派生，并隐藏这两个保留的签名。 该示例生成以下输出：
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>为事件保留的成员名称

对于 `T`委托类型的事件 `E` （[事件](classes.md#events)），将保留以下签名：
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>为索引器保留的成员名称

对于类型 `T` `L`的索引器（[索引器](classes.md#indexers)），将保留以下签名：
```csharp
T get_Item(L);
void set_Item(L, T value);
```

即使索引器是只读的或只写的，也会保留两个签名。

而且，成员名称 `Item` 保留。

#### <a name="member-names-reserved-for-destructors"></a>为析构函数保留的成员名称

对于包含析构函数（[析构函数](classes.md#destructors)）的类，将保留以下签名：
```csharp
void Finalize();
```

## <a name="constants"></a>常量

***常量***是表示常量值的类成员：可在编译时计算的值。 *Constant_declaration*引入了给定类型的一个或多个常量。

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Constant_declaration*可以包含一组*特性*（[特性](attributes.md)）、一个 `new` 修饰符（[新修饰符](classes.md#the-new-modifier)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）的有效组合。 特性和修饰符适用于*constant_declaration*声明的所有成员。 即使常量被视为静态成员， *constant_declaration*也既不需要也不允许 `static` 修饰符。 同一修饰符在常数声明中多次出现是错误的。

*Constant_declaration*的*类型*指定声明引入的成员类型。 该类型后跟一个*constant_declarator*s 列表，其中每个都引入一个新成员。 *Constant_declarator*由命名成员的*标识符*组成，后跟一个 "`=`" 标记，后跟一个提供成员值*constant_expression* （[常数表达式](expressions.md#constant-expressions)）。

在常量声明中指定的*类型*必须是 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`string`、 *enum_type*或*reference_type*。 每个*constant_expression*必须生成一个目标类型的值，或一个可通过隐式转换转换为目标类型的类型（[隐式转换](conversions.md#implicit-conversions)）。

常数的*类型*必须至少具有与常量本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

常量的值是使用*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）在表达式中获取的。

常数本身可以参与*constant_expression*。 因此，在需要*constant_expression*的任何构造中都可以使用常量。 此类构造的示例包括 `case` 标签、`goto case` 语句、`enum` 成员声明、特性和其他常量声明。

如[常数表达式](expressions.md#constant-expressions)中所述， *constant_expression*是可在编译时完全计算的表达式。 由于不 `string` 的*reference_type*的非 null 值的唯一创建方法是应用 `new` 运算符，因此，由于*constant_expression*不允许使用 `new` 运算符，因此 reference_type 之外*的 `string` 的*常量的唯一可能值为 `null`。

如果需要常数值的符号名称，但在常数声明中不允许该值的类型，或者无法在*constant_expression*编译时计算该值，则可以改用 `readonly` 字段（[Readonly 字段](classes.md#readonly-fields)）。

声明多个常量的常量声明等效于多个具有相同属性、修饰符和类型的单个常量声明。 例如
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
等效于
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

只要依赖项不属于循环本质，就允许常量依赖于同一个程序中的其他常量。 编译器会自动排列，以适当的顺序计算常量声明。 示例中
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
编译器首先计算 `A.Y`，然后计算 `B.Z`，最后计算 `A.X`，并 `10`、`11`和 `12`生成值。 常数声明可能依赖于其他程序中的常量，但这种依赖关系只能在一个方向上进行。 参考上面的示例，如果在不同的程序中声明了 `A` 和 `B`，则 `A.X` 可能依赖于 `B.Z`，但 `B.Z` 随后不会同时依赖于 `A.Y`。

## <a name="fields"></a>字段

***字段***是表示与对象或类关联的变量的成员。 *Field_declaration*引入了给定类型的一个或多个字段。

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

*Field_declaration*可以包含一组*属性*（[特性](attributes.md)）、一个 `new` 修饰符（[新修饰符](classes.md#the-new-modifier)）、四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）和一个 `static` 修饰符（[静态和实例字段](classes.md#static-and-instance-fields)）的有效组合。 此外， *field_declaration*可以包括 `readonly` 修饰符（[Readonly 字段](classes.md#readonly-fields)）或 `volatile` 修饰符（[可变字段](classes.md#volatile-fields)），但不能同时包含两者。 特性和修饰符适用于*field_declaration*声明的所有成员。 同一修饰符在字段声明中多次出现是错误的。

*Field_declaration*的*类型*指定声明引入的成员类型。 该类型后跟一个*variable_declarator*s 列表，其中每个都引入一个新成员。 *Variable_declarator*包含一个标识符，*该标识符*对成员进行命名，可选择后跟 "`=`" 标记和提供该成员的初始值的*variable_initializer* （[变量初始值设定项](classes.md#variable-initializers)）。

字段的*类型*必须至少与字段本身具有相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

字段的值是在表达式中使用*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）获取的。 使用*赋值*（[赋值运算符](expressions.md#assignment-operators)）修改非只读字段的值。 非只读字段的值可以使用后缀增量和减量运算符（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)）以及前缀增量和减量运算符（[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）来获取和修改。

声明多个字段的字段声明等效于具有相同属性、修饰符和类型的单个字段的多个声明。 例如
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
等效于
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>静态和实例字段

当字段声明包括 `static` 修饰符时，该声明引入的字段是***静态字段***。 如果不存在 `static` 修饰符，则声明引入的字段为***实例字段***。 静态字段和实例字段是受支持的几种变量（[变量](variables.md)） C#中的两种，有时它们分别被称为***静态变量***和***实例变量***。

静态字段不是特定实例的一部分;而是在已关闭类型的所有实例（[开放式类型和已关闭类型](types.md#open-and-closed-types)）之间共享。 无论已创建已关闭类类型的多少实例，都只有一个静态字段副本用于关联的应用程序域。

例如：
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

实例字段属于实例。 具体而言，类的每个实例都包含该类的所有实例字段的单独集。

如果在窗体 `E.M`的*member_access* （[成员访问](expressions.md#member-access)）中引用字段，`M` 为静态字段，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例字段，则 E 必须表示包含 `M`的类型的实例。

静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。

### <a name="readonly-fields"></a>只读字段

当*field_declaration*包含 `readonly` 修饰符时，由声明引入的字段为***只读字段***。 向 readonly 字段的直接赋值只能作为声明的一部分出现，或者出现在同一类的实例构造函数或静态构造函数中。 （Readonly 字段可以在这些上下文中指定多次。）具体而言，仅允许在以下上下文中对 `readonly` 字段进行直接赋值：

*  在引入字段的*variable_declarator*中（通过在声明中包含*variable_initializer* ）。
*  对于实例字段，在包含字段声明的类的实例构造函数中;对于静态字段，在包含字段声明的类的静态构造函数中。 它们也是将 `readonly` 字段作为 `out` 或 `ref` 参数传递的唯一上下文。

尝试分配到 `readonly` 字段，或在任何其他上下文中将其作为 `out` 或 `ref` 参数传递是编译时错误。

#### <a name="using-static-readonly-fields-for-constants"></a>使用常量的静态 readonly 字段

如果需要常数值的符号名称，但在 `const` 声明中不允许使用值的类型，或者在编译时无法计算该值时，`static readonly` 字段非常有用。 示例中
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
`Black`、`White`、`Red`、`Green`和 `Blue` 成员不能声明为 `const` 成员，因为它们的值不能在编译时进行计算。 但是，将它们声明 `static readonly` 的效果大致相同。

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>常量和静态 readonly 字段的版本控制

常量和 readonly 字段的二进制版本控制语义不同。 当表达式引用常量时，将在编译时获取常量的值，但当表达式引用 readonly 字段时，不会在运行时获取字段的值。 假设应用程序包含两个单独的程序：
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

`Program1` 和 `Program2` 命名空间表示单独编译的两个程序。 由于 `Program1.Utils.X` 声明为静态只读字段，因此 `Console.WriteLine` 语句输出的值在编译时是未知的，而是在运行时获取。 因此，如果更改了 `X` 的值并重新编译了 `Program1`，则 `Console.WriteLine` 语句将输出新值，即使未重新编译 `Program2` 也是如此。 但是，`X` 是一个常量，则 `X` 的值将在编译 `Program2` 时获得，并将不受 `Program1` 中的更改的影响，直到重新编译 `Program2` 为止。

### <a name="volatile-fields"></a>可变字段

当*field_declaration*包含 `volatile` 修饰符时，该声明引入的字段是***可变字段***。

对于非易失性字段，重新排序指令的优化方法可能导致意外且不可预知的结果，这会导致多线程程序在没有同步的情况下（如*lock_statement*提供的字段）访问字段（[lock 语句](statements.md#the-lock-statement)）。 这些优化可以由编译器、运行时系统或硬件执行。 对于可变字段，此类重新排序优化将受到限制：

*  读取可变字段称为***易失性读取***。 可变读取具有 "获取语义";也就是说，保证在对指令序列中出现其后的内存进行引用之前发生。
*  可变字段的写入称为***可变写入***。 可变写入具有 "发布语义";也就是说，保证在指令序列中写入指令之前的任何内存引用后发生。

这些限制确保所有线程观察易失性写入操作（由任何其他线程执行）时的观察顺序与写入操作的执行顺序一致。 一致性实现不需要提供可变写入的单个总体顺序，如所有执行线程中所示。 可变字段的类型必须是以下类型之一：

*  一个*reference_type*。
*  类型 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`char`、`float`、`bool`、`System.IntPtr`或 `System.UIntPtr`。
*  具有 `byte`、`sbyte`、`short`、`ushort`、`int`或 `uint`的枚举基类型的*enum_type* 。

示例
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
生成输出：
```console
result = 143
```

在此示例中，方法 `Main` 启动一个新线程，该线程运行 `Thread2`的方法。 此方法将值存储到名为 `result`的非易失性字段中，然后将 `true` 存储在可变字段 `finished`中。 主线程等待字段 `finished` 设置为 `true`，然后读取 `result`的字段。 由于已 `volatile`声明 `finished`，因此主线程必须从字段 `result`读取值 `143`。 如果未 `volatile`声明字段 `finished`，则允许存储区 `result` 在存储到 `finished`后对主线程可见，因此，主线程将从该字段 `0` 中读取值 `result`。 将 `finished` 声明为 `volatile` 字段可防止任何此类不一致。

### <a name="field-initialization"></a>字段初始化

字段的初始值，无论是静态字段还是实例字段，都是字段的类型的默认值（[默认值](variables.md#default-values)）。 在发生此默认初始化之前，不可能观察字段的值，并且字段永远不会 "未初始化"。 示例
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
生成输出
```console
b = False, i = 0
```
因为 `b` 和 `i` 都自动初始化为默认值。

### <a name="variable-initializers"></a>变量初始值设定项

字段声明可能包括*variable_initializer*。 对于静态字段，变量初始值设定项对应于在类初始化过程中执行的赋值语句。 对于实例字段，变量初始值设定项对应于创建类的实例时执行的赋值语句。

示例
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
生成输出
```console
x = 1.4142135623731, i = 100, s = Hello
```
由于在执行实例字段初始值设定项时，会执行静态字段初始值设定项并分配给 `i` 并 `s`，因此，对 `x` 的赋值。

所有字段都将发生[字段初始化](classes.md#field-initialization)中所述的默认值初始化，其中包括具有变量初始值设定项的字段。 因此，初始化某个类时，该类中的所有静态字段都首先初始化为其默认值，然后以文本顺序执行静态字段初始值设定项。 同样，在创建类的实例时，该实例中的所有实例字段都将首先初始化为其默认值，然后以文本顺序执行实例字段初始值设定项。

具有变量初始值设定项的静态字段可以在其默认值状态中观察到。 不过，强烈建议不要使用这种样式。 示例
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
展示了这种行为。 尽管 a 和 b 的循环定义，但程序有效。 这会导致输出
```console
a = 1, b = 2
```
由于静态字段 `a` 和 `b` 会在其初始值设定项执行之前初始化为 `0` （`int`的默认值）。 当 `a` 的初始值设定项运行时，`b` 的值为零，因此 `a` 初始化为 `1`。 当 `b` 的初始值设定项运行时，`a` 的值已 `1`，因此 `b` 初始化为 `2`。

#### <a name="static-field-initialization"></a>静态字段初始化

类的静态字段变量初始值设定项对应于按其在类声明中出现的文本顺序执行的一系列赋值。 如果类中存在静态构造函数（[静态](classes.md#static-constructors)构造函数），则在执行该静态构造函数之前，会立即执行静态字段初始值设定项。 否则，在首次使用该类的静态字段之前，将在依赖于实现的时间执行静态字段初始值设定项。 示例
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
可能产生以下输出：
```console
Init A
Init B
1 1
```
或输出：
```console
Init B
Init A
1 1
```
由于 `X`的初始值设定项和 `Y`的初始值设定项的执行可能会按以下顺序发生：它们只会在对这些字段的引用之前发生。 但在示例中：
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
输出必须为：
```console
Init B
Init A
1 1
```
由于静态构造函数的执行规则（在[静态构造函数](classes.md#static-constructors)中定义），因此必须在 `A`的静态构造函数和字段初始值设定项之前运行 `B`的静态构造函数（因此 `B`的静态字段初始值设定项）。

#### <a name="instance-field-initialization"></a>实例字段初始化

类的实例字段变量初始值设定项对应于进入该类的任何一个实例构造函数（[构造函数初始值设定](classes.md#constructor-initializers)项）后立即执行的一系列赋值。 变量初始值设定项将按照它们在类声明中的显示顺序执行。 [实例构造函数](classes.md#instance-constructors)中对类实例创建和初始化过程进行了进一步说明。

实例字段的变量初始值设定项不能引用正在创建的实例。 因此，在变量初始值设定项中引用 `this` 是编译时错误，因为变量初始值设定项通过*simple_name*引用任何实例成员时出现编译时错误。 示例中
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
`y` 的变量初始值设定项会导致编译时错误，因为它引用正在创建的实例的成员。

## <a name="methods"></a>方法

***方法***是实现对象或类可执行的计算或操作的成员。 方法使用*method_declaration*s 来声明：

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

*Method_declaration*可以包含一组*属性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`static` （[静态和实例方法](classes.md#static-and-instance-methods)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。

如果满足以下所有条件，则声明具有有效的修饰符组合：

*  声明中包含有效的访问修饰符（[访问修饰符](classes.md#access-modifiers)）组合。
*  声明不包含同一修饰符多次。
*  声明最多包含以下修饰符之一： `static`、`virtual`和 `override`。
*  声明最多包含以下修饰符之一： `new` 和 `override`。
*  如果声明包含 `abstract` 修饰符，则声明不包括以下任何修饰符： `static`、`virtual`、`sealed` 或 `extern`。
*  如果声明包含 `private` 修饰符，则声明不包括以下任何修饰符： `virtual`、`override`或 `abstract`。
*  如果声明包含 `sealed` 修饰符，则声明还包括 `override` 修饰符。
*  如果声明包含 `partial` 修饰符，则不包含以下任何修饰符： `new`、`public`、`protected`、`internal`、`private`、`virtual`、`sealed`、`override`、`abstract`或 `extern`。

具有 `async` 修饰符的方法是一个异步函数，并遵循[异步函数](classes.md#async-functions)中所述的规则。

方法声明的*return_type*指定由方法计算并返回的值的类型。 如果方法不返回值，则 `void` *return_type* 。 如果声明包含 `partial` 修饰符，则返回类型必须为 `void`。

*Member_name*指定方法的名称。 除非此方法是显式接口成员实现（[显式接口成员实现](interfaces.md#explicit-interface-member-implementations)），否则*member_name*只是一个*标识符*。 对于显式接口成员实现， *member_name*由*interface_type*后跟 "`.`" 和*标识符*组成。

可选*type_parameter_list*指定方法的类型参数（[类型参数](classes.md#type-parameters)）。 如果指定了*type_parameter_list* ，则该方法是***泛型方法***。 如果该方法具有 `extern` 修饰符，则无法指定*type_parameter_list* 。

可选*formal_parameter_list*指定方法的参数（[方法参数](classes.md#method-parameters)）。

可选*type_parameter_constraints_clause*指定对单个类型参数的约束（[类型参数约束](classes.md#type-parameter-constraints)），并且仅当还提供了*type_parameter_list*并且方法没有 `override` 修饰符时，才可以指定。

在方法的*formal_parameter_list*中引用的*return_type*和每个类型必须至少具有与方法本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

*Method_body*为分号、***语句体***或***表达式主体***。 语句体由*块*组成，它指定在调用方法时要执行的语句。 表达式主体由 `=>` 后跟*表达式*和分号组成，并表示在调用方法时要执行的单个表达式。 

对于 `abstract` 和 `extern` 方法， *method_body*只包含一个分号。 对于 `partial` 方法， *method_body*可能包含分号、块体或表达式主体。 对于所有其他方法， *method_body*为块体或表达式主体。

如果*method_body*包含分号，则声明可能不包括 `async` 修饰符。

方法的名称、类型参数列表和形参列表定义方法的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)）。 具体而言，方法的签名由其名称、类型参数的数目以及其形参的数量、修饰符和类型组成。 出于这些目的，形参的类型中出现的方法的任何类型形参均由其名称标识，但其在方法的类型实参列表中的序号位置。返回类型不是方法签名的一部分，也不是类型参数或形参的名称。

方法的名称必须不同于同一个类中声明的所有其他非方法的名称。 此外，方法的签名必须不同于同一个类中声明的所有其他方法的签名，同一类中声明的两个方法的签名可能不只是 `ref` 和 `out`不同。

此方法的*type_parameter*在整个*method_declaration*的范围内，可用于在*return_type*、 *method_body*和*type_parameter_constraints_clause*中的整个范围内形成类型，而不是在*属性*中。

所有形参和类型形参必须具有不同的名称。

### <a name="method-parameters"></a>方法参数

方法的参数（如果有）由方法的*formal_parameter_list*声明。

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

形参列表包含一个或多个以逗号分隔的参数，其中只有最后一个参数可以是*parameter_array*。

*Fixed_parameter*包含一组可选的*特性*（[特性](attributes.md)）、可选的 `ref`、`out` 或 `this` 修饰符、*类型*、*标识符*和可选的*default_argument*。 每个*fixed_parameter*都声明具有给定名称的给定类型的参数。 `this` 修饰符将方法指定为扩展方法，并且只允许在静态方法的第一个参数上使用。 [扩展方法中进一步](classes.md#extension-methods)介绍了扩展方法。

使用*default_argument*的*fixed_parameter*称为***可选参数***，而没有*default_argument*的*fixed_parameter*是***必需参数***。 必需的参数不能出现在*formal_parameter_list*中的可选参数之后。

`ref` 或 `out` 参数不能具有*default_argument*。 *Default_argument*中的*表达式*必须是下列其中一项：

*  一个*constant_expression*
*  格式 `new S()` 其中 `S` 为值类型的表达式
*  格式 `default(S)` 其中 `S` 为值类型的表达式

*表达式*必须可通过标识或可为 null 的类型转换为参数的类型。

如果在实现分部方法声明（[分部方法](classes.md#partial-methods)）中出现可选参数，则显式接口成员实现（[显式接口成员实现](interfaces.md#explicit-interface-member-implementations)），或在单参数索引器声明（[索引器](classes.md#indexers)）中出现警告，因为这些成员永远无法通过允许省略参数的方式进行调用。

*Parameter_array*包含一组可选的*特性*（[特性](attributes.md)）、`params` 修饰符、 *array_type*和*标识符*。 参数数组声明具有给定名称的给定数组类型的单一参数。 参数数组的*array_type*必须为一维数组类型（[数组类型](arrays.md#array-types)）。 在方法调用中，参数数组允许指定给定数组类型的单个自变量，或允许指定数组元素类型的零个或多个参数。 参数数组中进一步介绍了参数[数组。](classes.md#parameter-arrays)

*Parameter_array*可能会在可选参数之后发生，但不能具有默认值，而是省略*parameter_array*的参数会导致创建一个空数组。

下面的示例演示了不同类型的参数：
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

在 `M`的*formal_parameter_list*中，`i` 是必需的 ref 参数，`d` 是必需的值参数，`b`、`s`、`o` 和 `t` 是可选值参数，`a` 是参数数组。

方法声明为参数、类型参数和局部变量创建了单独的声明空间。 名称通过方法的类型形参列表和形参列表以及方法*块*中的局部变量声明引入此声明空间。 方法声明空间的两个成员具有相同的名称是错误的。 如果嵌套声明空间的方法声明空间和局部变量声明空间包含具有相同名称的元素，则是错误的。

方法调用（[方法调用](expressions.md#method-invocations)）创建特定于该方法的形参和局部变量的副本，并且调用的参数列表将值或变量引用分配给新创建的形参。 在方法的*块*中，可以通过参数*Simple_name*表达式（[简单名称](expressions.md#simple-names)）中的标识符引用形参。

有四种形参：

*  值参数，在没有任何修饰符的情况下声明。
*  引用参数，它们是用 `ref` 修饰符声明的。
*  输出参数，它们是用 `out` 修饰符声明的。
*  参数数组，用 `params` 修饰符声明。

如[签名和重载](basic-concepts.md#signatures-and-overloading)中所述，`ref` 和 `out` 修饰符是方法签名的一部分，但是 `params` 修饰符不是。

#### <a name="value-parameters"></a>值参数

使用不带修饰符声明的参数是一个值参数。 值参数对应于一个局部变量，该局部变量从方法调用中提供的相应参数获取其初始值。

如果形参为值形参，则方法调用中的相应参数必须是可隐[式转换为](conversions.md#implicit-conversions)形参类型的表达式。

允许方法为值参数赋值。 此类分配只会影响 value 参数所表示的本地存储位置，它们对方法调用中给定的实参不起作用。

#### <a name="reference-parameters"></a>引用参数

使用 `ref` 修饰符声明的参数是一个引用参数。 与值参数不同，引用参数不会创建新的存储位置。 相反，引用参数表示作为方法调用中的自变量提供的变量所在的存储位置。

如果形参是引用参数，则方法调用中的相应参数必须包含关键字 `ref` 后跟与形参相同的类型的*variable_reference* （[确定明确赋值的确切规则](variables.md#precise-rules-for-determining-definite-assignment)）。 在将变量作为引用参数传递之前，必须对其进行明确赋值。

在方法中，引用参数始终被视为明确赋值。

声明为迭代器（[迭代](classes.md#iterators)器）的方法不能有引用参数。

示例
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
生成输出
```console
i = 2, j = 1
```

对于 `Main`中 `Swap` 的调用，`x` 表示 `i`，`y` 表示 `j`。 因此，调用具有交换 `i` 和 `j`的值的效果。

在采用引用参数的方法中，多个名称可以表示相同的存储位置。 示例中
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
`G` 中的 `F` 的调用同时为 `a` 和 `b`传递了对 `s` 的引用。 因此，对于该调用，名称 `s`、`a`和 `b` 都引用相同的存储位置，三个分配都将修改实例字段 `s`。

#### <a name="output-parameters"></a>输出参数

使用 `out` 修饰符声明的参数是一个输出参数。 与引用参数类似，output 参数不会创建新的存储位置。 相反，output 参数表示作为方法调用中的自变量提供的变量所在的存储位置。

如果形参为 output 参数，则方法调用中的相应参数必须包含关键字 `out` 后跟与形参相同的类型的*variable_reference* （[确定明确赋值的确切规则](variables.md#precise-rules-for-determining-definite-assignment)）。 在将变量作为 output 参数传递之前，不需要明确赋值，但在将变量作为 output 参数传递的调用之后，该变量被视为明确赋值。

在方法中，就像局部变量一样，output 参数最初被视为未分配，在使用其值之前必须进行明确赋值。

方法返回之前，必须为方法的每个输出参数进行明确赋值。

声明为分部方法（[分部方法](classes.md#partial-methods)）或迭代器（[迭代](classes.md#iterators)器）的方法不能有输出参数。

输出参数通常用于生成多个返回值的方法。 例如：
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

该示例生成以下输出：
```console
c:\Windows\System\
hello.txt
```

请注意，可以在将 `dir` 和 `name` 变量传递到 `SplitPath`之前取消对它们的分配，并且这些变量在调用后被视为明确赋值。

#### <a name="parameter-arrays"></a>参数数组

使用 `params` 修饰符声明的参数是一个参数数组。 如果形参表包含参数数组，则它必须是列表中的最后一个形参，并且必须是一维数组类型。 例如，`string[]` 和 `string[][]` 的类型可用作参数数组的类型，但类型 `string[,]` 不能。 不能将 `params` 修饰符与修饰符结合 `ref` 和 `out`。

参数数组允许在方法调用中使用以下两种方法之一指定参数：

*  为参数数组提供的参数可以是可隐[式转换为](conversions.md#implicit-conversions)参数数组类型的单个表达式。 在这种情况下，参数数组的作用与值参数完全相同。
*  此外，调用可以为参数数组指定零个或多个参数，其中每个参数都是可隐[式转换为](conversions.md#implicit-conversions)参数数组元素类型的表达式。 在这种情况下，调用会创建一个参数数组类型的实例，该实例的长度与参数的数量相对应，使用给定的参数值初始化数组实例的元素，并使用新创建的数组实例作为实际实际.

除了允许在调用中使用可变数量的自变量，参数数组完全等效于同一类型的值参数（[值参数](classes.md#value-parameters)）。

示例
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
生成输出
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

`F` 的第一次调用只是将数组作为值参数传递 `a`。 第二次调用 `F` 会自动创建包含给定元素值的四元素 `int[]` 并将该数组实例作为值参数进行传递。 同样，第三次调用 `F` 会创建一个零元素 `int[]` 并将该实例作为值参数进行传递。 第二次和第三次调用完全等效于写入：
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

在执行重载决策时，具有参数数组的方法可能以其普通形式或其展开形式（[适用的函数成员](expressions.md#applicable-function-member)）的形式应用。 仅当方法的正常形式不适用，并且仅当与展开的窗体具有相同签名的适用方法未在同一类型中声明时，方法的展开形式才可用。

示例
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
生成输出
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

在此示例中，具有参数数组的方法的两个可能的展开形式都已作为常规方法包含在类中。 因此，在执行重载决策时不考虑这些扩展窗体，第一个和第三个方法调用会选择常规方法。 当类声明具有参数数组的方法时，也不太常见，还应将一些展开形式包含为常规方法。 这样做可以避免在调用具有参数数组的方法的扩展形式时，分配发生的数组实例。

当参数数组的类型 `object[]`时，方法的正常形式和单个 `object` 参数的所花费形式之间可能存在歧义。 歧义的原因是，`object[]` 本身可隐式转换为类型 `object`。 但这种歧义不会带来任何问题，因为它可以通过在需要时插入强制转换来解决。

示例
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
生成输出
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

在 `F`的第一次和最后一次调用中，`F` 的正常形式适用，因为存在从实参类型到形参类型的隐式转换（两者都是 `object[]`类型）。 因此，重载决策选择 `F`的正常形式，并将参数作为常规值参数进行传递。 在第二次和第三次调用中，`F` 的正常形式不适用，因为不存在从实参类型到形参类型的隐式转换（类型 `object` 无法隐式转换为类型 `object[]`）。 但 `F` 的展开形式适用，因此重载决策选择它。 因此，调用会创建一个单元素 `object[]`，并使用给定的参数值（其本身是对 `object[]`的引用）初始化数组的单个元素。

### <a name="static-and-instance-methods"></a>静态和实例方法

当方法声明包含 `static` 修饰符时，该方法被称为静态方法。 如果不存在 `static` 修饰符，则称该方法为实例方法。

静态方法不在特定实例上操作，并且在静态方法中引用 `this` 是编译时错误。

实例方法对类的给定实例进行操作，并且该实例可以作为 `this` （[此访问](expressions.md#this-access)）进行访问。

当 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用方法时，如果 `M` 为静态方法，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例方法，则 `E` 必须表示包含 `M`的类型的实例。

静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。

### <a name="virtual-methods"></a>虚方法

当实例方法声明包含 `virtual` 修饰符时，该方法被称为虚拟方法。 如果不存在 `virtual` 修饰符，则称该方法为非虚拟方法。

非虚方法的实现是不变的，无论是在声明方法的类的实例上调用方法，还是在派生类的实例中调用方法，实现都是相同的。 相反，虚方法的实现可由派生类取代。 取代继承的虚方法的实现的过程称为***重写***该方法（[重写方法](classes.md#override-methods)）。

在虚方法调用中，调用所针对的实例的***运行时类型***将确定要调用的实际方法实现。 在非虚拟方法调用中，实例的***编译时类型***是确定因素。 确切地说，当使用参数 `A` 列表调用名为 `N` 的方法时，在编译时类型 `C` 和运行时类型 `R` （其中 `R` 是 `C` 或派生自 `C`的类）时，将按如下所示处理调用：

*  首先，将重载决策应用于 `C`、`N`和 `A`，以便从在中声明的方法集中选择特定方法 `M`，并通过 `C`继承。 [方法调用](expressions.md#method-invocations)中对此进行了说明。
*  如果 `M` 为非虚拟方法，则调用 `M`。
*  否则，`M` 是一个虚拟方法，将调用与 `R` 有关的 `M` 的派生程度最高的实现。

对于在类中声明或继承的每个虚方法，存在与该类相关的方法的***最大派生实现***。 与类 `R` 相关 `M` 的虚拟方法的派生程度最高的实现如下所示：

*  如果 `R` 包含 `M`的声明引入 `virtual`，则这是 `M`的派生程度最高的实现。
*  否则，如果 `R` 包含 `M`的 `override`，则这是 `M`的派生程度最高的实现。
*  否则，与 `R` 的 `M` 的派生程度最高的实现与 `R`的直接基类的 `M` 的派生程度相同。

下面的示例阐释了虚方法和非虚方法之间的差异：
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

在此示例中，`A` 引入了一个非虚方法 `F` 和一个虚方法 `G`。 类 `B` 引入了新的非虚方法 `F`，从而隐藏了继承的 `F`，并且还会重写继承的方法 `G`。 该示例生成以下输出：
```console
A.F
B.F
B.G
B.G
```

请注意，语句 `a.G()` 调用 `B.G`，而不是 `A.G`。 这是因为实例的运行时类型（`B`），而不是实例的编译时类型（即 `A`）确定要调用的实际方法实现。

由于允许方法隐藏继承方法，因此类可能包含多个具有相同签名的虚拟方法。 这并不会造成多义性问题，因为所有派生方法都是隐藏的。 示例中
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
`C` 和 `D` 类包含两个具有相同签名的虚拟方法： `A` 引入的两个虚拟方法，以及 `C`引入的两种方法。 `C` 引入的方法隐藏了继承自 `A`的方法。 因此，`D` 中的重写声明会重写 `C`引入的方法，并且 `D` 重写 `A`引入的方法。 该示例生成以下输出：
```console
B.F
B.F
D.F
D.F
```

请注意，可以通过不隐藏方法的派生程度较低的类型访问 `D` 实例，从而调用隐藏的虚拟方法。

### <a name="override-methods"></a>重写方法

当实例方法声明包含 `override` 修饰符时，该方法被称为***替代方法***。 重写方法使用相同的签名重写继承的虚方法。 但如果虚方法声明中引入新方法，重写方法声明通过提供相应方法的新实现代码，专门针对现有的继承虚方法。

`override` 声明重写的方法称为***重写基方法***。 对于在类 `C`中声明 `M` 重写方法，重写的基方法是通过检查 `C`的每个基类类型（从 `C` 的直接基类类型开始，并继续每个连续的直接基类类型）确定的，直到至少有一个可访问方法与替换类型参数后，该方法的签名与 `M` 具有相同的签名。 为了定位重写的基方法，如果 `protected``public`，则会认为该方法是可访问的（如果它是 `protected internal`的）; 或者，如果它是在 `C`的同一程序中 `internal` 并声明的，则认为该方法是可访问的。

除非以下所有条件都适用于重写声明，否则将发生编译时错误：

*  重写的基方法可以按上文所述进行定位。
*  正好有一个这种重写基方法。 仅当基类类型为构造类型时，此限制才会生效，在这种情况下，类型参数的替换会使两个方法的签名相同。
*  重写的基方法为虚拟、抽象或重写方法。 换言之，重写的基方法不能是静态的或非虚的。
*  重写的基方法不是密封的方法。
*  重写方法和重写的基方法具有相同的返回类型。
*  重写声明和重写的基方法具有相同的声明的可访问性。 换言之，重写声明不能更改虚拟方法的可访问性。 但是，如果重写的基方法是受保护的内部方法，并且在与包含 override 方法的程序集不同的程序集中声明，则必须保护 override 方法的声明的可访问性。
*  重写声明不指定类型参数约束子句。 相反，约束是从基方法继承而来的。 请注意，在已重写的方法中属于类型参数的约束可能会替换为继承约束中的类型参数。 当显式指定时，这可能导致约束不合法，如值类型或密封类型。

下面的示例演示重写规则如何适用于泛型类：
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

重写声明可使用*base_access* （[基本访问](expressions.md#base-access)）访问重写基方法。 示例中
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
`B` 中的 `base.PrintFields()` 调用调用在 `A`中声明的 `PrintFields` 方法。 *Base_access*禁用虚拟调用机制，只是将基方法视为非虚拟方法。 在 `B` 中编写调用后 `((A)this).PrintFields()`，它将以递归方式调用在 `B`中声明的 `PrintFields` 方法，而不是在 `A`中声明的方法，因为 `PrintFields` 是虚拟的，`((A)this)` 的运行时类型 `B`。

只有通过包含 `override` 修饰符，方法才能重写另一个方法。 在所有其他情况下，具有与继承方法相同的签名的方法只是隐藏了继承方法。 示例中
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
`B` 中的 `F` 方法不包括 `override` 修饰符，因此不会重写 `A`中的 `F` 方法。 相反，`B` 中的 `F` 方法隐藏 `A`中的方法，并且会报告警告，因为声明不包括 `new` 修饰符。

示例中
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
`B` 中的 `F` 方法隐藏了从 `A`继承的虚拟 `F` 方法。 由于 `B` 中的新 `F` 具有私有访问权限，因此其范围仅包括 `B` 的类体，不会扩展到 `C`。 因此，允许在 `C` 中 `F` 的声明重写继承自 `A`的 `F`。

### <a name="sealed-methods"></a>密封方法

当实例方法声明包含 `sealed` 修饰符时，该方法被称为***密封方法***。 如果实例方法声明包含 `sealed` 修饰符，则它还必须包括 `override` 修饰符。 使用 `sealed` 修饰符可防止派生类进一步重写方法。

示例中
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
类 `B` 提供两个重写方法：具有 `sealed` 修饰符的 `F` 方法和不具有的 `G` 方法。 `B`使用密封的 `modifier` 可防止 `C` 进一步重写 `F`。

### <a name="abstract-methods"></a>抽象方法

当实例方法声明包含 `abstract` 修饰符时，该方法被称为***抽象方法***。 尽管抽象方法还隐式成为虚方法，但它不能具有修饰符 `virtual`。

抽象方法声明引入新的虚方法，但不提供该方法的实现。 相反，非抽象派生类需要通过重写方法来提供自己的实现。 因为抽象方法不提供实际的实现，所以抽象方法的*method_body*只包含一个分号。

抽象方法声明仅允许在抽象类（[抽象类](classes.md#abstract-classes)）中使用。

示例中
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
`Shape` 类定义可自行绘制的几何形状对象的抽象概念。 `Paint` 方法是抽象的，因为没有有意义的默认实现。 `Ellipse` 和 `Box` 类是具体的 `Shape` 实现。 由于这些类是非抽象类，因此需要重写 `Paint` 方法，并提供实际的实现。

*Base_access* （[基本访问](expressions.md#base-access)）引用抽象方法的编译时错误。 示例中
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
为 `base.F()` 调用报告编译时错误，因为它引用了抽象方法。

允许抽象方法声明重写虚拟方法。 这允许抽象类强制在派生类中重新实现方法，并使方法的原始实现不可用。 示例中
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
类 `A` 声明虚方法，类 `B` 使用抽象方法重写此方法，类 `C` 重写抽象方法以提供其自己的实现。

### <a name="external-methods"></a>外部方法

当方法声明包含 `extern` 修饰符时，该方法被称为***外部方法***。 外部方法在外部实现，通常使用以外的语言C#。 由于外部方法声明不提供实际的实现，因此外部方法的*method_body*只包含一个分号。 外部方法不能是泛型的。

`extern` 修饰符通常与 `DllImport` 特性结合使用（[与 COM 和 Win32 组件互操作](attributes.md#interoperation-with-com-and-win32-components)），允许 Dll （动态链接库）实现外部方法。 执行环境可能支持可提供外部方法实现的其他机制。

当外部方法包含 `DllImport` 特性时，该方法声明还必须包含 `static` 修饰符。 此示例演示了 `extern` 修饰符和 `DllImport` 属性的用法：
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>分部方法（概述）

当方法声明包含 `partial` 修饰符时，该方法被称为***分部方法***。 分部方法只能声明为分部类型的成员（[分部类型](classes.md#partial-types)），并且受到多种限制。 [分部方法中进一步](classes.md#partial-methods)介绍了分部方法。

### <a name="extension-methods"></a>扩展方法

如果方法的第一个参数包含 `this` 修饰符，则称该方法是***扩展方法***。 扩展方法只能在非泛型非嵌套静态类中声明。 扩展方法的第一个参数不能包含除 `this`以外的任何修饰符，并且参数类型不能是指针类型。

下面是一个声明两个扩展方法的静态类示例：
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

扩展方法是一个常规静态方法。 此外，在其封闭静态类处于范围内时，可以使用实例方法调用语法（[扩展方法调用](expressions.md#extension-method-invocations)）来调用扩展方法，使用接收方表达式作为第一个参数。

下面的程序使用上面声明的扩展方法：
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

`Slice` 方法在 `string[]`上可用，`ToInt32` 方法可在 `string`上使用，因为它们已被声明为扩展方法。 使用普通的静态方法调用，程序的含义与下面相同：
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>方法主体

方法声明的*method_body*包含一个块体、一个表达式体或一个分号。

如果返回类型为 `void`，则 `void` 方法的***结果类型***; 如果该方法是异步的并且返回类型为 `System.Threading.Tasks.Task`，则为。 否则，非异步方法的结果类型是其返回类型，并且 `T`具有返回类型 `System.Threading.Tasks.Task<T>` 的异步方法的结果类型。

当方法具有 `void` 结果类型和块体时，不允许在块中使用 `return` 语句（[return 语句](statements.md#the-return-statement)）来指定表达式。 如果 void 方法的执行操作正常完成（即，控制流从方法体的结尾处），该方法只会返回到当前调用方。
    
如果方法具有 `void` 结果和表达式体，则表达式 `E` 必须是*statement_expression*，并且正文完全等同于窗体 `{ E; }`的块体。
    
当方法具有非 void 结果类型和块体时，块中的每个 `return` 语句必须指定一个可隐式转换为结果类型的表达式。 返回值的方法的块体的终结点必须是无法访问的。 换言之，在带有块体的值返回方法中，不允许控件流过方法体的末尾。
    
如果方法具有非 void 结果类型和表达式体，则表达式必须可隐式转换为结果类型，并且正文完全等效于窗体 `{ return E; }`的块体。
    
示例中
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
返回 `F` 方法返回的值会导致编译时错误，因为控件可以流过方法体的末尾。 `G` 和 `H` 方法正确，因为所有可能的执行路径都以指定返回值的 return 语句结尾。 `I` 方法是正确的，因为它的主体等效于其中只包含一个返回语句的语句块。

### <a name="method-overloading"></a>方法重载

[类型推理](expressions.md#type-inference)中介绍了方法重载决策规则。

## <a name="properties"></a>属性

***属性***是提供对对象或类的特征的访问的成员。 属性的示例包括字符串的长度、字体的大小、窗口的标题、客户的名称，等等。 属性是字段的自然扩展，它们都是具有关联类型的命名成员，并且用于访问字段和属性的语法相同。 不过，与字段不同的是，属性不指明存储位置。 相反，属性包含***访问器***，用于指定在读取或写入属性值时要执行的语句。 因此，属性提供了一种机制，用于将操作与对象的属性的读取和写入操作相关联;而且，它们允许计算此类属性。

属性使用*property_declaration*s 来声明：

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

*Property_declaration*可以包含一组*属性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`static` （[静态和实例方法](classes.md#static-and-instance-methods)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。

属性声明服从与方法声明（[方法](classes.md#methods)）相同的规则，与修饰符的有效组合有关。

属性声明的*类型*指定声明引入的属性的类型， *member_name*指定属性的名称。 除非该属性是显式接口成员实现，否则*member_name*只是一个*标识符*。 对于显式接口成员实现（[显式接口成员实现](interfaces.md#explicit-interface-member-implementations)）， *member_name*由*interface_type*后跟 "`.`" 和*标识符*组成。

属性的*类型*必须至少具有与属性本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

*Property_body*可以包含***访问器体***或***表达式主体***。 在访问器主体中， *accessor_declarations*（必须将其括在 "`{`" 和 "`}`" 标记中），请声明属性的访问器（[访问](classes.md#accessors)器）。 访问器指定与读取和写入属性相关联的可执行语句。

由 `=>` 后跟*表达式*`E` 并且分号完全等效于语句体 `{ get { return E; } }`的表达式体，因此仅可用于指定仅限 getter 的属性，其中，getter 的结果由单个表达式提供。

只能为自动实现的属性（[自动实现的属性](classes.md#automatically-implemented-properties)）提供*property_initializer* ，并导致使用*表达式*提供的值初始化此类属性的基础字段。

尽管用于访问属性的语法与字段的语法相同，但属性未分类为变量。 因此，不能将属性作为 `ref` 或 `out` 参数进行传递。

如果属性声明包含 `extern` 修饰符，则该属性被称为***外部属性***。 由于外部属性声明不提供实际的实现，因此它的每个*accessor_declarations*都包含一个分号。

### <a name="static-and-instance-properties"></a>静态属性和实例属性

如果属性声明包含 `static` 修饰符，则该属性被称为***静态属性***。 如果不存在 `static` 修饰符，则将属性称为***实例属性***。

静态属性不与特定实例相关联，并且是在静态属性的访问器中引用 `this` 的编译时错误。

实例属性与类的给定实例相关联，并且该实例可以作为该属性的访问器中 `this` （[此访问](expressions.md#this-access)）进行访问。

当 `E.M`格式的*member_access* （[成员访问](expressions.md#member-access)）中引用属性时，如果 `M` 为静态属性，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例属性，则 E 必须表示包含 `M`的类型的实例。

静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。

### <a name="accessors"></a>Remove

属性的*accessor_declarations*指定与读取和写入属性相关联的可执行语句。

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

访问器声明由*get_accessor_declaration*和/或*set_accessor_declaration*组成。 每个访问器声明都包含标记 `get` 或 `set` 后跟可选的*accessor_modifier*和*accessor_body*。

*Accessor_modifier*的使用受以下限制的制约：

*  *Accessor_modifier*不能在接口或显式接口成员实现中使用。
*  对于没有 `override` 修饰符的属性或索引器，仅当属性或索引器同时具有 `get` 和 `set` 访问器时，才允许*accessor_modifier* ，然后只允许在其中一种访问器上使用。
*  对于包含 `override` 修饰符的属性或索引器，访问器必须与要重写的访问器的*accessor_modifier*（如果有）匹配。
*  *Accessor_modifier*必须声明一个可访问性，严格比属性或索引器本身的声明的可访问性更严格。 精确：
   * 如果属性或索引器具有 `public`的声明的可访问性，则*accessor_modifier*可能是 `protected internal`、`internal`、`protected`或 `private`。
   * 如果属性或索引器具有 `protected internal`的声明的可访问性，则*accessor_modifier*可能是 `internal`、`protected`或 `private`。
   * 如果属性或索引器具有 `internal` 或 `protected`的声明的可访问性，则*accessor_modifier*必须 `private`。
   * 如果属性或索引器具有 `private`的声明的可访问性，则不能使用任何*accessor_modifier* 。

对于 `abstract` 和 `extern` 属性，所指定的每个访问器的*accessor_body*只是一个分号。 非抽象的非 extern 属性可能具有每个*accessor_body*都是一个分号，在这种情况下，它是一个***自动实现的属性***（[自动实现的属性](classes.md#automatically-implemented-properties)）。 自动实现的属性必须至少具有 get 访问器。 对于任何其他非抽象非 extern 属性的访问器， *accessor_body*是一个*块*，它指定在调用相应的访问器时要执行的语句。

`get` 访问器与具有属性类型返回值的无参数方法相对应。 除了作为赋值的目标以外，在表达式中引用属性时，将调用属性的 `get` 访问器来计算属性的值（[表达式的值](expressions.md#values-of-expressions)）。 `get` 访问器的主体必须符合[方法体](classes.md#method-body)中所述的返回值的方法的规则。 特别是，`get` 访问器体中的所有 `return` 语句都必须指定一个可隐式转换为属性类型的表达式。 此外，`get` 访问器的终结点必须是无法访问的。

`set` 访问器对应于一个方法，该方法具有属性类型的单个值参数和一个 `void` 返回类型。 `set` 访问器的隐式参数始终命名为 `value`。 当属性作为赋值的目标（[赋值运算符](expressions.md#assignment-operators)）进行引用时，或者作为 `++` 或 `--` （[后缀增量和减量运算符](expressions.md#postfix-increment-and-decrement-operators)、[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）的操作数，使用参数（其值是赋值的右侧或 `++` 或 `--` 运算符的操作数）提供新值（[简单赋值](expressions.md#simple-assignment)）来调用 `set` 访问器。 `set` 访问器的主体必须符合[方法体](classes.md#method-body)中所述的 `void` 方法的规则。 特别是，不允许 `set` 访问器正文中的 `return` 语句指定表达式。 由于 `set` 访问器隐式包含一个名为 `value`的参数，因此，`set` 访问器中的局部变量或常量声明的编译时错误是具有该名称。

根据 `get` 和 `set` 访问器的存在情况，将按如下方式对属性进行分类：

*  同时包含 `get` 访问器和 `set` 访问器的属性称为***读写***属性。
*  仅具有 `get` 访问器的属性称为***只读***属性。 只读属性是赋值的目标时，这是一个编译时错误。
*  仅具有 `set` 访问器的属性称为***只写***属性。 除非作为赋值的目标，否则在表达式中引用只写属性是编译时错误。

示例中
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
`Button` 控件声明公共 `Caption` 属性。 `Caption` 属性的 `get` 访问器返回私有 `caption` 字段中存储的字符串。 `set` 访问器检查新值是否不同于当前值，如果是，则它存储新值并重新绘制控件。 属性通常遵循上面所示的模式： `get` 访问器只返回一个存储在私有字段中的值，而 `set` 访问器则修改该私有字段，然后执行完全更新对象状态所需的任何其他操作。

假设上述 `Button` 类，以下是使用 `Caption` 属性的示例：
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

此处，通过将值分配给属性来调用 `set` 访问器，通过引用表达式中的属性来调用 `get` 访问器。

属性的 `get` 和 `set` 访问器不是不同的成员，并且不能单独声明属性的访问器。 因此，读写属性的两个访问器不可能具有不同的可访问性。 示例
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
不声明单个读写属性。 相反，它声明了两个具有相同名称的属性，一个为只读，一个只写。 由于在同一类中声明的两个成员不能具有相同的名称，因此该示例将导致发生编译时错误。

当派生类用与继承属性相同的名称声明属性时，派生属性将隐藏与读取和写入相关的继承属性。 示例中
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
`B` 中的 `P` 属性隐藏 `A` 中的 `P` 属性，与读取和写入有关。 因此，在语句中
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
分配到 `b.P` 会导致报告编译时错误，因为 `B` 中的只读 `P` 属性将隐藏 `A`中的只写 `P` 属性。 但请注意，强制转换可用于访问隐藏的 `P` 属性。

与公共字段不同，属性可在对象的内部状态和其公共接口之间提供分隔。 请看下面的示例：
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

此处，`Label` 类使用两个 `int` 字段 `x` 和 `y`来存储其位置。 该位置公开为 `X` 和 `Y` 属性，并作为 `Point`类型的 `Location` 属性公开。 如果在 `Label`的未来版本中，在内部将位置存储为 `Point` 会更方便，而不会影响类的公共接口，则可以进行更改：
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

`x` 和 `y` `public readonly` 字段，则不可能对 `Label` 类进行此类更改。

通过属性公开状态不一定比直接公开字段的效率低。 特别是，当某个属性是非虚拟的并且只包含少量的代码时，执行环境可能会将对访问器的调用替换为访问器的实际代码。 此过程称为***内联***，它使属性访问与字段访问一样高效，同时还保留了属性的灵活性。

由于调用 `get` 访问器在概念上等同于读取字段的值，因此 `get` 访问器的编程样式被视为不正确的编程样式，使其具有可观察的副作用。 示例中
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
`Next` 属性的值取决于属性以前被访问的次数。 因此，访问属性会产生一个可观察到的副作用，而属性应作为方法实现。

`get` 访问器的 "无副作用" 约定并不意味着应始终将 `get` 访问器编写为仅返回存储在字段中的值。 事实上，`get` 访问器通常通过访问多个字段或调用方法来计算属性的值。 但是，正确设计的 `get` 访问器不会执行任何操作，从而导致对象的状态发生变化。

在第一次引用资源之前，可以使用属性来延迟资源的初始化。 例如：
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

`Console` 类包含三个属性： `In`、`Out`和 `Error`，分别表示标准输入、输出和错误设备。 通过将这些成员作为属性公开，`Console` 类可以延迟其初始化，直到实际使用它们。 例如，在第一次引用 `Out` 属性时，如下所示
```csharp
Console.Out.WriteLine("hello, world");
```
将创建输出设备的基础 `TextWriter`。 但如果应用程序不引用 `In` 和 `Error` 属性，则不会为这些设备创建任何对象。

### <a name="automatically-implemented-properties"></a>自动实现的属性

自动实现的属性（short 的***自动属性***）是具有仅包含分号的访问器正文的非抽象非 extern 属性。 自动属性必须具有 get 访问器，并且可以选择包含 set 访问器。

当属性指定为自动实现的属性时，隐藏的支持字段将自动提供给该属性，并实现访问器来读取和写入该支持字段。 如果 auto 属性没有 set 访问器，则会将支持字段视为 `readonly` （[Readonly 字段](classes.md#readonly-fields)）。 与 `readonly` 字段一样，仅 getter 自动属性也可在封闭类的构造函数的主体中分配给。 此类赋值直接分配给属性的 readonly 支持字段。

自动属性可以有选择地具有*property_initializer*，该属性作为*variable_initializer* （[变量初始值设定项](classes.md#variable-initializers)）直接应用于支持字段。

如下示例中：
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
等效于以下声明：
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

如下示例中：
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
等效于以下声明：
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

请注意，只读字段的赋值是合法的，因为它们发生在构造函数中。


### <a name="accessibility"></a>辅助功能

如果访问器具有*accessor_modifier*，则访问器的可访问性域（[可访问](basic-concepts.md#accessibility-domains)域）是使用*accessor_modifier*的声明的可访问性确定的。 如果访问器没有*accessor_modifier*，则访问器的可访问性域由属性或索引器的声明的可访问性决定。

*Accessor_modifier*的存在从不影响成员查找（[运算符](expressions.md#operators)）或重载决策（[重载决策](expressions.md#overload-resolution)）。 属性或索引器的修饰符始终决定绑定到的属性或索引器，而不考虑访问的上下文。

选择特定属性或索引器后，所涉及的特定访问器的可访问域将用于确定此使用是否有效：

*  如果用法为值（[表达式的值](expressions.md#values-of-expressions)），则 `get` 访问器必须存在并且可访问。
*  如果使用情况是作为简单赋值的目标（[简单分配](expressions.md#simple-assignment)），则 `set` 访问器必须存在并且可访问。
*  如果使用情况是作为复合赋值（[复合赋值](expressions.md#compound-assignment)）的目标，或者作为 `++` 或 `--` 运算符（[函数成员](expressions.md#function-members).9、[调用表达式](expressions.md#invocation-expressions)）的目标，则 `get` 访问器和 `set` 访问器都必须存在并且可访问。

在下面的示例中，属性 `B.Text``A.Text` 隐藏属性，即使仅在调用 `set` 访问器的上下文中也是如此。 相反，类 `M`无法访问属性 `B.Count`，因此改用 `A.Count` 的可访问属性。

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

用于实现接口的访问器可能没有*accessor_modifier*。 如果只使用一个访问器来实现接口，则可以使用*accessor_modifier*声明另一个访问器：
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>虚拟、密封、重写和抽象属性访问器

`virtual` 属性声明指定属性的访问器是虚拟的。 `virtual` 修饰符适用于读写属性的这两个访问器，即读写属性的一个取值函数是虚拟的。

`abstract` 属性声明指定属性的访问器是虚拟的，但是不提供访问器的实际实现。 相反，非抽象派生类需要通过重写属性来为访问器提供自己的实现。 因为抽象属性声明的访问器不提供实际的实现，所以其*accessor_body*只包含一个分号。

同时包含 `abstract` 和 `override` 修饰符的属性声明指定属性是抽象的并且会重写基属性。 此类属性的访问器也是抽象的。

仅在抽象类（[抽象类](classes.md#abstract-classes)）中允许抽象属性声明。通过包括指定 `override` 指令的属性声明，可在派生类中重写继承的虚拟属性的访问器。 这称为 "***重写" 属性声明***。 重写属性声明未声明新的属性。 相反，它只是专用化现有虚拟属性的访问器的实现。

重写的属性声明必须将完全相同的可访问性修饰符、类型和名称指定为继承的属性。 如果继承的属性只有一个访问器（即，如果继承的属性为只读或只写），则重写属性必须仅包含该访问器。 如果继承的属性包含两个访问器（即，如果继承的属性是读写的），则重写属性可以包含单个访问器或两个访问器。

重写的属性声明可以包括 `sealed` 修饰符。 使用此修饰符可防止派生类进一步重写属性。 密封属性的访问器也是密封的。

除了声明和调用语法中的差异外，virtual、sealed、override 和 abstract 访问器的行为与虚拟、密封、重写和抽象方法完全相同。 具体而言，在[虚拟方法](classes.md#virtual-methods)、[替代方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中描述的规则将应用为：访问器是相应形式的方法：

*  `get` 访问器对应于一个无参数方法，该方法具有属性类型的返回值以及与包含属性相同的修饰符。
*  `set` 访问器对应于一个方法，该方法具有属性类型的单个值参数、一个 `void` 返回类型，以及与包含属性相同的修饰符。

示例中
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` 是虚拟的只读属性，`Y` 为虚拟读写属性，`Z` 是抽象的读写属性。 由于 `Z` 是抽象的，因此，包含类 `A` 也必须声明为抽象类。

下面显示了一个派生自 `A` 的类：
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

此处，`X`、`Y`和 `Z` 的声明都是重写属性声明。 每个属性声明都与相应的继承属性的可访问性修饰符、类型和名称完全匹配。 `X` 的 `get` 访问器和 `Y` 的 `set` 访问器使用 `base` 关键字访问继承的访问器。 `Z` 的声明会重写抽象访问器，因此，`B`中没有未完成的抽象函数成员，并且允许 `B` 为非抽象类。

当某个属性声明为 `override`时，重写的代码必须能够访问所有重写的访问器。 此外，属性或索引器本身以及访问器的声明的可访问性必须与重写的成员和访问器的可访问性匹配。 例如：
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>事件

***事件***是允许对象或类提供通知的成员。 客户端可以通过提供***事件处理程序***来附加事件的可执行代码。

使用*event_declaration*s 声明事件：

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

*Event_declaration*可能包括一组*属性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`static` （[静态和实例方法](classes.md#static-and-instance-methods)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。

事件声明遵循与方法声明（[方法](classes.md#methods)）相同的规则，与修饰符的有效组合有关。

事件声明的*类型*必须是*delegate_type* （[引用类型](types.md#reference-types)），并且*delegate_type*必须至少与事件本身具有相同的可访问性（[可访问性约束](basic-concepts.md#accessibility-constraints)）。

事件声明可能包括*event_accessor_declarations*。 但是，如果不是，则对于非 extern 非抽象事件，编译器会自动提供这些事件（[类似于字段的事件](classes.md#field-like-events)）;对于 extern 事件，访问器是在外部提供的。

*Event_accessor_declarations*省略的事件声明定义了一个或多个事件，每个*variable_declarator*一个事件。 特性和修饰符适用于由此类*event_declaration*声明的所有成员。

*Event_declaration*包括 `abstract` 修饰符和用大括号分隔的*event_accessor_declarations*，则会发生编译时错误。

当事件声明包含 `extern` 修饰符时，该事件被称为***外部事件***。 由于外部事件声明不提供实际的实现，因此它可以同时包含 `extern` 修饰符和*event_accessor_declarations*。

使用 `abstract` 或 `external` 修饰符来包含*variable_initializer*时，事件声明的*variable_declarator*会出现编译时错误。

事件可用作 `+=` 和 `-=` 运算符（[事件分配](expressions.md#event-assignment)）的左操作数。 这些运算符分别用于将事件处理程序附加到事件处理程序或从事件中删除事件处理程序，事件的访问修饰符控制允许此类操作的上下文。

由于 `+=` 和 `-=` 是对声明事件的类型之外的事件允许的唯一操作，因此外部代码可以添加和移除事件的处理程序，但不能以任何其他方式获取或修改事件处理程序的基础列表。

在 `x += y` 或 `x -= y`形式的操作中，当 `x` 为事件并且引用出现在包含 `x`声明的类型之外时，操作的结果将具有类型 `void` （与具有 `x`的类型相反，而在赋值后，`x` 的值为。 此规则禁止外部代码间接检查事件的基础委托。

下面的示例演示如何将事件处理程序附加到 `Button` 类的实例：
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

此处，`LoginDialog` 实例构造函数创建两个 `Button` 实例，并将事件处理程序附加到 `Click` 事件。

### <a name="field-like-events"></a>类似字段的事件

在包含事件声明的类或结构的程序文本中，某些事件可以像字段一样使用。 若要以这种方式使用，事件不得 `abstract` 或 `extern`，而且不能显式包含*event_accessor_declarations*。 此类事件可用于任何允许字段的上下文中。 该字段包含一个委托（[委托](delegates.md)），表示已添加到事件的事件处理程序的列表。 如果未添加任何事件处理程序，则字段将包含 `null`。

示例中
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` 用作 `Button` 类中的字段。 如示例所示，可以检查、修改字段，并将其用于委托调用表达式。 `Button` 类中的 `OnClick` 方法 "引发" `Click` 事件。 引发事件的概念恰恰等同于调用由事件表示的委托，因此，没有用于引发事件的特殊语言构造。 请注意，委托调用前面有一个检查，该检查可确保委托为非 null。

在 `Button` 类的声明外部，`Click` 成员只能在 `+=` 和 `-=` 运算符的左侧使用，如下所示
```csharp
b.Click += new EventHandler(...);
```
它将一个委托追加到 `Click` 事件的调用列表中，并
```csharp
b.Click -= new EventHandler(...);
```
这将从 `Click` 事件的调用列表中移除委托。

在编译类似于字段的事件时，编译器会自动创建存储来保存委托，并为事件创建访问器，以便在委托字段中添加或删除事件处理程序。 添加和移除操作是线程安全的，并且在持有实例事件的包含对象的锁（[lock 语句](statements.md#the-lock-statement)）或静态事件的类型对象（[匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)）的情况下，可能（但不需要）执行此操作。

因此，以下形式的实例事件声明：
```csharp
class X
{
    public event D Ev;
}
```
将编译为与等效的对象：
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
在类 `X`中，对 `+=` 和 `-=` 运算符左侧 `Ev` 的引用会导致调用 add 和 remove 访问器。 对 `Ev` 的所有其他引用将被编译为引用隐藏字段（[成员访问](expressions.md#member-access)） `__Ev`。 名称 "`__Ev`" 是任意的;隐藏的字段可能有任何名称，或者根本没有名称。

### <a name="event-accessors"></a>事件访问器

事件声明通常会忽略*event_accessor_declarations*，如上面的 `Button` 示例中所示。 执行此操作的一种情况是，这种情况涉及到每个事件的一个字段的存储成本是不可接受的。 在这种情况下，类可以包括*event_accessor_declarations* ，并使用专用机制来存储事件处理程序列表。

事件的*event_accessor_declarations*指定与添加和移除事件处理程序相关联的可执行语句。

访问器声明由*add_accessor_declaration*和*remove_accessor_declaration*组成。 每个访问器声明都包含标记 `add` 或 `remove` 后跟一个*块*。 与*add_accessor_declaration*关联的*块*指定在添加事件处理程序时要执行的语句，与*remove_accessor_declaration*关联的*块*指定在删除事件处理程序时要执行的语句。

每个*add_accessor_declaration*和*remove_accessor_declaration*对应于一个方法，其中包含事件类型的单个值参数和一个 `void` 返回类型。 事件访问器的隐式参数名为 `value`。 在事件分配中使用事件时，将使用适当的事件访问器。 具体而言，如果 `+=` 赋值运算符，则使用 add 访问器，如果赋值运算符 `-=`，则使用 remove 访问器。 在任一情况下，赋值运算符的右操作数用作事件访问器的参数。 *Add_accessor_declaration*或*remove_accessor_declaration*的块必须符合[方法体](classes.md#method-body)中所述的 `void` 方法的规则。 特别是，不允许此类块中的 `return` 语句指定表达式。

由于事件访问器隐式具有名为 `value`的参数，因此在事件访问器中声明的局部变量或常量具有该名称是编译时错误。

示例中
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
`Control` 类实现事件的内部存储机制。 `AddEventHandler` 方法将委托值与某个键相关联，`GetEventHandler` 方法返回当前与某个键相关联的委托，并且 `RemoveEventHandler` 方法将委托作为指定事件的事件处理程序移除。 可能的基础存储机制的设计使 `null` 委托值与密钥关联不会产生费用，因此未处理的事件不会消耗任何存储。

### <a name="static-and-instance-events"></a>静态和实例事件

当事件声明包含 `static` 修饰符时，该事件被称为***静态事件***。 如果不存在任何 `static` 修饰符，则该事件被称为***实例事件***。

静态事件不与特定实例相关联，并且是在静态事件的访问器中引用 `this` 的编译时错误。

实例事件与类的给定实例相关联，并且可以在该事件的访问器中将此实例作为 `this` （[此访问](expressions.md#this-access)）进行访问。

当 `E.M`形式的*member_access* （[成员访问](expressions.md#member-access)）中引用事件时，如果 `M` 为静态事件，则 `E` 必须表示包含 `M`的类型，如果 `M` 是实例事件，则 E 必须表示包含 `M`的类型的实例。

静态成员和实例成员之间的差异在[静态成员和实例成员](classes.md#static-and-instance-members)中进行了进一步讨论。

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>虚拟、密封、重写和抽象事件访问器

`virtual` 事件声明指定该事件的访问器是虚拟的。 `virtual` 修饰符适用于事件的两个访问器。

`abstract` 事件声明指定事件的访问器是虚拟的，但不提供访问器的实际实现。 相反，非抽象派生类需要通过重写事件为访问器提供自己的实现。 因为抽象事件声明不提供实际的实现，所以它不能提供用括号分隔的*event_accessor_declarations*。

同时包含 `abstract` 和 `override` 修饰符的事件声明指定事件是抽象的并且会重写基事件。 此类事件的访问器也是抽象的。

仅在抽象类（[抽象类](classes.md#abstract-classes)）中允许抽象事件声明。

通过包括指定 `override` 修饰符的事件声明，可在派生类中重写继承的虚拟事件的访问器。 这称为***重写事件声明***。 重写事件声明未声明新事件。 相反，它只是专用化现有虚拟事件的访问器的实现。

重写事件声明必须指定与重写事件完全相同的可访问性修饰符、类型和名称。

重写事件声明可能包括 `sealed` 修饰符。 使用此修饰符可防止派生类进一步重写事件。 密封事件的访问器也是密封的。

重写事件声明包含 `new` 修饰符是编译时错误。

除了声明和调用语法中的差异外，virtual、sealed、override 和 abstract 访问器的行为与虚拟、密封、重写和抽象方法完全相同。 具体而言，在[虚拟方法](classes.md#virtual-methods)、[重写方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中描述的规则就像访问器是相应窗体的方法一样。 每个访问器都对应于一个方法，该方法具有事件类型的单个值参数、一个 `void` 返回类型，以及与包含事件相同的修饰符。

## <a name="indexers"></a>索引器

***索引器***是一个成员，使对象能够以与数组相同的方式进行索引。 使用*indexer_declaration*s 声明索引器：

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

*Indexer_declaration*可能包括一组*特性*（[特性](attributes.md)）和四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）、`new` （[新修饰符](classes.md#the-new-modifier)）、`virtual` （[虚拟方法](classes.md#virtual-methods)）、`override` （[重写方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。

索引器声明遵循与方法声明（[方法](classes.md#methods)）相同的规则，这些修饰符与修饰符的有效组合有关，但有一个例外，即索引器声明上不允许使用 static 修饰符。

除了一种情况外，修饰符 `virtual`、`override`和 `abstract` 都是互斥的。 `abstract` 和 `override` 修饰符可一起使用，以便抽象索引器可以重写虚拟一个。

索引器声明的*类型*指定声明引入的索引器的元素类型。 除非索引器是显式接口成员实现，否则，该*类型*后跟关键字 `this`。 对于显式接口成员实现，该*类型*后跟一个*interface_type*、"`.`" 和关键字 "`this`"。 与其他成员不同，索引器没有用户定义的名称。

*Formal_parameter_list*指定索引器的参数。 索引器的形参列表与方法（[方法参数](classes.md#method-parameters)）的形参列表相同，不同之处在于至少必须指定一个形参，并且不允许 `ref` 和 `out` 参数修饰符。

索引器的*类型*和*formal_parameter_list*中引用的每个类型必须至少与索引器本身相同（[可访问性约束](basic-concepts.md#accessibility-constraints)）。

*Indexer_body*可以包含***访问器体***或***表达式主体***。 在访问器主体中， *accessor_declarations*（必须将其括在 "`{`" 和 "`}`" 标记中），请声明属性的访问器（[访问](classes.md#accessors)器）。 访问器指定与读取和写入属性相关联的可执行语句。

由 "`=>`" 后跟表达式 `E` 并且分号完全等效于语句体 `{ get { return E; } }`的表达式体，因此仅用于指定仅限 getter 的索引器，其中，getter 的结果由单个表达式提供。

尽管访问索引器元素的语法与数组元素的语法相同，但不会将索引器元素分类为变量。 因此，不能将索引器元素作为 `ref` 或 `out` 参数进行传递。

索引器的形参列表定义索引器的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)）。 具体而言，索引器的签名由其形参的数量和类型组成。 形参的元素类型和名称不属于索引器的签名。

索引器的签名必须与同一类中声明的所有其他索引器的签名不同。

索引器和属性在概念上非常类似，但在以下方面有所不同：

*  属性由其名称标识，而索引器由其签名标识。
*  属性可通过*simple_name* （[简单名称](expressions.md#simple-names)）或*member_access* （[成员访问](expressions.md#member-access)）访问，而索引器元素则通过*element_access* （[索引器访问](expressions.md#indexer-access)）访问。
*  属性可以是 `static` 成员，而索引器始终是实例成员。
*  属性的 `get` 访问器对应于不带参数的方法，而索引器的 `get` 访问器对应于具有与索引器相同的形参列表的方法。
*  属性的 `set` 访问器对应于一个方法，该方法具有一个名为 `value`的参数，而索引器的 `set` 访问器对应于一个方法，该方法具有与索引器相同的形参列表，另外还有一个名为 `value`的附加形参。
*  索引器访问器使用与索引器参数相同的名称声明局部变量是编译时错误。
*  在重写属性声明中，使用 `base.P`语法访问继承属性，其中 `P` 是属性名称。 在重写索引器声明中，继承的索引器是使用语法 `base[E]`访问的，其中 `E` 是逗号分隔的表达式列表。
*  没有 "自动实现的索引器" 的概念。 使用具有分号访问器的非抽象非外部索引器是错误的。

除了这些差异以外，在[访问器](classes.md#accessors)和[自动实现的属性](classes.md#automatically-implemented-properties)中定义的所有规则都适用于索引器访问器以及属性访问器。

当索引器声明包含 `extern` 修饰符时，该索引器被称为***外部索引***器。 由于外部索引器声明不提供实际的实现，因此它的每个*accessor_declarations*都包含一个分号。

下面的示例声明一个 `BitArray` 类，该类实现索引器以访问位数组中的单个位。
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

`BitArray` 类的实例使用的内存量明显小于相应的 `bool[]` （因为前者的每个值仅占用一位而不是后者的一个字节），但它允许与 `bool[]`相同的操作。

以下 `CountPrimes` 类使用 `BitArray` 和传统的 "埃拉托色" 算法来计算介于1和 a 之间的 primes：
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

请注意，用于访问 `BitArray` 的元素的语法与 `bool[]`的语法完全相同。

下面的示例显示一个 26 * 10 网格类，该类具有一个具有两个参数的索引器。 第一个参数必须是 A-z 范围内的大写或小写字母，第二个参数必须是0-9 范围内的整数。

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>索引器重载

[类型推理](expressions.md#type-inference)中介绍了索引器重载决策规则。

## <a name="operators"></a>运算符

***运算符***是一个成员，用于定义可应用于类的实例的表达式运算符的含义。 使用*operator_declaration*s 声明运算符：

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

有三种类型的可重载运算符：一元运算符（[一元运算符](classes.md#unary-operators)）、二元运算符（[二元运算符](classes.md#binary-operators)）和转换运算符（[转换运算符](classes.md#conversion-operators)）。

*Operator_body*为分号、***语句体***或***表达式主体***。 语句体由*块*组成，它指定在调用运算符时要执行的语句。 *块*必须符合[方法体](classes.md#method-body)中所述的返回值的方法的规则。 表达式主体由 `=>` 后跟表达式和分号组成，表示在调用运算符时要执行的单个表达式。

对于 `extern` 运算符， *operator_body*只包含一个分号。 对于所有其他运算符， *operator_body*为块体或表达式主体。

以下规则适用于所有运算符声明：

*  运算符声明必须同时包含 `public` 和 `static` 修饰符。
*  运算符的参数必须是值参数（[值参数](variables.md#value-parameters)）。 运算符声明指定 `ref` 或 `out` 参数是编译时错误。
*  运算符（[一元运算符](classes.md#unary-operators)、[二元运算符](classes.md#binary-operators)、[转换运算符](classes.md#conversion-operators)）的签名必须不同于同一个类中声明的所有其他运算符的签名。
*  运算符声明中引用的所有类型必须至少具有与运算符本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。
*  同一修饰符在运算符声明中多次出现是错误的。

每个运算符类别都有其他限制，如以下各节所述。

与其他成员一样，基类中声明的运算符由派生类继承。 由于运算符声明始终需要声明运算符的类或结构参与运算符的签名，因此，派生类中声明的运算符不能隐藏在基类中声明的运算符。 因此，在运算符声明中绝不需要并且不允许使用 `new` 修饰符。

有关一元运算符和二元运算符的其他信息，请参阅[运算符](expressions.md#operators)。

有关转换运算符的其他信息，请参阅[用户定义的转换](conversions.md#user-defined-conversions)。

### <a name="unary-operators"></a>一元运算符

以下规则适用于一元运算符声明，其中 `T` 表示包含运算符声明的类或结构的实例类型：

*  一元 `+`、`-`、`!`或 `~` 运算符必须采用类型 `T` 或 `T?` 的单个参数，并可以返回任何类型。
*  一元 `++` 或 `--` 运算符必须采用 `T` 或 `T?` 类型的单个参数，并且必须返回该类型或从其派生的类型。
*  一元 `true` 或 `false` 运算符必须采用 `T` 或 `T?` 类型的单个参数，并且必须返回类型 `bool`。

一元运算符的签名由运算符标记（`+`、`-`、`!`、`~`、`++`、`--`、`true`或 `false`）和单个形参的类型组成。 返回类型不是一元运算符的签名的一部分，也不是形参的名称。

`true` 和 `false` 一元运算符要求成对声明。 如果类声明了这些运算符中的一个，而没有同时声明其他运算符，则会发生编译时错误。 [用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)和[布尔表达式](expressions.md#boolean-expressions)中进一步介绍了 `true` 和 `false` 运算符。

下面的示例演示了一个实现，并为整数向量类 `operator ++` 了后续用法：
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

请注意，operator 方法如何返回通过将1添加到操作数而生成的值，就像后缀增量和减量运算符（[后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)）以及前缀增量和减量运算符（[前缀增量和减量运算符](expressions.md#prefix-increment-and-decrement-operators)）。 与在C++中不同的是，此方法不需要直接修改其操作数的值。 事实上，修改操作数值将违反后缀增量运算符的标准语义。

### <a name="binary-operators"></a>二元运算符

以下规则适用于二元运算符声明，其中 `T` 表示包含运算符声明的类或结构的实例类型：

*  二元非移位运算符必须采用两个参数，其中至少一个参数必须具有类型 `T` 或 `T?`，并且可以返回任何类型。
*  二进制 `<<` 或 `>>` 运算符必须采用两个参数，其中第一个参数必须具有类型 `T` 或 `T?`，而第二个参数必须具有类型 `int` 或 `int?`，并且可以返回任何类型。

二元运算符的签名由运算符标记（`+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`<<`、`>>`、`==`、`!=`、`>`、`<`、`>=`或 `<=`）和两个形参的类型组成。 返回类型和形参的名称不是二元运算符的签名的一部分。

某些二元运算符要求成对声明。 对于每个运算符的每个声明，都必须有一个对的另一个运算符的匹配声明。 当两个运算符声明具有相同的返回类型，并且每个参数具有相同的类型时，它们将匹配。 以下运算符要求成对声明：

*  `operator ==` 和 `operator !=`
*  `operator >` 和 `operator <`
*  `operator >=` 和 `operator <=`

### <a name="conversion-operators"></a>转换运算符

转换运算符声明引入了***用户定义的转换***（[用户定义](conversions.md#user-defined-conversions)的转换），以补充预定义的隐式和显式转换。

包含 `implicit` 关键字的转换运算符声明引入了用户定义的隐式转换。 隐式转换可能会在多种情况下发生，包括函数成员调用、强制转换表达式和赋值。 [隐式转换](conversions.md#implicit-conversions)中对此进行了进一步说明。

包含 `explicit` 关键字的转换运算符声明引入了用户定义的显式转换。 显式转换可在强制转换表达式中发生，并在[显式转换](conversions.md#explicit-conversions)中进一步说明。

转换运算符从转换运算符的参数类型指示的源类型转换为目标类型，由转换运算符的返回类型指示。

对于给定的源类型 `S` 和目标类型 `T`，如果 `S` 或 `T` 是可以为 null 的类型，则允许 `S0` 和 `T0` 引用它们的基础类型，否则 `S0` 和 `T0` 分别等于 `S` 和 `T`。 仅当满足以下所有条件时，才允许类或结构声明从源类型 `S` 转换为目标类型 `T`：

*  `S0` 和 `T0` 属于不同类型。
*  `S0` 或 `T0` 是发生运算符声明的类或结构类型。
*  `S0` 和 `T0` 都不是*interface_type*的。
*  如果不包括用户定义的转换，则从 `S` 到 `T` 或从 `T` 到 `S`的转换不存在。

对于这些规则，与 `S` 或 `T` 关联的任何类型参数都视为唯一的类型，这些类型与其他类型没有继承关系，并忽略这些类型参数上的任何约束。

示例中
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
允许使用前两个运算符声明，因为对于[索引器](classes.md#indexers).3，分别 `T` 和 `int` 和 `string` 被视为唯一的类型，而无任何关系。 但第三个运算符是错误，因为 `C<T>` 是 `D<T>`的基类。

从第二个规则开始，转换运算符必须在声明运算符的类或结构类型中转换为或。 例如，类或结构类型 `C` 可以定义从 `C` 到 `int` 之间的转换和从 `int` 到 `C`的转换，但不能定义从 `int` 到 `bool`的转换。

不能直接重新定义预定义的转换。 因此，不允许转换运算符从或转换为 `object`，因为 `object` 和所有其他类型之间已存在隐式和显式转换。 同样，转换的源类型和目标类型都不能是另一类型的基类型，因为转换已经存在。

但是，可以在针对特定类型参数的泛型类型上声明运算符，指定已作为预定义转换存在的转换。 示例中
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
当类型 `object` 指定为 `T`的类型参数时，第二个运算符将声明已存在的转换（隐式，因此也是从任何类型到类型 `object`的显式转换）。

如果两种类型之间存在预定义的转换，则将忽略这些类型之间的任何用户定义的转换。 尤其是在下列情况下：

*  如果存在从类型 `S` 到类型 `T`的预定义隐式转换（[隐式](conversions.md#implicit-conversions)转换），则将忽略从 `S` 到 `T` 的所有用户定义的转换（隐式或显式转换）。
*  如果从类型 `S` 中存在预定义的显式转换（[显式转换](conversions.md#explicit-conversions)）到类型 `T`，则将忽略从 `S` 到 `T` 的任何用户定义的显式转换。 此外：

如果 `T` 是接口类型，则将忽略从 `S` 到 `T` 的用户定义隐式转换。

否则，仍会考虑从 `S` 到 `T` 的用户定义隐式转换。

对于所有类型但 `object`，以上 `Convertible<T>` 类型声明的运算符不会与预定义转换冲突。 例如：
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

但是，对于类型 `object`，预定义的转换在所有情况下都会隐藏用户定义的转换，但会隐藏其中一项：

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

不允许用户定义的转换从或转换到*interface_type*。 特别是，此限制可确保在转换为*interface_type*时不会发生用户定义的转换，并且仅当被转换的对象真正实现指定的*interface_type*时，才能成功转换为*interface_type* 。

转换运算符的签名包含源类型和目标类型。 （请注意，这是返回类型参与签名的唯一成员形式。）转换运算符的 `implicit` 或 `explicit` 分类不是运算符签名的一部分。 因此，类或结构不能声明具有相同源和目标类型的 `implicit` 和 `explicit` 转换运算符。

通常，用户定义的隐式转换应设计为从不引发异常，并且永远不会丢失信息。 如果用户定义的转换可以增加异常（例如，由于源参数超出范围）或丢失信息（如丢弃高序位），则应将该转换定义为显式转换。

示例中
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
从 `Digit` 到 `byte` 的转换是隐式的，因为它从不引发异常或丢失信息，但从 `byte` 到 `Digit` 的转换是显式的，因为 `Digit` 只能表示 `byte`的可能值的子集。

## <a name="instance-constructors"></a>实例构造函数

***实例构造函数***是实现初始化类实例所需执行的操作的成员。 实例构造函数是使用*constructor_declaration*s 声明的：

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

*Constructor_declaration*可以包含一组*特性*（[特性](attributes.md)）、四个访问修饰符（[访问修饰符](classes.md#access-modifiers)）和 `extern` （[外部方法](classes.md#external-methods)）修饰符的有效组合。 不允许构造函数声明多次包含同一修饰符。

*Constructor_declarator*的*标识符*必须命名声明实例构造函数的类。 如果指定了其他名称，则会发生编译时错误。

实例构造函数的可选*formal_parameter_list*遵从与方法（[方法](classes.md#methods)） *formal_parameter_list*相同的规则。 形参表定义实例构造函数的签名（[签名和重载](basic-concepts.md#signatures-and-overloading)），并控制重载决策（[类型推理](expressions.md#type-inference)）在调用中选择特定实例构造函数的过程。

实例构造函数的*formal_parameter_list*中引用的每个类型必须至少具有与构造函数本身相同的可访问性（[辅助功能约束](basic-concepts.md#accessibility-constraints)）。

可选*constructor_initializer*指定要在执行此实例构造函数的*constructor_body*中给定的语句之前调用的另一个实例构造函数。 [构造函数初始值设定项](classes.md#constructor-initializers)中对此进行了进一步说明。

当构造函数声明包含 `extern` 修饰符时，构造函数被称为***外部构造函数***。 由于外部构造函数声明不提供实际的实现，因此它的*constructor_body*由分号组成。 对于所有其他构造函数， *constructor_body*包含一个*块*，它指定用于初始化类的新实例的语句。 这与实例方法的*块*完全对应，具有 `void` 返回类型（[方法体](classes.md#method-body)）。

不继承实例构造函数。 因此，类没有实例构造函数，而不是类中实际声明的构造函数。 如果类不包含实例构造函数声明，则会自动提供默认实例构造函数（[默认构造](classes.md#default-constructors)函数）。

实例构造函数由*object_creation_expression*（[对象创建表达式](expressions.md#object-creation-expressions)）和*constructor_initializer*来调用。

### <a name="constructor-initializers"></a>构造函数初始值设定项

所有实例构造函数（类 `object`的类构造函数除外）都隐式包含对*constructor_body*之前的其他实例构造函数的调用。 隐式调用的构造函数由*constructor_initializer*确定：

*  `base(argument_list)` 或 `base()` 形式的实例构造函数初始值设定项将导致调用直接基类的实例构造函数。 使用*argument_list*如果存在，则选择该构造函数，并[使用重载决策](expressions.md#overload-resolution)的重载决策规则。 如果未在直接基类中声明任何实例构造函数，候选实例构造函数集则包含直接基类中包含的所有可访问实例构造函数，或者默认构造函数（[默认构造函数](classes.md#default-constructors)）。 如果此集为空，或者无法识别单个最佳实例构造函数，则会发生编译时错误。
*  `this(argument-list)` 或 `this()` 形式的实例构造函数初始值设定项会导致调用类自身的实例构造函数。 如果存在，则使用*argument_list* ，并使用[重载](expressions.md#overload-resolution)决策的重载决策规则来选择构造函数。 候选实例构造函数集包含类本身中声明的所有可访问实例构造函数。 如果此集为空，或者无法识别单个最佳实例构造函数，则会发生编译时错误。 如果实例构造函数声明包含调用构造函数本身的构造函数初始值设定项，则会发生编译时错误。

如果实例构造函数没有构造函数初始值设定项，则会隐式提供形式 `base()` 的构造函数初始值设定项。 因此，形式的实例构造函数声明
```csharp
C(...) {...}
```
完全等效于
```csharp
C(...): base() {...}
```

实例构造函数声明的*formal_parameter_list*给定的参数的范围包含该声明的构造函数初始值设定项。 因此，可以使用构造函数初始值设定项来访问构造函数的参数。 例如：
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

实例构造函数初始值设定项无法访问正在创建的实例。 因此，在构造函数初始值设定项的参数表达式中引用 `this` 是编译时错误，这是因为自变量表达式的编译时错误通过*simple_name*引用任何实例成员。

### <a name="instance-variable-initializers"></a>实例变量初始值设定项

如果实例构造函数不具有构造函数初始值设定项，或者它具有形式 `base(...)`形式的构造函数初始值设定项，则该构造函数将隐式执行由其类中声明的实例字段的*variable_initializer*指定的初始化。 这对应于在进入构造函数之后、直接调用直接基类构造函数之前立即执行的一系列赋值。 变量初始值设定项将按照它们在类声明中的显示顺序执行。

### <a name="constructor-execution"></a>构造函数执行

变量初始值设定项转换为赋值语句，并在调用基类实例构造函数之前执行这些赋值语句。 此顺序可确保在执行有权访问该实例的任何语句之前，通过其变量初始值设定项来初始化所有实例字段。

给定示例
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
`new B()` 用于创建 `B`的实例时，将生成以下输出：
```console
x = 1, y = 0
```

`x` 的值为1，因为在调用基类实例构造函数之前将执行变量初始值设定项。 但 `y` 的值为0（`int`的默认值），因为在基类构造函数返回后才会执行 `y` 的赋值。

将实例变量初始值设定项和构造函数初始值设定项视为自动插入*constructor_body*前面的语句是非常有用的。 示例
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
包含若干变量初始值设定项;它还包含两个窗体的构造函数初始值设定项（`base` 和 `this`）。 该示例与下面显示的代码相对应，其中每个注释指示一个自动插入的语句（用于自动插入的构造函数调用的语法无效，但仅用于说明这种机制）。

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>默认构造函数

如果类不包含实例构造函数声明，则会自动提供一个默认实例构造函数。 该默认构造函数只调用直接基类的无参数构造函数。 如果类是抽象类，则对默认构造函数的声明的可访问性是受保护的。 否则，默认构造函数的声明的可访问性是公共的。 因此，默认构造函数始终为形式

```csharp
protected C(): base() {}
```
或
```csharp
public C(): base() {}
```
其中 `C` 是类的名称。 如果重载决策无法确定基类构造函数初始值设定项的唯一最佳候选项，则会发生编译时错误。

示例中
```csharp
class Message
{
    object sender;
    string text;
}
```
由于类不包含实例构造函数声明，因此提供了默认的构造函数。 因此，该示例完全等效于
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>私有构造函数

当类 `T` 仅声明私有实例构造函数时，不能 `T` 的程序文本外的类从 `T` 派生或直接创建 `T`的实例。 因此，如果某个类只包含静态成员，并且不应实例化，则添加一个空的私有实例构造函数会阻止实例化。 例如：
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

`Trig` 类将相关方法和常量分组，但不能进行实例化。 因此，它声明了一个空的私有实例构造函数。 至少必须将一个实例构造函数声明为禁止自动生成默认构造函数。

### <a name="optional-instance-constructor-parameters"></a>可选实例构造函数参数

构造函数初始值设定项的 `this(...)` 形式通常与重载一起使用，以实现可选的实例构造函数参数。 示例中
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
前两个实例构造函数只为缺少的参数提供默认值。 这两种方法都使用 `this(...)` 构造函数初始值设定项调用第三个实例构造函数，该构造函数实际上用于初始化新实例。 其结果是可选的构造函数参数：
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>静态构造函数

***静态构造函数***是实现初始化封闭式类类型所需的操作的成员。 静态构造函数使用*static_constructor_declaration*来声明：

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

*Static_constructor_declaration*可以包含一组*特性*（[特性](attributes.md)）和一个 `extern` 修饰符（[外部方法](classes.md#external-methods)）。

*Static_constructor_declaration*的*标识符*必须命名声明静态构造函数的类。 如果指定了其他名称，则会发生编译时错误。

如果静态构造函数声明包含 `extern` 修饰符，则将静态构造函数称为***外部静态构造函数***。 由于外部静态构造函数声明不提供实际的实现，因此它的*static_constructor_body*由分号组成。 对于所有其他静态构造函数声明， *static_constructor_body*包含一个*块*，它指定要执行的语句，以便初始化类。 这与 `void` 返回类型（[方法体](classes.md#method-body)）的静态方法的*method_body*完全对应。

静态构造函数不是继承的，不能直接调用。

封闭式类类型的静态构造函数在给定的应用程序域中最多执行一次。 在应用程序域中，以下事件的第一个事件会触发静态构造函数的执行：

*  创建类类型的实例。
*  引用类类型的任何静态成员。

如果类包含执行开始时所处的 `Main` 方法（[应用程序启动](basic-concepts.md#application-startup)），则在调用 `Main` 方法之前，将执行该类的静态构造函数。

若要初始化新的封闭式类类型，请先创建一组新的已关闭类型的静态字段（[静态字段和实例字段](classes.md#static-and-instance-fields)）。 每个静态字段都初始化为其默认值（[默认值](variables.md#default-values)）。 接下来，对这些静态字段执行静态字段初始值设定项（[静态字段初始化](classes.md#static-field-initialization)）。 最后，执行静态构造函数。

示例
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
必须生成输出：
```console
Init A
A.F
Init B
B.F
```
因为对 `A.F`的调用触发了 `A`的静态构造函数的执行，并且 `B`的静态构造函数的执行由对 `B.F`的调用触发。

可以构造循环依赖关系，以便在其默认值状态中观察包含变量初始值设定项的静态字段。

示例
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
生成输出
```console
X = 1, Y = 2
```

若要执行 `Main` 方法，系统首先在类 `B`的静态构造函数之前运行 `B.Y`的初始化表达式。 `Y`初始值设定项会导致运行 `A`的静态构造函数，因为引用 `A.X` 的值。  `A` 的静态构造函数将继续计算 `X`的值，并且在执行此操作时，将获取 `Y`的默认值，即零。 `A.X` 将初始化为1。 运行 `A`的静态字段初始值设定项和静态构造函数的过程随后完成，返回 `Y`初始值的计算结果为2。

由于静态构造函数仅对每个封闭式构造类类型执行一次，因此，在不能通过约束（[类型参数约束](classes.md#type-parameter-constraints)）在编译时无法检查的类型参数上强制执行运行时检查。 例如，下面的类型使用静态构造函数来强制类型参数是一个枚举：
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>析构函数。

***析构函数***是实现析构类的实例所需的操作的成员。 使用*destructor_declaration*声明析构函数：

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

*Destructor_declaration*可以包含一组*属性*（[特性](attributes.md)）。

*Destructor_declaration*的*标识符*必须命名声明析构函数的类。 如果指定了其他名称，则会发生编译时错误。

当析构函数声明包含 `extern` 修饰符时，析构函数被称为***外部析构函数***。 由于外部析构函数声明不提供实际的实现，因此它的*destructor_body*由分号组成。 对于所有其他析构函数， *destructor_body*包含一个*块*，它指定要执行的语句，以便销毁类的实例。 *Destructor_body*完全对应于具有 `void` 返回类型（[方法主体](classes.md#method-body)）的实例方法的*method_body* 。

析构函数不是继承的。 因此，一个类不包含析构函数，而析构函数可能在该类中声明。

由于析构函数不需要任何参数，因此不能重载，因此一个类最多只能有一个析构函数。

析构函数是自动调用的，不能被显式调用。 当某个实例对于任何代码都无法使用该实例时，该实例将可用于销毁。 在实例符合析构条件后，可能会在任何时间执行实例的析构函数。 当实例销毁时，该实例的继承链中的析构函数将按顺序调用，从最常派生到最小派生。 析构函数可以在任何线程上执行。 若要进一步讨论控制何时以及如何执行析构函数，请参阅[自动内存管理](basic-concepts.md#automatic-memory-management)。

示例的输出
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
由于继承链中的析构函数按顺序调用，从最常派生到最小派生。

析构函数是通过重写 `System.Object`上的虚拟方法 `Finalize` 来实现的。 C#不允许程序重写此方法或直接对其进行调用（或重写）。 例如，程序
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
包含两个错误。

编译器的行为就像此方法和它的替代一样，根本就不存在。 因此，此程序：
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
有效，并显示的方法隐藏 `System.Object`的 `Finalize` 方法。

有关从析构函数引发异常时的行为的讨论，请参阅[如何处理异常](exceptions.md#how-exceptions-are-handled)。

## <a name="iterators"></a>迭代器

使用迭代器块（[块](statements.md#blocks)）实现的函数成员（[函数成员](expressions.md#function-members)）称为***迭代器***。

迭代器块可以用作函数成员的主体，前提是相应函数成员的返回类型是一个枚举器接口（[枚举器接口](classes.md#enumerator-interfaces)），或者是一个可枚举接口（可[枚举](classes.md#enumerable-interfaces)接口）。 它可以作为*method_body*、 *operator_body*或*accessor_body*出现，而事件、实例构造函数、静态构造函数和析构函数不能作为迭代器实现。

使用迭代器块实现函数成员时，函数成员的形参表的编译时错误是指定任何 `ref` 或 `out` 参数。

### <a name="enumerator-interfaces"></a>枚举器接口

***枚举器接口***是 `System.Collections.IEnumerator` 的非泛型接口和泛型接口的所有实例化 `System.Collections.Generic.IEnumerator<T>`。 为了简单起见，在本章中，这些接口分别作为 `IEnumerator` 和 `IEnumerator<T>`进行引用。

### <a name="enumerable-interfaces"></a>可枚举接口

可***枚举接口***是 `System.Collections.IEnumerable` 的非泛型接口和泛型接口的所有实例化 `System.Collections.Generic.IEnumerable<T>`。 为了简单起见，在本章中，这些接口分别作为 `IEnumerable` 和 `IEnumerable<T>`进行引用。

### <a name="yield-type"></a>Yield 类型

迭代器生成一个值序列，该序列具有相同的类型。 此类型称为迭代器的***yield 类型***。

*  返回 `IEnumerator` 或 `IEnumerable` 的迭代器的 yield 类型 `object`。
*  返回 `IEnumerator<T>` 或 `IEnumerable<T>` 的迭代器的 yield 类型 `T`。

### <a name="enumerator-objects"></a>枚举器对象

当使用迭代器块实现返回枚举器接口类型的函数成员时，调用函数成员不会立即执行迭代器块中的代码。 相反，将创建并返回一个***枚举器对象***。 此对象封装在迭代器块中指定的代码，并在调用枚举器对象的 `MoveNext` 方法时，执行迭代器块中的代码。 枚举器对象具有以下特征：

*  它实现 `IEnumerator` 和 `IEnumerator<T>`，其中 `T` 是迭代器的 yield 类型。
*  它实现 `System.IDisposable`。
*  使用传递到函数成员的参数值（如果有）和实例值的副本对其进行初始化。
*  它有四种可能的状态： "***之前***"、"***正在运行***"、"已***暂停***" 和 "***之后***"，最初处于***之前***的状态。

枚举器对象通常是编译器生成的枚举器类的一个实例，它封装迭代器块中的代码并实现枚举器接口，但也可以实现其他方法。 如果枚举器类由编译器生成，则该类将直接或间接嵌套在包含函数成员的类中，它将具有私有可访问性，并且它将具有为编译器使用而保留的名称（[标识符](lexical-structure.md#identifiers)）。

枚举器对象可以实现比上面指定的接口更多的接口。

以下各节描述了枚举器对象提供的 `IEnumerable` 和 `IEnumerable<T>` 接口实现的 `MoveNext`、`Current`和 `Dispose` 成员的确切行为。

请注意，枚举器对象不支持 `IEnumerator.Reset` 方法。 调用此方法将导致引发 `System.NotSupportedException`。

#### <a name="the-movenext-method"></a>MoveNext 方法

枚举器对象的 `MoveNext` 方法封装迭代器块的代码。 调用 `MoveNext` 方法将执行迭代器块中的代码，并根据需要设置枚举器对象的 `Current` 属性。 `MoveNext` 执行的精确操作取决于调用 `MoveNext` 时枚举器对象的状态：

*  如果枚举器对象的状态在***之前***，则调用 `MoveNext`：
   * 将状态更改为***正在运行***。
   * 将迭代器块的参数（包括 `this`）初始化到参数值和初始化枚举器对象时保存的实例值。
   * 从一开始就执行迭代器块，直到中断执行（如下所述）。
*  如果枚举器对象的状态为***正在运行***，则不指定调用 `MoveNext` 的结果。
*  如果枚举器对象的状态为 "已***挂起***"，则调用 `MoveNext`：
   * 将状态更改为***正在运行***。
   * 将所有局部变量和参数的值（包括此值）还原到上次挂起执行迭代器块时保存的值。 请注意，这些变量所引用的任何对象的内容可能自上一次调用 MoveNext 后发生了更改。
   * 在导致执行挂起的 `yield return` 语句之后立即继续执行迭代器块，并继续执行，直到中断执行（如下所述）。
*  如果枚举数对象的状态为***after***，则调用 `MoveNext` 将返回 `false`。


当 `MoveNext` 执行迭代器块时，可以通过以下四种方式中断执行：通过 `yield return` 语句、`yield break` 语句、遇到迭代器块的末尾以及引发的异常并传播到迭代器块中。

*  遇到 `yield return` 语句时（[yield 语句](statements.md#the-yield-statement)）：
   * 计算语句中给定的表达式，将其隐式转换为 yield 类型，并将其分配给枚举器对象的 `Current` 属性。
   * 迭代器主体的执行将被挂起。 保存所有局部变量和参数的值（包括 `this`），这与此 `yield return` 语句的位置相同。 如果 `yield return` 语句位于一个或多个 `try` 块中，则不会执行关联的 `finally` 块。
   * 枚举器对象的状态将更改为 "已***挂起***"。
   * `MoveNext` 方法将 `true` 返回到其调用方，这表示迭代已成功地推进到下一个值。
*  遇到 `yield break` 语句时（[yield 语句](statements.md#the-yield-statement)）：
   * 如果 `yield break` 语句在一个或多个 `try` 块中，则将执行关联的 `finally` 块。
   * 枚举器对象的状态将更改为***after***。
   * `MoveNext` 方法将 `false` 返回到其调用方，指示迭代已完成。
*  当遇到迭代器主体的末尾时：
   * 枚举器对象的状态将更改为***after***。
   * `MoveNext` 方法将 `false` 返回到其调用方，指示迭代已完成。
*  当引发异常并将其传播到迭代器块外时：
   * 迭代器正文中的相应 `finally` 块将由异常传播执行。
   * 枚举器对象的状态将更改为***after***。
   * 异常传播继续到 `MoveNext` 方法的调用方。

#### <a name="the-current-property"></a>当前属性

枚举器对象的 `Current` 属性受迭代器块中 `yield return` 语句的影响。

当枚举器对象处于***挂起***状态时，`Current` 的值为之前对 `MoveNext`所设置的值。 如果枚举器对象位于 "***之前***"、"***正在运行***" 或 "***之后***" 状态，则不指定访问 `Current` 的结果。

对于 yield 类型不是 `object`的迭代器，通过枚举器对象的 `IEnumerable` 实现访问 `Current` 的结果对应于通过枚举器对象的 `IEnumerator<T>` 实现访问 `Current`，并将结果强制转换为 `object`。

#### <a name="the-dispose-method"></a>Dispose 方法

`Dispose` 方法用于通过将枚举器对象的状态设置为***after***来清理迭代。

*  如果枚举数对象的状态在***之前***，则调用 `Dispose` 会将状态更改为***after***。
*  如果枚举器对象的状态为***正在运行***，则不指定调用 `Dispose` 的结果。
*  如果枚举器对象的状态为 "已***挂起***"，则调用 `Dispose`：
   * 将状态更改为***正在运行***。
   * 执行所有 finally 块，就好像最后执行的 `yield return` 语句是 `yield break` 语句一样。 如果这导致引发异常并将其传播到迭代器主体外，则枚举器对象的状态将设置为***after*** ，并将异常传播到 `Dispose` 方法的调用方。
   * 将状态更改为***after***。
*  如果枚举数对象的状态为***after***，则调用 `Dispose` 不会有任何影响。

### <a name="enumerable-objects"></a>可枚举对象

当使用迭代器块实现返回可枚举接口类型的函数成员时，调用函数成员不会立即执行迭代器块中的代码。 相反，将创建并返回一个可***枚举对象***。 可枚举对象的 `GetEnumerator` 方法返回一个枚举器对象，该对象封装迭代器块中指定的代码，并在调用枚举器对象的 `MoveNext` 方法时，执行迭代器块中的代码。 可枚举对象具有以下特征：

*  它实现 `IEnumerable` 和 `IEnumerable<T>`，其中 `T` 是迭代器的 yield 类型。
*  使用传递到函数成员的参数值（如果有）和实例值的副本对其进行初始化。

可枚举对象通常是编译器生成的可枚举类的实例，它封装迭代器块中的代码并实现可枚举接口，但也可以实现其他方法。 如果可枚举的类由编译器生成，则该类将直接或间接嵌套在包含函数成员的类中，它将具有私有可访问性，并且它将具有为编译器使用而保留的名称（[标识符](lexical-structure.md#identifiers)）。

可枚举对象可以实现比上面指定的接口更多的接口。 具体而言，可枚举对象还可以实现 `IEnumerator` 和 `IEnumerator<T>`，从而使其既可用作可枚举的，也可用作枚举器。 在该类型的实现中，第一次调用可枚举对象的 `GetEnumerator` 方法时，将返回可枚举对象本身。 对可枚举对象的 `GetEnumerator`（如果有）的后续调用会返回可枚举对象的副本。 因此，每个返回的枚举器都有其自己的状态，并且一个枚举器中的更改不会影响另一个枚举

#### <a name="the-getenumerator-method"></a>GetEnumerator 方法

可枚举对象提供 `IEnumerable` 和 `IEnumerable<T>` 接口的 `GetEnumerator` 方法的实现。 这两个 `GetEnumerator` 方法共享一个公共实现，该实现获取并返回可用的枚举器对象。 初始化枚举器对象时，将在初始化可枚举对象时保存的参数值和实例值进行初始化，否则枚举器对象的功能如[枚举器对象](classes.md#enumerator-objects)中所述。

### <a name="implementation-example"></a>实现示例

本部分介绍了在标准C#构造中可能的迭代器实现。 此处所述的实现基于 Microsoft C#编译器使用的相同原则，但它并不意味着强制实施或唯一可能。

以下 `Stack<T>` 类使用迭代器来实现其 `GetEnumerator` 方法。 迭代器按从上到下的顺序枚举堆栈中的元素。

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

可以将 `GetEnumerator` 方法转换为编译器生成的枚举器类的实例化，该类封装迭代器块中的代码，如下所示。

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

在前面的转换中，迭代器块中的代码将转换为状态机并放置在枚举器类的 `MoveNext` 方法中。 此外，局部变量 `i` 会转换为枚举器对象中的字段，因此它可以在 `MoveNext`的调用中继续存在。

下面的示例将打印整数1到10的简单乘法表。 示例中的 `FromTo` 方法返回一个可枚举对象，并使用迭代器实现。

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

可以将 `FromTo` 方法转换为编译器生成的可枚举类的实例化，该枚举类封装迭代器块中的代码，如下所示。

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

可枚举的类既实现可枚举接口又实现枚举器接口，使其既可用作可枚举的，也可用作枚举器。 第一次调用 `GetEnumerator` 方法时，将返回可枚举对象本身。 对可枚举对象的 `GetEnumerator`（如果有）的后续调用会返回可枚举对象的副本。 因此，每个返回的枚举器都有其自己的状态，并且一个枚举器中的更改不会影响另一个枚举 `Interlocked.CompareExchange` 方法用于确保线程安全的操作。

`from` 和 `to` 参数会转换为可枚举类中的字段。 因为在迭代器块中修改了 `from`，所以引入了一个附加的 `__from` 字段来保存提供给每个枚举器中的 `from` 的初始值。

如果 `0``__state`，则 `MoveNext` 方法会引发 `InvalidOperationException`。 这可以防止将可枚举对象用作枚举器对象，而无需先调用 `GetEnumerator`。

下面的示例演示一个简单的 tree 类。 `Tree<T>` 类使用迭代器来实现其 `GetEnumerator` 方法。 迭代器按中缀顺序枚举树的元素。

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

可以将 `GetEnumerator` 方法转换为编译器生成的枚举器类的实例化，该类封装迭代器块中的代码，如下所示。

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

将 `foreach` 语句中使用的编译器生成的临时内存提升为枚举器对象的 `__left` 和 `__right` 字段。 将仔细更新枚举器对象的 `__state` 字段，以便在引发异常时正确调用正确的 `Dispose()` 方法。 请注意，不能用简单的 `foreach` 语句来编写翻译后的代码。

## <a name="async-functions"></a>异步函数

带有 `async` 修饰符的方法（[方法](classes.md#methods)）或匿名函数（[匿名函数表达式](expressions.md#anonymous-function-expressions)）称为***异步函数***。 通常，术语***async***用于描述具有 `async` 修饰符的任何类型的函数。

异步函数的形参列表的编译时错误是指定任何 `ref` 或 `out` 参数。

异步方法的*return_type*必须是 `void` 或***任务类型***。 任务类型是 `System.Threading.Tasks.Task` 的，并且是从 `System.Threading.Tasks.Task<T>`构造的类型。 为了简单起见，在本章中，这些类型分别作为 `Task` 和 `Task<T>`进行引用。 返回任务类型的异步方法称作任务返回。

任务类型的确切定义是定义的实现，但从语言的角度来看，任务类型处于 "未完成"、"成功" 或 "出错" 状态之一。 出错的任务记录相关的异常。 成功的 `Task<T>` 记录 `T`类型的结果。 任务类型是可等待的，因此可以是 await 表达式（[await 表达式](expressions.md#await-expressions)）的操作数。

异步函数调用能够通过其主体中的 await 表达式（[await 表达式](expressions.md#await-expressions)）来挂起计算。 稍后可以通过***恢复委托***在挂起 await 表达式时恢复计算。 恢复委托的类型为 `System.Action`，当调用该函数时，异步函数调用的计算将从其停止的等待表达式中继续。 如果从未挂起函数调用，则异步函数调用的***当前调用方***是原始调用方，否则为恢复委托的最新调用方。

### <a name="evaluation-of-a-task-returning-async-function"></a>任务返回的异步函数的计算

调用任务返回的 async 函数会导致生成返回的任务类型的实例。 这称为 async 函数的***返回任务***。 任务最初处于 "未完成" 状态。

然后，将计算异步函数主体，直到它被挂起（通过等待表达式）或终止，此时，将控件返回给调用方，同时返回 task。

当 async 函数的主体终止时，返回的任务将不再处于 "未完成" 状态：

*  如果函数体因到达 return 语句或正文末尾而终止，则返回的任务中会记录任何结果值，这会置于成功状态。
*  如果函数体由于未捕获的异常而终止（[throw 语句](statements.md#the-throw-statement)），则会在返回任务中记录异常，并将其置于出错状态。

### <a name="evaluation-of-a-void-returning-async-function"></a>Void 返回的 async 函数的计算

如果异步函数的返回类型为 `void`，则计算方式与上述方法不同：由于没有返回任何任务，因此函数会将完成和异常传达给当前线程的***同步上下文***。 同步上下文的确切定义是依赖于实现的，但它表示当前线程正在运行的 "where"。 当对返回 void 的异步函数开始、成功完成或导致引发未捕获的异常时，会通知同步上下文。

这样，上下文就可以跟踪正在其下运行的空返回异步函数的数量，并决定如何传播它们发出的异常。
