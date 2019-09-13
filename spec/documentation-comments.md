---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912439"
---
# <a name="documentation-comments"></a><span data-ttu-id="5c1b1-101">文档注释</span><span class="sxs-lookup"><span data-stu-id="5c1b1-101">Documentation comments</span></span>

<span data-ttu-id="5c1b1-102">C#为程序员提供一种机制，以使用包含 XML 文本的特殊注释语法记录其代码。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="5c1b1-103">在源代码文件中，具有特定窗体的注释可用于指示工具从这些注释生成 XML，并将其置于后面。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="5c1b1-104">使用此语法的注释称为***文档注释***。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="5c1b1-105">它们必须紧跟在用户定义的类型（如类、委托或接口）或成员（如字段、事件、属性或方法）之前。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="5c1b1-106">XML 生成工具称为***文档生成器***。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="5c1b1-107">（此生成器可能是，但不一定是C#编译器本身。）文档生成器生成的输出称为 "***文档文件***"。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="5c1b1-108">文档文件用作***文档查看器***的输入;一种用于生成类型信息及其关联文档的某种视觉显示方式的工具。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="5c1b1-109">此规范建议在文档注释中使用一组标记，但不需要使用这些标记，如果需要，还可以使用其他标记，前提是符合格式正确的 XML 的规则。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="5c1b1-110">介绍</span><span class="sxs-lookup"><span data-stu-id="5c1b1-110">Introduction</span></span>

<span data-ttu-id="5c1b1-111">具有特殊形式的注释可用于指示工具从这些注释生成 XML，并将其置于之前。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="5c1b1-112">此类注释是以三个斜杠（`///`）开头或以斜杠和两个星号（`/**`）开头的分隔注释。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="5c1b1-113">它们必须紧跟在用户定义的类型（如类、委托或接口）或它们所批注的成员（如字段、事件、属性或方法）之前。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="5c1b1-114">特性部分（[特性规范](attributes.md#attribute-specification)）被视为声明的一部分，因此文档注释必须位于应用到类型或成员的特性之前。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="5c1b1-115">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="5c1b1-116">在*single_line_doc_comment*中，如果在与当前*single_line_doc_comment*相邻的每`///`个*single_line_doc_comment*上的字符后面有一个*空格*字符，则该XML 输出中不包含空格字符。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="5c1b1-117">在分隔的文档注释中，如果第二行中的第一个非空白字符为星号并且具有相同的可选空白字符模式，并且在分隔的文档注释中每一行的开头重复星号字符，然后，XML 输出中不包括重复模式的字符。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="5c1b1-118">此模式可能包含后面和星号字符后面的空白字符。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="5c1b1-119">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-119">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-120">文档注释中的文本必须根据 XML （ https://www.w3.org/TR/REC-xml) 。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="5c1b1-121">如果 XML 格式不正确，则会生成警告，并且文档文件将包含一条注释，指出遇到了错误。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="5c1b1-122">尽管开发人员可以自由地创建自己的一组标记，但建议的[标记](documentation-comments.md#recommended-tags)中定义了一个建议的集。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="5c1b1-123">部分建议标记具有特殊含义：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="5c1b1-124">`<param>`标记用于描述参数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="5c1b1-125">如果使用此类标记，文档生成器必须验证指定的参数是否存在以及文档注释中是否描述了所有参数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="5c1b1-126">如果此类验证失败，文档生成器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="5c1b1-127">`cref` 属性可以附加到任何标记，以提供对代码元素的引用。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="5c1b1-128">文档生成器必须验证此代码元素是否存在。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="5c1b1-129">如果验证失败，文档生成器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="5c1b1-130">查找`cref`特性中描述的名称时，文档生成器必须根据源代码中出现的语句来`using`区分命名空间可见性。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="5c1b1-131">对于泛型代码元素，不能使用常规泛型语法（即 "`List<T>`"），因为它会生成无效的 XML。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="5c1b1-132">可以使用大括号代替方括号（即 "`List{T}`"），也可以使用 XML 转义语法（即 "`List&lt;T&gt;`"）。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="5c1b1-133">`<summary>`标记用于文档查看器，用于显示有关类型或成员的其他信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="5c1b1-134">`<include>`标记包含外部 XML 文件中的信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="5c1b1-135">请注意，文档文件不提供有关类型和成员（例如，它不包含任何类型信息）的完整信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="5c1b1-136">若要获取有关某个类型或成员的此类信息，文档文件必须与实际类型或成员的反射一起使用。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="5c1b1-137">建议的标记</span><span class="sxs-lookup"><span data-stu-id="5c1b1-137">Recommended tags</span></span>

<span data-ttu-id="5c1b1-138">文档生成器必须接受并处理根据 XML 规则有效的任何标记。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="5c1b1-139">以下标记提供用户文档中的常用功能。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="5c1b1-140">（当然，其他标记是可能的。）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="5c1b1-141">__符__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-141">__Tag__</span></span>          | <span data-ttu-id="5c1b1-142">__节__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-142">__Section__</span></span>                                            | <span data-ttu-id="5c1b1-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="5c1b1-144">设置类似代码的字体中的文本</span><span class="sxs-lookup"><span data-stu-id="5c1b1-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="5c1b1-145">设置一个或多个源代码或程序输出行</span><span class="sxs-lookup"><span data-stu-id="5c1b1-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="5c1b1-146">指示示例</span><span class="sxs-lookup"><span data-stu-id="5c1b1-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="5c1b1-147">标识方法可以引发的异常</span><span class="sxs-lookup"><span data-stu-id="5c1b1-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="5c1b1-148">包含来自外部文件的 XML</span><span class="sxs-lookup"><span data-stu-id="5c1b1-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="5c1b1-149">创建列表或表</span><span class="sxs-lookup"><span data-stu-id="5c1b1-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="5c1b1-150">允许将结构添加到文本中</span><span class="sxs-lookup"><span data-stu-id="5c1b1-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="5c1b1-151">描述方法或构造函数的参数</span><span class="sxs-lookup"><span data-stu-id="5c1b1-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="5c1b1-152">确定某一词为参数名称</span><span class="sxs-lookup"><span data-stu-id="5c1b1-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="5c1b1-153">记录成员的安全可访问性</span><span class="sxs-lookup"><span data-stu-id="5c1b1-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="5c1b1-154">描述有关类型的其他信息</span><span class="sxs-lookup"><span data-stu-id="5c1b1-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="5c1b1-155">描述方法的返回值</span><span class="sxs-lookup"><span data-stu-id="5c1b1-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="5c1b1-156">指定链接</span><span class="sxs-lookup"><span data-stu-id="5c1b1-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="5c1b1-157">生成 "另请参阅" 条目</span><span class="sxs-lookup"><span data-stu-id="5c1b1-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="5c1b1-158">描述类型或类型的成员</span><span class="sxs-lookup"><span data-stu-id="5c1b1-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="5c1b1-159">描述属性</span><span class="sxs-lookup"><span data-stu-id="5c1b1-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="5c1b1-160">描述泛型类型参数</span><span class="sxs-lookup"><span data-stu-id="5c1b1-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="5c1b1-161">确定某个单词为类型参数名称</span><span class="sxs-lookup"><span data-stu-id="5c1b1-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="5c1b1-162">此标记提供一种机制，用于指示说明中的文本片段应设置为特殊字体，如用于代码块的。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="5c1b1-163">对于实际代码行，请使用`<code>` （[`<code>`](documentation-comments.md#code)）。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="5c1b1-164">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="5c1b1-165">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="5c1b1-166">此标记用于设置一个或多个源代码或程序输出行，采用某种特殊字体。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="5c1b1-167">对于叙述性的小型代码片段， `<c>`请[`<c>`](documentation-comments.md#c)使用（）。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="5c1b1-168">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="5c1b1-169">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-169">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-170">此标记允许在注释内使用示例代码来指定如何使用方法或其他库成员。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="5c1b1-171">通常，这也会涉及到标记`<code>` （[`<code>`](documentation-comments.md#code)）的使用。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="5c1b1-172">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="5c1b1-173">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-173">__Example:__</span></span>

<span data-ttu-id="5c1b1-174">有关`<code>`示例[`<code>`](documentation-comments.md#code)，请参见（）。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="5c1b1-175">此标记提供了一种方法，用于记录方法可能引发的异常。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="5c1b1-176">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="5c1b1-177">where</span><span class="sxs-lookup"><span data-stu-id="5c1b1-177">where</span></span>

* <span data-ttu-id="5c1b1-178">`member`成员的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-178">`member` is the name of a member.</span></span> <span data-ttu-id="5c1b1-179">文档生成器检查给定成员是否存在，并转换`member`为文档文件中的规范元素名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="5c1b1-180">`description`描述引发异常的情况。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="5c1b1-181">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-181">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-182">此标记允许包含源代码文件外部的 XML 文档中的信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="5c1b1-183">外部文件必须是格式正确的 XML 文档，并将 XPath 表达式应用于该文档以指定要包含的文档的 XML。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="5c1b1-184">然后，将标记替换为外部文档中所选的 XML。 `<include>`</span><span class="sxs-lookup"><span data-stu-id="5c1b1-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="5c1b1-185">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="5c1b1-186">where</span><span class="sxs-lookup"><span data-stu-id="5c1b1-186">where</span></span>

* <span data-ttu-id="5c1b1-187">`filename`外部 XML 文件的文件名。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="5c1b1-188">文件名是相对于包含标记的文件进行解释的。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="5c1b1-189">`xpath`选择外部 XML 文件中的某些 XML 的 XPath 表达式。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="5c1b1-190">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-190">__Example:__</span></span>

<span data-ttu-id="5c1b1-191">如果源代码包含如下所示的声明：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="5c1b1-192">外部文件 "文档" 具有以下内容：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="5c1b1-193">然后输出相同的文档，就像源代码中所含的一样：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="5c1b1-194">此标记用于创建列表或项的表。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="5c1b1-195">它可能包含`<listheader>`用于定义表或定义列表的标题行的块。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="5c1b1-196">（定义表时，只需提供标题`term`中的条目。）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="5c1b1-197">列表中的每一项均使用`<item>`块来指定。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="5c1b1-198">创建定义列表时，必须同时`term`指定`description`和。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="5c1b1-199">但是，对于表、项目符号列表或编号列表，只`description`需指定。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="5c1b1-200">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-200">__Syntax:__</span></span>

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

<span data-ttu-id="5c1b1-201">where</span><span class="sxs-lookup"><span data-stu-id="5c1b1-201">where</span></span>

* <span data-ttu-id="5c1b1-202">`term`要定义的术语，其定义在中`description`。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="5c1b1-203">`description`为项目符号列表或编号列表中的项，或者为的定义`term`。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="5c1b1-204">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-204">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-205">此标记在其他标记内使用， `<summary>`如（[`<remarks>`](documentation-comments.md#remarks)）或`<returns>` （[`<returns>`](documentation-comments.md#returns)），并允许将结构添加到文本中。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="5c1b1-206">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="5c1b1-207">其中`content` ，为段落的文本。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="5c1b1-208">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-208">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-209">此标记用于描述方法、构造函数或索引器的参数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="5c1b1-210">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="5c1b1-211">where</span><span class="sxs-lookup"><span data-stu-id="5c1b1-211">where</span></span>

* <span data-ttu-id="5c1b1-212">`name`参数的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="5c1b1-213">`description`参数的说明。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="5c1b1-214">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-214">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-215">此标记用于指示字词是参数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="5c1b1-216">可以采用某种不同的方式处理文档文件以设置此参数的格式。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="5c1b1-217">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="5c1b1-218">其中`name`是参数的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="5c1b1-219">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-219">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-220">此标记允许记录成员的安全可访问性。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="5c1b1-221">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="5c1b1-222">where</span><span class="sxs-lookup"><span data-stu-id="5c1b1-222">where</span></span>

* <span data-ttu-id="5c1b1-223">`member`成员的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-223">`member` is the name of a member.</span></span> <span data-ttu-id="5c1b1-224">文档生成器检查给定的代码元素是否存在，并将*成员*转换为文档文件中的规范元素名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="5c1b1-225">`description`是对成员的访问权限的说明。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="5c1b1-226">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="5c1b1-227">此标记用于指定有关类型的额外信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="5c1b1-228">（使用`<summary>` （[`<summary>`](documentation-comments.md#summary)）描述类型本身和类型成员。）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="5c1b1-229">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="5c1b1-230">其中`description`是注释的文本。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="5c1b1-231">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-231">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-232">此标记用于描述方法的返回值。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="5c1b1-233">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="5c1b1-234">其中`description`是返回值的说明。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="5c1b1-235">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="5c1b1-236">此标记允许在文本中指定链接。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="5c1b1-237">使用`<seealso>` [（`<seealso>`](documentation-comments.md#seealso)）指示要在 "另请参见" 部分中显示的文本。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="5c1b1-238">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="5c1b1-239">其中`member`是成员的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-239">where `member` is the name of a member.</span></span> <span data-ttu-id="5c1b1-240">文档生成器检查给定的代码元素是否存在，并在生成的文档文件中将*成员*更改为元素名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="5c1b1-241">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-241">__Example:__</span></span>

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

<span data-ttu-id="5c1b1-242">此标记允许为 "另请参阅" 部分生成一个条目。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="5c1b1-243">使用`<see>` [（`<see>`](documentation-comments.md#see)）可指定文本中的链接。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="5c1b1-244">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="5c1b1-245">其中`member`是成员的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-245">where `member` is the name of a member.</span></span> <span data-ttu-id="5c1b1-246">文档生成器检查给定的代码元素是否存在，并在生成的文档文件中将*成员*更改为元素名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="5c1b1-247">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-247">__Example:__</span></span>

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

此标记可用于描述类型或类型的成员。 <span data-ttu-id="5c1b1-249">使用`<remarks>` [（`<remarks>`](documentation-comments.md#remarks)）描述类型本身。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="5c1b1-250">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="5c1b1-251">其中`description` ，是类型或成员的汇总。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="5c1b1-252">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="5c1b1-253">此标记允许描述属性。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="5c1b1-254">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="5c1b1-255">其中`property description`是属性的说明。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="5c1b1-256">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="5c1b1-257">此标记用于描述类、结构、接口、委托或方法的泛型类型参数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="5c1b1-258">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="5c1b1-259">`name` 其中`description` ，是类型参数的名称，是其说明。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="5c1b1-260">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="5c1b1-261">此标记用于指示字词为类型参数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="5c1b1-262">可以对文档文件进行处理，以便以某种不同的方式设置此类型参数的格式。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="5c1b1-263">__语法__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="5c1b1-264">其中`name` ，为类型参数的名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="5c1b1-265">__示例：__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="5c1b1-266">处理文档文件</span><span class="sxs-lookup"><span data-stu-id="5c1b1-266">Processing the documentation file</span></span>

<span data-ttu-id="5c1b1-267">文档生成器为源代码中使用文档注释标记的每个元素生成一个 ID 字符串。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="5c1b1-268">此 ID 字符串唯一标识源元素。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="5c1b1-269">文档查看器可以使用 ID 字符串来标识文档应用到的相应元数据/反射项。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="5c1b1-270">文档文件不是源代码的层次结构表示形式;相反，它是一个平面列表，其中包含每个元素的生成的 ID 字符串。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="5c1b1-271">ID 字符串格式</span><span class="sxs-lookup"><span data-stu-id="5c1b1-271">ID string format</span></span>

<span data-ttu-id="5c1b1-272">文档生成器在生成 ID 字符串时遵循以下规则：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="5c1b1-273">字符串不得包含空格。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="5c1b1-274">字符串的第一部分通过一个字符后跟一个冒号来标识所记录的成员的种类。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="5c1b1-275">定义以下类型的成员：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="5c1b1-276">字符</span><span class="sxs-lookup"><span data-stu-id="5c1b1-276">__Character__</span></span> | <span data-ttu-id="5c1b1-277">__说明__</span><span class="sxs-lookup"><span data-stu-id="5c1b1-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="5c1b1-278">E</span><span class="sxs-lookup"><span data-stu-id="5c1b1-278">E</span></span>             | <span data-ttu-id="5c1b1-279">Event</span><span class="sxs-lookup"><span data-stu-id="5c1b1-279">Event</span></span>                                                       |
   | <span data-ttu-id="5c1b1-280">F</span><span class="sxs-lookup"><span data-stu-id="5c1b1-280">F</span></span>             | <span data-ttu-id="5c1b1-281">字段</span><span class="sxs-lookup"><span data-stu-id="5c1b1-281">Field</span></span>                                                       |
   | <span data-ttu-id="5c1b1-282">M</span><span class="sxs-lookup"><span data-stu-id="5c1b1-282">M</span></span>             | <span data-ttu-id="5c1b1-283">方法（包括构造函数、析构函数和运算符）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="5c1b1-284">N</span><span class="sxs-lookup"><span data-stu-id="5c1b1-284">N</span></span>             | <span data-ttu-id="5c1b1-285">命名空间</span><span class="sxs-lookup"><span data-stu-id="5c1b1-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="5c1b1-286">P</span><span class="sxs-lookup"><span data-stu-id="5c1b1-286">P</span></span>             | <span data-ttu-id="5c1b1-287">属性（包括索引器）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="5c1b1-288">T</span><span class="sxs-lookup"><span data-stu-id="5c1b1-288">T</span></span>             | <span data-ttu-id="5c1b1-289">类型（如类、委托、枚举、接口和结构）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="5c1b1-290">!</span><span class="sxs-lookup"><span data-stu-id="5c1b1-290">!</span></span>             | <span data-ttu-id="5c1b1-291">错误字符串;其余字符串提供有关错误的信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="5c1b1-292">例如，文档生成器为无法解析的链接生成错误信息。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="5c1b1-293">字符串的第二部分是元素的完全限定名，从命名空间的根开始。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="5c1b1-294">元素的名称、其封闭类型和命名空间用句点分隔。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="5c1b1-295">如果项本身的名称包含句点，则这些名称将替换为`#(U+0023)`个字符。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="5c1b1-296">（假定没有元素在其名称中包含此字符。）</span><span class="sxs-lookup"><span data-stu-id="5c1b1-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="5c1b1-297">对于包含参数的方法和属性，将在参数列表的后面加上括号。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="5c1b1-298">对于没有参数的那些参数，将省略括号。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="5c1b1-299">确保自变量之间用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-299">The arguments are separated by commas.</span></span> <span data-ttu-id="5c1b1-300">每个自变量的编码都与 CLI 签名相同，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="5c1b1-301">参数由其文档名称表示，其文档名称基于其完全限定名称，如下所示进行修改：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="5c1b1-302">表示泛型类型的参数包含一个追加`` ` ``的（反撇号）字符，后跟类型参数的数目</span><span class="sxs-lookup"><span data-stu-id="5c1b1-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="5c1b1-303">具有`out` `@`或`ref`修饰符的参数具有以下其类型名称。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="5c1b1-304">通过值传递的或通过`params`的参数没有特殊的表示法。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="5c1b1-305">作为数组的参数的表示形式`[lowerbound:size, ... , lowerbound:size]`为，其中逗号的数目小于1，并且每个维度的下限和大小（如果已知）以十进制表示。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="5c1b1-306">如果未指定下限或大小，则省略它。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="5c1b1-307">如果省略特定维度的下限和大小， `:`则也将省略。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="5c1b1-308">交错数组由每个级别`[]`一个表示。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="5c1b1-309">具有 void 以外的指针类型的参数使用`*`后面的类型名称表示。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="5c1b1-310">Void 指针使用类型名称`System.Void`表示。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="5c1b1-311">引用类型上定义的泛型类型参数的参数使用`` ` `` （反撇号）字符进行编码，后跟类型参数的从零开始的索引。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="5c1b1-312">使用在方法中定义的泛型类型参数的参数使用双反撇号``` `` ``` ，而不`` ` ``是用于类型的。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="5c1b1-313">引用构造的泛型类型的参数使用泛型类型`{`进行编码，后面跟有一个逗号分隔的类型参数列表， `}`后跟。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="5c1b1-314">ID 字符串示例</span><span class="sxs-lookup"><span data-stu-id="5c1b1-314">ID string examples</span></span>

<span data-ttu-id="5c1b1-315">下面的示例分别显示C#代码片段，以及从每个支持文档注释的源元素生成的 ID 字符串：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="5c1b1-316">类型使用其完全限定名称进行表示，并与一般信息一起使用：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="5c1b1-317">字段由其完全限定名称表示：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="5c1b1-318">构造函数。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-318">Constructors.</span></span>

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

*  <span data-ttu-id="5c1b1-319">函数.</span><span class="sxs-lookup"><span data-stu-id="5c1b1-319">Destructors.</span></span>

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

*  <span data-ttu-id="5c1b1-320">方法.</span><span class="sxs-lookup"><span data-stu-id="5c1b1-320">Methods.</span></span>

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

*  <span data-ttu-id="5c1b1-321">属性和索引器。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="5c1b1-322">事件.</span><span class="sxs-lookup"><span data-stu-id="5c1b1-322">Events.</span></span>

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

*  <span data-ttu-id="5c1b1-323">一元运算符。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-323">Unary operators.</span></span>

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

   <span data-ttu-id="5c1b1-324">所使用的一元运算符函数名称的完整集合如下所示`op_UnaryPlus`： `op_UnaryNegation`、 `op_LogicalNot` `op_True`、 `op_OnesComplement`、 `op_Increment`、 `op_Decrement`、、和`op_False`。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="5c1b1-325">二元运算符。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-325">Binary operators.</span></span>

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

   <span data-ttu-id="5c1b1-326">所使用的二元运算符函数名称的完整集合如下所示`op_Addition`： `op_Subtraction`、 `op_Multiply` `op_BitwiseOr`、 `op_Division`、 `op_Modulus`、 `op_BitwiseAnd` `op_ExclusiveOr` `op_LeftShift` `op_RightShift`、、、、、、`op_Equality`、 、、`op_LessThan`、和`op_GreaterThan`。 `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`</span><span class="sxs-lookup"><span data-stu-id="5c1b1-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="5c1b1-327">转换运算符的尾随 "`~`" 后跟返回类型。</span><span class="sxs-lookup"><span data-stu-id="5c1b1-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="5c1b1-328">示例</span><span class="sxs-lookup"><span data-stu-id="5c1b1-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="5c1b1-329">C#源代码</span><span class="sxs-lookup"><span data-stu-id="5c1b1-329">C# source code</span></span>

<span data-ttu-id="5c1b1-330">下面的示例演示了`Point`类的源代码：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="5c1b1-331">生成的 XML</span><span class="sxs-lookup"><span data-stu-id="5c1b1-331">Resulting XML</span></span>

<span data-ttu-id="5c1b1-332">下面是在给定类`Point`的源代码时由一个文档生成器生成的输出，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c1b1-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
