# <a name="classes"></a>类

类是可以包含数据成员 （常量和字段），函数成员 （方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数和静态构造函数） 以及嵌套的类型的数据结构。 类类型支持继承，因此派生的类可以扩展和专门针对基类的机制。

## <a name="class-declarations"></a>类声明

一个*class_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations))，其用于声明新类。

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

一个*class_declaration*包含一组可选*特性*([特性](attributes.md)) 后, 跟一组可选的*class_modifier*s ([类修饰符](classes.md#class-modifiers)) 后, 跟可选`partial`修饰符，关键字后跟`class`和一个*标识符*命名的类后, 跟可选*type_parameter_list* ([类型参数](classes.md#type-parameters)) 后, 跟一个可选*class_base*规范 ([类 base规范](classes.md#class-base-specification)) 后, 跟一组可选*type_parameter_constraints_clause*s ([类型参数约束](classes.md#type-parameter-constraints)) 后, 跟*class_body* ([类正文](classes.md#class-body)) 后, 跟分号 （可选）。

类声明不能提供*type_parameter_constraints_clause*s 除非还提供*type_parameter_list*。

提供的类声明*type_parameter_list*是***泛型类声明***。 此外，嵌套在泛型类声明或泛型结构声明中的任何类本身就是泛型类声明，因为必须提供包含类型的类型参数创建构造的类型。

### <a name="class-modifiers"></a>类修饰符

一个*class_declaration*可以根据需要包含一系列类修饰符：

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

它是在类声明中多次出现同一修饰符的编译时错误。

`new`嵌套类上允许使用修饰符。 它指定类隐藏继承的成员相同的名称，如中所述[的新修饰符](classes.md#the-new-modifier)。 它是编译时错误`new`修饰符可以出现在类声明不是嵌套的类声明中。

`public`， `protected`， `internal`，和`private`修饰符控制类的可访问性。 具体取决于类声明发生的上下文，其中某些修饰符可能不允许使用 ([声明可访问性](basic-concepts.md#declared-accessibility))。

`abstract`，`sealed`和`static`修饰符将在以下各节中讨论。

#### <a name="abstract-classes"></a>抽象类

`abstract`修饰符用于指示类是不完整，它旨在用于只能用作基类。 一个抽象类不同于非抽象类通过以下方式：

*  不能直接实例化抽象类，它会导致编译时错误使用`new`的抽象类上的运算符。 虽然可能有变量和其编译时类型是抽象的值，但此类变量和值将一定是`null`或包含对派生自抽象类型的非抽象类的实例的引用。
*  允许 （但不是必需的） 的抽象类包含抽象成员。
*  无法密封的抽象类。

当非抽象类派生自抽象类时，非抽象类必须包括所有继承的抽象成员，从而重写这些抽象成员的实际实现。 在示例
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
抽象类`A`引入了一个抽象方法`F`。 类`B`引入了其他方法`G`，但由于它不提供的实现`F`，`B`也必须声明为抽象。 类`C`重写`F`并提供实际实现。 由于没有抽象成员在`C`，`C`是允许 （但不是必需的） 为非抽象。

#### <a name="sealed-classes"></a>密封的类

`sealed`修饰符用于防止从类派生。 如果密封的类指定为另一个类的基类，会发生编译时错误。

密封的类也不能为抽象类。

`sealed`修饰符主要用于防止无意的派生，但它还使某些运行时优化。 具体而言，由于密封的类永远不会有任何派生的类，则可以将密封的类实例上的虚拟函数成员调用转换为非虚拟调用。

#### <a name="static-classes"></a>静态类

`static`修饰符用于声明为类标记***静态类***。 静态类不能实例化，不能用作类型只能包含静态成员。 只有一个静态类可以包含扩展方法的声明 ([扩展方法](classes.md#extension-methods))。

静态类声明会受到以下限制：

*  可能不包括静态类`sealed`或`abstract`修饰符。 但请注意，因为静态类无法实例化或派生自，其行为就像它已密封和抽象。
*  可能不包括静态类*class_base*规范 ([类基规范](classes.md#class-base-specification)) 和不能显式指定的基类或实现的接口的列表。 静态类隐式继承自类型`object`。
*  静态类只能包含静态成员 ([静态和实例成员](classes.md#static-and-instance-members))。 请注意，常量和嵌套的类型归为静态成员。
*  静态类不能具有与成员`protected`或`protected internal`声明可访问性。

它是一个编译时错误违反任何这些限制。

静态类有实例构造函数。 不能在静态类中的实例构造函数，没有默认实例构造函数声明 ([默认构造函数](classes.md#default-constructors)) 提供静态的类。

静态类的成员不是自动静态的并且必须显式包含成员声明`static`（除外常量和嵌套的类型） 的修饰符。 当一个类嵌套在外部静态类时，嵌套的类不是静态类除非显式包括`static`修饰符。

__引用静态类类型__

一个*namespace_or_type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 允许以引用静态类，如果

*  *Namespace_or_type_name*是`T`中*namespace_or_type_name*窗体的`T.I`，或
*  *Namespace_or_type_name*是`T`中*typeof_expression* ([自变量列表](expressions.md#argument-lists)1) 的窗体`typeof(T)`。

一个*primary_expression* ([函数成员](expressions.md#function-members)) 允许以引用静态类，如果

*  *Primary_expression*是`E`中*member_access* ([编译时检查的动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 的窗体`E.I`。

在任何其他上下文中是编译时错误，以引用静态类。 例如，它是错误的一个静态类用作基类，构成类型 ([嵌套类型](classes.md#nested-types)) 的成员、 泛型类型参数或类型参数约束。 同样，在数组类型、 指针类型，不能使用静态类`new`表达式、 强制转换表达式，`is`表达式`as`表达式，`sizeof`表达式或默认值表达式。

### <a name="partial-modifier"></a>Partial 修饰符

`partial`修饰符用于指示此*class_declaration*是分部类型声明。 具有相同名称的封闭命名空间或类型声明中的多个分部类型声明组合到窗体的一种类型声明，遵循的规则中指定[分部类型](classes.md#partial-types)。

如果生产或在不同的上下文中维护这些段具有分布在单独的程序文本段的类的声明可以很有用。 例如，类声明的一部分可能是由计算机生成，而手动编写的其他。 两个文本分隔可以防止用一个与更新冲突由其他更新。

### <a name="type-parameters"></a>类型参数

类型参数是一个简单标识符，表示所提供的用于创建构造的类型的类型参数的占位符。 类型参数是将更高版本提供的类型的正式占位符。 与之相反，类型参数 ([类型参数](types.md#type-arguments)) 是创建构造的类型时替换为类型参数的实际类型。

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

在类声明中的每个类型参数声明空间中定义一个名称 ([声明](basic-concepts.md#declarations)) 的类。 因此，它不能与另一个类型参数相同的名称或成员在该类中声明。 类型参数不能具有类型本身相同的名称。

### <a name="class-base-specification"></a>类的基规范

类声明可能包括*class_base*规格，它定义类和接口的类的直接基类 ([接口](interfaces.md)) 直接由类实现。

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

在类声明中指定的基类可以是构造的类类型 ([构造类型](types.md#constructed-types))。 基类不能是自身、 类型参数，但它可能涉及到作用域中的类型参数。

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>基类

当*class_type*中包含*class_base*，它指定要声明的类的直接基类。 如果类声明没有*class_base*，或者如果*class_base*列表只有接口类型、 直接基类被假定为`object`。 类从其直接基类继承成员，如中所述[继承](classes.md#inheritance)。

在示例
```csharp
class A {}

class B: A {}
```
类`A`称其直接基类`B`，并`B`称为派生自`A`。 由于`A`does 未显式指定直接基类，其直接基类是隐式`object`。

对于构造的类类型，如果在泛型类声明中，指定基类的构造类型的基类通过替换为每个*type_parameter*中相应的基类声明*type_argument*构造类型。 给定泛型类声明
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
构造类型的基类`G<int>`将为`B<string,int[]>`。

类类型的直接基类必须至少与类类型本身相同的可访问性 ([可访问性域](basic-concepts.md#accessibility-domains))。 例如，它是用于编译时错误`public`类进行派生`private`或`internal`类。

类类型的直接基类必须不能是任何以下类型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。 此外，不能使用泛型类声明`System.Attribute`作为直接或间接的基类。

确定直接基类规范的含义时`A`类的`B`，直接基类`B`暂时被假定为`object`。 直观地这可确保基类规范的含义，不能以递归方式依赖于自身。 下面的示例：
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
基类规范中自出错`A<C.B>`的直接基类`C`被视为可`object`，，因此 (通过的规则[Namespace 和类型名称](basic-concepts.md#namespace-and-type-names))`C`不被视为到有一个成员`B`。

类类型的基类都是直接基类和其基类。 换而言之，组的基本类是直接基类关系的传递闭包。 以下示例中，基类的引用`B`都`A`和`object`。 在示例
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
类的基类`D<int>`都`C<int[]>`， `B<IComparable<int[]>>`， `A`，和`object`。

除了类`object`，每个类类型都有一个直接基类。 `object`类没有直接的基类，并且所有其他类的最终基类。

当类`B`派生自类`A`，它是编译时错误`A`依赖于`B`。 一个类***直接依赖于***它 （如果有） 的直接基类并***直接依赖于***在其中它立即嵌套 （如果有） 的类。 根据此定义，完整的类所依赖的类集是反身和可传递闭包***直接依赖于***关系。

该示例
```csharp
class A: A {}
```
因为此类依赖于本身是错误的。 同样，示例
```csharp
class A: B {}
class B: C {}
class C: A {}
```
错误是因为类之间循环依赖。 最后，示例
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
导致编译时错误，因为`A`取决于`B.C`（它的直接基类），取决于`B`（其最近的封闭类），该循环依赖于`A`。

请注意类不依赖于嵌套在它的类。 在示例
```csharp
class A
{
    class B: A {}
}
```
`B` 取决于`A`(因为`A`是其直接基类和其最近的封闭类)，但`A`不依赖于`B`(因为`B`既不的基类，也不是封闭类的`A`). 因此，此示例是有效的。

不能从派生`sealed`类。 在示例
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
类`B`是错误，因为它会尝试派生自`sealed`类`A`。

#### <a name="interface-implementations"></a>接口实现

一个*class_base*规范可能包括接口类型，认为字母大写类直接实现给定的接口类型的列表。 讨论了接口实现中进一步[接口实现代码](interfaces.md#interface-implementations)。

### <a name="type-parameter-constraints"></a>类型参数约束

泛型类型和方法声明可以选择通过包含指定类型参数约束*type_parameter_constraints_clause*s。

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

每个*type_parameter_constraints_clause*包含令牌`where`后, 跟类型参数的名称后, 跟一个冒号和为该类型形参的约束的列表。 可以是最多有`where`子句为每个类型参数，和`where`子句可以任何顺序列出。 像`get`并`set`属性访问器中的令牌`where`令牌不是一个关键字。

给出的约束列表`where`子句可以按以下顺序包含任何以下组件： 单个主约束、 一个或多个辅助约束和构造函数约束`new()`。

主键约束可以是类类型或***引用类型约束***`class`或***值类型约束*** `struct`。 辅助约束可以是*type_parameter*或*interface_type*。

引用类型约束指定类型参数所用的类型实参必须是引用类型。 所有类类型、 接口类型、 委托类型、 数组类型和已知为引用类型 （如定义如下） 的类型参数都满足此约束。

值类型约束指定的类型参数使用的类型实参必须是不可以为 null 的值类型。 所有不可为 null 的结构类型、 枚举类型和值类型约束的类型参数满足此约束。 请注意，尽管分类为值类型，可以为 null 的类型 ([为 Null 的类型](types.md#nullable-types)) 不满足的值类型约束。 具有值类型约束的类型形参不能同时具有*constructor_constraint*。

指针类型从不允许作为类型参数，并且不会考虑以满足引用类型或值类型约束。

如果约束是类类型、 接口类型或类型参数，该类型指定的最小"基类型"为该类型形参使用每个类型实参必须支持。 只要使用的构造的类型或泛型方法，将针对在编译时类型参数的约束检查的类型参数。 提供的类型实参必须满足的条件中所述[满足约束](types.md#satisfying-constraints)。

一个*class_type*约束必须满足以下规则：

*  类型必须是类类型。
*  类型不能`sealed`。
*  类型不能属于以下类型之一： `System.Array`， `System.Delegate`， `System.Enum`，或`System.ValueType`。
*  类型不能`object`。 由于所有类型都派生`object`，如果已允许此类约束会产生任何影响。
*  最多一个约束为给定的类型参数可以是类类型。

为指定的类型*interface_type*约束必须满足以下规则：

*  类型必须是接口类型。
*  必须未指定类型不止一次在给定`where`子句。

在任一情况下，该约束可以包括任何关联的类型或方法声明的构造类型一部分的类型参数，也可以包含要声明的类型。

指定类型参数约束必须至少与可访问性为任何类或接口类型 ([可访问性约束](basic-concepts.md#accessibility-constraints)) 作为泛型类型或所声明的方法。

为指定的类型*type_parameter*约束必须满足以下规则：

*  类型必须是类型参数。
*  必须未指定类型不止一次在给定`where`子句。

此外必须没有循环中的类型参数，其中依赖项是可传递关系定义的依赖项关系图：

*  如果类型形参`T`用作类型参数约束`S`然后`S`***取决于*** `T`。
*  如果类型形参`S`取决于类型参数`T`并`T`取决于类型参数`U`然后`S`***取决于*** `U`。

根据此关系，则会用于依赖于自身 （直接或间接） 的类型参数的编译时错误。

任何约束必须依赖类型参数之间保持一致。 如果类型形参`S`取决于类型参数`T`然后：

*  `T` 必须具有值类型约束。 否则为`T`有效地密封的这样`S`将被强制为相同的类型为`T`，无需两个类型参数。
*  如果`S`具有值类型约束，然后`T`不能*class_type*约束。
*  如果`S`已*class_type*约束`A`并`T`具有*class_type*约束`B`则必须是标识转换或隐式引用转换从`A`到`B`或从隐式引用转换`B`到`A`。
*  如果`S`还取决于类型参数`U`并`U`具有*class_type*约束`A`并`T`具有*class_type*约束`B`则必须为标识转换或从隐式引用转换`A`到`B`或从隐式引用转换`B`到`A`。

它是有效的`S`具有值类型约束和`T`具有引用类型约束。 这将有效地限制`T`到类型`System.Object`， `System.ValueType`， `System.Enum`，和任何接口类型。

如果`where`的类型参数的子句包括构造函数约束 (其中包含窗体`new()`)，则可以使用`new`运算符来创建该类型的实例 ([对象创建表达式](expressions.md#object-creation-expressions)). 任何类型使用参数的构造函数约束的类型参数必须具有公共的无参数构造函数 （此构造函数隐式存在任何值类型） 也是具有值类型约束或构造函数约束 （请参阅的类型形参[类型参数约束](classes.md#type-parameter-constraints)有关详细信息)。

约束的示例如下：
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

下面的示例是错误的因为它将导致发生循环中的类型参数的依赖项关系图：
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

下面的示例演示了一些无效的情况：
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

***有效基类***的类型参数`T`定义，如下所示：

*  如果`T`没有主约束或类型参数约束，其有效的基类是`object`。
*  如果`T`具有值类型约束，其有效的基类是`System.ValueType`。
*  如果`T`已*class_type*约束`C`但没有*type_parameter*约束，其有效的基类是`C`。
*  如果`T`不具有*class_type*约束但具有一个或多个*type_parameter*约束，其有效的基本类是包含程度最大的类型 ([提升转换运算符](conversions.md#lifted-conversion-operators)) 中的一套有效的基类及其*type_parameter*约束。 一致性规则可确保不存在此类包含程度最大的类型。
*  如果`T`同时具有*class_type*约束和一个或多*type_parameter*约束，其有效的基本类是包含程度最大的类型 ([提升转换运算符](conversions.md#lifted-conversion-operators)) 组成的集合中*class_type*约束`T`和有效的基类的其*type_parameter*约束。 一致性规则可确保不存在此类包含程度最大的类型。
*  如果`T`具有引用类型约束但不会*class_type*约束，其有效的基类是`object`。

在这些规则，如果 T 具有约束`V`也就是说*value_type*，改为使用的最具体的基类型`V`即*class_type*。 这可以永远不会发生中显式给定的约束，但隐式继承通过重写方法声明或显式实现的接口方法的泛型方法的约束时可能会发生。

这些规则可确保始终是有效基类*class_type*。

***有效接口集中***的类型参数`T`定义，如下所示：

*  如果`T`不具有*secondary_constraints*，其有效的接口集为空。
*  如果`T`已*interface_type*约束，但没有*type_parameter*约束，其有效的接口集是其一套*interface_type*约束。
*  如果`T`不具有*interface_type*约束，但具有*type_parameter*约束，其有效的接口集是有效的接口集的并集及其*得以参数*约束。
*  如果`T`同时具有*interface_type*约束和*type_parameter*约束，其有效的接口集是其一组联合*interface_type*约束和有效的接口集及其*type_parameter*约束。

类型参数是***已知为引用类型***如果它具有引用类型约束，或者不是其有效基类`object`或`System.ValueType`。

受约束的类型参数类型的值可以用于访问实例成员权限隐含的约束。 在示例
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
方法`IPrintable`可以直接上调用`x`因为`T`被约束为始终实现`IPrintable`。

### <a name="class-body"></a>类的主体

*Class_body*的类定义了该类的成员。

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>分部类型

类型声明可以跨多个拆分***分部类型声明***。 类型声明构造从其各个部分按照规则在此部分中，先将其视为编译时和运行时处理程序的其余部分在单个声明。

一个*class_declaration*， *struct_declaration*或*interface_declaration*表示分部类型声明，如果它包含`partial`修饰符。 `partial` 不是一个关键字，并仅充当一个修饰符，如果后面紧接着关键字之一`class`，`struct`或`interface`在类型声明中，或在类型前面`void`方法声明中。 在其他上下文中它可以用作常规标识符。

分部类型声明的每个部分必须包含`partial`修饰符。 它必须具有相同的名称，并在相同的命名空间或作为其他部件的类型声明中声明。 `partial`修饰符表示类型声明的其他部分可能存在其他位置，但此类中的其他部件存在并不是一项要求; 它是与单个声明类型，包括有效的`partial`修饰符。

分部类型的所有部分都必须一起都编译，以便这些部分可为单个类型声明在编译时合并。 特别是分部类型不允许已编译的类型进行扩展。

嵌套的类型可能会在多个部分中声明使用`partial`修饰符。 通常情况下，使用声明包含类型`partial`，并且每个部分的嵌套类型声明中包含的类型的不同部分。

`partial`对委托或枚举声明不允许使用修饰符。

### <a name="attributes"></a>特性

未指定顺序组合的每个组成部分的属性取决于在分部类型的属性。 如果特性被放置在多个部分，相当于在类型上多次指定该属性。 例如，两个部分：

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
如是等效于声明的：
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

类型参数的属性以类似方式进行组合。

### <a name="modifiers"></a>修饰符

如果分部类型声明包含的可访问性规范 ( `public`， `protected`， `internal`，和`private`修饰符) 与所有包含的可访问性规范的其他部分，它必须一致。 如果在分部类型的任何部分不包含的可访问性规范，该类型将获得适当的默认可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility))。

如果一个或多个分部声明的嵌套类型包括`new`修饰符，如果嵌套的类型会隐藏继承的成员，则会报告任何警告 ([通过继承隐藏](basic-concepts.md#hiding-through-inheritance))。

如果一个或多个分部声明的类包括`abstract`修饰符，类被视为抽象 ([抽象类](classes.md#abstract-classes))。 否则，此类被视为非抽象类。

如果一个或多个分部声明的类包括`sealed`修饰符，该类都被视为密封 ([密封类](classes.md#sealed-classes))。 否则，视为类未密封。

请注意，一个类不能为抽象和密封。

当`unsafe`修饰符用于在分部类型声明，只有该特定部分被视为不安全的上下文 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。

### <a name="type-parameters-and-constraints"></a>类型参数和约束

如果在多个部分中声明一个泛型类型，每个部分都必须声明的类型参数。 每个部分必须具有顺序相同数量的类型参数和每个类型形参，相同的名称。

当部分泛型类型声明包括约束 (`where`子句)，约束必须同意包含约束的所有其他部分。 具体而言，每个包含约束的一部分必须为同一组的类型参数的约束，并且为每个类型参数设置的主要，辅助数据库，并且构造函数约束必须等效。 如果它们包含相同的成员的约束的两个集合相等。 如果部分泛型类型的任何部分指定类型参数约束的类型参数被视为不受约束。

该示例
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
是正确的因为有效地包含约束 （前两个） 的这些部分指定的相同设置的主要，辅助，和为同一组的类型参数的构造函数约束分别。

### <a name="base-class"></a>基类

分部类声明包含基类规范时，它必须与包含基类规范的所有其他部分一致。 如果分部类中的任何部分不包含基类规范，则基类将成为`System.Object`([基类](classes.md#base-classes))。

### <a name="base-interfaces"></a>基接口

基接口在多个部分中声明的类型集是指定每个部分的基接口的并集。 特定的基接口中只能指定一次每个部分，但它允许多个部分来命名相同的基接口。 必须仅有的任何给定的基接口成员的一种实现。

在示例
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
基接口的类的一套`C`是`IA`， `IB`，和`IC`。

通常情况下，每个部分提供声明在该部分; 上的接口的实现但是，这不是一项要求。 一个部件可能会提供声明的其他部分上的接口的实现：
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

### <a name="members"></a>成员

分部方法除外 ([分部方法](classes.md#partial-methods))，在多个部分中声明类型的成员的集是只需在每个部分中声明成员的集的并集。 正文类型声明的所有部件共享同一声明空间 ([声明](basic-concepts.md#declarations))，以及每个成员的范围 ([作用域](basic-concepts.md#scopes)) 扩展到所有部件的正文。 任何成员的可访问域始终包括封闭类型; 所有的部分`private`成员声明为一个部分是从另一个部分可免费访问。 它是要声明多个类型的部分中的同一成员的编译时错误，除非该成员的类型`partial`修饰符。

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

在类型成员的排序到 C# 代码中，很少有效，但与其他语言和环境交互时可能很大。 在这些情况下，在多个部分中声明的类型中的成员的排序是未定义。

### <a name="partial-methods"></a>分部方法

可以定义的类型声明的一部分中并在另一个中实现分部方法。 实现是可选的;如果任何部分不实现分部方法，从生成的各个部分组合的类型声明中删除分部方法声明和对它的所有调用。

分部方法不能定义访问修饰符，但为隐式`private`。 它们的返回类型必须是`void`，并且其参数不能具有`out`修饰符。 标识符`partial`仅当它出现之前被识别为一个方法声明中的特殊关键字`void`类型; 否则可以使用它作为普通标识符。 分部方法不能显式实现接口方法。

有两种类型的分部方法声明： 如果在方法声明的正文是分号，声明称***定义分部方法声明***。 如果在正文中指定为*块*，在声明称为***实现分部方法声明***。 在类型声明的部分之间可以有只有一个定义分部方法声明具有给定签名，并且可能只有一个实现分部方法声明具有给定签名。 如果给定实现分部方法声明中，相应定义分部方法声明必须存在，并且声明必须匹配指定所示：

* 声明必须具有相同的修饰符 （尽管不一定按相同顺序），方法名称、 类型参数的数目和数量的参数。
* 在声明中的相应参数必须具有相同的修饰符 （尽管不一定按相同顺序） 和 （取模的类型参数名称的差异） 相同的类型。
* 在声明中的相应类型参数必须具有相同的约束 （取模的类型参数名称的差异）。

实现分部方法声明可以出现在为相应定义分部方法声明的相同部分。

定义分部方法参与重载决策。 因此，无论给定实现声明，则调用表达式可能会解析为分部方法的调用。 因为分部方法始终返回`void`，此类调用表达式将始终为表达式语句。 此外，因为分部方法是隐式`private`，此类语句将总是出现在分部方法声明中的类型声明的部分之一。

如果分部类型声明的任何部分不包含有关给定的分部方法实现声明，只需从组合的类型声明中删除调用它的任何表达式语句。 因此调用表达式，包括任何构成的表达式，在运行时没有任何影响。 分部方法本身也会删除并不是组合的类型声明的成员。

如果实现声明存在给定的分部方法，将保留分部方法的调用。 分部方法导致在方法声明类似于实现的分部方法声明，但以下字符串除外：

* `partial`修饰符不包含
* 生成的方法声明中的属性中未指定的顺序是组合定义和实现的分部方法声明的属性。 不删除重复项。
* 生成的方法声明的参数的属性中未指定的顺序是特性的相应参数的定义和实现的分部方法声明的结合。 不删除重复项。

如果定义声明，但不是实现声明为分部方法 M 指定，则以下限制适用：

* 它会导致编译时错误创建方法的委托 ([委托创建表达式](expressions.md#delegate-creation-expressions))。
* 它会导致编译时错误是指`M`内部转换为表达式目录树类型的匿名函数 ([匿名函数转换为表达式树类型的计算](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。
* 表达式一部分的调用发生`M`不会影响明确赋值状态 ([明确赋值](variables.md#definite-assignment))，这可能会导致编译时错误。
* `M` 不能为应用程序的入口点 ([应用程序启动](basic-concepts.md#application-startup))。

分部方法可用于允许自定义的另一个部件，例如，一个由工具生成的行为的类型声明的一部分。 请考虑下面的分部类声明：
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

如果没有任何其他部分的情况下编译此类后，定义分部方法声明和其调用将被删除，并将等效于以下的结果组合的类声明：
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

假定，另一个部分给出，但是，其中提供了实现的分部方法声明：
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

则生成的组合的类声明将等效于以下：
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

虽然必须在相同的命名空间中声明的可扩展类型的每个部分，通常在不同的命名空间声明内编写部分。 因此，不同`using`指令 ([Using 指令](namespaces.md#using-directives)) 可能会显示每个部分。 解释的简单名称时 ([类型推理](expressions.md#type-inference)) 中一个部分，仅`using`被视为封闭该部分的命名空间定义的指令。 这可能会导致相同的标识符不同部分中具有不同的含义：
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

类的成员包含引入的成员及其*class_member_declaration*从直接基类和成员继承。

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

类类型的成员分为以下类别：

*  常量，表示与类关联的常量值 ([常量](classes.md#constants))。
*  字段，它们将类的变量 ([字段](classes.md#fields))。
*  方法，用于实现计算和可以由类执行的操作 ([方法](classes.md#methods))。
*  属性，用于定义名为特征和与读取和写入这些特性关联的操作 ([属性](classes.md#properties))。
*  定义可以由类生成的通知的事件 ([事件](classes.md#events))。
*  索引器，使相同的方式要编制索引的类的实例 （语法） 作为数组 ([索引器](classes.md#indexers))。
*  运算符定义可以应用于类的实例的表达式运算符 ([运算符](classes.md#operators))。
*  实例构造函数，实现初始化类的实例所需的操作 ([实例构造函数](classes.md#instance-constructors))
*  析构函数，实现永久放弃类的实例之前要执行的操作 ([析构函数](classes.md#destructors))。
*  静态构造函数，用于实现初始化类本身所需的操作 ([静态构造函数](classes.md#static-constructors))。
*  表示类型的本地类的类型 ([嵌套类型](classes.md#nested-types))。

可以包含可执行代码的成员统称为*函数成员*类类型。 类类型的函数成员是方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数和类类型的静态构造函数。

一个*class_declaration*创建一个新的声明空间 ([声明](basic-concepts.md#declarations))，并且*class_member_declaration*s 立即所包含的*类_declaration*引入此声明空间的新成员。 以下规则适用于*class_member_declaration*s:

*  实例构造函数、 析构函数和静态构造函数必须具有与最近的封闭类同名。 所有其他成员必须具有不同于最近的封闭类的名称的名称。
*  常量、 字段、 属性、 事件或类型的名称必须不同于在同一类中声明的所有其他成员的名称。
*  方法的名称必须不同于其他所有非-中声明的方法相同的类的名称。 此外，签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的一种方法必须不同于同一类中声明的所有其他方法的签名和完全不同的签名不能在同一个类中声明的两种方法通过`ref`和`out`。
*  实例构造函数的签名必须不同于在同一类中声明所有其他实例构造函数的签名，并且完全由不同的签名不能在同一个类中声明的两个构造函数`ref`和`out`。
*  索引器的签名必须不同于在同一类中声明的所有其他索引器的签名。
*  运算符的签名必须不同于在同一类中声明的所有其他运算符的签名。

类类型的继承的成员 ([继承](classes.md#inheritance)) 不是一个类的声明空间的一部分。 因此，允许派生的类声明一个具有相同名称或签名为继承的成员 （这实质上隐藏继承的成员） 的成员。

### <a name="the-instance-type"></a>实例类型

每个类声明有关联的绑定的类型 ([绑定，并且未绑定的类型](types.md#bound-and-unbound-types))，则***实例类型***。 泛型类声明，该实例类型形成通过创建构造的类型 ([构造类型](types.md#constructed-types)) 从类型声明中，与每个所提供的类型自变量的相应类型参数。 由于实例类型使用的类型参数，它只能在类型参数的位置是在范围内;也就是说，在类声明中。 实例类型是一种`this`在类声明的内部编写的代码。 对于非泛型类的实例类型是只需声明的类。 下面显示了多个类声明，以及其实例类型： 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>构造类型的成员

构造类型的非继承成员获取，只需替换为每个*type_parameter*在成员声明中，相应*type_argument*构造类型。 替换进程取决于语义含义的类型声明，并不是只是文本替换。

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
构造的类型`Gen<int[],IComparable<string>>`具有下列成员：
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

成员类型`a`泛型类声明中`Gen`是"的二维数组`T`"，因此该成员的类型`a`在上面的构造类型是"二维数组的一维数组`int`"，或`int[,][]`。

中的类型的实例函数成员`this`是使用实例类型 ([的实例类型](classes.md#the-instance-type)) 包含声明。

泛型类的所有成员可以都使用任何封闭类中的类型参数直接或作为构造类型的一部分。 当关闭特定构造类型 ([开放类型和封闭类型](types.md#open-and-closed-types)) 使用在运行时，每次使用类型参数替换为提供给构造的类型的实际类型实参。 例如：
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

一个类***继承***其直接基类类型的成员。 继承是指一个类隐式包含其直接基类类型，除实例构造函数、 析构函数和静态构造函数的类的基类的所有成员。 继承的一些重要方面是：

*  继承是可传递的。 如果`C`派生自`B`，并`B`派生自`A`，然后`C`继承中声明成员`B`中声明的成员以及`A`。
*  在派生的类扩展了其直接基类。 派生类可以其继承的类添加新成员，但无法删除继承成员的定义。
*  实例构造函数、 析构函数和静态构造函数不会继承，但所有其他成员，而不考虑其声明的可访问性 ([成员访问](basic-concepts.md#member-access))。 但是，具体取决于其声明的可访问性，继承的成员可能无法访问派生类中。
*  在派生的类可以***隐藏***([通过继承隐藏](basic-concepts.md#hiding-through-inheritance)) 通过声明具有相同名称或签名的新成员的继承成员。 请注意但是隐藏继承的成员不会删除该成员 — 它只是使该成员无法直接通过派生类访问。
*  类的实例包含的一组声明中的类以及其基本类和隐式转换的所有实例字段 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 存在从派生的类类型到任何基类类型。 因此，对某些派生类的实例的引用可视为对任何其基类，这些类的实例的引用。
*  虚方法、 属性和索引器，可以声明一个类和派生的类可以重写这些函数成员的实现。 这可使类显示其中一个函数成员调用执行的操作而异通过其调用该函数成员的实例的运行时类型的多态行为。

构造的类类型的继承的成员是直接基类类型的成员 ([基类](classes.md#base-classes))，它出现，只需替换相应类型的每个匹配项的类型参数的构造类型中的参数*class_base*规范。 反过来，这些成员进行转换，只需替换为每个*type_parameter*在成员声明中，相应*type_argument*的*class_base*规范。

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

在上面的示例中，构造类型`D<int>`有一个非继承的成员`public int G(string s)`通过替换类型参数来获取`int`类型参数`T`。 `D<int>` 还有类声明的继承的成员`B`。 确定此继承的成员时，首先确定基类类型`B<int[]>`的`D<int>`通过替换来`int`有关`T`基类规范中`B<T[]>`。 然后，作为类型参数`B`，`int[]`替换为`U`中`public U F(long index)`，无法完成继承的成员`public int[] F(long index)`。

### <a name="the-new-modifier"></a>New 修饰符

一个*class_member_declaration*允许声明一个具有相同名称或签名为继承的成员的成员。 派生的类成员时出现这种情况，称***隐藏***基类成员。 隐藏继承的成员不被视为错误，但它确实会导致编译器发出警告。 若要禁止显示警告，请在派生的类成员的声明可以包括`new`修饰符以指示派生的成员用于隐藏基类成员。 本主题讨论中进一步[通过继承隐藏](basic-concepts.md#hiding-through-inheritance)。

如果`new`修饰符包含在不隐藏继承的成员的声明，该结果会发出警告。 通过删除禁止显示此警告`new`修饰符。

### <a name="access-modifiers"></a>访问修饰符

一个*class_member_declaration*可以具有下列任一五个的可能类型的声明可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)): `public`， `protected internal`， `protected`， `internal`或`private`。 除`protected internal`组合，它会导致编译时错误指定多个访问修饰符。 当*class_member_declaration*不包括任何访问修饰符，`private`假定。

### <a name="constituent-types"></a>构成类型

成员的声明中使用的类型称为构成该成员的类型。 可能的构成类型是一个常量、 字段、 属性、 事件或索引器的类型、 方法或运算符的返回类型和方法、 索引器、 运算符或实例构造函数的参数类型。 成员的构成类型必须至少与该成员本身具有同样的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。

### <a name="static-and-instance-members"></a>静态和实例成员

类的成员为***静态成员***或***实例成员***。 通常情况下，最好考虑为属于类类型和实例成员为属于对象 （类类型的实例） 的静态成员。

当字段、 方法、 属性、 事件、 运算符或构造函数声明包括`static`修饰符，它声明了静态成员。 此外，常量或类型声明隐式声明的静态成员。 静态类成员具有以下特征：

*  当静态成员`M`中引用*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，`E`必须表示一个类型，其中包含`M`。 它是编译时错误`E`来表示实例。
*  静态字段用于标识由给定已关闭的类类型的所有实例共享一个存储位置。 无论创建多少个实例的已关闭的给定的类类型，是的静态字段只有一个副本。
*  静态函数成员 （方法、 属性、 事件、 运算符或构造函数） 不会进行特定的实例、 操作，并且它会导致编译时错误是指`this`中这样的函数成员。

当字段、 方法、 属性、 事件、 索引器、 构造函数或析构函数声明中不包括`static`修饰符，它声明实例成员。 （实例成员有时称为非静态成员。）实例成员具有以下特征：

*  当实例成员`M`中引用*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，`E`必须表示一个包含类型的一个实例`M`. 它是为绑定时错误`E`来表示一种类型。
*  每个实例的类包含一组单独的类的所有实例字段。
*  类的给定实例上运行的实例函数成员 （方法、 属性、 索引器、 实例构造函数或析构函数） 和此实例可作为访问`this`([此访问权限](expressions.md#this-access))。

下面的示例说明了用于访问静态的规则和实例成员：
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

`F`方法演示的实例函数成员，在*simple_name* ([简单名称](expressions.md#simple-names)) 可用来访问实例成员和静态成员。 `G`方法显示在静态函数成员中，它是编译时错误，若要访问通过实例成员*simple_name*。 `Main`方法显示在*member_access* ([成员访问](expressions.md#member-access))，必须通过情况下，访问实例成员，并且必须通过类型来访问静态成员。

### <a name="nested-types"></a>嵌套类型

在类或结构声明中声明的类型称为***嵌套类型***。 编译单元或命名空间中声明的类型称为***非嵌套类型***。

在示例
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
类`B`是嵌套的类型，因为它在类内声明`A`，和类`A`是非嵌套类型，因为它被声明为编译单元中。

#### <a name="fully-qualified-name"></a>完全限定的名称

完全限定的名称 ([完全限定名](basic-concepts.md#fully-qualified-names)) 为嵌套的类型`S.N`其中`S`是哪种类型中的类型的完全限定的名称`N`声明。

#### <a name="declared-accessibility"></a>声明的可访问性

非嵌套类型可以具有`public`或`internal`声明可访问性，而且有`internal`默认情况下声明可访问性。 嵌套的类型可以有这些形式的声明可访问性，以及一个或多个其他形式的声明可访问性，具体取决于包含类型是类或结构：

*  类中声明的嵌套的类型可以具有任何声明可访问性的五个窗体 (`public`， `protected internal`， `protected`， `internal`，或`private`) 和其他类成员，如默认为`private`声明可访问性。
*  在结构中声明的嵌套的类型可以具有任何三种形式的声明可访问性 (`public`， `internal`，或`private`) 和其他结构与成员一样，默认为`private`声明可访问性。

该示例
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
声明的私有嵌套的类`Node`。

#### <a name="hiding"></a>隐藏

嵌套的类型可能会隐藏 ([名称隐藏](basic-concepts.md#name-hiding)) 基成员。 `new`允许使用修饰符对嵌套的类型声明，以便明确表示隐藏。 该示例
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
显示了嵌套的类`M`隐藏的方法`M`中定义`Base`。

#### <a name="this-access"></a>此访问权限

嵌套的类型和它的包含类型没有与具有特殊关系*this_access* ([此访问权限](expressions.md#this-access))。 具体而言，`this`内嵌套类型不能用于引用包含类型的实例成员。 在其中的嵌套的类型需要对它的包含类型的实例成员访问的情况下，可以通过提供提供访问`this`包含作为嵌套类型的构造函数参数类型的实例。 下面的示例
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
显示了这个方法。 实例`C`创建的实例`Nested`，并将传递其自身`this`到`Nested`的构造函数，提供对后续访问`C`的实例成员。

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>对包含类型的私有和受保护成员的访问

嵌套的类型有权访问它的包含类型，包括具有包含类型的成员可以访问的成员的所有`private`和`protected`声明可访问性。 该示例
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
显示了一个类`C`，其中包含嵌套的类`Nested`。 内`Nested`，该方法`G`调用静态方法`F`中定义`C`，和`F`具有私有声明可访问性。

嵌套的类型还可以访问它的包含类型的基类型中定义的受保护的成员。 在示例
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
嵌套的类`Derived.Nested`访问受保护的方法`F`中定义`Derived`的基类， `Base`，也可由的实例通过调用`Derived`。

#### <a name="nested-types-in-generic-classes"></a>泛型类中的嵌套的类型

泛型类声明可以包含嵌套的类型声明。 在嵌套类型，可以使用封闭类的类型参数。 嵌套的类型声明可以包含其他类型参数仅适用于嵌套的类型。

包含在泛型类声明中的每个类型声明是隐式泛型类型声明。 当编写类型的引用嵌套在泛型类型时，包含构造的类型，包括其类型参数，必须具有名称。 但是，从外部类中的嵌套的类型可以使用而无需限定;构造的嵌套的类型时，可以隐式使用外部类的实例类型。 下面的示例演示三种不同的正确方式来指从创建一个构造类型`Inner`; 前两个等效：
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

尽管错误编程风格，嵌套类型中的类型参数可以隐藏成员或类型参数中的外部类型声明：
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

为了促进的基础 C# 运行时实现，为每个源成员声明中，为属性、 事件或索引器，该实现必须保留基于的类型成员声明中，其名称，以及其类型的两个方法签名。 它是一个程序来声明其签名与匹配其中一种成员保留签名，即使基础运行时实现不会进行编译时错误使用这些保留。

保留的名称不会引入声明，因此它们不参与成员查找。 但是，在声明关联的签名是否参与继承的保留的方法 ([继承](classes.md#inheritance))，并可以使用隐藏起来`new`修饰符 ([的新修饰符](classes.md#the-new-modifier))。

这些名称的预订有三种用途：

*  若要允许使用一个普通的标识符作为 get 方法名称或访问权限设置为 C# 语言功能的基础实现。
*  若要允许其他语言进行互操作作为 get 方法名称中使用一个普通的标识符或设置对 C# 语言功能的访问。
*  若要帮助确保由一个符合的编译器接受的源接受由另一个，使细节保留成员的名称一致在所有 C# 实现。

析构函数声明 ([析构函数](classes.md#destructors)) 也会导致签名被保留 ([析构函数为成员名称](classes.md#member-names-reserved-for-destructors))。

#### <a name="member-names-reserved-for-properties"></a>属性为保留的成员名称

为属性`P`([属性](classes.md#properties)) 的类型`T`，保留了以下签名：

```csharp
T get_P();
void set_P(T value);
```

保留这两个签名，即使该属性是只读的还是只写。

在示例
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
一个类`A`定义了一个只读属性`P`，因此保留签名`get_P`和`set_P`方法。 一个类`B`派生自`A`并隐藏这两个保留的签名。 该示例生成输出：
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>事件保留的成员名称

事件`E`([事件](classes.md#events)) 的委托类型`T`，保留了以下签名：
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>索引器的保留的成员名称

索引器 ([索引器](classes.md#indexers)) 的类型`T`与参数列表`L`，保留了以下签名：
```csharp
T get_Item(L);
void set_Item(L, T value);
```

保留这两个签名，即使索引器是只读或只写。

此外成员名称`Item`被保留。

#### <a name="member-names-reserved-for-destructors"></a>成员名称保留为析构函数

类包含析构函数 ([析构函数](classes.md#destructors))，保留以下签名：
```csharp
void Finalize();
```

## <a name="constants"></a>常量

一个***常量***是类成员，它表示常量值： 可以在编译时计算的值。 一个*constant_declaration*引入了给定类型的一个或多个常量。

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

一个*constant_declaration*可能包括一套*特性*([属性](attributes.md))，`new`修饰符 ([的新修饰符](classes.md#the-new-modifier))，和有效组合的四种访问修饰符 ([访问修饰符](classes.md#access-modifiers))。 特性和修饰符应用于所有由声明的成员*constant_declaration*。 即使常量被视为静态成员，也是如此*constant_declaration*既不需要也不允许`static`修饰符。 它是同一修饰符在常量声明中多次出现的错误的。

*类型*的*constant_declaration*指定该声明引入的成员的类型。 键入后跟一系列*constant_declarator*s，其中每个引入了新的成员。 一个*constant_declarator*组成*标识符*命名的成员后, 跟"`=`"令牌后, 跟*constant_expression* ([常量表达式](expressions.md#constant-expressions)) 提供的成员的值。

*类型*常量中指定声明必须是`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，`float`， `double`， `decimal`， `bool`， `string`、 一个*enum_type*，或*reference_type*。 每个*constant_expression*必须生成的目标类型或可以隐式转换为目标类型的类型的值 ([隐式转换](conversions.md#implicit-conversions))。

*类型*的一个常量必须至少与常量本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。

在表达式中使用获取的值的常量*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access))。

一个常量本身可以参与*constant_expression*。 因此，可在任何需要的构造函数中使用常量*constant_expression*。 此类构造的示例包括`case`标签`goto case`语句，`enum`成员声明、 属性和其他常量声明。

如中所述[常量表达式](expressions.md#constant-expressions)即*constant_expression*是可以在编译时完全计算的表达式。 以来创建的非 null 值的唯一办法*reference_type*而不`string`是应用`new`运算符，并且`new`运算符中不允许使用*constant_表达式*，唯一可能的值的常量*reference_type*以外的其他 s`string`是`null`。

时需要的常量值的符号名称，但该值的类型时不允许在常量声明中，或当不能在通过编译时计算的值*constant_expression*、`readonly`字段 （[只读字段](classes.md#readonly-fields)) 可以改为使用。

声明多个常数在常量声明是等效于单个常量与相同的属性、 修饰符和类型的多个声明。 例如
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

常量可以，只要依赖项不是循环依赖于在同一程序中其他常量。 编译器自动排列要评估的常量声明中的相应顺序。 在示例
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
编译器将首先计算`A.Y`，然后计算结果`B.Z`，并最后评估`A.X`，生成值`10`， `11`，并`12`。 常量声明可能取决于常量从其他程序，但只能按一个方向可能此类依赖项。 上述示例中，如果引用`A`和`B`声明，因此在单独程序中，将有可能`A.X`依赖`B.Z`，但`B.Z`无法则不能同时取决于`A.Y`。

## <a name="fields"></a>字段

一个***字段***是表示与对象或类关联的变量的成员。 一个*field_declaration*引入了给定类型的一个或多个字段。

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

一个*field_declaration*可能包括一套*特性*([属性](attributes.md))，`new`修饰符 ([new 修饰符](classes.md#the-new-modifier))、有效组合的四种访问修饰符 ([访问修饰符](classes.md#access-modifiers))，和一个`static`修饰符 ([静态和实例字段](classes.md#static-and-instance-fields))。 此外， *field_declaration*可能包括`readonly`修饰符 ([只读字段](classes.md#readonly-fields)) 或`volatile`修饰符 ([可变字段](classes.md#volatile-fields))，但不可同时使用两者。 特性和修饰符应用于所有由声明的成员*field_declaration*。 它是相同的修饰符，若要在字段声明中多次出现的错误。

*类型*的*field_declaration*指定该声明引入的成员的类型。 键入后跟一系列*variable_declarator*s，其中每个引入了新的成员。 一个*variable_declarator*组成*标识符*命名该成员，可以选择后跟"`=`"令牌和一个*variable_initializer* ([变量初始值设定项](classes.md#variable-initializers)) 使该成员的初始值。

*类型*的字段必须至少与字段本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。

在中使用表达式获取字段的值*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access))。 使用修改非只读字段的值*赋值*([赋值运算符](expressions.md#assignment-operators))。 非只读字段的值可以是获取和修改使用后缀递增和递减运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)) 和前缀递增和递减运算符 ([前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。

声明多个字段的字段声明是等效于多个具有相同的属性、 修饰符和类型的单个字段的声明。 例如
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

当字段声明包括`static`修饰符，该声明引入的字段是***静态字段***。 如果未`static`修饰符存在，该声明引入的字段***实例字段***。 静态字段和实例字段是两个有几种变量 ([变量](variables.md)) 和支持 C# 中，有时它们被称为***静态变量***和***的实例变量***分别。

静态字段不是实例的特定; 的一部分相反，它均共享已关闭类型的所有实例 ([打开和关闭类型](types.md#open-and-closed-types))。 无论创建多少个实例的已关闭的类类型，没有关联的应用程序域的静态字段只有一个副本。

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

实例所属的实例字段。 具体而言，每个实例的类包含一组单独的该类的所有实例字段。

字段中的引用时*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是一个静态字段`E`必须表示一个类型，其中包含`M`如果`M`为实例字段，则 E 必须表示一个类型，其中包含的实例`M`。

静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。

### <a name="readonly-fields"></a>只读字段

当*field_declaration*包括`readonly`修饰符，该声明引入的字段不***只读字段***。 Readonly 字段的直接赋值只能作为的一部分，该声明或在实例构造函数或同一个类中的静态构造函数中发生。 （只读字段可以被分配给多个时间在这些上下文中。）具体而言，直接分配到`readonly`字段允许仅在以下上下文中：

*  在*variable_declarator*这就带来了字段 (包括*variable_initializer*声明中)。
*  对于实例字段，在包含字段声明; 的类的实例构造函数包含字段声明的类的静态构造函数中的静态字段。 这些事件也是它是有效地传递的唯一上下文`readonly`字段作为`out`或`ref`参数。

尝试将分配给`readonly`字段，或将其作为传递`out`或`ref`在任何其他上下文中的参数是一个编译时错误。

#### <a name="using-static-readonly-fields-for-constants"></a>使用常量的静态只读字段

一个`static readonly`时需要的常量值的符号名称，但当中不允许的值的类型，字段很有用`const`声明，或当不能在编译时计算的值。 在示例
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
`Black`， `White`， `Red`， `Green`，并`Blue`成员不能声明为`const`成员由于不能在编译时计算其值。 但是，声明它们`static readonly`改为具有更相同的效果。

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>常量和静态只读字段的版本控制

常量和只读字段具有不同的二进制版本控制语义。 当表达式引用了一个常量时，在编译时，获取常量的值，但当表达式引用只读字段，直到运行时才获取字段的值。 请考虑包含两个单独的程序的应用程序：
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

`Program1`和`Program2`命名空间表示两个单独编译的程序。 因为`Program1.Utils.X`被声明为静态只读字段，输出的值`Console.WriteLine`语句不在编译时已知，但而不是在运行时获取。 因此，如果的值`X`更改和`Program1`重新编写`Console.WriteLine`语句会输出新值即使`Program2`不重新编译。 但是，有`X`已常量的值`X`将时获取`Program2`编译，并且将保持不受更改影响`Program1`直到`Program2`重新编译。

### <a name="volatile-fields"></a>可变字段

当*field_declaration*包括`volatile`修饰符，该声明引入的字段不***可变字段***。

对于非易失性字段，对指令重新排序的优化技术可能会导致意外的且不可预测结果中访问而不进行同步，例如提供的字段的多线程程序*lock_statement* ([Lock 语句](statements.md#the-lock-statement))。 由编译器、 运行时系统或硬件，可以执行这些优化。 对于可变字段，此类重新排序优化受到以下限制：

*  名为可变字段读取***易失读取***。 易失读取具有"获取语义;"也就是说，它保证任何对内存的引用后它对指令序列中出现之前发生。
*  可变字段的写入称为***可变写入***。 可变写入具有"发布语义;"也就是说，它保证操作之后在对指令序列中的写入指令之前的任何内存引用。

这些限制确保所有线程观察易失性写入操作（由任何其他线程执行）时的观察顺序与写入操作的执行顺序一致。 符合标准的实现不需要提供的单一总排序的所有线程的执行中显示的易失性写入。 可变字段的类型必须是以下值之一：

*  一个*reference_type*。
*  类型`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`， `char`， `float`， `bool`， `System.IntPtr`，或` System.UIntPtr`。
*  *Enum_type*具有枚举基类型的`byte`， `sbyte`， `short`， `ushort`， `int`，或`uint`。

该示例
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
```
result = 143
```

在此示例中，该方法`Main`可以启动一个新线程的运行方法`Thread2`。 此方法将值存储到一个名为的非易失性字段`result`，然后将存储`true`可变字段中`finished`。 主线程等待字段`finished`设置为`true`，然后读取字段`result`。 由于`finished`已声明`volatile`，在主线程必须读取的值`143`字段中`result`。 如果该字段`finished`尚未声明`volatile`，则将允许到应用商店`result`后的存储为对主线程可见`finished`，，因此读取值的主线程`0`从字段`result`。 声明`finished`作为`volatile`字段会阻止任何此类不一致。

### <a name="field-initialization"></a>字段初始化

字段的初始值的静态字段或实例字段，无论是默认值 ([默认值](variables.md#default-values)) 的字段的类型。 不能观察到的字段值之前此默认值初始化已发生，并且字段未因此永远不会"初始化"。 该示例
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
```
b = False, i = 0
```
因为`b`和`i`都被自动初始化为默认值。

### <a name="variable-initializers"></a>变量初始值设定项

字段声明可能包括*variable_initializer*s。 对于静态字段，变量初始值设定项对应于类初始化过程中执行赋值语句。 对于实例字段，变量初始值设定项对应于在创建类的实例时执行的赋值语句。

该示例
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
```
x = 1.4142135623731, i = 100, s = Hello
```
因为向赋值`x`发生时执行静态字段初始值设定项和分配到`i`和`s`实例字段初始值设定项执行时发生。

中所述的默认值初始化[字段初始化](classes.md#field-initialization)出现的所有字段，包括具有变量初始值设定项的字段。 因此，初始化类时，首先将该类中的所有静态字段初始化为其默认值，并且然后按文本顺序执行的静态字段初始值设定项。 同样，创建一个类的实例后，首先将该实例中的所有实例字段都初始化为其默认值，而且然后按文本顺序执行的实例字段初始值设定项。

很可能具有变量初始值设定项在其默认值状态中观察到的静态字段。 但是，这是作为一种样式，强烈建议不要使用。 该示例
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
演示这一行为。 尽管的循环定义，而 b，该程序是有效。 这会导致输出
```
a = 1, b = 2
```
因为静态字段`a`并`b`初始化为`0`(的默认值为`int`) 执行其初始值设定项之前。 时的初始值设定项`a`运行时，值`b`为零，因此`a`初始化为`1`。 时的初始值设定项`b`运行时，值`a`已经`1`，，因此`b`初始化为`2`。

#### <a name="static-field-initialization"></a>静态字段初始化

静态字段变量初始值设定项类的对应于一系列类声明中显示的文本顺序执行的分配。 如果静态构造函数 ([静态构造函数](classes.md#static-constructors)) 存在在类中，执行静态字段初始值设定项在执行该静态构造函数之前立即发生。 否则，在首次使用该类的静态字段之前实现相关时所执行的静态字段初始值设定项。 该示例
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
可能会产生下列输出：
```
Init A
Init B
1 1
```
或输出：
```
Init B
Init A
1 1
```
因为在执行`X`的初始值设定项和`Y`的初始值设定项可能会发生上述两种顺序; 它们仅限制发生后才对这些字段的引用。 但是，在下面的示例：
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
必须是输出：
```
Init B
Init A
1 1
```
因为当静态构造函数执行的规则 (中定义[静态构造函数](classes.md#static-constructors)) 提供该`B`的静态构造函数 (并因此`B`的静态字段初始值设定项)之前，必须运行`A`的静态构造函数和字段初始值设定项。

#### <a name="instance-field-initialization"></a>实例字段的初始化

一个类的实例字段变量初始值设定项对应于分配到任何一个实例构造函数在输入时立即执行的一系列 ([构造函数初始值设定项](classes.md#constructor-initializers)) 的类。 变量的初始值设定项类声明中显示的文本顺序执行。 类实例创建和初始化过程进行了进一步说明了[实例构造函数](classes.md#instance-constructors)。

变量的初始值设定项，对于实例字段不能引用要创建的实例。 因此，它是一个编译时错误，以引用`this`中变量的初始值设定项，因为它是变量的初始值设定项来通过引用任何实例成员的编译时错误*simple_name*。 在示例
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
有关变量的初始值设定项`y`会导致编译时错误，因为它引用了要创建的实例的成员。

## <a name="methods"></a>方法

***方法***是实现对象或类可执行的计算或操作的成员。 方法使用声明*method_declaration*s:

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

一个*method_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `static` ([静态和实例方法](classes.md#static-and-instance-methods))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，并`extern` ([外部方法](classes.md#external-methods)) 修饰符。

如果满足以下所有的声明就具有修饰符的有效组合：

*  声明包含一个有效的访问修饰符组合 ([访问修饰符](classes.md#access-modifiers))。
*  声明不包括相同的修饰符多次。
*  声明包含最多一个的下列修饰符： `static`， `virtual`，和`override`。
*  声明包含最多一个的下列修饰符：`new`和`override`。
*  如果声明包含`abstract`修饰符，然后声明不包含任何下列修饰符： `static`， `virtual`，`sealed`或`extern`。
*  如果声明包含`private`修饰符，然后声明不包含任何下列修饰符： `virtual`， `override`，或`abstract`。
*  如果声明包含`sealed`修饰符，然后声明还包括`override`修饰符。
*  如果声明包含`partial`修饰符，则它不包含任何下列修饰符： `new`， `public`， `protected`， `internal`， `private`， `virtual`， `sealed`， `override``abstract`，或`extern`。

一种具有方法`async`修饰符是异步函数，并遵循中所述的规则[异步函数](classes.md#async-functions)。

*Return_type*方法的声明指定的计算和方法返回的值的类型。 *Return_type*是`void`如果方法不返回值。 如果声明包含`partial`修饰符，则返回类型必须可`void`。

*Member_name*指定方法的名称。 除非该方法是显式接口成员实现 ([显式接口成员实现代码](interfaces.md#explicit-interface-member-implementations))，则*member_name*是只需*标识符*。 有关显式接口成员实现， *member_name*组成*interface_type*跟"`.`"和一个*标识符*。

可选*type_parameter_list*指定的类型参数的方法 ([类型参数](classes.md#type-parameters))。 如果*type_parameter_list*的方法指定***泛型方法***。 如果该方法具有`extern`修饰符， *type_parameter_list*不能指定。

可选*formal_parameter_list*指定的参数的方法 ([方法参数](classes.md#method-parameters))。

可选*type_parameter_constraints_clause*指定单个类型参数的约束 ([类型参数约束](classes.md#type-parameter-constraints))，并且如果可能仅指定*type_parameter_列表*还提供，并且该方法没有`override`修饰符。

*Return_type*和每种类型中引用*formal_parameter_list*的一种方法必须是至少与该方法本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints)).

*Method_body*是一个分号***语句体***或***表达式主体***。 语句体组成*块*，它指定要调用的方法时执行的语句。 表达式主体组成`=>`跟*表达式*和一个分号，表示单个表达式执行时调用的方法。 

有关`abstract`并`extern`方法， *method_body*只需包含一个分号。 有关`partial`方法*method_body*可能包含分号、 块正文或正文为表达式。 对于所有其他方法， *method_body*块正文或正文为表达式。

如果*method_body*包含一个分号，则可能不包括在声明`async`修饰符。

名称、 类型参数列表和形参列表的方法定义的签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的方法。 具体而言，一种方法的签名由其名称、 类型参数的数量、 修饰符和其形参类型的数组成。 出于这些目的，不是按其名称，但该方法的类型实参列表中其序号位置通过标识的方法的形参的类型中发生的任何类型参数。返回类型不是方法的签名的一部分，也不是类型形参或正式参数的名称。

方法的名称必须不同于其他所有非-中声明的方法相同的类的名称。 此外，一种方法的签名必须不同于同一类中声明的所有其他方法的签名并完全由不同的签名不能在同一个类中声明的两种方法`ref`和`out`。

该方法的*type_parameter*s 处于范围内整个*method_declaration*，并可用于对整个该作用域中的窗体类型*return_type*， *method_body*，并*type_parameter_constraints_clause*s 但不能在*属性*。

所有形参和类型参数必须都具有不同的名称。

### <a name="method-parameters"></a>方法参数

参数的方法，如果有，声明的方法的*formal_parameter_list*。

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

形参列表包含的一个或多个以逗号分隔的参数其中只有最后一个可能*parameter_array*。

一个*fixed_parameter*包含一组可选*特性*([属性](attributes.md))，一个可选`ref`，`out`或`this`修饰符，*类型*，则*标识符*和可选*default_argument*。 每个*fixed_parameter*声明了具有给定名称的给定类型的参数。 `this`修饰符将方法指定为扩展方法，并只允许一个静态方法的第一个参数。 中进一步介绍扩展方法[扩展方法](classes.md#extension-methods)。

一个*fixed_parameter*与*default_argument*称为***可选参数***，而*fixed_parameter*而无需*default_argument*是***所需的参数***。 所需的参数可能不会显示在中的可选参数后*formal_parameter_list*。

一个`ref`或`out`参数不能具有*default_argument*。 *表达式*中*default_argument*必须是以下值之一：

*  *constant_expression*
*  形式的表达式`new S()`其中`S`是值类型
*  形式的表达式`default(S)`其中`S`是值类型

*表达式*必须可由标识或可以为 null 转换为参数的类型隐式转换。

如果在实现分部方法声明中出现的可选参数 ([分部方法](classes.md#partial-methods))，显式接口成员实现 ([显式接口成员实现代码](interfaces.md#explicit-interface-member-implementations)) 中或在单参数索引器声明 ([索引器](classes.md#indexers)) 编译器应发出警告，因为这些成员可以永远不会调用允许省略的参数的方式。

一个*parameter_array*包含一组可选*特性*([特性](attributes.md))，`params`修饰符， *array_type*，和一个*标识符*。 参数数组声明了具有给定名称的给定的数组类型的单个参数。 *Array_type*的参数数组必须是一个一维数组类型 ([数组类型](arrays.md#array-types))。 在方法调用中，参数数组允许任一单个参数的给定的数组类型来指定，或要指定的数组元素类型的零个或多个参数，它允许。 参数数组中进行了描述进一步[参数数组](classes.md#parameter-arrays)。

一个*parameter_array*可能是后一个可选参数，但不能有默认值--参数省略*parameter_array*改为将导致创建一个空数组。

下面的示例说明了不同类型的参数：
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

中*formal_parameter_list*有关`M`，`i`是必需的 ref 参数，而`d`是必需的值参数，而`b`， `s`，`o`和`t`可选值的参数和`a`是参数数组。

方法声明为创建一个单独的声明空间参数、 类型参数和本地变量。 名称引入此声明空间通过类型参数列表和形参列表的方法以及局部变量声明中*块*的方法。 它是错误的方法声明空间的两个成员具有相同的名称。 它是错误的方法声明空间和嵌套的声明空间以包含具有相同名称的元素的本地变量声明空间。

方法调用 ([方法调用](expressions.md#method-invocations)) 创建特定于该调用的副本，形参和局部变量的方法和调用的参数列表的值或变量引用赋给新创建的正式参数。 内*块*的一种方法，可以通过在其标识符引用形参*simple_name*表达式 ([简单名称](expressions.md#simple-names))。

有四种类型的正式参数：

*  值参数，而无需任何修饰符声明。
*  引用参数，使用声明`ref`修饰符。
*  输出参数，使用声明`out`修饰符。
*  参数的数组，使用声明`params`修饰符。

如中所述[签名和超载](basic-concepts.md#signatures-and-overloading)，则`ref`并`out`修饰符是方法的签名的一部分，但`params`修饰符不是。

#### <a name="value-parameters"></a>值参数

用任何修饰符声明的参数是值参数。 值参数对应于从方法调用中提供的相应参数中获取其初始值的局部变量。

方法调用中的相应参数的值参数的形参时，必须是隐式转换的表达式 ([隐式转换](conversions.md#implicit-conversions)) 到形参类型。

一种方法被允许将新值分配给 value 参数。 此类分配只会影响由值参数表示的本地存储位置 — 它们不会影响在方法调用中给出的实参。

#### <a name="reference-parameters"></a>引用参数

与声明的参数`ref`修饰符是一个引用参数。 值参数与引用参数不会创建新的存储位置。 相反，引用参数表示相同的存储位置与作为自变量在方法调用中的变量。

在方法调用中的相应参数的形参时引用参数，必须包含关键字`ref`跟*variable_reference* ([精确规则，用于确定明确赋值](variables.md#precise-rules-for-determining-definite-assignment)) 的形式参数的类型相同。 它可以作为引用参数传递之前，必须明确赋值变量。

在方法中，引用参数始终被视为明确赋值。

一个方法声明为一个迭代器 ([迭代器](classes.md#iterators)) 不能具有引用参数。

该示例
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
```
i = 2, j = 1
```

调用`Swap`中`Main`，`x`表示`i`并`y`表示`j`。 因此，调用具有交换的值的效果`i`和`j`。

在采用的方法，可以为多个名称来表示相同的存储位置的引用参数。 在示例
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
在调用`F`中`G`将传递到引用`s`两个`a`和`b`。 因此，对于该调用，名称`s`， `a`，并`b`所有引用相同的存储位置，并且所有三个赋值修改实例字段`s`。

#### <a name="output-parameters"></a>输出参数

与声明的参数`out`修饰符是一个 output 参数。 与引用参数类似，输出参数不会创建新的存储位置。 而是输出参数表示相同的存储位置与作为自变量在方法调用中的变量。

输出参数的形参时，在方法调用中的相应参数必须包含关键字`out`跟*variable_reference* ([精确规则，用于确定明确赋值](variables.md#precise-rules-for-determining-definite-assignment)) 的形式参数的类型相同。 变量在之前它可以传递作为输出参数，但以下其中一个变量作为输出参数传递的调用，该变量明确赋值被视为不需要明确赋值。

在方法中，就像本地变量时，输出参数最初被认为是未分配和使用它的值之前必须明确赋值。

该方法返回之前，必须明确赋值方法的每个输出参数。

为分部方法声明的方法 ([分部方法](classes.md#partial-methods)) 或迭代器 ([迭代器](classes.md#iterators)) 不能具有输出参数。

生成多个返回值的方法中通常使用输出参数。 例如：
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

该示例生成输出：
```
c:\Windows\System\
hello.txt
```

请注意，`dir`并`name`传递到之前的变量可以是未分配`SplitPath`，它们被看作明确赋值调用之后。

#### <a name="parameter-arrays"></a>参数数组

与声明的参数`params`修饰符是参数数组。 如果形参列表中包含的参数数组，它必须在列表中的最后一个参数，并且它必须是一维数组类型。 例如，类型`string[]`并`string[][]`可以用作参数数组的类型，但类型`string[,]`不可以。 不能合并`params`修饰符和修饰符`ref`和`out`。

参数数组允许在方法调用中的两种方式之一中指定的参数：

*  给定的参数数组可以是一个隐式转换的表达式的参数 ([隐式转换](conversions.md#implicit-conversions)) 为参数数组类型。 在这种情况下，参数数组的作用与值参数完全一样。
*  或者，此调用可以指定为参数数组，其中每个参数都是隐式转换的表达式的零个或多个参数 ([隐式转换](conversions.md#implicit-conversions)) 为参数数组的元素类型。 在这种情况下，调用创建具有对应的参数的数目的长度参数数组类型的实例、 初始化具有给定的参数值的数组实例的元素并将新创建的数组实例用作实际自变量。

除了允许数目可变的参数调用中，参数数组是恰好等同于值参数 ([值参数](classes.md#value-parameters)) 的相同的类型。

该示例
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
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

第一个调用`F`只需将该数组传递`a`作为值参数。 第二次调用`F`会自动创建四个元素`int[]`与给定的元素的值并将该数组实例作为值参数传递。 同样，第三个调用的`F`创建一个零元素`int[]`并将该实例作为值参数传递。 第二个和第三个调用都完全等效于编写：
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

具有参数数组的方法执行重载决策时，可能在其正常形式或以其扩展形式适用 ([适用函数成员](expressions.md#applicable-function-member))。 仅当该方法的标准形式不适用并且仅当具有相同签名的扩展形式适用的方法未声明相同的类型中，展开的形式的一种方法才可用。

该示例
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
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

在示例中，两个可能的扩展方法使用参数数组形式的已包含在类中作为常规方法。 因此执行重载决策时不考虑这些扩展的形式和第一个和第三个方法调用将因此选择常规方法。 当类声明具有参数数组的方法时，它不少见还包含一些扩展形式作为常规方法。 通过这样做可以避免的数组分配将调用具有参数数组的方法的扩展形式出现的实例。

当参数数组的类型是`object[]`，该方法的标准形式和单个的扩展的形式之间产生潜在的多义性`object`参数。 不明确的原因在于`object[]`本身就是隐式转换为键入`object`。 二义性造成任何问题，但是，因为它可通过插入一个强制转换，如果需要解决。

该示例
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
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

中的第一个和最后一个调用`F`的范式`F`是适用的因为存在从实参类型到形参类型隐式转换 (两个均为类型`object[]`)。 因此，重载决策选择的标准形式`F`，并作为常规的值参数传递自变量。 在第二个和第三个调用的范式`F`因为隐式转换存在从实参类型到形参类型不适用 (类型`object`不能隐式转换为类型`object[]`)。 但是的展开的形式`F`是适用，因此重载决策选择。 因此，一个元素`object[]`创建的调用，并使用给定的参数值初始化数组的单个元素 (其本身是对的引用`object[]`)。

### <a name="static-and-instance-methods"></a>静态和实例方法

当一个方法声明包括`static`修饰符，称方法为静态方法。 如果未`static`修饰符存在，则称该方法为实例方法。

静态方法不对特定实例进行操作，它会导致编译时错误是指`this`中的静态方法。

实例方法作用于给定类的实例，并且该实例可作为访问`this`([此访问权限](expressions.md#this-access))。

当中引用的方法*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是静态方法，`E`必须表示一个类型，包含`M`，并且如果`M`是实例方法，`E`必须表示一个类型，其中包含的一个实例`M`。

静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。

### <a name="virtual-methods"></a>虚拟方法

如果实例方法声明包含`virtual`修饰符，称方法为虚方法。 如果未`virtual`修饰符存在，则称该方法为非虚拟方法。

非虚方法的实现是不变： 实现都是相同的方法调用中的类的实例上是否已声明的或派生类的实例。 与此相反，虚拟方法的实现可以由派生类所取代。 取代继承的虚方法的实现的过程被称为***重写***该方法 ([重写方法](classes.md#override-methods))。

在虚拟方法调用中，***运行时类型***为其调用方法的实例的位置确定要调用的实际方法实现。 在非虚拟方法调用中，***编译时类型***的实例是决定性因素。 准确地说，当名为一种方法中`N`使用的参数列表调用`A`编译时类型的实例上`C`和运行时类型`R`(其中`R`是`C`或派生的类从`C`)，其调用处理过程，如下所示：

*  首先，重载决策应用于`C`， `N`，并`A`以选择特定方法`M`从一组的方法中声明并由继承`C`。 这中所述[方法调用](expressions.md#method-invocations)。
*  然后，如果`M`是一个非虚拟方法，`M`调用。
*  否则为`M`是虚拟方法的派生程度最高的实现`M`与`R`调用。

对于每个虚拟方法中声明或继承的类，都存在***实现的派生程度最***与该类相关的方法。 虚拟方法的派生程度最高的实现`M`相对于一个类`R`，如下所示确定：

*  如果`R`包含引入`virtual`的声明`M`，则此操作的派生程度最高的实现`M`。
*  否则为如果`R`包含`override`的`M`，则此操作的派生程度最高的实现`M`。
*  否则，大多数派生的实现`M`相对于`R`的派生程度最高实现相同`M`方面的直接基类`R`。

下面的示例说明了虚拟和非虚拟方法之间的差异：
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

在示例中，`A`引入了非虚方法`F`和虚拟方法`G`。 该类`B`引入了新的非虚拟方法`F`，从而隐藏继承`F`，并且还重写继承的方法`G`。 该示例生成输出：
```
A.F
B.F
B.G
B.G
```

请注意，该语句`a.G()`调用`B.G`，而不`A.G`。 这是因为运行时类型的实例 (即`B`)，不是实例的编译时类型 (即`A`)，确定要调用的实际方法实现。

方法允许隐藏继承的方法，因为很可能包含多个具有相同签名的虚拟方法的类。 这不会造成多义性问题，因为除了派生程度最高的方法的所有隐藏。 在示例
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
`C`和`D`类包含两个具有相同的签名的虚拟方法： 一个引入`A`引入的一个`C`。 引入的方法`C`隐藏继承的方法`A`。 因此，重写中的声明`D`重写方法，通过引入`C`，并不适用于`D`若要重写方法由引入`A`。 该示例生成输出：
```
B.F
B.F
D.F
D.F
```

请注意，它是可以通过访问的实例调用隐藏虚方法`D`小于通过派生的类型中的方法不会隐藏。

### <a name="override-methods"></a>重写方法

如果实例方法声明包含`override`修饰符，该方法称为***重写方法***。 重写方法重写继承的虚方法具有相同的签名。 但如果虚方法声明中引入新方法，重写方法声明通过提供相应方法的新实现代码，专门针对现有的继承虚方法。

通过重写的方法`override`声明被称为***重写基方法***。 用于重写方法`M`类中声明`C`，重写基方法通过检查确定的每个基类类型`C`开头的直接基类类型`C`并继续每个连续直接基类类型，直到至少一个可访问的方法是在给定的基类类型中位于其具有相同的签名`M`后替换的类型参数。 为了查找重写基方法，方法考虑它是否可访问`public`，如果它是`protected`，则`protected internal`，或者它是否`internal`作为在同一程序中声明和`C`。

除非以下条件都适用于重写声明，否则，将发生编译时错误：

*  重写基方法可位于上文所述。
*  没有一个此类重写基方法。 基类类型是构造的类型，其中类型参数替换使两个方法的签名相同，此限制就会起作用。
*  重写基方法是虚拟的抽象，或重写方法。 换而言之，重写基方法不能为静态或非虚拟。
*  重写基方法不是密封的方法。
*  重写方法和重写基方法具有相同的返回类型。
*  重写声明和重写基方法具有相同的声明可访问性。 换而言之，重写声明不能更改虚拟方法的可访问性。 但是，如果重写基方法受保护的内部并不是声明为包含的重写方法，然后重写方法的程序集在不同的程序集中声明可访问性必须受到保护。
*  重写声明未指定类型参数约束子句。 而是约束均继承于重写基方法。 请注意，可能会通过继承约束中的类型参数替换中重写的方法的类型形参的约束。 这可能会导致不是合法时显式指定，如值类型或密封的类型的约束。

下面的示例演示如何重写规则适用于泛型类：
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

重写声明可以重写基方法使用访问*base_access* ([基访问](expressions.md#base-access))。 在示例
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
`base.PrintFields()`中的调用`B`调用`PrintFields`方法中声明`A`。 一个*base_access*禁用虚调用机制，并只需将视为非虚方法的基方法。 中有调用`B`已写入`((A)this).PrintFields()`，它会以递归方式调用`PrintFields`方法中声明`B`中, 声明一个不`A`，因为`PrintFields`是虚拟运行时类型和`((A)this)`是`B`。

仅通过包括`override`修饰符可以一种方法重写另一种方法。 在所有其他情况下，具有与继承的方法相同的签名的方法只是隐藏继承的方法。 在示例
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
`F`中的方法`B`不包括`override`修饰符，因此不会覆盖`F`中的方法`A`。 而是`F`中的方法`B`中的方法将隐藏`A`，并将报告警告，因为在声明中不包含`new`修饰符。

在示例
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
`F`中的方法`B`隐藏虚拟`F`方法继承自`A`。 因为新`F`中`B`具有私有访问权限，其作用域仅包含的类的主体`B`并不会扩展到`C`。 因此，声明`F`中`C`允许重写`F`继承自`A`。

### <a name="sealed-methods"></a>密封的方法

如果实例方法声明包含`sealed`修饰符、 说方法是可***密封方法***。 如果实例方法声明`sealed`修饰符，它还必须包括`override`修饰符。 使用`sealed`修饰符可防止派生的类进一步重写方法。

在示例
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
类`B`提供了两个重写方法：`F`方法具有`sealed`修饰符和`G`不的方法。 `B`使用密封`modifier`可防止`C`进一步重写`F`。

### <a name="abstract-methods"></a>抽象方法

如果实例方法声明包含`abstract`修饰符、 说方法是可***抽象方法***。 尽管一个抽象方法隐式也是一个虚拟方法，但它不能有修饰符`virtual`。

抽象方法声明引入了新的虚拟方法，但不提供该方法的实现。 相反，非抽象派生的类所需重写该方法提供其自己的实现。 一个抽象方法不提供任何实际的实现，因为*method_body*抽象方法的只包含一个分号。

抽象类中才允许抽象方法声明 ([抽象类](classes.md#abstract-classes))。

在示例
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
`Shape`类定义本身就可以绘制一个几何形状对象的抽象概念。 `Paint`方法是抽象的因为没有有意义的默认实现。 `Ellipse`并`Box`类是具体`Shape`实现。 由于这些类非抽象的他们需要重写`Paint`方法，并提供实际实现。

它是编译时错误*base_access* ([基访问](expressions.md#base-access)) 来引用一个抽象方法。 在示例
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
编译时错误报告为`base.F()`调用因为它引用了一个抽象方法。

抽象方法声明允许重写虚拟方法。 这允许用于强制重新实现派生类中方法的抽象类，并使该方法的原始实现不可用。 在示例
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
类`A`声明虚拟方法，类`B`重写此方法与抽象方法和类`C`重写抽象方法以提供其自己的实现。

### <a name="external-methods"></a>外部方法

当一个方法声明包括`extern`修饰符、 说方法是可***外部方法***。 外部方法是在外部实现，通常使用 C# 以外的语言。 外部方法声明不提供任何实际的实现，因为*method_body*外部方法的只包含一个分号。 外部方法可能不是泛型。

`extern`修饰符通常与结合使用`DllImport`属性 ([与 COM 和 Win32 组件互操作](attributes.md#interoperation-with-com-and-win32-components))，从而允许外部方法由 Dll （动态链接库） 来实现。 执行环境可以支持外部方法的实现可以来提供其他机制。

当外部方法中包括`DllImport`属性，还必须包括在方法声明`static`修饰符。 此示例演示如何使用`extern`修饰符和`DllImport`属性：
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

### <a name="partial-methods-recap"></a>分部方法 （回顾）

当一个方法声明包括`partial`修饰符、 说方法是可***分部方法***。 分部方法只能声明为分部类型的成员 ([分部类型](classes.md#partial-types))，并受到的限制数。 分部方法将进一步描述中[分部方法](classes.md#partial-methods)。

### <a name="extension-methods"></a>扩展方法

当方法的第一个参数包括`this`修饰符、 说方法是可***扩展方法***。 只能在非泛型的非嵌套静态类中声明的扩展方法。 扩展方法的第一个参数而不可以使用任何修饰符`this`，且参数类型不能为指针类型。

下面是一个静态类，声明两个扩展方法的示例：
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

扩展方法是常规的静态方法。 此外，其中它包含静态类是在作用域中，扩展方法可以使用来调用实例方法调用语法 ([扩展方法调用](expressions.md#extension-method-invocations))，作为第一个参数使用接收方的表达式。

以下程序使用上面已声明的扩展方法：
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

`Slice`方法是在可用`string[]`，并`ToInt32`方法位于`string`，因为它们具有已声明为扩展方法。 该程序的含义等同于以下，并使用普通静态方法调用：
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

*Method_body*方法的声明包含的块主体中，表达式主体或分号。

***结果类型***的一种方法是`void`如果返回类型为`void`，或如果的方法是异步和返回类型是`System.Threading.Tasks.Task`。 否则，非异步方法的结果类型是其返回类型和异步方法的结果类型，返回类型`System.Threading.Tasks.Task<T>`是`T`。

当某个方法具有`void`导致类型和块正文`return`语句 ([return 语句](statements.md#the-return-statement)) 在块中不允许指定的表达式。 如果块的 void 方法的执行正常完成 （也就是说，控制流末尾的方法主体），方法只需返回到其当前的调用方。
    
当某个方法具有`void`结果和表达式主体，表达式`E`必须是*statement_expression*，且正文是完全相当于窗体的块主体`{ E; }`。
    
当某个方法含有非空结果类型和一个块的正文，每个`return`块中的语句必须指定隐式转换为结果类型的表达式。 返回值的方法的块主体的终结点不能访问。 换而言之，在具有块主体的返回值的方法，不是允许控制流的方法正文末尾。
    
当某个方法具有非 void 结果类型，表达式主体，该表达式必须是隐式转换为结果类型，且正文是完全相当于窗体的块主体`{ return E; }`。
    
在示例
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
返回值的`F`方法导致在编译时错误，因为控制可以超出方法正文末尾。 `G`和`H`方法是否正确，因为所有可能的执行路径中指定一个返回值的 return 语句结束。 `I`方法是否正确，因为其主体等效于只是单个的返回语句中包含的语句块。

### <a name="method-overloading"></a>方法重载

方法重载决策规则所述[类型推理](expressions.md#type-inference)。

## <a name="properties"></a>属性

一个***属性***成员提供访问权限的对象或类的特征。 属性的示例包括字符串的长度，一种字体，窗口中，客户的名称的标题的大小等等。 属性是字段的自然扩展，都命名的成员与相关的类型，并且访问字段和属性的语法相同。 不过，与字段不同的是，属性不指明存储位置。 相反，属性包含***访问器***，用于指定在读取或写入属性值时要执行的语句。 因此，属性提供用于将操作与读取和写入对象的属性，则相关联的机制此外，不允许此类属性，将进行计算。

属性使用声明*property_declaration*s:

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

一个*property_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `static` ([静态和实例方法](classes.md#static-and-instance-methods))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，并`extern` ([外部方法](classes.md#external-methods)) 修饰符。

属性声明可能会有所与方法声明相同的规则 ([方法](classes.md#methods)) 关于修饰符的有效组合。

*类型*声明的属性指定该声明引入的属性的类型和*member_name*指定属性的名称。 该属性是显式接口成员的实现，除非*member_name*是只需*标识符*。 为了使显式接口成员实现 ([显式接口成员实现代码](interfaces.md#explicit-interface-member-implementations))，则*member_name*组成*interface_type*跟"`.`"和一个*标识符*。

*类型*的属性必须至少与该属性本身具有同样的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。

一个*property_body*可能是组成***访问器正文***或***表达式主体***。 在访问器正文中， *accessor_declarations*，必须在用"`{`"和"`}`"令牌，声明访问器 ([访问器](classes.md#accessors)) 的属性。 访问器指定与读取和写入属性相关联的可执行语句。

表达式主体组成`=>`跟*表达式*`E`分号，完全等效于在语句体`{ get { return E; } }`，并因此仅可指定仅定义了 getter属性的 getter 的结果由单个表达式。

一个*property_initializer*可能仅被授予以自动实现属性 ([自动实现的属性](classes.md#automatically-implemented-properties))，并导致此类的基础字段的初始化具有给定值的属性*表达式*。

即使访问属性的语法是相同的字段，一个属性是未归类为变量。 因此，不能将属性作为`ref`或`out`参数。

如果属性声明包含`extern`修饰符，该属性称为***外部属性***。 因为外部属性声明不提供任何实际的实现，每个其*accessor_declarations*包含一个分号。

### <a name="static-and-instance-properties"></a>静态和实例属性

如果属性声明包含`static`修饰符，该属性称为***静态属性***。 如果未`static`修饰符存在，该属性称为***实例属性***。

静态属性不与特定实例相关联，它是指导致编译时错误`this`的静态属性访问器中。

实例属性与给定类的实例相关联，并且该实例可作为访问`this`([此访问权限](expressions.md#this-access)) 中的该属性访问器。

中引用属性时*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是一个静态属性，`E`必须表示一个类型，包含`M`，并且如果`M`是实例属性，则 E 必须表示一个类型，其中包含的实例`M`。

静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。

### <a name="accessors"></a>访问器

*Accessor_declarations*属性指定与读取和写入该属性相关联的可执行语句。

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

访问器声明组成*get_accessor_declaration*即*set_accessor_declaration*，和 / 或。 每个访问器声明包含令牌`get`或`set`跟一个可选*accessor_modifier*和一个*accessor_body*。

利用*accessor_modifier*s 受到以下限制：

*  *Accessor_modifier*不能在接口或显式接口成员实现中使用。
*  属性或索引器没有`override`修饰符， *accessor_modifier*仅当属性或索引器同时具有允许`get`和`set`访问器，然后允许仅在其中一个访问器。
*  属性或索引器，包括`override`修饰符，访问器必须匹配*accessor_modifier*(如果有） 的访问器被重写。
*  *Accessor_modifier*必须声明一个严格限制性比属性或索引器本身的声明可访问性的可访问性。 若要确切地说：
   * 如果属性或索引器已声明可访问性`public`，则*accessor_modifier*可能是`protected internal`， `internal`， `protected`，或`private`。
   * 如果属性或索引器已声明可访问性`protected internal`，则*accessor_modifier*可能是`internal`， `protected`，或`private`。
   * 如果属性或索引器已声明可访问性`internal`或`protected`，则*accessor_modifier*必须是`private`。
   * 如果属性或索引器已声明可访问性`private`，无*accessor_modifier*可能会使用。

有关`abstract`并`extern`属性， *accessor_body*指定每个访问器是只需一个分号。 一个非抽象、 非外部属性可以具有每个*accessor_body*是一个分号，在这种情况下很***自动实现的属性***([自动实现的属性](classes.md#automatically-implemented-properties)). 自动实现的属性必须具有至少一个 get 访问器。 任何其他非抽象、 非外部属性，访问器的*accessor_body*是*块*它指定要调用的相应的访问器时要执行的语句。

一个`get`访问器对应于具有返回值的属性类型的无参数方法。 当在表达式中，引用属性除了作为赋值目标`get`属性访问器调用以计算属性的值 ([表达式的值](expressions.md#values-of-expressions))。 正文`get`访问器必须符合有关返回值的规则中所述方法[方法体](classes.md#method-body)。 具体而言，所有`return`语句的正文中`get`访问器必须指定隐式转换为属性类型的表达式。 此外，终结点的`get`访问器不能访问。

一个`set`访问器对应于具有单个值的属性类型参数的方法和一个`void`返回类型。 隐式参数`set`访问器始终命名为`value`。 将属性作为赋值的目标的引用时 ([赋值运算符](expressions.md#assignment-operators))，或为操作数`++`或`--`([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)， [前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))，则`set`用的参数调用访问器 (其值为赋值的右侧或的操作数`++`或`--`运算符)，提供新值 ([简单的赋值](expressions.md#simple-assignment))。 正文`set`访问器必须遵守的规则`void`中所述方法[方法主体](classes.md#method-body)。 具体而言，`return`中的语句`set`访问器正文不允许指定的表达式。 由于`set`访问器隐式具有一个名为参数`value`，它是本地变量或常量声明中的编译时错误`set`访问器具有该名称。

基于是否存在或缺少`get`和`set`访问器属性分类，如下所示：

*  一个属性，包括这两个`get`访问器和一个`set`访问器称为***读写***属性。
*  仅具有一个属性`get`访问器称为***只读***属性。 它是只读的属性赋值的目标的编译时错误。
*  仅具有一个属性`set`访问器称为***只写***属性。 除了作为赋值的目标，它是以引用在表达式中的只写属性的编译时错误。

在示例
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
`Button`控制声明一个公共`Caption`属性。 `get`访问器的`Caption`属性将返回字符串存储在私有`caption`字段。 `set`访问器会检查是否不同于当前值，新值，如果是这样，它存储的新值并重新绘制控件。 属性通常遵循上面所示的模式：`get`访问器仅返回存储的私有字段中的值和`set`访问器修改该私有字段，然后执行完全更新的状态所需的任何其他操作对象。

给定`Button`上面的类，以下是使用的示例`Caption`属性：
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

在这里，`set`通过将值分配给属性，调用访问器和`get`通过引用在表达式中的属性来调用访问器。

`get`和`set`属性访问器不是不同的成员，并且不能单独声明属性访问器。 在这种情况下，不能使读写属性的两个访问器具有不同的可访问性。 该示例
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
未声明的单一的读-写属性。 相反，它声明了两个具有相同的名称，其中一个只读属性，另一个只写。 由于在同一类中声明的两个成员不能具有相同的名称，则示例将导致编译时错误发生。

当在派生的类继承的属性与相同的名称声明一个属性时, 派生的属性将会隐藏继承的属性相对于读取和写入。 在示例
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
`P`中的属性`B`隐藏`P`中的属性`A`相对于读取和写入。 因此，在语句中
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
分配给`b.P`会导致编译时错误报告，因为只读`P`属性中的`B`隐藏只写`P`中的属性`A`。 但请注意，强制转换可用于访问隐藏`P`属性。

与公共字段、 属性提供对象的内部状态和它的公共接口之间的分隔。 为例：
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

在这里，`Label`类使用两个`int`字段中，`x`和`y`，以存储其位置。 公开的位置是公开既用作`X`和一个`Y`属性和 as`Location`类型的属性`Point`。 如果在未来版本的`Label`，将变得更方便地存储所在的位置`Point`可以在内部，而不会影响类的公共接口进行更改：
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

有`x`并`y`改为已`public readonly`字段中，重要的是不可能进行这样的更改到`Label`类。

通过属性公开状态不一定比直接公开字段效率低。 具体而言，当属性为非虚拟的且包含只有少量的代码，执行环境可能对访问器的调用将替换为实际代码的访问器。 此过程被称为***内联***，和它，使属性访问不如字段访问，同时还保留属性的更高的灵活性。

由于调用`get`访问器，从概念上讲相当于读取字段的值，它被认为是好的编程风格的`get`访问器具有明显的副作用。 在示例
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
值`Next`属性依赖于以前访问的属性的次数。 因此，访问该属性会产生明显负面影响，并且此属性应作为一种方法实现。

为"无其他作用"约定`get`访问器并不意味着`get`访问器始终应编写为只返回字段中存储的值。 实际上，`get`访问器通常通过访问多个字段或调用方法计算属性的值。 但是，在正确设计`get`访问器执行的任何操作都导致中对象的状态的可观察的更改。

属性可用于资源直到首次引用的时的初始化延迟。 例如：
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

`Console`类包含三个属性`In`， `Out`，和`Error`，分别表示标准输入、 输出和错误设备。 通过公开为属性，这些成员`Console`类可以延迟其初始化，直到它们被实际使用。 例如，在第一次引用时`Out`属性，如下所示
```csharp
Console.Out.WriteLine("hello, world");
```
基础`TextWriter`创建输出设备。 但如果应用程序不引用`In`和`Error`为这些设备创建属性，则任何对象。

### <a name="automatically-implemented-properties"></a>自动实现的属性

自动实现的属性 (或***自动属性***简称)，是具有仅限分号的访问器正文的非抽象非外部属性。 自动属性必须具有 get 访问器，并可根据需要 set 访问器。

属性指定为自动实现的属性，对于属性，自动提供一个隐藏的支持字段，并且访问器的实现是为了读取和写入到该支持字段。 如果自动属性没有 set 访问器，支持字段被视为`readonly`([只读字段](classes.md#readonly-fields))。 就像`readonly`字段中，仅定义了 getter 的自动属性还中可以分配到封闭类的构造函数的正文。 此类赋值将直接分配到只读支持字段的属性。

自动属性还可以具有*property_initializer*，这将直接应用到为支持字段*variable_initializer* ([变量初始值设定项](classes.md#variable-initializers)).

下面的示例：
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

下面的示例：
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

请注意，分配到只读字段合法的因为它们构造函数内发生。


### <a name="accessibility"></a>可访问性

如果访问器具有*accessor_modifier*，可访问域 ([可访问性域](basic-concepts.md#accessibility-domains)) 的访问器确定使用的声明可访问性*accessor_modifier*. 如果访问器没有*accessor_modifier*，访问器的可访问性域由属性或索引器的声明可访问性。

是否存在*accessor_modifier*永远不会影响成员查找 ([运算符](expressions.md#operators)) 或重载决策 ([重载决策](expressions.md#overload-resolution))。 属性或索引器上的修饰符始终确定哪些属性或索引器绑定到，而不考虑访问的上下文。

一旦选择了特定属性或索引器，所涉及的特定访问器的可访问性域用于确定使用情况是否有效：

*  如果用法是作为一个值 ([表达式的值](expressions.md#values-of-expressions))，则`get`访问器必须存在并且可访问。
*  如果用法是作为简单赋值的目标 ([简单的赋值](expressions.md#simple-assignment))，则`set`访问器必须存在并且可访问。
*  使用情况是否为目标的复合赋值 ([复合赋值](expressions.md#compound-assignment))，或为目标的`++`或`--`运算符 ([函数成员](expressions.md#function-members).9， [调用表达式](expressions.md#invocation-expressions))，这两个`get`访问器和`set`访问器必须存在并且可访问。

在下面的示例中，该属性`A.Text`由属性隐藏`B.Text`，在仅在上下文中甚至`set`调用访问器。 与之相反，该属性`B.Count`不能访问类`M`，因此可访问属性`A.Count`改为使用。

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

用于实现接口的取值函数可能没有*accessor_modifier*。 如果只有一个访问器用于实现一个接口，可以使用声明其他访问器*accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>虚拟，密封的重写方法和抽象属性访问器

一个`virtual`属性声明指定的属性访问器是虚拟。 `virtual`修饰符将应用于这两个访问器的读写属性，不可能只有一个访问器的读写属性必须是虚拟的。

`abstract`属性声明指定的属性访问器是虚拟的但不提供访问器的实际实现。 相反，非抽象派生的类所需通过重写该属性提供自己的访问器的实现。 因为抽象属性声明的访问器不提供任何实际的实现，其*accessor_body*只包含一个分号。

属性声明，包括这两个`abstract`和`override`修饰符指定该属性是抽象的重写基属性。 此类属性访问器也是抽象的。

抽象类中才允许抽象属性声明 ([抽象类](classes.md#abstract-classes))。继承的虚属性访问器可以重写派生类中通过包括指定的属性声明`override`指令。 这称为***重写属性声明***。 重写属性声明不声明一个新的属性。 相反，它只是专用化现有虚拟属性的访问器的实现。

重写属性声明必须指定与继承的属性完全相同的可访问性修饰符、 类型和名称。 如果继承的属性具有单个访问器 （例如，如果该属性是只读或只写），则重写属性必须只包含该访问器。 如果继承的属性包含这两个访问器 （即，如果该属性是读写），则重写属性可以包含单个访问器或这两个访问器。

重写属性声明可能包括`sealed`修饰符。 使用此修饰符可以防止进一步重写该属性派生的类。 密封属性的访问器也被密封。

声明和调用之间的差异除外语法、 虚拟、 密封的重写和抽象访问器的行为与虚拟、 密封的重写和抽象方法完全相同。 具体而言，这些规则中所述[虚拟方法](classes.md#virtual-methods)，[重写方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，并[抽象方法](classes.md#abstract-methods)应用像访问器是相应窗体的方法：

*  一个`get`访问器对应于具有返回值的属性类型和包含的属性与相同的修饰符的无参数方法。
*  一个`set`访问器对应于具有单个值的属性类型参数的方法`void`返回类型，以及与包含属性相同的修饰符。

在示例
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
`X` 是虚拟的只读属性，`Y`是虚拟的读写属性，并`Z`是抽象的读写属性。 因为`Z`是抽象的包含类`A`也必须声明为抽象。

从派生的类`A`下面显示了：
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

此处的声明`X`， `Y`，和`Z`将重写属性声明。 每个属性声明完全匹配可访问性修饰符、 类型和相应的继承属性的名称。 `get`访问器的`X`并`set`访问器`Y`使用`base`关键字访问继承的访问器。 声明`Z`重写这两个抽象访问器，因此，没有在抽象的函数成员`B`，和`B`允许为非抽象类。

当某个属性声明为`override`，任何重写的访问器必须可供重写代码。 此外，声明可访问性的属性或索引器本身，和的访问器，必须与匹配的重写的成员和访问器。 例如：
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

***事件***是使对象或类能够提供通知的成员。 客户端可以通过提供附加事件的可执行代码***事件处理程序***。

使用声明事件*event_declaration*s:

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

*Event_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `static` ([静态和实例方法](classes.md#static-and-instance-methods))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，并`extern` ([外部方法](classes.md#external-methods)) 修饰符。

事件声明可能会有所与方法声明相同的规则 ([方法](classes.md#methods)) 关于修饰符的有效组合。

*类型*事件的声明必须*delegate_type* ([引用类型](types.md#reference-types))，并且*delegate_type*必须至少为事件本身的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。

事件声明可能包括*event_accessor_declarations*。 但是，如果不是，请为非 extern 非抽象事件，编译器会自动提供它们 ([类似字段的事件](classes.md#field-like-events)); 对于外部事件，从外部提供访问器。

事件声明省略*event_accessor_declarations*定义了一个或多个事件，每个*variable_declarator*s。 特性和修饰符应用于所有通过此类声明的成员*event_declaration*。

它是编译时错误*event_declaration*包含这两`abstract`修饰符，大括号分隔*event_accessor_declarations*。

当事件声明包括`extern`修饰符，该事件则称***外部事件***。 因为外部事件声明不提供任何实际的实现，则返回错误，以便同时包含`extern`修饰符并*event_accessor_declarations*。

它是编译时错误*variable_declarator*的使用的事件声明`abstract`或`external`修饰符以包括*variable_initializer*。

事件可以用作左操作数的`+=`并`-=`运算符 ([事件分配](expressions.md#event-assignment))。 这些运算符的使用分别，附加到事件处理程序或删除事件处理程序从事件，以及事件的访问修饰符控制在其中执行此类操作的上下文。

由于`+=`和`-=`是之外声明的事件、 外部代码的类型的事件允许的唯一操作可以添加和移除处理程序的事件，但不能在另一种方法获取或修改事件的基础列表处理程序。

在窗体的操作中`x += y`或`x -= y`，当`x`是一个事件，该引用包含的声明的类型的外部发生`x`，该操作的结果具有类型`void`（而不是类型`x`，值为`x`赋值后)。 此规则禁止从间接检查事件的基础委托的外部代码。

下面的示例演示如何将事件处理程序附加到的实例`Button`类：
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

在这里，`LoginDialog`实例构造函数创建两个`Button`实例，并将附加到事件处理程序`Click`事件。

### <a name="field-like-events"></a>类似字段的事件

中的类或结构包含一个事件声明的程序文本，可以像字段一样使用某些事件。 若要使用这种方式，事件不能`abstract`或`extern`，并且必须显式包括*event_accessor_declarations*。 可以在任何允许使用字段的上下文中使用此类事件。 该字段将包含一个委托 ([委托](delegates.md)) 这是指已添加到该事件的事件处理程序的列表。 如果尚未添加任何事件处理程序，该字段包含`null`。

在示例
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
`Click` 用作字段内`Button`类。 如示例所示，该字段可以检查、 修改和使用委托调用表达式中。 `OnClick`中的方法`Button`类"引发"`Click`事件。 引发事件的概念恰恰等同于调用由事件表示的委托，因此，没有用于引发事件的特殊语言构造。 请注意检查，以确保委托为非 null 的前面有委托调用。

声明的外部`Button`类，`Click`成员只能上的左侧使用`+=`和`-=`运算符，如下所示
```csharp
b.Click += new EventHandler(...);
```
这将委托附加到的调用列表`Click`事件，并
```csharp
b.Click -= new EventHandler(...);
```
它从调用列表中删除委托`Click`事件。

当编译类似字段的事件时，编译器将自动创建一个存储区来存放委托，并创建事件访问器的添加或删除事件处理程序委托字段。 添加和删除操作都是线程安全的并可能 （但都不需要） 将完成的时持有锁 ([lock 语句](statements.md#the-lock-statement)) 对实例事件时，包含对象或类型对象 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)) 的静态事件。

因此，下列窗体的实例事件声明：
```csharp
class X
{
    public event D Ev;
}
```
将编译为如下语句：
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
在类中`X`，对引用`Ev`的左侧`+=`和`-=`运算符导致 add 和 remove 访问器要调用。 对所有其他引用`Ev`进行编译以引用该隐藏的字段`__Ev`改为 ([成员访问](expressions.md#member-access))。 名称"`__Ev`"是任意; 隐藏的字段可以在所有具有任何名称或无名称。

### <a name="event-accessors"></a>事件访问器

事件声明通常省略*event_accessor_declarations*，如下所示：`Button`上面示例中。 执行此操作的一种情况时，需要在其中每个事件的一个字段的存储成本是不可接受的情况。 在这种情况下，一个类可以包含*event_accessor_declarations*和使用私有机制来存储事件处理程序的列表。

*Event_accessor_declarations*事件指定与添加和移除事件处理程序相关联的可执行语句。

访问器声明组成*add_accessor_declaration*和一个*remove_accessor_declaration*。 每个访问器声明包含令牌`add`或`remove`跟*块*。 *块*与关联*add_accessor_declaration*指定时添加事件处理程序，要执行的语句和*块*与相关联*remove_accessor_declaration*指定要删除的事件处理程序时执行的语句。

每个*add_accessor_declaration*并*remove_accessor_declaration*对应于具有事件类型的单值参数的方法和一个`void`返回类型。 事件访问器的隐式参数名为`value`。 当事件分配中使用事件时，使用相应的事件访问器。 具体而言，如果赋值运算符是`+=`使用的 add 访问器，然后赋值运算符是`-=`则使用 remove 访问器。 在任一情况下，赋值运算符的右侧操作数用作事件访问器的参数。 块*add_accessor_declaration*或*remove_accessor_declaration*必须符合有关规则`void`中所述方法[方法体](classes.md#method-body)。 具体而言，`return`不允许此类块中的语句指定的表达式。

由于事件访问器隐式具有一个名为参数`value`，它是一个编译时错误的事件访问器具有该名称中声明本地变量或常量。

在示例
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
`Control`类实现事件的内部存储机制。 `AddEventHandler`方法将委托值与键相关联`GetEventHandler`方法返回当前与键关联的委托和`RemoveEventHandler`方法将委托移除与指定的事件的事件处理程序。 据估计，却设计的基础存储机制为： 没有为关联的成本`null`委托值的密钥，并因此在未处理的事件使用任何存储。

### <a name="static-and-instance-events"></a>静态和实例事件

当事件声明包括`static`修饰符，该事件则称***静态事件***。 如果未`static`修饰符存在，该事件则称***实例事件***。

静态事件不与特定实例相关联，它是指导致编译时错误`this`的静态事件访问器中。

实例事件与给定类的实例相关联，并且可作为访问此实例`this`([此访问权限](expressions.md#this-access)) 中的该事件访问器。

事件中的引用时*member_access* ([成员访问](expressions.md#member-access)) 的窗体`E.M`，如果`M`是静态事件，`E`必须表示一个类型，包含`M`，并且如果`M`是实例事件，则 E 必须表示一个类型，其中包含的实例`M`。

静态之间的差异和实例成员进行讨论中进一步[静态和实例成员](classes.md#static-and-instance-members)。

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>虚拟，密封的重写方法和抽象事件访问器

一个`virtual`事件声明指定该事件的访问器是虚拟。 `virtual`修饰符将应用于事件的两个访问器。

`abstract`事件声明指定事件的访问器是虚拟的但不提供访问器的实际实现。 相反，非抽象派生的类所需的重写事件提供自己的访问器的实现。 由于抽象事件声明不提供任何实际的实现，因此它不能提供大括号分隔*event_accessor_declarations*。

事件声明同时包含`abstract`和`override`修饰符指定该事件是抽象的重写基事件。 此类事件的访问器也是抽象的。

抽象类中才允许抽象事件声明 ([抽象类](classes.md#abstract-classes))。

可以通过包含指定的事件声明派生类中替代继承虚拟事件访问器`override`修饰符。 这称为***重写事件声明***。 重写事件声明不声明新的事件。 相反，它只是专用化的现有虚拟事件访问器的实现。

重写事件声明必须指定为重写事件完全相同的可访问性修饰符、 类型和名称。

重写事件声明可能包括`sealed`修饰符。 使用此修饰符可防止进一步重写事件派生的类。 密封的事件的访问器也被密封。

它是重写事件声明中包含的编译时错误`new`修饰符。

声明和调用之间的差异除外语法、 虚拟、 密封的重写和抽象访问器的行为与虚拟、 密封的重写和抽象方法完全相同。 具体而言，这些规则中所述[虚拟方法](classes.md#virtual-methods)，[重写方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，并[抽象方法](classes.md#abstract-methods)应用像访问器是相应窗体的方法。 每个访问器对应于具有事件类型的单值参数的方法`void`返回类型，以及与包含事件相同的修饰符。

## <a name="indexers"></a>索引器

***索引器***是使对象可以为数组相同的方式进行索引的成员。 使用声明索引器*indexer_declaration*s:

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

*Indexer_declaration*可能包括一套*特性*([特性](attributes.md)) 和四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，则`new`([的新修饰符](classes.md#the-new-modifier))， `virtual` ([虚拟方法](classes.md#virtual-methods))， `override` ([重写方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern`([外部方法](classes.md#external-methods)) 修饰符。

索引器声明可能会有所与方法声明相同的规则 ([方法](classes.md#methods)) 方面的修饰符的有效组合，有一个例外是，静态修饰符不允许在索引器声明。

修饰符`virtual`， `override`，和`abstract`是一种情况下除外互相排斥。 `abstract`和`override`修饰符可能以便抽象索引器可以重写虚拟一起使用。

*类型*声明索引器的指定索引器声明引入的元素类型。 索引器是显式接口成员的实现，除非*类型*跟关键字`this`。 有关显式接口成员实现，*类型*后跟*interface_type*、"`.`"，和关键字`this`。 与其他成员不同索引器不具有用户定义的名称。

*Formal_parameter_list*指定的索引器的参数。 索引器的形参列表对应于一种方法 ([方法参数](classes.md#method-parameters))，但必须指定至少一个参数，并且`ref`和`out`不允许参数修饰符.

*类型*的索引器和每种类型中引用*formal_parameter_list*必须至少与索引器本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints)).

*Indexer_body*可能是组成***访问器正文***或***表达式主体***。 在访问器正文中， *accessor_declarations*，必须在用"`{`"和"`}`"令牌，声明访问器 ([访问器](classes.md#accessors)) 的属性。 访问器指定与读取和写入属性相关联的可执行语句。

包含的表达式主体"`=>`"表达式后跟`E`分号，完全等效于在语句体`{ get { return E; } }`，并因此仅可指定仅定义了 getter 的索引器的 getter 的结果由单个表达式给定。

即使访问索引器元素的语法是相同的数组元素，索引器元素是未归类为变量。 因此，不能作为索引器元素传递`ref`或`out`参数。

索引器的形参列表定义签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的索引器。 具体而言，索引器的签名组成的数量和其形参的类型。 元素类型和正式参数的名称不是签名的索引器的一部分。

索引器的签名必须不同于在同一类中声明的所有其他索引器的签名。

索引器和属性的概念，非常类似，但在以下方面有所不同：

*  属性是由其名称标识，而索引器由其签名。
*  通过访问属性*simple_name* ([简单名称](expressions.md#simple-names)) 或*member_access* ([成员访问](expressions.md#member-access))，而索引器通过访问元素*element_access* ([索引器访问](expressions.md#indexer-access))。
*  属性可以是`static`成员，而索引器始终是一个实例成员。
*  一个`get`的属性访问器对应于不带任何参数的方法而`get`索引器访问器对应于具有索引器相同的形参列表的方法。
*  一个`set`的属性访问器对应于一种方法具有一个名为的单个参数`value`，而`set`索引器访问器对应于具有索引器，以及一个附加参数相同的形参列表的方法名为`value`。
*  它是索引器访问器声明索引器参数与同名的局部变量的编译时错误。
*  在重写属性声明中，使用语法访问继承的属性`base.P`，其中`P`是属性名称。 在重写索引器声明中，使用语法访问继承的索引器`base[E]`，其中`E`是表达式的以逗号分隔列表。
*  不存在"自动实现索引器"的概念。 它是错误的非抽象、 非外部索引器中的，使用分号访问器。

除了这些差异中定义的所有规则[访问器](classes.md#accessors)并[自动实现的属性](classes.md#automatically-implemented-properties)应用于索引器访问器以及属性访问器。

当索引器声明包括`extern`修饰符，索引器称为***外部索引器***。 因为外部索引器声明不提供任何实际的实现，每个其*accessor_declarations*包含一个分号。

下面的示例声明`BitArray`类，该类实现索引器，以便访问中的位数组的单个位。
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

实例`BitArray`类会占用内存远少于相应`bool[]`（因为前者的每个值所占的仅有一位而不是后者的一个字节），但与相同的操作，它允许`bool[]`。

以下`CountPrimes`类使用`BitArray`和传统的"埃拉托色"算法，来计算的 1 到给定的最大值之间的质数：
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

请注意，用于访问的元素的语法`BitArray`准确地说是相同`bool[]`。

下面的示例演示具有两个参数使用一个索引器的 26 * 10 网格类。 第一个参数必须是大写或小写字母 A-Z 范围内，第二个需是范围 0-9 内的整数。

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

索引器重载决策规则所述[类型推理](expressions.md#type-inference)。

## <a name="operators"></a>运算符

***运算符***是定义可应用于类的实例的表达式运算符的含义的成员。 使用声明运算符*operator_declaration*s:

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
    | 'right_shift' | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
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

有三个类别的可重载运算符： 一元运算符 ([一元运算符](classes.md#unary-operators))，二元运算符 ([二元运算符](classes.md#binary-operators))，和转换运算符 ([转换运算符](classes.md#conversion-operators)).

*Operator_body*是一个分号***语句体***或***表达式主体***。 语句体组成*块*，它指定要执行时调用运算符的语句。 *块*必须符合有关返回值的规则中所述方法[方法主体](classes.md#method-body)。 表达式主体组成`=>`表达式和一个分号后, 跟，表示要执行时调用运算符的单个表达式。

有关`extern`运算符*operator_body*只需包含一个分号。 对于所有其他运算符， *operator_body*块正文或正文为表达式。

以下规则适用于所有运算符声明：

*  在运算符声明必须同时包含`public`和一个`static`修饰符。
*  运算符的参数必须为值参数 ([值参数](variables.md#value-parameters))。 它是在运算符声明中指定的编译时错误`ref`或`out`参数。
*  运算符的签名 ([一元运算符](classes.md#unary-operators)，[二元运算符](classes.md#binary-operators)，[转换运算符](classes.md#conversion-operators)) 必须不同于声明中的所有其他运算符的签名同一个类。
*  在运算符声明中引用的所有类型都必须至少与运算符本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。
*  它是同一修饰符在运算符声明中多次出现的错误的。

每个运算符类别都有其他限制，如以下各节中所述。

与其他成员一样在基类中声明的运算符由派生类继承。 运算符声明始终需要的类或结构声明运算符参与运算符的签名，因为它不能在派生类中声明的运算符，若要隐藏基类中声明的运算符。 因此，`new`修饰符永远不会是必需的并因此永远不会允许，在运算符声明中。

一元和二元运算符的其他信息可在[运算符](expressions.md#operators)。

转换运算符的其他信息可在[用户定义的转换](conversions.md#user-defined-conversions)。

### <a name="unary-operators"></a>一元运算符

以下规则适用于一元运算符声明，其中`T`表示类或结构都包含在运算符声明的实例类型：

*  一元`+`， `-`， `!`，或`~`运算符必须采用一个参数的类型`T`或`T?`和可以返回任何类型。
*  一元`++`或`--`运算符必须将单个类型的参数`T`或`T?`并且必须返回相同类型派生自它。
*  一元`true`或`false`运算符必须将单个类型的参数`T`或`T?`，并且必须返回类型`bool`。

一元运算符的签名包含的运算符标记 (`+`， `-`， `!`， `~`， `++`， `--`， `true`，或`false`) 和单个规范参数的类型。 返回类型不是一个一元运算符的签名的一部分，也不是正式参数的名称。

`true`和`false`一元运算符要求成对的声明。 如果一个类中声明以下运算符之一而没有声明其他，发生编译时错误。 `true`并`false`运算符是中作了进一步介绍[用户定义的条件逻辑运算符](expressions.md#user-defined-conditional-logical-operators)并[布尔表达式](expressions.md#boolean-expressions)。

下面的示例演示一个实现和后续使用`operator ++`对整数向量类：
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

请注意如何运算符方法将返回通过将 1 添加到的操作数，就像后缀递增而产生的值和递减运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators))，和前缀递增和递减运算符 ([前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators))。 不同于在 c + +，此方法需要不修改其操作数的值直接。 事实上，修改操作数的值违反了后缀递增运算符的标准语义。

### <a name="binary-operators"></a>二元运算符

以下规则适用于二元运算符的声明，其中`T`表示类或结构都包含在运算符声明的实例类型：

*  二进制非移位运算符必须采用两个参数，在至少一个必须具有类型`T`或`T?`，并且可以返回任何类型。
*  二进制`<<`或`>>`运算符必须将两个参数，其中第一个类型必须`T`或`T?`，其中第二个必须具有类型`int`或`int?`，并且可以返回任何类型。

二元运算符的签名包含的运算符标记 (`+`， `-`， `*`， `/`， `%`， `&`， `|`， `^`， `<<`， `>>`，`==`， `!=`， `>`， `<`， `>=`，或`<=`) 和两个形参的类型。 返回类型和形参的名称不是签名的二进制运算符的一部分。

某些二元运算符要求成对的声明。 对于一对任一运算符的每个声明，必须是对的匹配中的其他运算符的声明。 两个运算符声明匹配时它们具有相同的返回类型和每个参数的同一类型。 以下运算符要求成对的声明：

*  `operator ==` 和 `operator !=`
*  `operator >` 和 `operator <`
*  `operator >=` 和 `operator <=`

### <a name="conversion-operators"></a>转换运算符

转换运算符声明引入***用户定义的转换***([用户定义的转换](conversions.md#user-defined-conversions)) 的增强的预定义隐式和显式转换。

包括的转换运算符声明`implicit`关键字引入的用户定义的隐式转换。 隐式转换可以在各种情况下，包括函数成员调用、 强制转换表达式和赋值发生。 这是中作了进一步介绍[隐式转换](conversions.md#implicit-conversions)。

包括的转换运算符声明`explicit`关键字引入的用户定义的显式转换。 显式转换进一步说明了，并且在强制转换表达式可能会发生[显式转换](conversions.md#explicit-conversions)。

转换运算符将源类型，即转换运算符，为目标类型，指示转换运算符的返回类型的参数类型转换。

给定的源类型`S`和目标类型`T`，如果`S`或`T`是可以为 null 的类型，让`S0`并`T0`指其基础类型，否则`S0`和`T0`是等于`S`和`T`分别。 类或结构允许以声明的源类型转换`S`为目标类型`T`只有在满足以下所有时：

*  `S0` 和`T0`是不同的类型。
*  要么`S0`或`T0`是在运算符声明发生的类或结构类型。
*  既不`S0`也不`T0`是*interface_type*。
*  除用户定义的转换，转换不存在从`S`到`T`或从`T`到`S`。

出于这些规则，任何类型与关联的参数`S`或`T`被视为具有不与其他类型的继承关系和参数将被忽略这些类型的任何约束的唯一类型。

在示例
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
第一个的两个运算符声明允许的因为出于[索引器](classes.md#indexers).3，`T`并`int`和`string`分别被视为与任何关系的唯一类型。 但是，第三个运算符是一个错误，因为`C<T>`是类的基类`D<T>`。

从第二条规则，它遵循的转换运算符必须转换到或从在其中声明该运算符的类或结构类型。 例如，就可以为类或结构类型`C`定义的转换从`C`到`int`并从`int`到`C`，但不能从`int`到`bool`。

不能直接重新定义的预定义的转换。 因此，转换运算符不允许将转换来自或发往`object`因为之间已存在隐式和显式转换`object`和所有其他类型。 同样，在源和目标类型都不可以是转换的基类型的另一个，因为已经存在这样的转换。

但是，就可以在泛型类型，对于特定类型的参数，指定的转换已存在的预定义的转换中声明运算符。 在示例
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
类型时`object`指定为的类型参数`T`，第二个运算符声明已存在的转换 (隐式的因此还显式存在从任何类型的转换`object`)。

在其中两个类型之间存在的预定义的转换的情况下，将忽略这些类型之间的任何用户定义转换。 尤其是在下列情况下：

*  如果预定义隐式转换 ([隐式转换](conversions.md#implicit-conversions)) 存在从类型`S`键入`T`，所有用户都定义的转换 （隐式或显式） 从`S`到`T`将被忽略。
*  如果预定义的显式转换 ([显式转换](conversions.md#explicit-conversions)) 存在从类型`S`键入`T`，从任何用户定义的显式转换`S`到`T`将被忽略。 此外：

如果`T`是一个接口类型，从用户定义的隐式转换`S`到`T`将被忽略。

否则为用户定义的隐式转换从`S`到`T`仍被视为。

对于所有类型，但`object`，运算符声明为通过`Convertible<T>`与预定义的转换不冲突类型更高版本。 例如：
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

但是，对于类型`object`，预定义的转换隐藏在所有情况下，但一个用户定义的转换：

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

用户定义的转换不允许将转换来自或发往*interface_type*s。 具体而言，此限制可确保没有用户定义的转换发生时将转换为*interface_type*，并且转换为*interface_type*时才会成功对象要转换成实际实现指定*interface_type*。

转换运算符的签名组成的源类型和目标类型。 （请注意这是在签名中参与的返回类型的成员的唯一形式。）`implicit`或`explicit`分类的转换运算符不是运算符的签名的一部分。 因此，类或结构不能声明两者`implicit`和一个`explicit`具有相同的源和目标类型的转换运算符。

通常情况下，应设计用户定义的隐式转换，永远不会引发异常，并且永远不会丢失信息。 如果用户定义的转换可能导致引发异常 （例如，因为源参数不在范围内） 或丢失的信息 （如放弃高顺序位），则该转换应定义为显式转换。

在示例
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
从转换`Digit`到`byte`因为它永远不会引发异常或丢失信息，但从转换是隐式`byte`到`Digit`是因为显式`Digit`只能表示可能的子集值`byte`。

## <a name="instance-constructors"></a>实例构造函数

***实例构造函数***是实现初始化类实例所需执行的操作的成员。 使用声明实例构造函数*constructor_declaration*s:

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

一个*constructor_declaration*可能包括一套*特性*([特性](attributes.md))，四种访问修饰符的有效组合 ([访问修饰符](classes.md#access-modifiers))，和一个`extern`([外部方法](classes.md#external-methods)) 修饰符。 构造函数声明不被允许多次包含同一修饰符。

*标识符*的*constructor_declarator*必须命名在其中声明的实例构造函数的类。 如果指定的任何其他名称，则发生编译时错误。

可选*formal_parameter_list*的实例构造函数受到相同的规则*formal_parameter_list*的一种方法 ([方法](classes.md#methods))。 形参列表定义签名 ([签名和超载](basic-concepts.md#signatures-and-overloading)) 的实例构造函数，并且凭此控制过程重载决策 ([类型推理](expressions.md#type-inference)) 选择特定实例构造函数调用中。

每种类型中引用*formal_parameter_list*的实例构造函数必须至少与构造函数本身相同的可访问性 ([可访问性约束](basic-concepts.md#accessibility-constraints))。

可选*constructor_initializer*指定另一个实例构造函数调用中给定的语句执行之前*constructor_body*的此实例构造函数。 这是中作了进一步介绍[构造函数初始值设定项](classes.md#constructor-initializers)。

当构造函数声明包括`extern`修饰符，构造函数称为***外部构造函数***。 因为外部构造函数声明不提供任何实际的实现，其*constructor_body*包含一个分号。 对于所有其他构造函数， *constructor_body*组成*块*它指定要初始化类的新实例的语句。 这完全对应*块*的实例方法使用`void`返回类型 ([方法体](classes.md#method-body))。

实例构造函数不会继承。 因此，类具有实例构造函数而非实际的类中声明。 如果类不包含任何实例构造函数声明，会自动提供默认实例构造函数 ([默认构造函数](classes.md#default-constructors))。

实例构造函数调用由*object_creation_expression*s ([对象创建表达式](expressions.md#object-creation-expressions)) 并通过*constructor_initializer*s。

### <a name="constructor-initializers"></a>构造函数初始值设定项

所有实例构造函数 (除类`object`) 隐式包括另一个实例构造函数的调用后面紧跟*constructor_body*。 构造函数隐式调用由*constructor_initializer*:

*  实例构造函数初始值设定项的窗体`base(argument_list)`或`base()`导致从要调用的直接基类的实例构造函数。 使用选择该构造函数*argument_list*如果存在且的重载决策规则[重载决策](expressions.md#overload-resolution)。 候选实例构造函数集包括的直接基类中包含的所有可访问的实例构造函数或默认构造函数 ([默认构造函数](classes.md#default-constructors))，如果没有实例的构造函数中声明直接基类。 如果此集为空，或者无法识别单个最佳实例构造函数，则发生编译时错误。
*  实例构造函数初始值设定项的窗体`this(argument-list)`或`this()`导致从本身要调用的类的实例构造函数。 使用选择构造函数*argument_list*如果存在且的重载决策规则[重载决策](expressions.md#overload-resolution)。 候选实例构造函数集包括的所有可访问的实例构造函数在类本身中声明。 如果此集为空，或者无法识别单个最佳实例构造函数，则发生编译时错误。 如果某个实例构造函数声明包括调用构造函数本身的构造函数初始值设定项，发生编译时错误。

如果实例构造函数具有没有构造函数初始值设定项，在窗体的构造函数初始值设定项`base()`隐式提供。 因此，实例构造函数声明的窗体
```csharp
C(...) {...}
```
完全相当于
```csharp
C(...): base() {...}
```

通过给定的参数的作用域*formal_parameter_list*的实例构造函数声明中包含该声明的构造函数初始值设定项。 因此，构造函数初始值设定项允许访问的构造函数的参数。 例如：
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

实例构造函数初始值设定项无法访问要创建的实例。 因此它会导致编译时错误引用`this`中参数的构造函数初始值设定项表达式，因为是它要通过引用任何实例成员的自变量表达式的编译时错误*simple_name*.

### <a name="instance-variable-initializers"></a>实例变量的初始值设定项

当实例构造函数具有没有构造函数初始值设定项，或它具有窗体的构造函数初始值设定项`base(...)`，该构造函数隐式执行指定的初始化*variable_initializer*的 s在其类中声明的实例字段。 这对应于分配给构造函数和直接基类构造函数隐式调用之前在输入时立即执行的一系列。 变量的初始值设定项类声明中显示的文本顺序执行。

### <a name="constructor-execution"></a>构造函数执行

变量初始值设定项被转换为赋值语句，而这些语句执行之前基类实例构造函数的调用。 此顺序可确保有权访问该实例的任何语句执行之前，所有实例字段进行都初始化其变量初始值设定项。

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
当`new B()`用于创建实例`B`，将生成以下输出：
```
x = 1, y = 0
```

值`x`为 1，因为在调用基类实例构造函数之前执行变量初始值设定项。 但是的值`y`为 0 (默认值`int`) 因为分配到`y`基类构造函数返回之后直到才执行。

可用于将实例变量的初始值设定项和构造函数初始值设定项之前自动插入语句视为*constructor_body*。 该示例
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
包含多个变量的初始值设定项;它还包含两个窗体的构造函数初始值设定项 (`base`和`this`)。 此示例对应于代码如下所示，其中每个注释指示的自动插入的语句 （用于自动插入的构造函数调用的语法无效，但只是用来演示了机制）。

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

如果类不包含任何实例构造函数声明，会自动提供默认实例构造函数。 该默认构造函数只是调用的直接基类的无参数构造函数。 如果此类为抽象的默认构造函数声明可访问性是受保护。 否则，默认构造函数声明可访问性是公共的。 默认构造函数因此，始终是窗体

```csharp
protected C(): base() {}
```
或
```csharp
public C(): base() {}
```
其中`C`是类的名称。 如果重载解析是无法确定基类构造函数初始值设定项的唯一最佳候选项，则会发生编译时错误。

在示例
```csharp
class Message
{
    object sender;
    string text;
}
```
提供默认构造函数，因为此类不包含任何实例构造函数声明。 因此，此示例是恰好等同于
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>私有构造函数

当类`T`声明仅专用实例构造函数，不能为类外部的程序文本`T`为派生`T`直接创建的实例`T`。 因此，如果一个类只包含静态成员，并且不能被实例化，则添加一个空的私有实例构造函数将阻止实例化。 例如：
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

`Trig`类组相关的方法和常量，但不是要实例化。 因此它会声明一个空的私有实例构造函数。 必须声明至少一个实例构造函数来取消自动生成的默认构造函数。

### <a name="optional-instance-constructor-parameters"></a>可选的实例构造函数参数

`this(...)`窗体的构造函数初始值设定项通常用于结合重载实现可选的实例构造函数参数。 在示例
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
前两个实例构造函数只是为缺少自变量提供的默认值。 两者都使用`this(...)`构造函数初始值设定项来调用第三个实例构造函数，它会真正初始化新实例的工作。 效果是，可选的构造函数参数：
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>静态构造函数

一个***静态构造函数***是实现初始化已关闭的类类型所需的操作的成员。 使用声明静态构造函数*static_constructor_declaration*s:

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

一个*static_constructor_declaration*可能包括一套*特性*([特性](attributes.md)) 和一个`extern`修饰符 ([外部方法](classes.md#external-methods)).

*标识符*的*static_constructor_declaration*必须命名在其中声明静态构造函数的类。 如果指定的任何其他名称，则发生编译时错误。

如果静态构造函数声明包含`extern`修饰符，静态构造函数称为***外部静态构造函数***。 因为外部静态构造函数声明不提供任何实际的实现，其*static_constructor_body*包含一个分号。 所有其他静态构造函数声明*static_constructor_body*组成*块*它指定要执行，以初始化类的语句。 这完全对应*method_body*的静态方法替换`void`返回类型 ([方法体](classes.md#method-body))。

静态构造函数不会继承，而且不能直接调用。

已关闭的类类型的静态构造函数在给定的应用程序域中执行最多一次。 静态构造函数的执行由第一次以下事件发生在应用程序域内触发：

*  创建类类型的实例。
*  引用任何类类型的静态成员。

如果类包含`Main`方法 ([应用程序启动](basic-concepts.md#application-startup)) 中的执行开始时，静态构造函数之前执行此类为`Main`调用方法。

若要初始化新的已关闭的类类型，第一次一组新的静态字段 ([静态和实例字段](classes.md#static-and-instance-fields)) 创建该特定的封闭的类型。 每个静态字段初始化为其默认值 ([默认值](variables.md#default-values))。 接下来，静态字段初始值设定项 ([静态字段初始化](classes.md#static-field-initialization)) 执行这些静态字段。 最后，执行静态构造函数。

该示例
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
必须生成的输出：
```
Init A
A.F
Init B
B.F
```
因为执行`A`的静态构造函数通过调用触发`A.F`，和的执行`B`的静态构造函数通过调用触发`B.F`。

就可以构造允许使用变量初始值设定项在其默认值状态中观察到的静态字段的循环依赖项。

该示例
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
```
X = 1, Y = 2
```

若要执行`Main`方法，在系统首次运行的初始值设定项`B.Y`前类`B`的静态构造函数。 `Y`初始值设定项导致`A`的静态构造函数来运行，因为值`A.X`引用。 静态构造函数`A`又继续计算的值`X`，并在此过程中，提取操作的默认值`Y`，这是零。 `A.X` 因此会初始化为 1。 正在运行的进程`A`的静态字段初始值设定项和静态构造函数，然后完成后，返回到初始值的计算`Y`，结果将变得 2。

为每个封闭式构造的类类型，一次执行静态构造函数，因为它是很方便地强制执行不能在编译时通过约束检查的类型参数的运行时检查 ([类型参数约束](classes.md#type-parameter-constraints))。 例如，以下类型使用静态构造函数来强制实施的类型参数是枚举：
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

## <a name="destructors"></a>析构函数

一个***析构函数***是实现析构类的实例所必需的操作的成员。 使用声明析构函数*destructor_declaration*:

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

一个*destructor_declaration*可能包括一套*特性*([属性](attributes.md))。

*标识符*的*destructor_declaration*必须命名在其中声明析构函数的类。 如果指定的任何其他名称，则发生编译时错误。

如果析构函数声明包含`extern`修饰符，析构函数称为***外部析构函数***。 因为外部析构函数声明不提供任何实际的实现，其*destructor_body*包含一个分号。 对于所有其他析构函数， *destructor_body*组成*块*指定为了析构类的实例执行的语句。 一个*destructor_body*完全对应*method_body*的实例方法具有`void`返回类型 ([方法体](classes.md#method-body))。

析构函数不会继承。 因此，类具有不析构函数不是可能在该类中声明。

析构函数需要不具有任何参数，因为它不能为重载，因此一个类可以有，最多一个析构函数。

析构函数自动调用和不能显式调用。 实例将成为符合销毁条件时不再可以为任何代码即可使用该实例。 执行实例的析构函数可能会发生在实例变得符合销毁条件之后任何时间。 当销毁实例时，该实例的继承链中的析构函数，按顺序调用，从大多数派生程度高到最低派生。 析构函数可能在任何线程上执行。 用于控制何时以及如何执行析构函数的规则的进一步讨论，请参阅[自动内存管理](basic-concepts.md#automatic-memory-management)。

该示例的输出
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
由于继承链中的析构函数的调用顺序情况下，从大多数派生程度高到最低派生。

析构函数通过重写虚拟方法来实现`Finalize`上`System.Object`。 C# 程序中不允许重写此方法或调用它 （或它的重写） 直接。 例如，下列程序
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

编译器行为就像此方法，并替代，根本不存在。 因此，此程序：
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
有效，并显示隐藏的方法`System.Object`的`Finalize`方法。

有关的行为的讨论时从析构函数引发异常，请参阅[如何处理异常](exceptions.md#how-exceptions-are-handled)。

## <a name="iterators"></a>Iterators

函数成员 ([函数成员](expressions.md#function-members)) 使用迭代器块实现 ([块](statements.md#blocks)) 称为***迭代器***。

迭代器块可能用作函数成员的正文，前提是相应的函数成员的返回类型是一个枚举器接口 ([枚举器接口](classes.md#enumerator-interfaces)) 或一个可枚举接口 ([可枚举接口](classes.md#enumerable-interfaces))。 它可能会出现*method_body*， *operator_body*或*accessor_body*，而不能为事件、 实例构造函数、 静态构造函数和析构函数迭代器作为实现。

使用迭代器块实现函数成员时，它是形参列表的函数成员指定任何的编译时错误`ref`或`out`参数。

### <a name="enumerator-interfaces"></a>枚举器接口

***枚举器接口***是非泛型接口`System.Collections.IEnumerator`和泛型接口的所有实例化`System.Collections.Generic.IEnumerator<T>`。 为了简洁起见，在这一章中这些接口称为`IEnumerator`和`IEnumerator<T>`分别。

### <a name="enumerable-interfaces"></a>可枚举接口

***可枚举接口***是非泛型接口`System.Collections.IEnumerable`和泛型接口的所有实例化`System.Collections.Generic.IEnumerable<T>`。 为了简洁起见，在这一章中这些接口称为`IEnumerable`和`IEnumerable<T>`分别。

### <a name="yield-type"></a>Yield 类型

一个迭代器，将生成一系列值，所有相同的类型。 这种类型称为***产生类型***的迭代器。

*  返回一个迭代器的 yield 类型`IEnumerator`或`IEnumerable`是`object`。
*  返回一个迭代器的 yield 类型`IEnumerator<T>`或`IEnumerable<T>`是`T`。

### <a name="enumerator-objects"></a>枚举器对象

使用迭代器块实现返回一个枚举器接口类型的函数成员时，调用的函数成员不会不立即执行的代码在迭代器块中。 相反，***枚举器对象***创建并返回。 此对象将封装在迭代器块中，指定的代码和执行的迭代器块中的代码发生时枚举器对象的`MoveNext`调用方法。 枚举器对象具有以下特征：

*  它实现`IEnumerator`并`IEnumerator<T>`，其中`T`是迭代器的 yield 类型。
*  它实现 `System.IDisposable`。
*  （如果有） 的参数值的副本初始化和实例值传递给函数成员。
*  它有四个可能的状态，***之前***，***运行***，***挂起***，以及***后***，并最初处于***之前***状态。

枚举器对象通常是一个封装迭代器块中的代码并实现枚举器接口的编译器生成的枚举器类的实例，但也可能实现的其他方法。 如果枚举器类由编译器生成的将嵌套的类、 直接或间接地，在类中包含的函数成员，它将具有私有可访问性，和它将具有名称保留供编译器使用 ([标识符](lexical-structure.md#identifiers)).

枚举器对象可以实现比上面指定的多个接口。

以下各节介绍的具体行为`MoveNext`， `Current`，并`Dispose`的成员`IEnumerable`和`IEnumerable<T>`接口提供的枚举器对象的实现。

请注意，不支持枚举器对象`IEnumerator.Reset`方法。 调用此方法会导致`System.NotSupportedException`引发。

#### <a name="the-movenext-method"></a>MoveNext 方法

`MoveNext`方法的枚举器对象封装的迭代器块的代码。 调用`MoveNext`方法中的迭代器块和集执行代码`Current`形式相应的枚举器对象的属性。 精确所执行的操作`MoveNext`取决于枚举器对象的状态时`MoveNext`调用：

*  如果枚举数对象的状态为***之前***，则调用`MoveNext`:
   * 将状态更改为***运行***。
   * 初始化参数 (包括`this`) 的迭代器块的自变量值和保存时枚举器对象已初始化的实例值。
   * 从一开始执行迭代器块，直到执行被中断 （如下所述）。
*  如果枚举数对象的状态为***运行***，调用的结果`MoveNext`未指定。
*  如果枚举数对象的状态为***挂起***，则调用`MoveNext`:
   * 将状态更改为***运行***。
   * 将所有本地变量和参数 （包括这） 的值还原为上次挂起的迭代器块执行时保存的值。 请注意，引用这些变量的任何对象的内容可能已更改自上一个调用 MoveNext。
   * 恢复执行迭代器块紧跟`yield return`语句导致的挂起执行，并将继续，直到执行被中断 （如下所述）。
*  如果枚举数对象的状态为***后***，则调用`MoveNext`返回`false`。


时`MoveNext`执行迭代器块中，可以中断执行四种方式： 通过`yield return`语句，也可由`yield break`语句由遇到迭代器块的末尾和异常被引发和传播出去迭代器块。

*  当`yield return`遇到语句 ([yield 语句](statements.md#the-yield-statement)):
   * 在语句中给定的表达式是计算、 隐式转换为 yield 类型和分配给`Current`枚举器对象的属性。
   * 挂起的迭代器体的执行。 所有本地变量和参数的值 (包括`this`) 保存，因为此位置`yield return`语句。 如果`yield return`语句是在一个或多个`try`阻止，关联`finally`块不会在这一次执行。
   * 枚举器对象的状态将变为***挂起***。
   * `MoveNext`方法将返回`true`给迭代成功地推进到下一个值，该值指示调用方。
*  当`yield break`遇到语句 ([yield 语句](statements.md#the-yield-statement)):
   * 如果`yield break`语句是在一个或多个`try`阻止，关联`finally`块被执行。
   * 枚举器对象的状态将变为***后***。
   * `MoveNext`方法将返回`false`到其调用方，指示迭代已完成。
*  遇到迭代器体的末尾时：
   * 枚举器对象的状态将变为***后***。
   * `MoveNext`方法将返回`false`到其调用方，指示迭代已完成。
*  当引发了异常并传播出去迭代器块：
   * 相应`finally`迭代器体中的块将执行已的异常传播。
   * 枚举器对象的状态将变为***后***。
   * 异常传播的调用方继续`MoveNext`方法。

#### <a name="the-current-property"></a>Current 属性

枚举器对象的`Current`属性受`yield return`迭代器块中的语句。

当枚举器对象处于***挂起***状态的值`Current`是通过以前调用设置的值`MoveNext`。 当枚举器对象处于***之前***，***运行***，或***后***状态的访问结果`Current`未指定。

使用 yield 的迭代器键入以外`object`，访问的结果`Current`通过枚举器对象的`IEnumerable`实现对应于访问`Current`通过枚举器对象的`IEnumerator<T>`实现并将结果转换为`object`。

#### <a name="the-dispose-method"></a>Dispose 方法

`Dispose`方法用于通过引入枚举器对象来清理迭代***后***状态。

*  如果枚举数对象的状态为***之前***，则调用`Dispose`将状态更改为***后***。
*  如果枚举数对象的状态为***运行***，调用的结果`Dispose`未指定。
*  如果枚举数对象的状态为***挂起***，则调用`Dispose`:
   * 将状态更改为***运行***。
   * 执行任何最后块就像上次执行的`yield return`语句已`yield break`语句。 如果此操作导致引发和传播出去迭代器体引发异常，枚举器对象的状态设置为***后***的调用方传播异常和`Dispose`方法。
   * 将状态更改为***后***。
*  如果枚举数对象的状态为***后***，则调用`Dispose`不产生任何影响。

### <a name="enumerable-objects"></a>可枚举对象

使用迭代器块实现返回一个可枚举接口类型的函数成员时，调用的函数成员不会不立即执行的代码在迭代器块中。 相反，***的可枚举对象***创建并返回。 可枚举对象的`GetEnumerator`方法返回在迭代器块中，指定枚举器对象封装代码和执行的迭代器块中的代码发生时枚举器对象的`MoveNext`调用方法。 一个可枚举对象具有以下特征：

*  它实现`IEnumerable`并`IEnumerable<T>`，其中`T`是迭代器的 yield 类型。
*  （如果有） 的参数值的副本初始化和实例值传递给函数成员。

一个可枚举对象通常是编译器生成可枚举类封装迭代器块中的代码和实现的可枚举接口的实例，但也可能实现的其他方法。 如果由编译器生成的可枚举类，则将嵌套的类、 直接或间接地，在类中包含的函数成员，它将具有私有可访问性，和它将具有名称保留供编译器使用 ([标识符](lexical-structure.md#identifiers)).

一个可枚举对象可以实现比上面指定的多个接口。 具体而言，一个可枚举对象，还可以实施`IEnumerator`和`IEnumerator<T>`，使其能够充当可枚举值和一个枚举器。 在实现该类型，第一次一个可枚举对象`GetEnumerator`调用方法时，返回可枚举对象本身。 可枚举对象的后续调用`GetEnumerator`，如果任何，返回可枚举对象的副本。 因此，每个返回的枚举器都具有其自己的状态和一个枚举器中的更改将不会影响另一个。

#### <a name="the-getenumerator-method"></a>GetEnumerator 方法

一个可枚举对象提供的实现`GetEnumerator`的方法`IEnumerable`和`IEnumerable<T>`接口。 这两个`GetEnumerator`方法共享通用的实现，用于获取并返回的可用枚举器对象。 枚举器对象使用的参数值初始化和实例值的可枚举对象时初始化，但其他情况下保存枚举器对象的函数中所述[枚举器对象](classes.md#enumerator-objects)。

### <a name="implementation-example"></a>实现示例

本部分介绍的迭代器在标准 C# 构造方面的一个可能实现。 此处所述的实现基于 Microsoft C# 编译器所使用的相同的原则，但它不是强制性的实现或只有一个可能的。

以下`Stack<T>`类实现其`GetEnumerator`使用迭代器方法。 迭代器，用于枚举自上而下的顺序中的堆栈中的元素。

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

`GetEnumerator`方法可以转换成封装在迭代器块中，代码的编译器生成的枚举器类的实例化，在下面的示例所示。

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

在前面的转换，转换为状态机并放入迭代器块中的代码`MoveNext`枚举器类的方法。 此外，本地变量`i`以便它可以继续存在于调用到枚举器对象中的字段启用`MoveNext`。

以下示例输出的整数 1 到 10 中的一个简单的乘法表。 `FromTo`在示例中的方法将返回一个可枚举对象并使用迭代器实现。

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

`FromTo`方法可以转换成封装在迭代器块中，代码的编译器生成的可枚举类的实例化，在下面的示例所示。

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

Enumerable 类实现的可枚举接口和枚举器接口，使其能够充当可枚举值和一个枚举器。 第一次`GetEnumerator`调用方法时，返回可枚举对象本身。 可枚举对象的后续调用`GetEnumerator`，如果任何，返回可枚举对象的副本。 因此，每个返回的枚举器都具有其自己的状态和一个枚举器中的更改将不会影响另一个。 `Interlocked.CompareExchange`方法用于确保线程安全操作。

`from`和`to`参数将转换为可枚举类中的字段。 因为`from`在附加的迭代器块中修改`__from`字段引入的用于保存到给定的初始值`from`中每个枚举器。

`MoveNext`方法会抛出`InvalidOperationException`如果它时，将调用`__state`是`0`。 这可避免使用的可枚举对象的枚举器对象，而无需第一个调用作为`GetEnumerator`。

下面的示例演示一个简单的树类。 `Tree<T>`类实现其`GetEnumerator`使用迭代器方法。 迭代器枚举顺序中缀的树中的元素。

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

`GetEnumerator`方法可以转换成封装在迭代器块中，代码的编译器生成的枚举器类的实例化，在下面的示例所示。

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

在中使用编译器生成的临时内存`foreach`语句提升到`__left`和`__right`枚举器对象的字段。 `__state`仔细更新枚举器对象的字段，以便正确`Dispose()`方法将正确调用，如果引发异常。 请注意，不能编写转换后的代码具有简单`foreach`语句。

## <a name="async-functions"></a>异步函数

一种方法 ([方法](classes.md#methods)) 或匿名函数 ([匿名函数表达式](expressions.md#anonymous-function-expressions)) 与`async`修饰符称为***异步函数***。 一般情况下，术语***异步***用于描述任何类型的函数具有`async`修饰符。

它是指定任何异步函数的形参列表的编译时错误`ref`或`out`参数。

*Return_type*的异步方法必须是`void`或***任务类型***。 任务类型：`System.Threading.Tasks.Task`和类型从构造`System.Threading.Tasks.Task<T>`。 为了简洁起见，在这一章中引用这些类型作为`Task`和`Task<T>`分别。 异步方法返回任务类型称为是返回任务的。

任务类型的精确定义是实现定义，但从语言的角度来看任务类型是中一种不完整，状态是成功还是出错。 出错的任务记录相关的异常。 已成功`Task<T>`记录类型的结果`T`。 任务类型都可等待，并且因此可的操作数 await 表达式 ([Await 表达式](expressions.md#await-expressions))。

异步函数调用具有能够挂起评估通过 await 表达式 ([Await 表达式](expressions.md#await-expressions)) 在其主体中。 评估可能更高版本会恢复点处挂起 await 表达式通过***恢复委托***。 恢复委托的类型是`System.Action`，和异步函数调用的计算调用它时，将从 await 表达式从中断处恢复。 ***当前调用方***异步函数调用原始调用方函数调用永远不会被挂起，如果是也就是最新的恢复委托调用方否则。

### <a name="evaluation-of-a-task-returning-async-function"></a>返回任务的异步函数的求值

返回任务的异步函数的调用会导致返回的任务类型为其生成的实例。 这称为***返回任务***的异步函数。 该任务是最初处于未完成状态。

异步函数体然后评估直到它 （通过到达 await 表达式） 挂起或终止，点控制返回给调用方，以及返回的任务。

异步函数的主体在终止时，返回的任务被移出未完成状态：

*  如果在函数体终止作为到达返回语句或正文的末尾的结果，返回 task，放入成功状态中记录任何结果值。
*  如果在函数体将终止作为未捕获的异常的结果 ([throw 语句](statements.md#the-throw-statement)) 将进入错误状态的返回任务中记录异常。

### <a name="evaluation-of-a-void-returning-async-function"></a>返回 void 的异步函数的求值

如果异步函数的返回类型为`void`，评估不同于上述按以下方式： 因为没有任务返回，该函数改为通信完成和当前线程的例外情况***同步上下文***。 同步上下文的精确定义取决于实现，但是表示形式的当前线程正在运行"where"。 返回 void 的异步函数的求值开始、 成功完成，或会导致引发未捕获的异常时，通知同步上下文。

这样，来跟踪在其下运行多少返回 void 的异步函数，并决定如何传播异常来自它们的上下文。
