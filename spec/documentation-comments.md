# <a name="documentation-comments"></a>文档注释

C# 提供了编程人员来记录其代码中使用一种特殊注释语法包含 XML 文本的一种机制。 在源代码文件中，具有特定格式的注释可以用于直接从这些注释和这些注释后面的源元素生成 XML 的工具。 注释使用这种语法称为***文档注释***。 它们必须紧跟 （如类、 委托或接口） 的用户定义的类型或成员 （例如字段、 事件、 属性或方法）。 调用 XML 生成工具***文档生成器***。 （此生成器可能是，但不是需要 C# 编译器本身。）调用文档生成器生成的输出***文档文件***。 文档文件用作输入***文档查看器***; 一个用于生成某种形式的类型信息和其关联的文档的可视显示的工具。

此规范建议一组标记用于文档注释，但使用这些标记不是必需的并且其他标记可能用作必要时，应遵循长时间的格式正确的 XML 规则。

## <a name="introduction"></a>介绍

具有一种特殊形式的注释可以用于直接从这些注释和这些注释后面的源元素生成 XML 的工具。 此类注释是以三个斜杠开头的单行注释 (`///`)，或带分隔符的注释以正斜杠和两个星号开头 (`/**`)。 它们必须紧跟 （如类、 委托或接口） 的用户定义的类型或其注释 （例如字段、 事件、 属性或方法） 的成员。 属性部分 ([属性规范](attributes.md#attribute-specification)) 被视为声明的一部分，因此文档注释必须位于之前应用于类型或成员的特性。

__语法：__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

在中*single_line_doc_comment*，如果没有*空白*之后的字符`///`上的每个字符*single_line_doc_comment*相邻的 s与当前*single_line_doc_comment*，然后该*空白*字符未包含在 XML 输出。

在分隔的文档注释，如果在第二行的第一个非空白字符是一个星号和相同的可选空格字符和星号字符模式重复的每个行中分隔的文档注释开头然后重复模式的字符不包含在 XML 输出中。 该模式可能包括空白字符后，以及星号字符之前。

__示例：__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

文档注释中的文本必须格式正确的 XML 的规则根据 (http://www.w3.org/TR/REC-xml)。 如果 XML 不符合标准格式，则会生成警告，并且文档文件将包含条注释，指出遇到错误。

尽管开发人员可以随意创建其自己的标记集，但中定义一组推荐[建议标记](documentation-comments.md#recommended-tags)。 部分建议标记具有特殊含义：

*  `<param>`标记用于描述参数。 如果使用这样的标记，则文档生成器必须验证存在指定的参数和文档注释中描述了所有参数。 如果此类验证失败，文档生成器会发出警告。
*  `cref` 属性可以附加到任何标记，以提供对代码元素的引用。 文档生成器必须验证此代码元素存在。 如果验证失败，文档生成器会发出警告。 当查找名称中所述`cref`属性，文档生成器必须遵循的命名空间可见性根据`using`语句出现在源代码中。 代码元素的泛型类型，正常的常规语法 (即"`List<T>`") 无法使用，因为它会生成无效的 XML。 可以使用大括号来代替方括号 (即"`List{T}`")，也可以使用 XML 转义语法 (即"`List&lt;T&gt;`")。
*  `<summary>`标记应使用文档查看器以显示有关类型或成员的其他信息。
*  `<include>`标记包括从外部 XML 文件的信息。

请仔细注意文档文件不提供类型和成员的完整信息 （例如，它不包含任何类型信息）。 若要获取此类信息的类型或成员，必须与实际类型或成员上反射结合使用文档文件。

## <a name="recommended-tags"></a>建议的标记

文档生成器必须接受并处理根据 XML 的规则是有效的任何标记。 以下标记提供用户文档中的常用的功能。 （当然，其他标记是可能的。）


| __标记__          | __节__                                            | __目的__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | 在类似于代码的字体中设置文本                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | 将一个或多个源的代码或程序输出行设置 |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | 指示一个示例                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | 标识一个方法可能会引发的异常           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | 包括从外部文件的 XML                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | 创建列表或表                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | 允许结构添加到文本                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | 描述方法或构造函数的参数       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | 标识一个字为参数名称               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | 文档成员的安全可访问性        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | 描述有关类型的其他信息           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | 描述一种方法的返回值                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | 指定的链接                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | 生成另请参阅项                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | 描述类型的成员                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | 描述的属性                                    |
| `<typeparam>`    |                                                        | 描述泛型类型参数                      |
| `<typeparamref>` |                                                        | 标识一个字为类型参数名称          |

### `<c>`

此标记提供了一种机制，以指示应如的代码块使用特殊字体中设置的说明中的文本段落。 对于实际代码行，请使用`<code>`([`<code>`](documentation-comments.md#code))。

__语法：__

```xml
<c>text</c>
```

__示例：__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

此标记用于为某种特殊字体设置的源的代码或程序输出的一个或多个行。 对于小型代码片段中说明，请使用`<c>`([`<c>`](documentation-comments.md#c))。

__语法：__

```xml
<code>source code or program output</code>
```

__示例：__

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

此标记允许注释，以指定如何可能使用的方法或其他库成员中的示例代码。 通常，这通常需要使用标记`<code>`([`<code>`](documentation-comments.md#code)) 以及。

__语法：__

```xml
<example>description</example>
```

__示例：__

请参阅`<code>`([`<code>`](documentation-comments.md#code)) 有关的示例。

### `<exception>`

此标记提供了一种方法来记录的方法可能引发的异常。

__语法：__

```xml
<exception cref="member">description</exception>
```

其中

* `member` 是的名称。 文档生成器检查是否存在给定的成员，并将转换`member`到文档文件中的规范的元素名称。
* `description` 是在其中引发异常的情况的说明。

__示例：__

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

此标记，包括从外部源的代码文件的 XML 文档的信息。 外部文件必须是格式正确的 XML 文档，并且 XPath 表达式应用于指定的哪些 XML 从该文档包含该文档。 `<include>`标记然后替换从外部文档中选定的 XML。

__语法：__

```
<include file="filename" path="xpath" />
```

其中

* `filename` 是外部 XML 文件的文件名。 相对于包含包含标记文件解释的文件的名称。
* `xpath` 是选择部分的 XML 的外部 XML 文件中的 XPath 表达式。

__示例：__

如果源代码包含了如下声明：

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

并"docs.xml"的外部文件具有以下内容：

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

然后同一文档是输出，如同的源代码包含：

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

此标记用于创建列表或项的表。 它可能包含`<listheader>`块来定义表或定义列表的标题行。 (定义的表，只有一个条目时`term`需要提供标题中。)

使用指定列表中的每个项`<item>`块。 当创建定义列表，同时`term`和`description`必须指定。 但是，对于表、 项目符号列表或编号的列表，仅`description`需指定。

__语法：__

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

其中

* `term` 是的术语来定义，其定义位于`description`。
* `description` 是一个项目符号或编号的列表中的项，或者的定义是`term`。

__示例：__

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

此标记是用于其他标记内，例如`<summary>`([`<remark>`](documentation-comments.md#remark)) 或`<returns>`([`<returns>`](documentation-comments.md#returns))，并允许结构添加到文本。

__语法：__

```xml
<para>content</para>
```

其中`content`是段落的文本。

__示例：__

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

此标记用于描述方法、 构造函数或索引器的参数。

__语法：__

```xml
<param name="name">description</param>
```

其中

* `name` 为参数的名称。
* `description` 是参数的说明。

__示例：__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

此标记用于指示一个单词是一个参数。 可以处理文档文件以设置此参数的格式以不同的方式。

__语法：__

```xml
<paramref name="name"/>
```

其中`name`是参数的名称。

__示例：__

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

此标记允许成员要记录的安全可访问性。

__语法：__

```xml
<permission cref="member">description</permission>
```

其中

* `member` 是的名称。 文档生成器检查是否存在给定的代码元素，并将转换*成员*到文档文件中的规范的元素名称。
* `description` 是对该成员的访问权限的说明。

__示例：__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

此标记用于指定有关类型的额外信息。 (使用`<summary>`([`<summary>`](documentation-comments.md#summary)) 来描述类型本身及其类型的成员。)

__语法：__

```xml
<remark>description</remark>
```

其中`description`是注释的文本。

__示例：__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

此标记用于描述一种方法的返回值。

__语法：__

```xml
<returns>description</returns>
```

其中`description`是返回值的说明。

__示例：__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

此标记允许用于在文本内指定的链接。 使用`<seealso>`([`<seealso>`](documentation-comments.md#seealso)) 以指示要在另请参见部分中显示的文本。

__语法：__

```xml
<see cref="member"/>
```

其中`member`是成员的名称。 文档生成器检查是否存在给定的代码元素，并更改*成员*到生成的文档文件中的元素名称。

__示例：__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

此标记允许另请参阅部分生成的项。 使用`<see>`([`<see>`](documentation-comments.md#see)) 来指定从文本中的链接。

__语法：__

```xml
<seealso cref="member"/>
```

其中`member`是成员的名称。 文档生成器检查是否存在给定的代码元素，并更改*成员*到生成的文档文件中的元素名称。

__示例：__

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

此标记可用于描述类型的成员。 使用`<remark>`([`<remark>`](documentation-comments.md#remark)) 来描述的类型本身。

__语法：__

```xml
<summary>description</summary>
```

其中`description`是类型或成员的摘要。

__示例：__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

该标记用于描述的属性。

__语法：__

```xml
<value>property description</value>
```

其中`property description`是属性的说明。

__示例：__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

此标记用于描述类、 结构、 接口、 委托或方法的泛型类型参数。

__语法：__

```xml
<typeparam name="name">description</typeparam>
```

其中`name`是类型参数的名称和`description`是其说明。

__示例：__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

此标记用于指示一个单词是一个类型参数。 可以处理文档文件以设置此类型参数的格式以不同的方式。

__语法：__

```xml
<typeparamref name="name"/>
```

其中`name`是类型参数的名称。

__示例：__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>处理文档文件

文档生成器生成的标记为文档注释的源代码中的每个元素的 ID 字符串。 此 ID 字符串唯一标识源元素。 文档查看器可以使用 ID 字符串来标识相应的文档适用的元数据/反射项目。

文档文件不是源代码的分层表示形式;相反，它是生成的 ID 字符串的每个元素的平面列表。

### <a name="id-string-format"></a>ID 字符串格式

文档生成器生成的 ID 字符串时遵循下列规则：

*  字符串不得包含空格。

*  字符串的第一部分标识成员记录，通过单个跟一个冒号字符的类型。 定义以下类型的成员：

   | 字符 | __说明__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | 事件                                                       |
   | F             | 字段                                                       |
   | M             | 方法 （包括构造函数、 析构函数和运算符） |
   | N             | 命名空间                                                   |
   | P             | 属性 （包括索引器）                               |
   | T             | 类型 （例如类、 委托、 枚举、 接口和结构） |
   | !             | 错误字符串;字符串的其余部分提供了有关错误的信息。 例如，文档生成器生成无法解析的链接的错误信息。 |

*  字符串的第二部分是元素的命名空间的根目录开始的完全限定的名称。 用句点分隔的元素，其包含的类型和命名空间名称。 如果项本身的名称包含句点，它们被替代`#(U+0023)`字符。 （假定没有任何元素的名称中包含此字符。）
*  对于方法和属性使用的参数，参数列表如下所示，括在括号中。 对于不带参数，则省略括号。 确保自变量之间用逗号分隔。 每个自变量的编码相同的 CLI 签名，如下所示：
   *  自变量都基于其完全限定的名称，按如下所示修改其文档名称是：
      * 表示泛型类型参数具有追加`` ` ``（反引号） 字符后跟类型参数的数目
      * 参数具有`out`或`ref`修饰符有`@`按照其类型名称。 按值或通过传递参数`params`没有特殊表示法。
      * 参数是数组表示为`[lowerbound:size, ... , lowerbound:size]`其中逗号的数量是减一，排名的下限和大小的每个维度，以十进制表示如果已知。 如果未指定下限或大小，则省略它。 如果省略的下限和某个特定维度的大小，`:`也会省略。 交错的数组由一个`[]`每个级别。
      * 具有非 void 指针类型的参数表示使用`*`类型名称。 使用的类型名称表示的 void 指针`System.Void`。
      * 使用引用类型上定义的泛型类型参数的参数进行编码`` ` ``（反引号） 字符后跟类型参数的从零开始的索引。
      * 使用泛型类型参数在方法中定义的参数使用双反引号``` `` ```而不是`` ` ``用于类型。
      * 使用泛型类型后, 跟指构造的泛型类型的参数进行编码`{`后, 跟以逗号分隔列表的类型参数，然后是`}`。

### <a name="id-string-examples"></a>ID 字符串示例

下面的示例分别演示 C# 代码，以及从支持的文档注释每个源元素生成的 ID 字符串的片段：

*  类型表示使用其完全限定的名称，增加泛型信息：

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  字段由其完全限定名称表示：

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  构造函数。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  析构函数。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  方法。

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  属性和索引器。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  事件。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  一元运算符。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   使用一元运算符函数名称的完整集合是按如下所示： `op_UnaryPlus`， `op_UnaryNegation`， `op_LogicalNot`， `op_OnesComplement`， `op_Increment`， `op_Decrement`， `op_True`，和`op_False`。

*  二元运算符。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   使用二元运算符函数名称的完整集合是按如下所示： `op_Addition`， `op_Subtraction`， `op_Multiply`， `op_Division`， `op_Modulus`， `op_BitwiseAnd`， `op_BitwiseOr`， `op_ExclusiveOr`， `op_LeftShift`， `op_RightShift`，`op_Equality`， `op_Inequality`， `op_LessThan`， `op_LessThanOrEqual`， `op_GreaterThan`，并`op_GreaterThanOrEqual`。

*  转换运算符具有一个尾随"`~`"跟的返回类型。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a>示例

### <a name="c-source-code"></a>C# 源代码

下面的示例显示了源代码`Point`类：

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a>生成的 XML

下面是一个文档生成器当给定类的源代码时生成的输出`Point`，上面所示：

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
