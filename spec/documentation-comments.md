---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488915"
---
# <a name="documentation-comments"></a><span data-ttu-id="6592c-101">文档注释</span><span class="sxs-lookup"><span data-stu-id="6592c-101">Documentation comments</span></span>

<span data-ttu-id="6592c-102">C# 提供了编程人员来记录其代码中使用一种特殊注释语法包含 XML 文本的一种机制。</span><span class="sxs-lookup"><span data-stu-id="6592c-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="6592c-103">在源代码文件中，具有特定格式的注释可以用于直接从这些注释和这些注释后面的源元素生成 XML 的工具。</span><span class="sxs-lookup"><span data-stu-id="6592c-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="6592c-104">注释使用这种语法称为***文档注释***。</span><span class="sxs-lookup"><span data-stu-id="6592c-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="6592c-105">它们必须紧跟 （如类、 委托或接口） 的用户定义的类型或成员 （例如字段、 事件、 属性或方法）。</span><span class="sxs-lookup"><span data-stu-id="6592c-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="6592c-106">调用 XML 生成工具***文档生成器***。</span><span class="sxs-lookup"><span data-stu-id="6592c-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="6592c-107">（此生成器可能是，但不是需要 C# 编译器本身。）调用文档生成器生成的输出***文档文件***。</span><span class="sxs-lookup"><span data-stu-id="6592c-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="6592c-108">文档文件用作输入***文档查看器***; 一个用于生成某种形式的类型信息和其关联的文档的可视显示的工具。</span><span class="sxs-lookup"><span data-stu-id="6592c-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="6592c-109">此规范建议一组标记用于文档注释，但使用这些标记不是必需的并且其他标记可能用作必要时，应遵循长时间的格式正确的 XML 规则。</span><span class="sxs-lookup"><span data-stu-id="6592c-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="6592c-110">介绍</span><span class="sxs-lookup"><span data-stu-id="6592c-110">Introduction</span></span>

<span data-ttu-id="6592c-111">具有一种特殊形式的注释可以用于直接从这些注释和这些注释后面的源元素生成 XML 的工具。</span><span class="sxs-lookup"><span data-stu-id="6592c-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="6592c-112">此类注释是以三个斜杠开头的单行注释 (`///`)，或带分隔符的注释以正斜杠和两个星号开头 (`/**`)。</span><span class="sxs-lookup"><span data-stu-id="6592c-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="6592c-113">它们必须紧跟 （如类、 委托或接口） 的用户定义的类型或其注释 （例如字段、 事件、 属性或方法） 的成员。</span><span class="sxs-lookup"><span data-stu-id="6592c-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="6592c-114">属性部分 ([属性规范](attributes.md#attribute-specification)) 被视为声明的一部分，因此文档注释必须位于之前应用于类型或成员的特性。</span><span class="sxs-lookup"><span data-stu-id="6592c-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="6592c-115">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="6592c-116">在中*single_line_doc_comment*，如果没有*空白*之后的字符`///`上的每个字符*single_line_doc_comment*相邻的 s与当前*single_line_doc_comment*，然后该*空白*字符未包含在 XML 输出。</span><span class="sxs-lookup"><span data-stu-id="6592c-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="6592c-117">在分隔的文档注释，如果在第二行的第一个非空白字符是一个星号和相同的可选空格字符和星号字符模式重复的每个行中分隔的文档注释开头然后重复模式的字符不包含在 XML 输出中。</span><span class="sxs-lookup"><span data-stu-id="6592c-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="6592c-118">该模式可能包括空白字符后，以及星号字符之前。</span><span class="sxs-lookup"><span data-stu-id="6592c-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="6592c-119">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-119">__Example:__</span></span>

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

<span data-ttu-id="6592c-120">文档注释中的文本必须格式正确的 XML 的规则根据 (https://www.w3.org/TR/REC-xml)。</span><span class="sxs-lookup"><span data-stu-id="6592c-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="6592c-121">如果 XML 不符合标准格式，则会生成警告，并且文档文件将包含条注释，指出遇到错误。</span><span class="sxs-lookup"><span data-stu-id="6592c-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="6592c-122">尽管开发人员可以随意创建其自己的标记集，但中定义一组推荐[建议标记](documentation-comments.md#recommended-tags)。</span><span class="sxs-lookup"><span data-stu-id="6592c-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="6592c-123">部分建议标记具有特殊含义：</span><span class="sxs-lookup"><span data-stu-id="6592c-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="6592c-124">`<param>`标记用于描述参数。</span><span class="sxs-lookup"><span data-stu-id="6592c-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="6592c-125">如果使用这样的标记，则文档生成器必须验证存在指定的参数和文档注释中描述了所有参数。</span><span class="sxs-lookup"><span data-stu-id="6592c-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="6592c-126">如果此类验证失败，文档生成器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="6592c-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="6592c-127">`cref` 属性可以附加到任何标记，以提供对代码元素的引用。</span><span class="sxs-lookup"><span data-stu-id="6592c-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="6592c-128">文档生成器必须验证此代码元素存在。</span><span class="sxs-lookup"><span data-stu-id="6592c-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="6592c-129">如果验证失败，文档生成器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="6592c-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="6592c-130">当查找名称中所述`cref`属性，文档生成器必须遵循的命名空间可见性根据`using`语句出现在源代码中。</span><span class="sxs-lookup"><span data-stu-id="6592c-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="6592c-131">代码元素的泛型类型，正常的常规语法 (即，"`List<T>`") 无法使用，因为它会生成无效的 XML。</span><span class="sxs-lookup"><span data-stu-id="6592c-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="6592c-132">可以使用大括号来代替方括号 (即，"`List{T}`")，也可以使用 XML 转义语法 (即"`List&lt;T&gt;`")。</span><span class="sxs-lookup"><span data-stu-id="6592c-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="6592c-133">`<summary>`标记应使用文档查看器以显示有关类型或成员的其他信息。</span><span class="sxs-lookup"><span data-stu-id="6592c-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="6592c-134">`<include>`标记包括从外部 XML 文件的信息。</span><span class="sxs-lookup"><span data-stu-id="6592c-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="6592c-135">请仔细注意文档文件不提供类型和成员的完整信息 （例如，它不包含任何类型信息）。</span><span class="sxs-lookup"><span data-stu-id="6592c-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="6592c-136">若要获取此类信息的类型或成员，必须与实际类型或成员上反射结合使用文档文件。</span><span class="sxs-lookup"><span data-stu-id="6592c-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="6592c-137">建议的标记</span><span class="sxs-lookup"><span data-stu-id="6592c-137">Recommended tags</span></span>

<span data-ttu-id="6592c-138">文档生成器必须接受并处理根据 XML 的规则是有效的任何标记。</span><span class="sxs-lookup"><span data-stu-id="6592c-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="6592c-139">以下标记提供用户文档中的常用的功能。</span><span class="sxs-lookup"><span data-stu-id="6592c-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="6592c-140">（当然，其他标记是可能的。）</span><span class="sxs-lookup"><span data-stu-id="6592c-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="6592c-141">__Tag__</span><span class="sxs-lookup"><span data-stu-id="6592c-141">__Tag__</span></span>          | <span data-ttu-id="6592c-142">__节__</span><span class="sxs-lookup"><span data-stu-id="6592c-142">__Section__</span></span>                                            | <span data-ttu-id="6592c-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="6592c-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="6592c-144">在类似于代码的字体中设置文本</span><span class="sxs-lookup"><span data-stu-id="6592c-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="6592c-145">将一个或多个源的代码或程序输出行设置</span><span class="sxs-lookup"><span data-stu-id="6592c-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="6592c-146">指示一个示例</span><span class="sxs-lookup"><span data-stu-id="6592c-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="6592c-147">标识一个方法可能会引发的异常</span><span class="sxs-lookup"><span data-stu-id="6592c-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="6592c-148">包括从外部文件的 XML</span><span class="sxs-lookup"><span data-stu-id="6592c-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="6592c-149">创建列表或表</span><span class="sxs-lookup"><span data-stu-id="6592c-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="6592c-150">允许结构添加到文本</span><span class="sxs-lookup"><span data-stu-id="6592c-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="6592c-151">描述方法或构造函数的参数</span><span class="sxs-lookup"><span data-stu-id="6592c-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="6592c-152">标识一个字为参数名称</span><span class="sxs-lookup"><span data-stu-id="6592c-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="6592c-153">文档成员的安全可访问性</span><span class="sxs-lookup"><span data-stu-id="6592c-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="6592c-154">描述有关类型的其他信息</span><span class="sxs-lookup"><span data-stu-id="6592c-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="6592c-155">描述一种方法的返回值</span><span class="sxs-lookup"><span data-stu-id="6592c-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="6592c-156">指定的链接</span><span class="sxs-lookup"><span data-stu-id="6592c-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="6592c-157">生成另请参阅项</span><span class="sxs-lookup"><span data-stu-id="6592c-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="6592c-158">描述类型的成员</span><span class="sxs-lookup"><span data-stu-id="6592c-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="6592c-159">描述的属性</span><span class="sxs-lookup"><span data-stu-id="6592c-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="6592c-160">描述泛型类型参数</span><span class="sxs-lookup"><span data-stu-id="6592c-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="6592c-161">标识一个字为类型参数名称</span><span class="sxs-lookup"><span data-stu-id="6592c-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="6592c-162">此标记提供了一种机制，以指示应如的代码块使用特殊字体中设置的说明中的文本段落。</span><span class="sxs-lookup"><span data-stu-id="6592c-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="6592c-163">对于实际代码行，请使用`<code>`([`<code>`](documentation-comments.md#code))。</span><span class="sxs-lookup"><span data-stu-id="6592c-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="6592c-164">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="6592c-165">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="6592c-166">此标记用于为某种特殊字体设置的源的代码或程序输出的一个或多个行。</span><span class="sxs-lookup"><span data-stu-id="6592c-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="6592c-167">对于小型代码片段中说明，请使用`<c>`([`<c>`](documentation-comments.md#c))。</span><span class="sxs-lookup"><span data-stu-id="6592c-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="6592c-168">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="6592c-169">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-169">__Example:__</span></span>

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

<span data-ttu-id="6592c-170">此标记允许注释，以指定如何可能使用的方法或其他库成员中的示例代码。</span><span class="sxs-lookup"><span data-stu-id="6592c-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="6592c-171">通常，这通常需要使用标记`<code>`([`<code>`](documentation-comments.md#code)) 以及。</span><span class="sxs-lookup"><span data-stu-id="6592c-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="6592c-172">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="6592c-173">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-173">__Example:__</span></span>

<span data-ttu-id="6592c-174">请参阅`<code>`([`<code>`](documentation-comments.md#code)) 有关的示例。</span><span class="sxs-lookup"><span data-stu-id="6592c-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="6592c-175">此标记提供了一种方法来记录的方法可能引发的异常。</span><span class="sxs-lookup"><span data-stu-id="6592c-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="6592c-176">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="6592c-177">其中</span><span class="sxs-lookup"><span data-stu-id="6592c-177">where</span></span>

* <span data-ttu-id="6592c-178">`member` 是的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-178">`member` is the name of a member.</span></span> <span data-ttu-id="6592c-179">文档生成器检查是否存在给定的成员，并将转换`member`到文档文件中的规范的元素名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="6592c-180">`description` 是在其中引发异常的情况的说明。</span><span class="sxs-lookup"><span data-stu-id="6592c-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="6592c-181">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-181">__Example:__</span></span>

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

<span data-ttu-id="6592c-182">此标记，包括从外部源的代码文件的 XML 文档的信息。</span><span class="sxs-lookup"><span data-stu-id="6592c-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="6592c-183">外部文件必须是格式正确的 XML 文档，并且 XPath 表达式应用于指定的哪些 XML 从该文档包含该文档。</span><span class="sxs-lookup"><span data-stu-id="6592c-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="6592c-184">`<include>`标记然后替换从外部文档中选定的 XML。</span><span class="sxs-lookup"><span data-stu-id="6592c-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="6592c-185">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="6592c-186">其中</span><span class="sxs-lookup"><span data-stu-id="6592c-186">where</span></span>

* <span data-ttu-id="6592c-187">`filename` 是外部 XML 文件的文件名。</span><span class="sxs-lookup"><span data-stu-id="6592c-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="6592c-188">相对于包含包含标记文件解释的文件的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="6592c-189">`xpath` 是选择部分的 XML 的外部 XML 文件中的 XPath 表达式。</span><span class="sxs-lookup"><span data-stu-id="6592c-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="6592c-190">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-190">__Example:__</span></span>

<span data-ttu-id="6592c-191">如果源代码包含了如下声明：</span><span class="sxs-lookup"><span data-stu-id="6592c-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="6592c-192">并"docs.xml"的外部文件具有以下内容：</span><span class="sxs-lookup"><span data-stu-id="6592c-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="6592c-193">然后同一文档是输出，如同的源代码包含：</span><span class="sxs-lookup"><span data-stu-id="6592c-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="6592c-194">此标记用于创建列表或项的表。</span><span class="sxs-lookup"><span data-stu-id="6592c-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="6592c-195">它可能包含`<listheader>`块来定义表或定义列表的标题行。</span><span class="sxs-lookup"><span data-stu-id="6592c-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="6592c-196">(定义的表，只有一个条目时`term`需要提供标题中。)</span><span class="sxs-lookup"><span data-stu-id="6592c-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="6592c-197">使用指定列表中的每个项`<item>`块。</span><span class="sxs-lookup"><span data-stu-id="6592c-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="6592c-198">当创建定义列表，同时`term`和`description`必须指定。</span><span class="sxs-lookup"><span data-stu-id="6592c-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="6592c-199">但是，对于表、 项目符号列表或编号的列表，仅`description`需指定。</span><span class="sxs-lookup"><span data-stu-id="6592c-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="6592c-200">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-200">__Syntax:__</span></span>

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

<span data-ttu-id="6592c-201">其中</span><span class="sxs-lookup"><span data-stu-id="6592c-201">where</span></span>

* <span data-ttu-id="6592c-202">`term` 是的术语来定义，其定义位于`description`。</span><span class="sxs-lookup"><span data-stu-id="6592c-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="6592c-203">`description` 是一个项目符号或编号的列表中的项，或者的定义是`term`。</span><span class="sxs-lookup"><span data-stu-id="6592c-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="6592c-204">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-204">__Example:__</span></span>

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

<span data-ttu-id="6592c-205">此标记是用于其他标记内，例如`<summary>`([`<remark>`](documentation-comments.md#remark)) 或`<returns>`([`<returns>`](documentation-comments.md#returns))，并允许结构添加到文本。</span><span class="sxs-lookup"><span data-stu-id="6592c-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="6592c-206">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="6592c-207">其中`content`是段落的文本。</span><span class="sxs-lookup"><span data-stu-id="6592c-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="6592c-208">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-208">__Example:__</span></span>

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

<span data-ttu-id="6592c-209">此标记用于描述方法、 构造函数或索引器的参数。</span><span class="sxs-lookup"><span data-stu-id="6592c-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="6592c-210">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="6592c-211">其中</span><span class="sxs-lookup"><span data-stu-id="6592c-211">where</span></span>

* <span data-ttu-id="6592c-212">`name` 为参数的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="6592c-213">`description` 是参数的说明。</span><span class="sxs-lookup"><span data-stu-id="6592c-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="6592c-214">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-214">__Example:__</span></span>

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

<span data-ttu-id="6592c-215">此标记用于指示一个单词是一个参数。</span><span class="sxs-lookup"><span data-stu-id="6592c-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="6592c-216">可以处理文档文件以设置此参数的格式以不同的方式。</span><span class="sxs-lookup"><span data-stu-id="6592c-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="6592c-217">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="6592c-218">其中`name`是参数的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="6592c-219">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-219">__Example:__</span></span>

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

<span data-ttu-id="6592c-220">此标记允许成员要记录的安全可访问性。</span><span class="sxs-lookup"><span data-stu-id="6592c-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="6592c-221">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="6592c-222">其中</span><span class="sxs-lookup"><span data-stu-id="6592c-222">where</span></span>

* <span data-ttu-id="6592c-223">`member` 是的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-223">`member` is the name of a member.</span></span> <span data-ttu-id="6592c-224">文档生成器检查是否存在给定的代码元素，并将转换*成员*到文档文件中的规范的元素名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="6592c-225">`description` 是对该成员的访问权限的说明。</span><span class="sxs-lookup"><span data-stu-id="6592c-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="6592c-226">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="6592c-227">此标记用于指定有关类型的额外信息。</span><span class="sxs-lookup"><span data-stu-id="6592c-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="6592c-228">(使用`<summary>`([`<summary>`](documentation-comments.md#summary)) 来描述类型本身及其类型的成员。)</span><span class="sxs-lookup"><span data-stu-id="6592c-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="6592c-229">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="6592c-230">其中`description`是注释的文本。</span><span class="sxs-lookup"><span data-stu-id="6592c-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="6592c-231">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-231">__Example:__</span></span>

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

<span data-ttu-id="6592c-232">此标记用于描述一种方法的返回值。</span><span class="sxs-lookup"><span data-stu-id="6592c-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="6592c-233">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="6592c-234">其中`description`是返回值的说明。</span><span class="sxs-lookup"><span data-stu-id="6592c-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="6592c-235">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="6592c-236">此标记允许用于在文本内指定的链接。</span><span class="sxs-lookup"><span data-stu-id="6592c-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="6592c-237">使用`<seealso>`([`<seealso>`](documentation-comments.md#seealso)) 以指示要在另请参见部分中显示的文本。</span><span class="sxs-lookup"><span data-stu-id="6592c-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="6592c-238">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="6592c-239">其中`member`是成员的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-239">where `member` is the name of a member.</span></span> <span data-ttu-id="6592c-240">文档生成器检查是否存在给定的代码元素，并更改*成员*到生成的文档文件中的元素名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="6592c-241">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-241">__Example:__</span></span>

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

<span data-ttu-id="6592c-242">此标记允许另请参阅部分生成的项。</span><span class="sxs-lookup"><span data-stu-id="6592c-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="6592c-243">使用`<see>`([`<see>`](documentation-comments.md#see)) 来指定从文本中的链接。</span><span class="sxs-lookup"><span data-stu-id="6592c-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="6592c-244">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="6592c-245">其中`member`是成员的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-245">where `member` is the name of a member.</span></span> <span data-ttu-id="6592c-246">文档生成器检查是否存在给定的代码元素，并更改*成员*到生成的文档文件中的元素名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="6592c-247">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-247">__Example:__</span></span>

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

此标记可用于描述类型的成员。 <span data-ttu-id="6592c-249">使用`<remark>`([`<remark>`](documentation-comments.md#remark)) 来描述的类型本身。</span><span class="sxs-lookup"><span data-stu-id="6592c-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="6592c-250">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="6592c-251">其中`description`是类型或成员的摘要。</span><span class="sxs-lookup"><span data-stu-id="6592c-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="6592c-252">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="6592c-253">该标记用于描述的属性。</span><span class="sxs-lookup"><span data-stu-id="6592c-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="6592c-254">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="6592c-255">其中`property description`是属性的说明。</span><span class="sxs-lookup"><span data-stu-id="6592c-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="6592c-256">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="6592c-257">此标记用于描述类、 结构、 接口、 委托或方法的泛型类型参数。</span><span class="sxs-lookup"><span data-stu-id="6592c-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="6592c-258">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="6592c-259">其中`name`是类型参数的名称和`description`是其说明。</span><span class="sxs-lookup"><span data-stu-id="6592c-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="6592c-260">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="6592c-261">此标记用于指示一个单词是一个类型参数。</span><span class="sxs-lookup"><span data-stu-id="6592c-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="6592c-262">可以处理文档文件以设置此类型参数的格式以不同的方式。</span><span class="sxs-lookup"><span data-stu-id="6592c-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="6592c-263">__语法：__</span><span class="sxs-lookup"><span data-stu-id="6592c-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="6592c-264">其中`name`是类型参数的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="6592c-265">__示例：__</span><span class="sxs-lookup"><span data-stu-id="6592c-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="6592c-266">处理文档文件</span><span class="sxs-lookup"><span data-stu-id="6592c-266">Processing the documentation file</span></span>

<span data-ttu-id="6592c-267">文档生成器生成的标记为文档注释的源代码中的每个元素的 ID 字符串。</span><span class="sxs-lookup"><span data-stu-id="6592c-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="6592c-268">此 ID 字符串唯一标识源元素。</span><span class="sxs-lookup"><span data-stu-id="6592c-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="6592c-269">文档查看器可以使用 ID 字符串来标识相应的文档适用的元数据/反射项目。</span><span class="sxs-lookup"><span data-stu-id="6592c-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="6592c-270">文档文件不是源代码的分层表示形式;相反，它是生成的 ID 字符串的每个元素的平面列表。</span><span class="sxs-lookup"><span data-stu-id="6592c-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="6592c-271">ID 字符串格式</span><span class="sxs-lookup"><span data-stu-id="6592c-271">ID string format</span></span>

<span data-ttu-id="6592c-272">文档生成器生成的 ID 字符串时遵循下列规则：</span><span class="sxs-lookup"><span data-stu-id="6592c-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="6592c-273">字符串不得包含空格。</span><span class="sxs-lookup"><span data-stu-id="6592c-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="6592c-274">字符串的第一部分标识成员记录，通过单个跟一个冒号字符的类型。</span><span class="sxs-lookup"><span data-stu-id="6592c-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="6592c-275">定义以下类型的成员：</span><span class="sxs-lookup"><span data-stu-id="6592c-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="6592c-276">字符</span><span class="sxs-lookup"><span data-stu-id="6592c-276">__Character__</span></span> | <span data-ttu-id="6592c-277">__说明__</span><span class="sxs-lookup"><span data-stu-id="6592c-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="6592c-278">E</span><span class="sxs-lookup"><span data-stu-id="6592c-278">E</span></span>             | <span data-ttu-id="6592c-279">Event</span><span class="sxs-lookup"><span data-stu-id="6592c-279">Event</span></span>                                                       |
   | <span data-ttu-id="6592c-280">F</span><span class="sxs-lookup"><span data-stu-id="6592c-280">F</span></span>             | <span data-ttu-id="6592c-281">字段</span><span class="sxs-lookup"><span data-stu-id="6592c-281">Field</span></span>                                                       |
   | <span data-ttu-id="6592c-282">M</span><span class="sxs-lookup"><span data-stu-id="6592c-282">M</span></span>             | <span data-ttu-id="6592c-283">方法 （包括构造函数、 析构函数和运算符）</span><span class="sxs-lookup"><span data-stu-id="6592c-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="6592c-284">N</span><span class="sxs-lookup"><span data-stu-id="6592c-284">N</span></span>             | <span data-ttu-id="6592c-285">命名空间</span><span class="sxs-lookup"><span data-stu-id="6592c-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="6592c-286">P</span><span class="sxs-lookup"><span data-stu-id="6592c-286">P</span></span>             | <span data-ttu-id="6592c-287">属性 （包括索引器）</span><span class="sxs-lookup"><span data-stu-id="6592c-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="6592c-288">T</span><span class="sxs-lookup"><span data-stu-id="6592c-288">T</span></span>             | <span data-ttu-id="6592c-289">类型 （例如类、 委托、 枚举、 接口和结构）</span><span class="sxs-lookup"><span data-stu-id="6592c-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="6592c-290">!</span><span class="sxs-lookup"><span data-stu-id="6592c-290">!</span></span>             | <span data-ttu-id="6592c-291">错误字符串;字符串的其余部分提供了有关错误的信息。</span><span class="sxs-lookup"><span data-stu-id="6592c-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="6592c-292">例如，文档生成器生成无法解析的链接的错误信息。</span><span class="sxs-lookup"><span data-stu-id="6592c-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="6592c-293">字符串的第二部分是元素的命名空间的根目录开始的完全限定的名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="6592c-294">用句点分隔的元素，其包含的类型和命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="6592c-295">如果项本身的名称包含句点，它们被替代`#(U+0023)`字符。</span><span class="sxs-lookup"><span data-stu-id="6592c-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="6592c-296">（假定没有任何元素的名称中包含此字符。）</span><span class="sxs-lookup"><span data-stu-id="6592c-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="6592c-297">对于方法和属性使用的参数，参数列表如下所示，括在括号中。</span><span class="sxs-lookup"><span data-stu-id="6592c-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="6592c-298">对于不带参数，则省略括号。</span><span class="sxs-lookup"><span data-stu-id="6592c-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="6592c-299">确保自变量之间用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="6592c-299">The arguments are separated by commas.</span></span> <span data-ttu-id="6592c-300">每个自变量的编码相同的 CLI 签名，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6592c-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="6592c-301">自变量都基于其完全限定的名称，按如下所示修改其文档名称是：</span><span class="sxs-lookup"><span data-stu-id="6592c-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="6592c-302">表示泛型类型参数具有追加`` ` ``（反引号） 字符后跟类型参数的数目</span><span class="sxs-lookup"><span data-stu-id="6592c-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="6592c-303">参数具有`out`或`ref`修饰符有`@`按照其类型名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="6592c-304">按值或通过传递参数`params`没有特殊表示法。</span><span class="sxs-lookup"><span data-stu-id="6592c-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="6592c-305">参数是数组表示为`[lowerbound:size, ... , lowerbound:size]`其中逗号的数量是减一，排名的下限和大小的每个维度，以十进制表示如果已知。</span><span class="sxs-lookup"><span data-stu-id="6592c-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="6592c-306">如果未指定下限或大小，则省略它。</span><span class="sxs-lookup"><span data-stu-id="6592c-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="6592c-307">如果省略的下限和某个特定维度的大小，`:`也会省略。</span><span class="sxs-lookup"><span data-stu-id="6592c-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="6592c-308">交错的数组由一个`[]`每个级别。</span><span class="sxs-lookup"><span data-stu-id="6592c-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="6592c-309">具有非 void 指针类型的参数表示使用`*`类型名称。</span><span class="sxs-lookup"><span data-stu-id="6592c-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="6592c-310">使用的类型名称表示的 void 指针`System.Void`。</span><span class="sxs-lookup"><span data-stu-id="6592c-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="6592c-311">使用引用类型上定义的泛型类型参数的参数进行编码`` ` ``（反引号） 字符后跟类型参数的从零开始的索引。</span><span class="sxs-lookup"><span data-stu-id="6592c-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="6592c-312">使用泛型类型参数在方法中定义的参数使用双反引号``` `` ```而不是`` ` ``用于类型。</span><span class="sxs-lookup"><span data-stu-id="6592c-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="6592c-313">使用泛型类型后, 跟指构造的泛型类型的参数进行编码`{`后, 跟以逗号分隔列表的类型参数，然后是`}`。</span><span class="sxs-lookup"><span data-stu-id="6592c-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="6592c-314">ID 字符串示例</span><span class="sxs-lookup"><span data-stu-id="6592c-314">ID string examples</span></span>

<span data-ttu-id="6592c-315">下面的示例分别演示 C# 代码，以及从支持的文档注释每个源元素生成的 ID 字符串的片段：</span><span class="sxs-lookup"><span data-stu-id="6592c-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="6592c-316">类型表示使用其完全限定的名称，增加泛型信息：</span><span class="sxs-lookup"><span data-stu-id="6592c-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="6592c-317">字段由其完全限定名称表示：</span><span class="sxs-lookup"><span data-stu-id="6592c-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="6592c-318">构造函数。</span><span class="sxs-lookup"><span data-stu-id="6592c-318">Constructors.</span></span>

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

*  <span data-ttu-id="6592c-319">析构函数。</span><span class="sxs-lookup"><span data-stu-id="6592c-319">Destructors.</span></span>

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

*  <span data-ttu-id="6592c-320">方法。</span><span class="sxs-lookup"><span data-stu-id="6592c-320">Methods.</span></span>

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

*  <span data-ttu-id="6592c-321">属性和索引器。</span><span class="sxs-lookup"><span data-stu-id="6592c-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="6592c-322">事件。</span><span class="sxs-lookup"><span data-stu-id="6592c-322">Events.</span></span>

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

*  <span data-ttu-id="6592c-323">一元运算符。</span><span class="sxs-lookup"><span data-stu-id="6592c-323">Unary operators.</span></span>

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

   <span data-ttu-id="6592c-324">使用一元运算符函数名称的完整集合是按如下所示： `op_UnaryPlus`， `op_UnaryNegation`， `op_LogicalNot`， `op_OnesComplement`， `op_Increment`， `op_Decrement`， `op_True`，和`op_False`。</span><span class="sxs-lookup"><span data-stu-id="6592c-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="6592c-325">二元运算符。</span><span class="sxs-lookup"><span data-stu-id="6592c-325">Binary operators.</span></span>

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

   <span data-ttu-id="6592c-326">使用二元运算符函数名称的完整集合是按如下所示： `op_Addition`， `op_Subtraction`， `op_Multiply`， `op_Division`， `op_Modulus`， `op_BitwiseAnd`， `op_BitwiseOr`， `op_ExclusiveOr`， `op_LeftShift`， `op_RightShift`，`op_Equality`， `op_Inequality`， `op_LessThan`， `op_LessThanOrEqual`， `op_GreaterThan`，并`op_GreaterThanOrEqual`。</span><span class="sxs-lookup"><span data-stu-id="6592c-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="6592c-327">转换运算符具有一个尾随"`~`"跟的返回类型。</span><span class="sxs-lookup"><span data-stu-id="6592c-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="6592c-328">示例</span><span class="sxs-lookup"><span data-stu-id="6592c-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="6592c-329">C# 源代码</span><span class="sxs-lookup"><span data-stu-id="6592c-329">C# source code</span></span>

<span data-ttu-id="6592c-330">下面的示例显示了源代码`Point`类：</span><span class="sxs-lookup"><span data-stu-id="6592c-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="6592c-331">生成的 XML</span><span class="sxs-lookup"><span data-stu-id="6592c-331">Resulting XML</span></span>

<span data-ttu-id="6592c-332">下面是一个文档生成器当给定类的源代码时生成的输出`Point`，上面所示：</span><span class="sxs-lookup"><span data-stu-id="6592c-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
