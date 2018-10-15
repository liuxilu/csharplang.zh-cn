# <a name="structs"></a>结构

结构与类很相似，因为它们表示可以包含数据成员和函数成员的数据结构。 但是，与类不同，结构是值类型，不需要堆分配。 结构类型的变量直接包含的数据结构，而类类型的变量包含数据后, 一种称为对象的引用。

结构对包含值语义的小型数据结构特别有用。 复数、坐标系中的点或字典中的键值对都是结构的典型示例。 这些数据结构的关键在于它们具有少量数据成员，它们不需要使用继承或引用的标识，而且，它们可以方便地使用实现值语义分配复制而不是引用的值的位置。

如中所述[简单类型](types.md#simple-types)，如由 C# 中，提供的简单类型`int`， `double`，和`bool`，实际上是所有结构类型。 就像这些预定义的类型是结构，它还有可能要使用的结构和运算符重载在 C# 语言中实现新的"基元"类型。 在本章末尾提供这种类型的两个示例 ([结构示例](structs.md#struct-examples))。

## <a name="struct-declarations"></a>结构声明

一个*struct_declaration*是*type_declaration* ([类型声明](namespaces.md#type-declarations)) 的声明的新结构：

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

一个*struct_declaration*包含一组可选*特性*([特性](attributes.md)) 后, 跟一组可选的*struct_modifier*s ([结构修饰符](structs.md#struct-modifiers)) 后, 跟可选`partial`修饰符，关键字后跟`struct`和一个*标识符*命名结构后, 跟可选*type_parameter_list*规范 ([类型参数](classes.md#type-parameters)) 后, 跟一个可选*struct_interfaces*规范 ([Partial 修饰符](structs.md#partial-modifier))) 后, 跟一个可选*type_parameter_constraints_clause*s 规范 ([类型参数约束](classes.md#type-parameter-constraints)) 后, 跟*struct_body* ([结构体](structs.md#struct-body)) 后, 跟分号 （可选）。

### <a name="struct-modifiers"></a>结构修饰符

一个*struct_declaration*可以根据需要包含一系列结构修饰符：

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

它是要在结构声明中多次出现的相同修饰符的编译时错误。

结构声明中的修饰符具有相同的含义的类声明 ([类声明](classes.md#class-declarations))。

### <a name="partial-modifier"></a>Partial 修饰符

`partial`修饰符指示这*struct_declaration*是分部类型声明。 具有相同名称的封闭命名空间或类型声明中的多个分部结构声明结合在一起形成一个结构声明中，遵循的规则中指定[分部类型](classes.md#partial-types)。

### <a name="struct-interfaces"></a>结构接口

结构声明可能包括*struct_interfaces*规范，说用例结构直接实现给定的接口类型。

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

讨论了接口实现中进一步[接口实现代码](interfaces.md#interface-implementations)。

### <a name="struct-body"></a>在结构体

*Struct_body*结构的定义结构的成员。

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>结构成员

结构的成员包括引入的成员及其*struct_member_declaration*s 和成员继承自类型`System.ValueType`。

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

除了中记下的区别之外[类和结构的区别](structs.md#class-and-struct-differences)，在提供的类成员的说明[类成员](classes.md#class-members)通过[迭代器](classes.md#iterators)适用于结构以及成员。

## <a name="class-and-struct-differences"></a>类和结构的区别

结构与类不同，在几个重要方面：

*  结构是值类型 ([值的语义](structs.md#value-semantics))。
*  所有结构类型隐式都继承自类`System.ValueType`([继承](structs.md#inheritance))。
*  结构类型的变量进行赋值会创建一份所赋的值 ([分配](structs.md#assignment))。
*  结构的默认值是生成通过将所有值类型字段都设置为其默认值和所有引用类型字段的值`null`([默认值](structs.md#default-values))。
*  装箱和取消装箱操作用于将结构类型之间进行转换并`object`([装箱和取消装箱](structs.md#boxing-and-unboxing))。
*  含义`this`是不同的结构 ([此访问权限](expressions.md#this-access))。
*  不允许在结构的实例字段声明包括变量的初始值设定项 ([字段初始值设定项](structs.md#field-initializers))。
*  结构不允许的无参数实例构造函数声明 ([构造函数](structs.md#constructors))。
*  结构不允许声明析构函数 ([析构函数](structs.md#destructors))。

### <a name="value-semantics"></a>值语义

结构是值类型 ([值类型](types.md#value-types)) 并被视为具有值语义。 类，但是，是引用类型 ([引用类型](types.md#reference-types)) 并被视为具有引用语义。

结构类型的变量直接包含的数据结构，而类类型的变量包含数据后, 一种称为对象的引用。 当结构`B`包含类型的实例字段`A`和`A`是一个结构类型，是编译时错误`A`依赖于`B`或从构造类型`B`。 结构`X`***直接依赖于***结构`Y`如果`X`包含类型的实例字段`Y`。 根据此定义，结构所依赖的结构的完整集合是可传递闭包的***直接依赖于***关系。  例如
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
是一个错误因为`Node`包含其自己的类型的实例字段。  另一个示例
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
是一个错误，因为每个类型`A`， `B`，和`C`彼此依赖。

借助类，它是两个变量来引用同一对象，并因此可能会对一个变量以影响其他变量所引用的对象的操作。 对于结构，每个变量都具有其自己的数据副本 (的情况下除外`ref`和`out`参数变量)，则可能会影响另一个操作。 此外，由于结构不是引用类型，不能为结构类型的值的`null`。

已知的声明
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
代码片段
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
输出值`10`。 分配`a`到`b`会创建一份值，并`b`因此不受分配到`a.x`。 有`Point`改为已声明为一个类，则输出将为`100`因为`a`和`b`将引用同一对象。

### <a name="inheritance"></a>继承

所有结构类型隐式都继承自类`System.ValueType`，而后者又继承自类`object`。 结构声明可以指定一组实现的接口，但不能为结构声明中指定的基类。

结构类型永远不会是抽象的始终隐式密封。 `abstract`和`sealed`修饰符因此不允许在结构声明中。

由于结构不支持继承，因此不能为结构成员的声明可访问性`protected`或`protected internal`。

结构中的函数成员不能`abstract`或`virtual`，并`override`修饰符仅可以重写方法继承自`System.ValueType`。

### <a name="assignment"></a>赋值

结构类型的变量进行赋值会创建一份所赋的值。 这不同于分配给变量的类类型，复制引用而不是由引用标识的对象。

类似于分配，将结构作为值参数传递或者作为函数成员的结果返回时，该结构的副本创建。 可以通过对使用函数成员的引用传递结构`ref`或`out`参数。

当属性或索引器的结构作为赋值目标时，与属性或索引器访问关联的实例表达式必须分类为变量中。 如果实例表达式分类为一个值，发生编译时错误。 进一步详细介绍[简单的赋值](expressions.md#simple-assignment)。

### <a name="default-values"></a>默认值

如中所述[默认值](variables.md#default-values)，在创建时，有几种变量将自动初始化为其默认值。 对于类类型和其他引用类型的变量，此默认值是`null`。 但是，由于结构是值类型，不能`null`，默认值的结构是通过将所有值类型字段都设置为其默认值和所有引用类型字段生成的值`null`。

指`Point`结构声明该示例上方
```csharp
Point[] a = new Point[100];
```
初始化每个`Point`中通过如下设置生成的值数组`x`和`y`为零的字段。

结构的默认值对应于该结构的默认构造函数返回的值 ([默认构造函数](types.md#default-constructors))。 与类不同，结构不允许声明一个无参数实例构造函数。 相反，每个结构都隐式具有一个无参数实例构造函数，它始终返回的值设置为其默认值和所有引用类型字段的所有值类型字段而得出的`null`。

结构应旨在有效状态，请考虑默认值初始化状态。 在示例
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
用户定义的实例构造函数可防止 null 值仅在显式调用。 在情况下，`KeyValuePair`变量可能会发生默认值初始化`key`和`value`字段将为 null，且该结构必须准备好处理此状态。

### <a name="boxing-and-unboxing"></a>装箱和取消装箱

类类型的值可以转换为类型`object`或只需通过将引用视为另一种类型在编译时由类实现的接口类型。 同样，类型的值`object`或接口类型的值可以无需更改的引用转换回类类型 （但当然运行时类型检查是必需的这种情况下）。

由于结构不是引用类型，这些操作以不同方式实现对结构类型。 当结构类型的值转换为类型`object`或由该结构实现的接口类型，装箱操作发生。 同样，当类型的值时`object`或接口类型的值转换为结构类型，取消装箱操作会发生。 从类类型相同的操作的主要差异是，装箱和取消装箱结构将值复制传入或传出的已装箱的实例。 因此，在装箱或取消装箱操作后，对未装箱结构所做更改将不反映在装箱的结构。

当结构类型重写继承的虚方法`System.Object`(如`Equals`， `GetHashCode`，或`ToString`)，通过结构类型的实例的虚拟方法调用不会导致装箱发生。 即使在该结构用作类型参数，并且调用发生通过类型参数类型的实例时，这是，则返回 true。 例如：
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

该程序的输出为：
```
1
2
3
```

尽管不是好的风格`ToString`有副作用，示例演示了针对第三次调用中的发生没有值类型装箱`x.ToString()`。

类似地，永远不会隐式进行装箱时访问受约束的类型参数上的成员。 例如，假设一个接口`ICounter`包含一个方法，`Increment`这可用于修改值。 如果`ICounter`用作约束的实现`Increment`使用对变量的引用来调用方法的`Increment`装箱副本永远不会对调用。

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

首次调用`Increment`修改变量中的值`x`。 这并不等同于第二次调用`Increment`，它修改的已装箱副本中的值`x`。 因此，程序的输出为：
```
0
1
1
```

装箱和取消装箱的更多详细信息，请参阅[装箱和取消装箱](types.md#boxing-and-unboxing)。

### <a name="meaning-of-this"></a>这其中的含义

实例构造函数或类的实例函数成员`this`分类为一个值。 因此，尽管`this`可用于引用的实例为其函数成员调用，它不能将分配给`this`函数成员的类中。

中的结构，实例构造函数`this`对应于`out`参数的结构类型，和中的结构，实例函数成员`this`对应于`ref`结构类型的参数。 在这两种情况下，`this`分类为变量，并且可能需要修改整个结构通过将分配给为其调用的函数成员`this`或者通过将此作为`ref`或`out`参数。

### <a name="field-initializers"></a>字段初始值设定项

如中所述[默认值](structs.md#default-values)，将所有值类型字段都设置为其默认值和所有引用类型字段而得出的值的结构的默认值包括`null`。 出于此原因，结构不允许实例字段声明中包含变量的初始值设定项。 此限制仅适用于实例字段。 结构的静态字段都可以包含变量的初始值设定项。

该示例
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
错误是因为实例字段声明包括变量的初始值设定项。

### <a name="constructors"></a>构造函数

与类不同，结构不允许声明一个无参数实例构造函数。 相反，每个结构都隐式具有一个无参数实例构造函数，它始终返回将所有值类型字段都设置为其默认值和所有引用类型字段为 null 而得出的值 ([默认构造函数](types.md#default-constructors)). 结构可以声明具有参数的实例构造函数。 例如
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

已知上面的声明语句
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
二者都可以创建`Point`与`x`和`y`初始化为零。

不允许使用结构实例构造函数以包括窗体的构造函数初始值设定项`base(...)`。

如果结构实例构造函数不会指定构造函数初始值设定项`this`变量对应于`out`参数的结构类型，并类似于`out`参数，`this`必须明确赋值 （[明确赋值](variables.md#definite-assignment)) 在构造函数将返回其中每个位置。 如果结构实例构造函数指定的构造函数初始值设定项，`this`变量对应于`ref`参数的结构类型，并类似于`ref`参数，`this`被视为上明确赋值向构造函数主体的项。 请考虑下面的实例构造函数实现：
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

任何实例成员函数 (包括属性 set 访问器`X`和`Y`) 可以调用，直到已明确赋值正在构造的结构的所有字段。 唯一的例外涉及自动实现的属性 ([自动实现的属性](classes.md#automatically-implemented-properties))。 明确赋值规则 ([简单的赋值表达式](variables.md#simple-assignment-expressions)) 专门免除该结构类型的结构类型的实例构造函数中的自动属性赋值： 此类赋值被视为决定性自动属性的隐藏的支持字段赋值。 因此，以下被允许使用：

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>析构函数

结构不是允许声明析构函数。

### <a name="static-constructors"></a>静态构造函数

结构的静态构造函数所遵循的与类相同的规则。 结构类型的静态构造函数的执行由第一次以下事件发生在应用程序域内触发：

*  引用结构类型的静态成员。
*  结构类型的显式声明的构造函数调用。

创建的默认值 ([默认值](structs.md#default-values)) 结构的类型不会触发该静态构造函数。 （此示例是数组中元素的初始值。）

## <a name="struct-examples"></a>结构示例

下面显示了使用的两个重要示例`struct`类型来创建可用于对预定义的类型的语言，但使用修改后的语义的同样的类型。

### <a name="database-integer-type"></a>数据库的整数类型

`DBInt`下面结构实现整数类型可表示的值的完整集`int`类型，以及指示未知的值的其他状态。 在数据库中通常使用具有这些特征的类型。

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>数据库的布尔值类型

`DBBool`下面结构实现三值逻辑类型。 此类型的可能的值为`DBBool.True`， `DBBool.False`，并`DBBool.Null`，其中`Null`成员指示未知的值。 在数据库中通常使用此类三值逻辑类型。

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
