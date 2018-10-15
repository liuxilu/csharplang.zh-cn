# <a name="attributes"></a>特性

C# 语言的许多使程序员能够指定有关在程序中定义的实体的声明性信息。 例如，在类中方法的可访问性通过修饰来指定其与*method_modifier*s `public`， `protected`， `internal`，并`private`。

C# 允许程序员发明的声明性信息，名为的新类型***属性***。 然后，程序员可以将属性附加到各种程序实体，并检索在运行时环境中的属性信息。 例如，可以定义一个框架`HelpAttribute`可以放在特定程序元素 （如类和方法） 以提供从这些程序元素到其文档的映射的属性。

通过用于特性类的声明定义特性 ([属性类](attributes.md#attribute-classes))，这可以具有位置和命名参数 ([位置和命名的参数](attributes.md#positional-and-named-parameters))。 特性附加到使用特性规范的 C# 程序中的实体 ([属性规范](attributes.md#attribute-specification))，并且可以作为属性实例在运行时检索 ([特性实例](attributes.md#attribute-instances))。

## <a name="attribute-classes"></a>属性类

从抽象类派生的类`System.Attribute`，无论直接或间接地，是***特性类***。 特性类的声明类型的定义一个新***特性***可放置在声明。 按照约定，特性类命名为后缀`Attribute`。 使用属性可以包括或省略此后缀。

### <a name="attribute-usage"></a>特性用法

该属性`AttributeUsage`([AttributeUsage 特性](attributes.md#the-attributeusage-attribute)) 用于描述如何使用特性类。

`AttributeUsage` 具有位置参数 ([位置和命名的参数](attributes.md#positional-and-named-parameters)) 使特性类来指定在其可以使用的声明的类型。 该示例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

定义一个名为属性类`SimpleAttribute`可在放置*class_declaration*s 和*interface_declaration*仅。 该示例

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

显示的几种用法`Simple`属性。 尽管此特性定义的名称`SimpleAttribute`，当使用此属性时，`Attribute`可能会省略后缀，从而导致的短名称`Simple`。 因此，上面的示例在语义上等效于下面：

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` 有一个命名的参数 ([位置和命名的参数](attributes.md#positional-and-named-parameters)) 调用`AllowMultiple`，指示是否可以多次指定该属性为给定实体。 如果`AllowMultiple`类的属性为 true，则该特性类是***多用途特性类***，并且在实体上多次指定。 如果`AllowMultiple`类的属性为 false 或是未指定，则该特性类是***一次性特性类***，并且在上一个实体最多一次指定。

该示例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

定义一个名为的多用途特性类`AuthorAttribute`。 该示例

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

演示了一个类声明的两种用法与`Author`属性。

`AttributeUsage` 具有名为的另一个命名的参数`Inherited`，指示是否属性，当指定为基类，也由派生自该基类的类继承。 如果`Inherited`类的属性为 true，则继承该属性。 如果`Inherited`类的属性为 false，则不会继承该属性。 如果未指定，其默认值为 true。

特性类`X`不具有`AttributeUsage`属性附加到它，如

```csharp
using System;

class X: Attribute {...}
```

等效于以下：

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>定位和命名参数

特性类可以具有***位置参数***并***命名参数***。 每个公共实例构造函数，为该特性类定义的该特性类的位置参数是有效的序列。 每个非静态公共读写字段和属性的特性类定义特性类的命名的参数。

该示例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

定义一个名为属性类`HelpAttribute`它具有一个位置参数`url`，和一个命名的参数， `Topic`。 尽管非静态和 public，该属性，则`Url`不定义一个命名的参数，因为它不是读写。

可能按如下所示使用此特性类：

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>特性参数类型

为属性类的位置和命名参数的类型仅限于***属性参数类型***，它们是：

*  以下类型之一： `bool`， `byte`， `char`， `double`， `float`， `int`， `long`， `sbyte`， `short`， `string`， `uint`， `ulong`，`ushort`.
*  `object` 类型。
*  `System.Type` 类型。
*  枚举类型，提供具有公共可访问性，并在其中它嵌套 （如果有） 的类型还具有公共可访问性 ([属性规范](attributes.md#attribute-specification))。
*  上述类型的一维数组。
*  构造函数参数或公共字段不包含其中一种类型，不能用作特性规范中的位置或命名参数。

## <a name="attribute-specification"></a>特性规范

***属性规范***是到声明此前定义的属性的应用程序。 属性是一种声明的指定的其他声明性信息。 可以在全局范围内 （以指定包含程序集或模块上的属性） 指定特性和有关*type_declaration*s ([类型声明](namespaces.md#type-declarations))， *class_member_declaration*s ([类型参数约束](classes.md#type-parameter-constraints))， *interface_member_declaration*s ([接口成员](interfaces.md#interface-members))， *struct_member_declaration*s ([结构成员](structs.md#struct-members))， *enum_member_declaration*s ([枚举成员](enums.md#enum-members))， *accessor_declarations* ([访问器](classes.md#accessors))， *event_accessor_declarations* ([类似字段的事件](classes.md#field-like-events))，和*formal_parameter_list*s ([方法参数](classes.md#method-parameters))。

以指定属性***属性部分***。 属性部分包含的一对方括号，环绕一个或多个属性的逗号分隔列表。 在此类列表中，指定属性和在其中部分附加到同一个程序实体的顺序排列的顺序并不重要。 例如，属性规范`[A][B]`， `[B][A]`， `[A,B]`，和`[B,A]`是等效的。

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

属性组成*attribute_name*和位置和命名参数的可选列表。 位置参数 （如果有） 优先于命名的参数。 位置自变量组成*attribute_argument_expression*; 命名的参数包含的名称后, 跟一个等号后, 跟*attribute_argument_expression*，而后者在一起与简单赋值的相同规则约束。 命名参数的顺序并不重要。

*Attribute_name*标识一个特性类。 如果的窗体*attribute_name*是*type_name* ，请参阅此名称必须是特性类。 否则，将发生编译时错误。 该示例

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

因为它尝试使用会导致编译时错误`Class1`作为特性类时`Class1`不是特性类。

某些上下文中允许多个目标属性的规范。 程序可以显式指定目标，通过包括*attribute_target_specifier*。 当属性放置在全局级别*global_attribute_target_specifier*是必需的。 在所有其他位置，将应用合理的默认值，但*attribute_target_specifier*可用于确认或重写中某些不明确的情况下的默认值 （或只需确认中非不明确的情况下的默认值）。 因此，通常情况下， *attribute_target_specifier*s 可以在全局级别省略除外。 可能不明确的上下文已得到解决，如下所示：

*  在全局范围内指定的属性可以应用到目标程序集或目标模块。 无默认值为此上下文中，因此存在*attribute_target_specifier*始终需要在此上下文中。 是否存在`assembly` *attribute_target_specifier*指示该特性应用于目标程序集; 是否存在`module` *attribute_target_specifier*指示该属性应用到目标模块。
*  在委托声明上指定的属性可以应用到所声明的委托或其返回值。 如果没有*attribute_target_specifier*，该属性应用于该委托。 是否存在`type` *attribute_target_specifier*指示该特性应用于该委托; 是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。
*  方法声明上指定的属性可以应用到所声明的方法或其返回值。 如果没有*attribute_target_specifier*，该特性应用于该方法。 是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`return` *attribute_target_specifier*指示该特性所应用于返回值。
*  在运算符声明上指定的属性可以应用到所声明的运算符或其返回值。 如果没有*attribute_target_specifier*，则此属性应用于该运算符。 是否存在`method` *attribute_target_specifier*指示该特性应用于该运算符; 是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。
*  指定在事件声明的省略了事件访问器的一个特性可以应用于所声明的事件、 关联的字段 （如果该事件不是抽象的），或关联添加和删除方法。 如果没有*attribute_target_specifier*，该特性应用于该事件。 是否存在`event` *attribute_target_specifier*指示该特性应用于事件; 是否存在`field` *attribute_target_specifier*指示该属性应用于字段;是否存在`method` *attribute_target_specifier*指示该特性应用于方法。
*  在属性或索引器声明上的 get 访问器声明上指定的属性可以应用到关联的方法或其返回值。 如果没有*attribute_target_specifier*，该特性应用于该方法。 是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`return` *attribute_target_specifier*指示该特性所应用于返回值。
*  Set 访问器属性或索引器声明上指定的属性可以应用到关联的方法或其单独的隐式参数。 如果没有*attribute_target_specifier*，该特性应用于该方法。 是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`param` *attribute_target_specifier*指示此特性应用于参数;是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。
*  为事件声明可以应用于关联的方法或其单独的参数指定在 add 或 remove 访问器声明属性。 如果没有*attribute_target_specifier*，该特性应用于该方法。 是否存在`method` *attribute_target_specifier*指示该特性应用于方法; 是否存在`param` *attribute_target_specifier*指示此特性应用于参数;是否存在`return` *attribute_target_specifier*指示该特性应用于返回值。

在其他上下文中，包含*attribute_target_specifier*允许但不必要。 例如，类声明可能包括或省略说明符`type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

它是错误指定了一个无效*attribute_target_specifier*。 例如，说明符`param`不能在类声明中使用：

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

按照约定，特性类命名为后缀`Attribute`。 *Attribute_name*窗体*type_name*可以包括或省略此后缀。 如果使用或不带此后缀找到特性类，则产生歧义，且会导致编译时错误。 如果*attribute_name*拼写，以便其右*标识符*是逐字字符串标识符 ([标识符](lexical-structure.md#identifiers))，则只能将不带后缀的属性匹配时，从而使这种多义性会得到解决。 该示例

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

显示两个属性名为的类`X`和`XAttribute`。 该属性`[X]`不明确，因为它可以为引用`X`或`XAttribute`。 通过使用逐字字符串标识符，在这种极少数情况下指定确切的意图。 该属性`[XAttribute]`不是不明确 (尽管它会很是否有一个名为属性类`XAttributeAttribute`！)。 如果类的声明`X`被删除，则这两个属性引用名为的特性类`XAttribute`，按如下所示：

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

它是相同的实体超过一次使用单用途特性类的编译时错误。 该示例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

因为它尝试使用会导致编译时错误`HelpString`，它是一个单次使用的特性类，一次以上的声明上`Class1`。

一个表达式`E`是*attribute_argument_expression*如果满足所有以下语句：

*  类型`E`特性参数类型 ([属性参数类型](attributes.md#attribute-parameter-types))。
*  在编译时，值`E`可以解析为以下值之一：
   * 常量的值。
   * 一个 `System.Type` 对象。
   * 一维数组*attribute_argument_expression*s。

例如：

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

一个*typeof_expression* ([typeof 运算符](expressions.md#the-typeof-operator)) 用作属性参数表达式可以引用非泛型类型、 封闭式构造的类型或未绑定的泛型类型，但它不能引用开放类型。 这是为了确保可以在编译时解析表达式。

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>特性实例

***特性实例***是表示在运行时的特性的实例。 属性是使用属性类，位置参数，定义和命名参数。 一个特性实例是使用位置和命名参数初始化特性类的实例。

检索一个特性实例涉及编译时和运行时处理，如以下各节中所述。

### <a name="compilation-of-an-attribute"></a>属性的编译

编译*特性*特性类与`T`， *positional_argument_list* `P`并*named_argument_list* `N`，包括以下步骤：

*  请按照用于编译的编译时处理步骤*object_creation_expression*窗体的`new T(P)`。 这些步骤导致编译时错误，或确定实例构造函数`C`上`T`，可以在运行时调用。
*  如果`C`不具有公共可访问性，则会发生编译时错误。
*  每个*named_argument* `Arg`中`N`:
   * 让`Name`进行*标识符*的*named_argument* `Arg`。
   * `Name` 必须标识的非静态读写公共字段或属性上`T`。 如果`T`有无此类字段或属性，则会发生编译时错误。
*  保留的该属性的运行时实例化的以下信息： 特性类`T`，实例构造函数`C`上`T`，则*positional_argument_list* `P`并*named_argument_list* `N`。

### <a name="run-time-retrieval-of-an-attribute-instance"></a>运行时检索的特性实例

编译*特性*产生一个属性类`T`，实例构造函数`C`上`T`、 一个*positional_argument_list* `P`，和一个*named_argument_list* `N`。 提供此信息，一个特性实例，可以检索在运行时使用以下步骤：

*  请按照用于执行运行时处理步骤*object_creation_expression*窗体`new T(P)`，使用的实例构造函数`C`在编译时确定。 这些步骤导致异常，或生成一个实例`O`的`T`。
*  每个*named_argument* `Arg`中`N`，按顺序：
   * 让`Name`进行*标识符*的*named_argument* `Arg`。 如果`Name`不会在标识的非静态公共读写字段或属性`O`，则会引发异常。
   * 让`Value`进行的计算结果*attribute_argument_expression*的`Arg`。
   * 如果`Name`标识的字段上`O`，然后将此字段设置为`Value`。
   * 否则为`Name`标识属性上`O`。 将此属性设置为`Value`。
   * 结果是`O`，特性类的实例`T`已初始化， *positional_argument_list* `P`并*named_argument_list*`N`.

## <a name="reserved-attributes"></a>保留的属性

少量的属性会影响以某种方式的语言。 这些属性包括：

*  `System.AttributeUsageAttribute` ([AttributeUsage 特性](attributes.md#the-attributeusage-attribute))，用于描述了可以在其中使用的特性类的方法。
*  `System.Diagnostics.ConditionalAttribute` ([Conditional 特性](attributes.md#the-conditional-attribute))，用于定义条件的方法。
*  `System.ObsoleteAttribute` ([Obsolete 特性](attributes.md#the-obsolete-attribute))，用于将成员标记为已过时。
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute``System.Runtime.CompilerServices.CallerFilePathAttribute`并`System.Runtime.CompilerServices.CallerMemberNameAttribute`([调用方信息特性](attributes.md#caller-info-attributes))，用于提供有关于可选参数的调用上下文的信息。

### <a name="the-attributeusage-attribute"></a>AttributeUsage 特性

该属性`AttributeUsage`用于描述可以在其中使用特性类的方式。

使用修饰的类`AttributeUsage`属性必须派生自`System.Attribute`，直接或间接。 否则，将发生编译时错误。

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>Conditional 特性

该属性`Conditional`，您可以***条件方法***并***条件特性类***。

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>条件方法

一种方法使用修饰`Conditional`属性是有条件的方法。 `Conditional`属性指示条件通过测试的条件编译符号。 对条件方法的调用是包含或省略具体取决于是否在调用时能够定义此符号。 如果定义了符号，则将包括调用;否则，省略 （包括评估的接收方和调用的参数） 的调用。

条件方法受到以下限制：

*  条件方法必须是中的方法*class_declaration*或*struct_declaration*。 如果会发生编译时错误`Conditional`接口声明中的方法上指定属性。
*  条件方法必须具有返回类型为`void`。
*  条件方法必须未标有`override`修饰符。 可以使用标记，条件方法`virtual`修饰符，但是。 此类方法的重写是隐式条件元素，不能显式标记与`Conditional`属性。
*  条件方法不得接口方法的实现。 否则，将发生编译时错误。

如果条件方法中使用，此外，发生编译时错误*delegate_creation_expression*。 该示例

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

声明`Class1.M`作为条件方法。 `Class2``Test`方法调用此方法。 由于条件编译符号`DEBUG`定义，如果`Class2.Test`是调用，它将调用`M`。 如果符号`DEBUG`有未定义，然后`Class2.Test`不会调用`Class1.M`。

请务必注意，包含或排除条件方法调用受进行调用的条件编译符号。 在示例

文件`class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

文件`class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

文件`class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

类`Class2`并`Class3`每个包含对条件方法的调用`Class1.F`，这是有条件基于是否`DEBUG`定义。 由于的上下文中定义此符号`Class2`但不是`Class3`，在调用`F`中`Class2`包括，则在调用时`F`中`Class3`省略。

使用继承链中的条件方法可以是令人困惑。 对通过条件方法的调用`base`，窗体的`base.M`，可能会有所条件的普通方法调用规则。 在示例

文件`class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

文件`class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

文件`class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` 包含对的调用`M`其基类中定义。 此调用省略，因为基方法是有条件根据的符号是否存在`DEBUG`，这是未定义。 因此，该方法将写入控制台"`Class2.M executed`"仅。 明智地使用*pp_declaration*s 可以消除此类问题。

#### <a name="conditional-attribute-classes"></a>条件属性类

特性类 ([属性类](attributes.md#attribute-classes)) 使用一个或多个修饰`Conditional`属性是***conditional 特性类***。 Conditional 特性类是因此与相关联的条件编译符号中声明其`Conditional`属性。 此示例中：

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

声明`TestAttribute`作为条件属性类与条件编译符号`ALPHA`和`BETA`。

属性规范 ([属性规范](attributes.md#attribute-specification)) 的条件属性都将包含如果一个或多个其关联的条件编译符号定义在规范中，否则该属性省略规范。

请务必注意，包含或排除条件特性类的属性规范受该规范所在位置的条件编译符号。 在示例

文件`test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

文件`class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

文件`class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

类`Class1`并`Class2`是每个使用属性修饰`Test`，这是有条件基于是否`DEBUG`定义。 由于的上下文中定义此符号`Class1`但不是`Class2`的规范`Test`特性，可以在`Class1`包含，时的规范`Test`特性，可以在`Class2`省略。

### <a name="the-obsolete-attribute"></a>Obsolete 特性

该属性`Obsolete`用于标记应不再使用的类型的类型和成员。

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

如果程序使用的类型或成员，使用修饰`Obsolete`属性，则编译器会发出警告或错误。 具体而言，编译器会发出警告如果没有提供任何错误参数，或如果错误参数提供了，并且具有值`false`。 如果指定了错误参数，并且具有值，则编译器会发出错误`true`。

在示例

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

该类`A`用修饰`Obsolete`属性。 每次使用都`A`在`Main`导致一条警告，包括指定的消息，"此类已过时;改为使用类 B。"

### <a name="caller-info-attributes"></a>调用方信息属性

对于如日志记录和报告目的，是有时很有用的函数成员，才能获取有关调用代码的某些编译时信息。 调用方信息特性提供一种方法来以透明方式传递此类信息。

一个可选参数，加上批注与调用方信息特性之一，省略的调用中的相应参数不一定会导致要替换的默认参数值。 相反，如果有关调用上下文的指定的信息不可用，该信息将作为参数值传递。

例如：

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

调用`Log()`不带任何参数将打印行号和文件路径的调用，以及在其中发生在调用的成员的名称。

调用方信息特性上可以出现任意位置，可选参数包括在委托声明中。 但是，特定的调用方信息特性限制对有可以属性的参数的类型，以便将始终从被替换的值为参数类型的隐式转换。

它是错误的同时定义和实现分部方法声明的一部分的参数具有相同的调用方信息属性。 应用仅在定义的一部分的调用方信息特性，而忽略调用方信息特性只能在实现部分中发生。

调用方信息不会影响重载决策。 重载决策特性化的可选参数仍从调用方的源代码中省略，因为它会忽略其他省略可选参数的方式相同忽略这些参数 ([重载决策](expressions.md#overload-resolution))。

在源代码中显式调用函数时，将仅替换调用方信息。 例如隐式父构造函数调用的隐式调用没有源位置，将不替换调用方信息。 此外，动态绑定的调用将不替换调用方信息。 当调用方信息特性化的参数中这种情况下省略时，改为使用参数指定的默认值。

一个例外是查询表达式。 这些地址被视为语法的扩展，并调用它们展开此项可以省略可选参数与调用方信息特性，如果调用方信息将被替换。 使用的位置是在查询子句从生成调用的位置。

如果为给定的参数指定多个调用方信息特性，则它们首选按以下顺序： `CallerLineNumber`， `CallerFilePath`， `CallerMemberName`。

#### <a name="the-callerlinenumber-attribute"></a>CallerLineNumber 属性

`System.Runtime.CompilerServices.CallerLineNumberAttribute`标准的隐式转换时允许可选参数 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 的常量值从`int.MaxValue`到参数的类型。 这可确保最多该值的任何非负行号，可以传递不会出错。

如果从源代码中的某个位置的函数调用中省略可选参数， `CallerLineNumberAttribute`，则作为而不是默认参数值调用的参数使用一个数值，表示该位置的行号。

如果调用跨多个行，选择的行是依赖于实现的。

请注意，可能会影响的行号`#line`指令 ([行指令](lexical-structure.md#line-directives))。

#### <a name="the-callerfilepath-attribute"></a>CallerFilePath 属性

`System.Runtime.CompilerServices.CallerFilePathAttribute`标准的隐式转换时允许可选参数 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 从`string`到参数的类型。

如果从源代码中的某个位置的函数调用中省略可选参数， `CallerFilePathAttribute`，则表示该位置的文件路径的字符串文字用作而不是默认参数值调用的参数。

文件路径的格式是依赖于实现的。

请注意，文件路径可能会受到`#line`指令 ([行指令](lexical-structure.md#line-directives))。

#### <a name="the-callermembername-attribute"></a>CallerMemberName 属性

`System.Runtime.CompilerServices.CallerMemberNameAttribute`标准的隐式转换时允许可选参数 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 从`string`到参数的类型。

如果函数调用的函数成员正文中或在特性中的位置从应用于的函数成员本身或其返回类型、 参数或类型参数在源代码省略了可选参数， `CallerMemberNameAttribute`，则表示该成员的名称的字符串文字用作而不是默认参数值调用的参数。

调用泛型方法中发生的仅对方法名称本身使用，而无需的类型参数列表。

显式接口成员实现代码中发生的调用，只能使用方法名称本身，而无需上述接口限定。

对于属性或事件访问器内发生的调用，使用的成员名称是属性或事件本身。

有关索引器访问器内发生的调用，使用的成员名称是提供的`IndexerNameAttribute`([IndexerName 特性](attributes.md#the-indexername-attribute)) 上的索引器成员，如果存在或默认名称`Item`否则为。

对于实例构造函数、 静态构造函数、 析构函数和运算符的声明中出现该成员的调用使用的名称是依赖于实现的。

## <a name="attributes-for-interoperation"></a>互操作的特性

注意： 此部分是仅适用于 C# 的 Microsoft.NET 实现。

### <a name="interoperation-with-com-and-win32-components"></a>与 COM 和 Win32 组件互操作

.NET 运行时提供了大量的属性，可使 C# 程序可以与使用 COM 和 Win32 Dll 编写的组件进行互操作。 例如，`DllImport`属性可用于`static extern`方法以指示要在 Win32 DLL 中找到该方法的实现。 中找不到这些属性`System.Runtime.InteropServices`命名空间，以及为这些属性的详细的文档位于.NET 运行时文档。

### <a name="interoperation-with-other-net-languages"></a>与其他.NET 语言的互操作

#### <a name="the-indexername-attribute"></a>IndexerName 特性

索引器中使用索引的属性的.NET 实现和.NET 元数据中具有的名称。 如果没有`IndexerName`属性时，将提供索引器，然后是名称`Item`默认情况下使用。 `IndexerName`属性使开发人员将覆盖此默认值并指定一个不同的名称。

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
