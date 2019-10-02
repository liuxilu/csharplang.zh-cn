---
ms.openlocfilehash: 2026fc1bf9d3576b967cbc2e9a670aa44b7eab3a
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704025"
---
# <a name="documentation-comments"></a>文档注释

C#为程序员提供一种机制，以使用包含 XML 文本的特殊注释语法记录其代码。 在源代码文件中，具有特定窗体的注释可用于指示工具从这些注释生成 XML，并将其置于后面。 使用此语法的注释称为***文档注释***。 它们必须紧跟在用户定义的类型（如类、委托或接口）或成员（如字段、事件、属性或方法）之前。 XML 生成工具称为***文档生成器***。 （此生成器可能是，但不一定是C#编译器本身。）文档生成器生成的输出称为 "***文档文件***"。 文档文件用作***文档查看器***的输入;一种用于生成类型信息及其关联文档的某种视觉显示方式的工具。

此规范建议在文档注释中使用一组标记，但不需要使用这些标记，如果需要，还可以使用其他标记，前提是符合格式正确的 XML 的规则。

## <a name="introduction"></a>介绍

具有特殊形式的注释可用于指示工具从这些注释生成 XML，并将其置于之前。 此类注释是以三个斜杠（`///`）开头或以斜杠和两个星号（`/**`）开头的分隔注释。 它们必须紧跟在用户定义的类型（如类、委托或接口）或它们所批注的成员（如字段、事件、属性或方法）之前。 特性部分（[特性规范](attributes.md#attribute-specification)）被视为声明的一部分，因此文档注释必须位于应用到类型或成员的特性之前。

__语法__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

在*single_line_doc_comment*中，如果在与当前*single_line_doc_comment*相邻的每个*single_line_doc_comment*上 @no__t 的第2个字符后面有一个*空格*字符，则该*空格*不在 XML 输出中包含字符。

在分隔的文档注释中，如果第二行中的第一个非空白字符为星号并且具有相同的可选空白字符模式，并且在分隔的文档注释中每一行的开头重复星号字符，然后，XML 输出中不包括重复模式的字符。 此模式可能包含后面和星号字符后面的空白字符。

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

文档注释中的文本必须根据 XML （ https://www.w3.org/TR/REC-xml) 。 如果 XML 格式不正确，则会生成警告，并且文档文件将包含一条注释，指出遇到了错误。

尽管开发人员可以自由地创建自己的一组标记，但建议的[标记](documentation-comments.md#recommended-tags)中定义了一个建议的集。 部分建议标记具有特殊含义：

*  `<param>`标记用于描述参数。 如果使用此类标记，文档生成器必须验证指定的参数是否存在以及文档注释中是否描述了所有参数。 如果此类验证失败，文档生成器会发出警告。
*  `cref` 属性可以附加到任何标记，以提供对代码元素的引用。 文档生成器必须验证此代码元素是否存在。 如果验证失败，文档生成器会发出警告。 查找`cref`特性中描述的名称时，文档生成器必须根据源代码中出现的语句来`using`区分命名空间可见性。 对于泛型代码元素，不能使用常规泛型语法（即 "`List<T>`"），因为它会生成无效的 XML。 可以使用大括号代替方括号（即 "`List{T}`"），也可以使用 XML 转义语法（即 "`List&lt;T&gt;`"）。
*  `<summary>`标记用于文档查看器，用于显示有关类型或成员的其他信息。
*  `<include>`标记包含外部 XML 文件中的信息。

请注意，文档文件不提供有关类型和成员（例如，它不包含任何类型信息）的完整信息。 若要获取有关某个类型或成员的此类信息，文档文件必须与实际类型或成员的反射一起使用。

## <a name="recommended-tags"></a>建议的标记

文档生成器必须接受并处理根据 XML 规则有效的任何标记。 以下标记提供用户文档中的常用功能。 （当然，其他标记是可能的。）


| __符__          | __节__                                            | __目的__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | 设置类似代码的字体中的文本                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | 设置一个或多个源代码或程序输出行 |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | 指示示例                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | 标识方法可以引发的异常           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | 包含来自外部文件的 XML                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | 创建列表或表                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | 允许将结构添加到文本中                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | 描述方法或构造函数的参数       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | 确定某一词为参数名称               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | 记录成员的安全可访问性        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | 描述有关类型的其他信息           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | 描述方法的返回值                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | 指定链接                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | 生成 "另请参阅" 条目                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | 描述类型或类型的成员                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | 描述属性                                    |
| `<typeparam>`    |                                                        | 描述泛型类型参数                      |
| `<typeparamref>` |                                                        | 确定某个单词为类型参数名称          |

### `<c>`

此标记提供一种机制，用于指示说明中的文本片段应设置为特殊字体，如用于代码块的。 对于实际代码行，请使用`<code>` （[`<code>`](documentation-comments.md#code)）。

__语法__

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

此标记用于设置一个或多个源代码或程序输出行，采用某种特殊字体。 对于叙述性的小型代码片段， `<c>`请[`<c>`](documentation-comments.md#c)使用（）。

__语法__

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

此标记允许在注释内使用示例代码来指定如何使用方法或其他库成员。 通常，这也会涉及到标记`<code>` （[`<code>`](documentation-comments.md#code)）的使用。

__语法__

```xml
<example>description</example>
```

__示例：__

有关`<code>`示例[`<code>`](documentation-comments.md#code)，请参见（）。

### `<exception>`

此标记提供了一种方法，用于记录方法可能引发的异常。

__语法__

```xml
<exception cref="member">description</exception>
```

where

* `member`成员的名称。 文档生成器检查给定成员是否存在，并转换`member`为文档文件中的规范元素名称。
* `description`描述引发异常的情况。

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

此标记允许包含源代码文件外部的 XML 文档中的信息。 外部文件必须是格式正确的 XML 文档，并将 XPath 表达式应用于该文档以指定要包含的文档的 XML。 然后，将标记替换为外部文档中所选的 XML。 `<include>`

__语法__

```xml
<include file="filename" path="xpath" />
```

where

* `filename`外部 XML 文件的文件名。 文件名是相对于包含标记的文件进行解释的。
* `xpath`选择外部 XML 文件中的某些 XML 的 XPath 表达式。

__示例：__

如果源代码包含如下所示的声明：

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

外部文件 "文档" 具有以下内容：

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

然后输出相同的文档，就像源代码中所含的一样：

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

此标记用于创建列表或项的表。 它可能包含`<listheader>`用于定义表或定义列表的标题行的块。 （定义表时，只需提供标题`term`中的条目。）

列表中的每一项均使用`<item>`块来指定。 创建定义列表时，必须同时`term`指定`description`和。 但是，对于表、项目符号列表或编号列表，只`description`需指定。

__语法__

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

where

* `term`要定义的术语，其定义在中`description`。
* `description`为项目符号列表或编号列表中的项，或者为的定义`term`。

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

此标记在其他标记内使用， `<summary>`如（[`<remarks>`](documentation-comments.md#remarks)）或`<returns>` （[`<returns>`](documentation-comments.md#returns)），并允许将结构添加到文本中。

__语法__

```xml
<para>content</para>
```

其中`content` ，为段落的文本。

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

此标记用于描述方法、构造函数或索引器的参数。

__语法__

```xml
<param name="name">description</param>
```

where

* `name`参数的名称。
* `description`参数的说明。

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

此标记用于指示字词是参数。 可以采用某种不同的方式处理文档文件以设置此参数的格式。

__语法__

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

此标记允许记录成员的安全可访问性。

__语法__

```xml
<permission cref="member">description</permission>
```

where

* `member`成员的名称。 文档生成器检查给定的代码元素是否存在，并将*成员*转换为文档文件中的规范元素名称。
* `description`是对成员的访问权限的说明。

__示例：__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

此标记用于指定有关类型的额外信息。 （使用`<summary>` （[`<summary>`](documentation-comments.md#summary)）描述类型本身和类型成员。）

__语法__

```xml
<remarks>description</remarks>
```

其中`description`是注释的文本。

__示例：__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

此标记用于描述方法的返回值。

__语法__

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

此标记允许在文本中指定链接。 使用`<seealso>` [（`<seealso>`](documentation-comments.md#seealso)）指示要在 "另请参见" 部分中显示的文本。

__语法__

```xml
<see cref="member"/>
```

其中`member`是成员的名称。 文档生成器检查给定的代码元素是否存在，并在生成的文档文件中将*成员*更改为元素名称。

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

此标记允许为 "另请参阅" 部分生成一个条目。 使用`<see>` [（`<see>`](documentation-comments.md#see)）可指定文本中的链接。

__语法__

```xml
<seealso cref="member"/>
```

其中`member`是成员的名称。 文档生成器检查给定的代码元素是否存在，并在生成的文档文件中将*成员*更改为元素名称。

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

此标记可用于描述类型或类型的成员。 使用`<remarks>` [（`<remarks>`](documentation-comments.md#remarks)）描述类型本身。

__语法__

```xml
<summary>description</summary>
```

其中`description` ，是类型或成员的汇总。

__示例：__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

此标记允许描述属性。

__语法__

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

此标记用于描述类、结构、接口、委托或方法的泛型类型参数。

__语法__

```xml
<typeparam name="name">description</typeparam>
```

`name` 其中`description` ，是类型参数的名称，是其说明。

__示例：__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

此标记用于指示字词为类型参数。 可以对文档文件进行处理，以便以某种不同的方式设置此类型参数的格式。

__语法__

```xml
<typeparamref name="name"/>
```

其中`name` ，为类型参数的名称。

__示例：__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>处理文档文件

文档生成器为源代码中使用文档注释标记的每个元素生成一个 ID 字符串。 此 ID 字符串唯一标识源元素。 文档查看器可以使用 ID 字符串来标识文档应用到的相应元数据/反射项。

文档文件不是源代码的层次结构表示形式;相反，它是一个平面列表，其中包含每个元素的生成的 ID 字符串。

### <a name="id-string-format"></a>ID 字符串格式

文档生成器在生成 ID 字符串时遵循以下规则：

*  字符串不得包含空格。

*  字符串的第一部分通过一个字符后跟一个冒号来标识所记录的成员的种类。 定义以下类型的成员：

   | 字符 | __说明__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Event                                                       |
   | F             | 字段                                                       |
   | M             | 方法（包括构造函数、析构函数和运算符） |
   | N             | 命名空间                                                   |
   | P             | 属性（包括索引器）                               |
   | T             | 类型（如类、委托、枚举、接口和结构） |
   | !             | 错误字符串;其余字符串提供有关错误的信息。 例如，文档生成器为无法解析的链接生成错误信息。 |

*  字符串的第二部分是元素的完全限定名，从命名空间的根开始。 元素的名称、其封闭类型和命名空间用句点分隔。 如果项本身的名称包含句点，则这些名称将替换为`#(U+0023)`个字符。 （假定没有元素在其名称中包含此字符。）
*  对于包含参数的方法和属性，将在参数列表的后面加上括号。 对于没有参数的那些参数，将省略括号。 确保自变量之间用逗号分隔。 每个自变量的编码都与 CLI 签名相同，如下所示：
   *  参数由其文档名称表示，其文档名称基于其完全限定名称，如下所示进行修改：
      * 表示泛型类型的参数包含一个追加`` ` ``的（反撇号）字符，后跟类型参数的数目
      * 具有`out` `@`或`ref`修饰符的参数具有以下其类型名称。 通过值传递的或通过`params`的参数没有特殊的表示法。
      * 作为数组的参数的表示形式`[lowerbound:size, ... , lowerbound:size]`为，其中逗号的数目小于1，并且每个维度的下限和大小（如果已知）以十进制表示。 如果未指定下限或大小，则省略它。 如果省略特定维度的下限和大小， `:`则也将省略。 交错数组由每个级别`[]`一个表示。
      * 具有 void 以外的指针类型的参数使用`*`后面的类型名称表示。 Void 指针使用类型名称`System.Void`表示。
      * 引用类型上定义的泛型类型参数的参数使用`` ` `` （反撇号）字符进行编码，后跟类型参数的从零开始的索引。
      * 使用在方法中定义的泛型类型参数的参数使用双反撇号``` `` ``` ，而不`` ` ``是用于类型的。
      * 引用构造的泛型类型的参数使用泛型类型`{`进行编码，后面跟有一个逗号分隔的类型参数列表， `}`后跟。

### <a name="id-string-examples"></a>ID 字符串示例

下面的示例分别显示C#代码片段，以及从每个支持文档注释的源元素生成的 ID 字符串：

*  类型使用其完全限定名称进行表示，并与一般信息一起使用：

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

*  函数.

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

*  方法.

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

*  事件.

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

   所使用的一元运算符函数名称的完整集合如下所示`op_UnaryPlus`： `op_UnaryNegation`、 `op_LogicalNot` `op_True`、 `op_OnesComplement`、 `op_Increment`、 `op_Decrement`、、和`op_False`。

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

   所使用的二元运算符函数名称的完整集合如下所示`op_Addition`： `op_Subtraction`、 `op_Multiply` `op_BitwiseOr`、 `op_Division`、 `op_Modulus`、 `op_BitwiseAnd` `op_ExclusiveOr` `op_LeftShift` `op_RightShift`、、、、、、`op_Equality`、 、、`op_LessThan`、和`op_GreaterThan`。 `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`

*  转换运算符的尾随 "`~`" 后跟返回类型。

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

### <a name="c-source-code"></a>C#源代码

下面的示例演示了`Point`类的源代码：

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

下面是在给定类`Point`的源代码时由一个文档生成器生成的输出，如下所示：

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
