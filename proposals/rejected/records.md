---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: ae114131069ca76e4a1ec8149b7bab81fce8965c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2020
ms.locfileid: "84228189"
---
# <a name="records"></a>records

* [x] 建议
* [] 原型：[完成](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] 实现：[正在进行](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] 规范：包含的草稿规范

## <a name="summary"></a>总结
[summary]: #summary

记录是 c # 类和结构类型的一个新的简化声明窗体，它们结合了许多更简单的功能的优点。 我们介绍了新功能（调用方-接收方参数和*带有表达式*的），为记录声明提供语法和语义，并提供一些示例。


## <a name="motivation"></a>动机
[motivation]: #motivation

在 c # 中，类型声明的数量很少比类型化数据的聚合集合少。 遗憾的是，声明此类类型需要大量的样板代码。 *记录*提供了一种机制，用于通过描述聚合的成员以及其他代码或与常用样本（如果有）的偏差来声明数据类型。

请参阅下面的[示例](#examples)。

## <a name="detailed-design"></a>详细设计
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>调用方-接收方参数

当前方法参数的*默认参数*必须是 
- *常数表达式*;或
- 格式为的表达式， `new S()` 其中 `S` 是值类型; 或
- 格式为的表达式， `default(S)` 其中 `S` 是值类型

我们对此进行了扩展，以添加以下
- 格式为的表达式`this.Identifier`

此新格式称为*调用方-接收方默认参数*，仅当满足以下所有条件时才允许使用
- 它在其中出现的方法是实例方法;与
- 表达式 `this.Identifier` 绑定到封闭类型的实例成员，该成员必须是字段或属性; 并且
- 它绑定到的成员（ `get` 如果它是属性，则为访问器）至少可作为方法访问; 和
- 的类型可 `this.Identifier` 通过标识或可为 null 的转换隐式转换为参数的类型（这是对*默认参数*的现有约束）。

当使用*调用方-接收方默认参数*的对应可选参数的函数成员调用省略参数时，将隐式传递接收方成员的值。 

> **设计说明**：调用方参数的主要原因是为了支持*with 表达式*。 其思路是可以声明如下所示的方法
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> 然后按如下所示使用它
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> 如果为，则创建一个新的， `Point` `Point` 它与已更改的值相同 `X` 。
> 
> 这是一个打开的问题，无论是否值得添加*表达式*的语法形式的调用方-接收方参数，都可以执行此操作，*而*不是*in addition to* *使用 with 表达式*。

- []**打开问题**：计算*调用方-接收方默认参数*的顺序与其他参数的计算顺序是什么？ 是否应指出它未指定？

### <a name="with-expressions"></a>with-表达式

建议使用新的表达式格式：

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

标记 `with` 是一个新的上下文相关的关键字。

*With_initializer*左侧的每个*标识符*都必须绑定到*with_expression*的*primary_expression*类型的可访问实例字段或属性。 给定*with_expression*的这些标识符中不能有重复的名称。

窗体的*with_expression*

> *e1* `with``{`*标识符*  =  *e2*，.。。`}`

被视为对窗体的调用

> *e1* `.With(`*identifier2* `:`*e2*，.。。`)`

其中，对于名为 `With` *e1*的可访问实例成员的每个方法，我们选择*identifier2*作为该方法中第一个参数的名称，该方法具有与实例字段或绑定到*标识符*的属性相同的成员。 如果无法识别此类参数，则不需要考虑方法。 要调用的方法是从其余候选项的重载决策中选择的。

> **设计说明**：如果调用方是接收方参数，则无需此特殊语法形式即可使用*带有表达式*的许多优点。 因此，我们考虑是否需要此方法。 它的主要好处是允许在字段和属性的名称（而不是参数名称）中进行编程。 这样，我们就可以提高可读性和工具质量（例如，对*with_expression*的标识符进行的定义将导航到属性而不是方法参数）。

- []**打开问题**：应修改此描述以支持扩展方法。

### <a name="pattern-matching"></a>模式匹配

查看规范的[模式匹配规范](csharp-8.0/patterns.md#positional-pattern) `Deconstruct` 及其与模式匹配的关系。

> **设计说明**：根据此处指定的编译器生成的 `Deconstruct` ，以及用于模式匹配的规范，记录声明
> ```cs
> public class Point(int X, int Y);
> ```
> 将支持位置模式匹配，如下所示
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>记录类型声明

或声明的语法 `class` `struct` 已扩展为支持值参数; 参数将变为类型为的属性：

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **设计说明**：由于在不需要在类体中显式声明任何成员的情况下，记录类型通常很有用，因此我们修改声明的语法，以允许主体只是一个分号。

使用*记录参数*声明的类（结构）称为*记录类*（*record struct*），其中的任何一个是*记录类型*。

- []**打开问题**：我们需要将*primary_constructor_body*包含在语法中，以便它可以出现在记录类型声明中。
- []**打开问题**：参数名称的名称冲突规则是什么？ 可能不允许一个类型参数或另一个*记录参数*冲突。
- []**打开问题**：需要指定记录参数的作用域。 在哪里可以使用它们？ 可能在实例字段初始值设定项和*primary_constructor_body*至少。
- []**打开问题**：记录类型声明是否是分部的？ 如果是这样，每个部分都必须重复参数吗？

#### <a name="members-of-a-record-type"></a>记录类型的成员

除了在*类体*中声明的成员以外，记录类型还具有下列附加成员：

##### <a name="primary-constructor"></a>主构造函数

记录类型具有一个 `public` 构造函数，该构造函数的签名对应于类型声明的值参数。 这称为类型的*主构造函数*，并导致禁止显示隐式声明的*默认构造函数*。

在运行时，主构造函数

* 为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的）;[请参阅 1.1.2](#1.1.2)）;接着
* 执行出现在*类体*中的实例字段初始值设定项;然后
* 调用基类构造函数：
    * 如果*record_base_arguments*中有参数，则调用由重载解析选择的具有这些参数的基构造函数;
    * 否则，将调用不带参数的基构造函数。
* 按源顺序执行每个*primary_constructor_body*（如果有）的正文。

- []**打开问题**：需要指定顺序，尤其是在分区的编译单元中。
- []**打开问题**：需要指定每个显式声明的构造函数必须链接到主构造函数。
- []**打开问题**：是否允许更改主构造函数的访问修饰符？
- []**打开问题**：在记录结构中，不存在任何记录参数，这是一个错误？

##### <a name="primary-constructor-body"></a>主构造函数正文

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

*Primary_constructor_body*只能在记录类型声明内使用。 *Primary_constructor_body*的*标识符*应命名声明它的记录类型。

*Primary_constructor_body*不会自行声明成员，但它是一种为程序员提供属性的方法，并指定了记录类型的主构造函数的访问权限。 它还使程序员能够提供在构造记录类型的实例时要执行的附加代码。

- []**打开问题**：应注意，结构默认构造函数会绕过这种情况。
- []**打开问题**：应指定初始化的执行顺序。
- []**打开问题**：我们是否应在非记录类型声明中允许诸如*primary_constructor_body* （可能没有属性和修饰符）之类的内容，并将其视为实例字段初始值设定项的代码？

##### <a name="properties"></a>属性

对于记录类型声明的每个记录参数，都有一个对应的 `public` 属性成员，其名称和类型取自值参数声明。 其名称是*record_property_name*的*标识符*（如果存在）或*record_parameter*的*标识符*，否则为。 如果没有 `get` 显式声明或继承具有访问器且具有此名称和类型的具体（即非抽象）公共属性，则编译器将生成，如下所示：

* 对于*记录结构*或 `sealed` *记录类*：
 * `private` `readonly` 字段作为属性的支持字段生成 `readonly` 。 在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。
 * 实现该属性的 `get` 访问器以返回支持字段的值。
 * 将重写每个 "匹配" 继承的虚拟属性的 `get` 访问器。

> **设计说明**：换言之，如果扩展基类或实现一个接口，该接口声明了具有与 record 参数相同的名称和类型的公共抽象属性，则会重写或实现该属性。

- []**打开问题**：当显式声明某个属性时，是否可以更改该属性的访问修饰符？
- []**打开问题**：是否可以用属性替换某个字段？

##### <a name="object-methods"></a>对象方法

对于*记录结构*或 `sealed` *记录类*，方法的实现 `object.GetHashCode()` 和 `object.Equals(object)` 由编译器生成，除非用户提供。

- []**打开问题**：我们应该精确指定其实现。
- []**打开问题**：我们还应 `IEquatable<T>` 为记录类型添加接口，并指定提供实现。
- []**打开问题**：我们还应指定实现每个 `IEquatable<T>.Equals` 。
- []**打开问题**：我们应确切地指定我们在记录继承面对的问题的解决方法：具体而言，我们如何生成相等方法，以使它们具有对称、可传递、反身等。
- []**打开问题**：已建议实施 `operator ==` 和 `operator !=` 记录类型。
- []**打开问题**：是否应自动生成的实现 `object.ToString` ？

##### `Deconstruct`

记录类型具有编译器生成的方法， `public` `void Deconstruct` 除非用户提供一个具有任何签名的方法。 每个参数都是与该 `out` 记录类型的相应参数具有相同名称和类型的参数。 此方法的编译器提供的实现应为每个 `out` 参数分配相应属性的值。

请参阅的语义的[模式匹配规范](csharp-8.0/patterns.md#positional-pattern) `Deconstruct` 。

##### <a name="with-method"></a>`With` 方法

除非存在名为 "已声明" 的用户声明的成员 `With` ，否则记录类型具有一个名为的编译器提供的方法 `With` ，该方法的返回类型为记录类型本身，并包含与每个*记录参数*相对应的值参数，这些参数在记录类型声明中出现的顺序相同。 每个参数都应具有相应属性的*调用方-接收方默认参数*。

在 `abstract` record 类中，编译器提供的 `With` 方法是抽象的。 在记录结构或密封记录类中，编译器提供的 `With` 方法是 `sealed` 。 否则，编译器提供的 `With` 方法是 "虚拟的，它的实现应返回一个新实例，该实例通过将参数作为参数来调用主构造函数以从参数创建新实例，并返回该新实例。

- []**打开问题**：我们还应指定在什么条件下重写或实现 `With` `With` 从实现的接口继承的虚方法或方法。
- []**打开问题**：我们应指出当继承非虚拟方法时会发生什么情况 `With` 。

> **设计说明**：由于记录类型在默认情况下是不可变的，因此该 `With` 方法提供了一种方法来创建新的实例，该实例与现有实例相同，但所选属性为新值。 例如，给定
> ```cs
> public class Point(int X, int Y);
> ```
> 存在编译器提供的成员
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> 这将启用记录类型的变量
> ```cs
> var p = new Point(3, 4);
> ```
> 替换为具有一个或多个属性的实例
> ```cs
>     p = p.With(X: 5);
> ```
> 这还可以使用*with_expression*来表示：
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>示例

#### <a name="compatibility-of-record-types"></a>记录类型的兼容性

由于程序员可以将成员添加到记录类型声明中，因此通常可以更改记录元素的集合，而不会影响现有的客户端。 例如，给定记录类型的初始版本

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

记录类型的新元素可以兼容性添加到该类型的下一版本中，而不会影响二进制或源兼容性：

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>record 结构示例

此记录结构

```cs
public struct Pair(object First, object Second);
```

转换为此代码

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- []**打开问题**： Equals 的实现（对等）是否是对的公共成员？
- []**打开问题**：此实现 `Equals` 在面对继承时不对称。
- []**打开问题**：记录是否应声明 `operator ==` 和 `operator !=` ？

> **设计说明**：由于一种记录类型可以从另一种记录类型继承，因此 `Equals` 在这种情况下，此实现不是对称的，这是不正确的。 建议以这种方式实现相等性：
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> 派生记录是 `override EqualityContract` 。 更具吸引力的替代方法是限制继承。

#### <a name="sealed-record-example"></a>密封记录示例

此密封记录类

```cs
public sealed class Student(string Name, decimal Gpa);
```

转换为此代码

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>abstract 记录类示例

此抽象记录类

```cs
public abstract class Person(string Name);
```

转换为此代码

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>合并抽象记录和密封记录

给定上述抽象记录类 `Person` ，此密封记录类

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

转换为此代码

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>缺点
[drawbacks]: #drawbacks

与任何语言功能一样，我们必须回答对 c # 程序正文提供的更清晰的偿还，以使其受益于该功能。

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

在 c # 6 中，我们考虑添加*主构造函数*。 尽管它们与此建议具有相同的语法图面，但我们发现它们的优点低于记录所提供的优势。

## <a name="unresolved-questions"></a>未解决的问题
[unresolved]: #unresolved-questions

建议的正文中出现打开的问题。

## <a name="design-meetings"></a>设计会议

TBD
