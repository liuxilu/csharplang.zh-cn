# <a name="lexical-structure"></a><span data-ttu-id="85122-101">词法结构</span><span class="sxs-lookup"><span data-stu-id="85122-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="85122-102">Programs</span><span class="sxs-lookup"><span data-stu-id="85122-102">Programs</span></span>

<span data-ttu-id="85122-103">C#***程序***包含一个或多个***源文件***，以前称为***编译单元***([编译单元](namespaces.md#compilation-units))。</span><span class="sxs-lookup"><span data-stu-id="85122-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="85122-104">源代码文件是 Unicode 字符的有序的序列。</span><span class="sxs-lookup"><span data-stu-id="85122-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="85122-105">源文件通常具有一对一的对应关系与文件在文件系统中，但这种对应关系不是必需的。</span><span class="sxs-lookup"><span data-stu-id="85122-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="85122-106">最大的可移植性，建议使用 utf-8 进行编码的文件系统中的文件编码。</span><span class="sxs-lookup"><span data-stu-id="85122-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="85122-107">从概念上讲，程序的编译使用三个步骤：</span><span class="sxs-lookup"><span data-stu-id="85122-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="85122-108">转换，可将文件从特定的字符库和编码方案转换为 Unicode 字符序列。</span><span class="sxs-lookup"><span data-stu-id="85122-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="85122-109">词法分析，Unicode 输入字符的流转换为标记流。</span><span class="sxs-lookup"><span data-stu-id="85122-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="85122-110">语法分析，标记流将转换为可执行代码。</span><span class="sxs-lookup"><span data-stu-id="85122-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="85122-111">语法</span><span class="sxs-lookup"><span data-stu-id="85122-111">Grammars</span></span>

<span data-ttu-id="85122-112">此规范提供了 C# 编程语言使用两种语法的语法。</span><span class="sxs-lookup"><span data-stu-id="85122-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="85122-113">***词法语法***([词法语法](lexical-structure.md#lexical-grammar)) 定义的 Unicode 字符组合窗体行终止符、 空格、 注释、 标记和预处理指令。</span><span class="sxs-lookup"><span data-stu-id="85122-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="85122-114">***句法语法***([句法语法](lexical-structure.md#syntactic-grammar)) 定义如何将从词法语法生成的令牌组合以构成 C# 程序。</span><span class="sxs-lookup"><span data-stu-id="85122-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="85122-115">语法表示法</span><span class="sxs-lookup"><span data-stu-id="85122-115">Grammar notation</span></span>

<span data-ttu-id="85122-116">巴科斯-诺尔范式使用 ANTLR 语法工具表示法中显示的词法和语法的语法。</span><span class="sxs-lookup"><span data-stu-id="85122-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="85122-117">词法语法</span><span class="sxs-lookup"><span data-stu-id="85122-117">Lexical grammar</span></span>

<span data-ttu-id="85122-118">C# 的词法语法所示[词法分析](lexical-structure.md#lexical-analysis)，[令牌](lexical-structure.md#tokens)，并[预处理指令](lexical-structure.md#pre-processing-directives)。</span><span class="sxs-lookup"><span data-stu-id="85122-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="85122-119">词法语法的结束符号是 Unicode 字符集的字符和词法语法指定如何对窗体标记组合字符 ([令牌](lexical-structure.md#tokens))，空白区域 ([空白](lexical-structure.md#white-space))，注释 ([注释](lexical-structure.md#comments))，和预处理指令 ([预处理指令](lexical-structure.md#pre-processing-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="85122-120">C# 程序中的每个源文件必须符合*输入*生产词法语法 ([词法分析](lexical-structure.md#lexical-analysis))。</span><span class="sxs-lookup"><span data-stu-id="85122-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="85122-121">语法的语法</span><span class="sxs-lookup"><span data-stu-id="85122-121">Syntactic grammar</span></span>

<span data-ttu-id="85122-122">C# 语法的语法所示的章节和附录遵循这一章。</span><span class="sxs-lookup"><span data-stu-id="85122-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="85122-123">语法的语法的结束符号是由词法语法，定义的令牌和语法的语法指定令牌组合以构成 C# 程序的方式。</span><span class="sxs-lookup"><span data-stu-id="85122-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="85122-124">每个源文件中的C#程序必须符合*compilation_unit*语法的语法的生产 ([编译单元](namespaces.md#compilation-units))。</span><span class="sxs-lookup"><span data-stu-id="85122-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="85122-125">词法分析</span><span class="sxs-lookup"><span data-stu-id="85122-125">Lexical analysis</span></span>

<span data-ttu-id="85122-126">*输入*生产定义 C# 源文件的词法结构。</span><span class="sxs-lookup"><span data-stu-id="85122-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="85122-127">C# 程序中的每个源文件必须符合此词法语法生产。</span><span class="sxs-lookup"><span data-stu-id="85122-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

<span data-ttu-id="85122-128">五个基本元素组成的词法结构的C#源文件：行终止符 ([行终止符](lexical-structure.md#line-terminators))，空白区域 ([空白](lexical-structure.md#white-space))，注释 ([注释](lexical-structure.md#comments))，令牌 ([令牌](lexical-structure.md#tokens))，并预处理指令 ([预处理指令](lexical-structure.md#pre-processing-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="85122-129">这些基本元素，只有标记非常重要，C# 程序的语法的语法 ([句法语法](lexical-structure.md#syntactic-grammar))。</span><span class="sxs-lookup"><span data-stu-id="85122-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="85122-130">C# 源文件的词法处理包括将文件缩减成的标记序列，它将成为 syntactic 分析的输入。</span><span class="sxs-lookup"><span data-stu-id="85122-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="85122-131">行终止符，空白区域和注释可以用于分隔标记，和预处理指令可能会导致部分的源文件要跳过，但除此之外这些词法元素上的 C# 程序的语法结构没有任何影响。</span><span class="sxs-lookup"><span data-stu-id="85122-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="85122-132">如果内插的字符串文本 ([内插字符串文本](lexical-structure.md#interpolated-string-literals)) 的单个令牌最初由词法分析，但将分解为多个输入元素的重复受制于词法分析直到所有内插的字符串文本已得到解决。</span><span class="sxs-lookup"><span data-stu-id="85122-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="85122-133">生成的令牌随后可充当句法分析的输入。</span><span class="sxs-lookup"><span data-stu-id="85122-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="85122-134">当多个词法语法生产匹配的源代码文件中的字符序列时，词法处理始终窗体的最长可能词法元素。</span><span class="sxs-lookup"><span data-stu-id="85122-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="85122-135">例如，字符序列`//`处理为单行注释的开头，因为该词法元素的长度超过单个`/`令牌。</span><span class="sxs-lookup"><span data-stu-id="85122-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="85122-136">行终止符</span><span class="sxs-lookup"><span data-stu-id="85122-136">Line terminators</span></span>

<span data-ttu-id="85122-137">行终止符将行分成若干 C# 源文件中的字符。</span><span class="sxs-lookup"><span data-stu-id="85122-137">Line terminators divide the characters of a C# source file into lines.</span></span>

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

<span data-ttu-id="85122-138">与源的兼容性代码编辑添加的文件结束标记的工具和以启用源文件以正确地查看序列的形式结束的行，被应用以下转换，以便 C# 程序中每个源文件：</span><span class="sxs-lookup"><span data-stu-id="85122-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="85122-139">源代码文件的最后一个字符是否是控制 Z 字符 (`U+001A`)，删除此字符。</span><span class="sxs-lookup"><span data-stu-id="85122-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="85122-140">回车符 (`U+000D`) 如果该源文件为非空和源代码文件的最后一个字符不是回车符后添加到源代码文件的末尾 (`U+000D`)，换行符 (`U+000A`)，行分隔符 (`U+2028`)，或段落分隔符 (`U+2029`)。</span><span class="sxs-lookup"><span data-stu-id="85122-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="85122-141">注释</span><span class="sxs-lookup"><span data-stu-id="85122-141">Comments</span></span>

<span data-ttu-id="85122-142">支持两种形式的注释： 单行注释和带分隔符的注释。</span><span class="sxs-lookup"><span data-stu-id="85122-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="85122-143">***单行注释***以字符开头`//`并将扩展到源行的末尾。</span><span class="sxs-lookup"><span data-stu-id="85122-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="85122-144">***带分隔符的注释***以字符开头`/*`和其中包含的字符结束`*/`。</span><span class="sxs-lookup"><span data-stu-id="85122-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="85122-145">带分隔符的注释可以跨多个行。</span><span class="sxs-lookup"><span data-stu-id="85122-145">Delimited comments may span multiple lines.</span></span>

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk* '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

<span data-ttu-id="85122-146">注释不嵌套。</span><span class="sxs-lookup"><span data-stu-id="85122-146">Comments do not nest.</span></span> <span data-ttu-id="85122-147">字符序列`/*`并`*/`中没有特殊含义`//`注释和字符序列`//`和`/*`分隔注释中没有特殊含义。</span><span class="sxs-lookup"><span data-stu-id="85122-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="85122-148">注释不会处理在字符和字符串文本中。</span><span class="sxs-lookup"><span data-stu-id="85122-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="85122-149">该示例</span><span class="sxs-lookup"><span data-stu-id="85122-149">The example</span></span>
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="85122-150">包括带分隔符的注释。</span><span class="sxs-lookup"><span data-stu-id="85122-150">includes a delimited comment.</span></span>

<span data-ttu-id="85122-151">该示例</span><span class="sxs-lookup"><span data-stu-id="85122-151">The example</span></span>
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="85122-152">显示多个单行注释。</span><span class="sxs-lookup"><span data-stu-id="85122-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="85122-153">空格</span><span class="sxs-lookup"><span data-stu-id="85122-153">White space</span></span>

<span data-ttu-id="85122-154">空白被定义为与 Unicode 类 Zs （其中包括空格字符） 的任何字符，以及水平制表符、 垂直制表符和窗体换页符。</span><span class="sxs-lookup"><span data-stu-id="85122-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="85122-155">标记</span><span class="sxs-lookup"><span data-stu-id="85122-155">Tokens</span></span>

<span data-ttu-id="85122-156">有几个类型的标记： 标识符、 关键字、 文字、 运算符和标点符号。</span><span class="sxs-lookup"><span data-stu-id="85122-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="85122-157">空白和注释不是标记，但它们可以充当标记的分隔符。</span><span class="sxs-lookup"><span data-stu-id="85122-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="85122-158">Unicode 字符转义序列</span><span class="sxs-lookup"><span data-stu-id="85122-158">Unicode character escape sequences</span></span>

<span data-ttu-id="85122-159">Unicode 字符转义序列表示 Unicode 字符。</span><span class="sxs-lookup"><span data-stu-id="85122-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="85122-160">在标识符中处理 Unicode 字符转义序列 ([标识符](lexical-structure.md#identifiers))，字符文本 ([字符文本](lexical-structure.md#character-literals))，和规则字符串文本 ([字符串文本](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="85122-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="85122-161">在任何其他位置 （例如，若要形成运算符、 标点符号或关键字） 不处理 Unicode 字符转义符。</span><span class="sxs-lookup"><span data-stu-id="85122-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="85122-162">Unicode 转义序列表示单个 Unicode 字符格式正确的后接十六进制数"`\u`"或"`\U`"字符。</span><span class="sxs-lookup"><span data-stu-id="85122-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="85122-163">由于 C# 使用字符和字符串值中的 Unicode 码位的 16 位编码，范围 u+10000 到 U + 10FFFF 中的 Unicode 字符的字符文本中不允许使用和文本字符串中使用 Unicode 代理项对表示。</span><span class="sxs-lookup"><span data-stu-id="85122-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="85122-164">不支持与 0x10FFFF 上面的代码点的 Unicode 字符。</span><span class="sxs-lookup"><span data-stu-id="85122-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="85122-165">不执行多个翻译。</span><span class="sxs-lookup"><span data-stu-id="85122-165">Multiple translations are not performed.</span></span> <span data-ttu-id="85122-166">例如，文字字符串"`\u005Cu005C`"是等效于"`\u005C`"而非"`\`"。</span><span class="sxs-lookup"><span data-stu-id="85122-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="85122-167">Unicode 值`\u005C`是字符"`\`"。</span><span class="sxs-lookup"><span data-stu-id="85122-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="85122-168">该示例</span><span class="sxs-lookup"><span data-stu-id="85122-168">The example</span></span>
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
<span data-ttu-id="85122-169">显示的几种用法`\u0066`，这是字母的转义序列"`f`"。</span><span class="sxs-lookup"><span data-stu-id="85122-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="85122-170">该计划相当于</span><span class="sxs-lookup"><span data-stu-id="85122-170">The program is equivalent to</span></span>
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a><span data-ttu-id="85122-171">标识符</span><span class="sxs-lookup"><span data-stu-id="85122-171">Identifiers</span></span>

<span data-ttu-id="85122-172">此节中给出的标识符的规则精确对应于这些建议由 Unicode 标准附录 31，只不过下划线允许一个初始字符 （这是传统 C 编程语言中），作为 Unicode 转义序列允许在标识符中使用和"`@`"字符允许作为前缀，以使关键字用作标识符。</span><span class="sxs-lookup"><span data-stu-id="85122-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

<span data-ttu-id="85122-173">上面提到的 Unicode 字符类的信息，请参阅 Unicode 标准、 3.0 版、 部分 4.5。</span><span class="sxs-lookup"><span data-stu-id="85122-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="85122-174">有效标识符的示例包括"`identifier1`"，"`_identifier2`"，并"`@if`"。</span><span class="sxs-lookup"><span data-stu-id="85122-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="85122-175">由 Unicode 标准附录 15 定义一致的程序中的标识符必须由 Unicode 范式 C，定义的规范格式。</span><span class="sxs-lookup"><span data-stu-id="85122-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="85122-176">遇到不在范式 C 中的标识符时的行为是实现定义的;但是，诊断则不需要。</span><span class="sxs-lookup"><span data-stu-id="85122-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="85122-177">前缀"`@`"可以将关键字用作标识符，这与其他编程语言进行交互时非常有用。</span><span class="sxs-lookup"><span data-stu-id="85122-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="85122-178">字符`@`不是实际的标识符的一部分，因此标识符可能被视为其他语言中的常规标识符，不带前缀。</span><span class="sxs-lookup"><span data-stu-id="85122-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="85122-179">具有的标识符`@`名为前缀***逐字字符串标识符***。</span><span class="sxs-lookup"><span data-stu-id="85122-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="85122-180">使用`@`允许使用，但强烈建议不要使用作为一种样式不是关键字的标识符的前缀。</span><span class="sxs-lookup"><span data-stu-id="85122-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="85122-181">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="85122-181">The example:</span></span>
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
<span data-ttu-id="85122-182">定义一个名为"`class`"具有静态方法，名为"`static`"，只需一个名为参数"`bool`"。</span><span class="sxs-lookup"><span data-stu-id="85122-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="85122-183">请注意，由于 Unicode 转义不允许在关键字令牌"`cl\u0061ss`"是一个标识符，和是相同的标识符"`@class`"。</span><span class="sxs-lookup"><span data-stu-id="85122-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="85122-184">如果它们是相同的应用以下转换，按顺序后，两个标识符被视为相同：</span><span class="sxs-lookup"><span data-stu-id="85122-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="85122-185">前缀"`@`"，如果使用，会删除。</span><span class="sxs-lookup"><span data-stu-id="85122-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="85122-186">每个*unicode_escape_sequence*转换为其对应的 Unicode 字符。</span><span class="sxs-lookup"><span data-stu-id="85122-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="85122-187">任何*formatting_character*已移除。</span><span class="sxs-lookup"><span data-stu-id="85122-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="85122-188">标识符包含两个连续的下划线字符 (`U+005F`) 仅供使用的实现。</span><span class="sxs-lookup"><span data-stu-id="85122-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="85122-189">例如，实现可能会提供两个下划线开头的扩展的关键字。</span><span class="sxs-lookup"><span data-stu-id="85122-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="85122-190">关键字</span><span class="sxs-lookup"><span data-stu-id="85122-190">Keywords</span></span>

<span data-ttu-id="85122-191">一个***关键字***是一个标识符-类似于序列的字符是保留的并且不能用作标识符的开头时除外`@`字符。</span><span class="sxs-lookup"><span data-stu-id="85122-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

<span data-ttu-id="85122-192">在语法中某些地方，特定标识符具有特殊含义，但不是关键字。</span><span class="sxs-lookup"><span data-stu-id="85122-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="85122-193">此类标识符有时称为"上下文关键字"。</span><span class="sxs-lookup"><span data-stu-id="85122-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="85122-194">例如，在属性声明中，"`get`"和"`set`"标识符具有特殊含义 ([访问器](classes.md#accessors))。</span><span class="sxs-lookup"><span data-stu-id="85122-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="85122-195">以外的其他标识符`get`或`set`永远不会允许在这些位置，因此此，请使用不与这些关键字作为标识符的使用冲突。</span><span class="sxs-lookup"><span data-stu-id="85122-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="85122-196">在其他情况下，例如为具有标识符"`var`"隐式类型本地变量声明中 ([局部变量声明](statements.md#local-variable-declarations))，上下文关键字可以与声明名称发生冲突。</span><span class="sxs-lookup"><span data-stu-id="85122-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="85122-197">在这种情况下，声明的名称将优先于使用上下文关键字作为标识符。</span><span class="sxs-lookup"><span data-stu-id="85122-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="85122-198">文本</span><span class="sxs-lookup"><span data-stu-id="85122-198">Literals</span></span>

<span data-ttu-id="85122-199">一个***文字***是一个值的源的代码表示形式。</span><span class="sxs-lookup"><span data-stu-id="85122-199">A ***literal*** is a source code representation of a value.</span></span>

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a><span data-ttu-id="85122-200">布尔文本</span><span class="sxs-lookup"><span data-stu-id="85122-200">Boolean literals</span></span>

<span data-ttu-id="85122-201">有两个布尔文本值：`true`和`false`。</span><span class="sxs-lookup"><span data-stu-id="85122-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="85122-202">类型*boolean_literal*是`bool`。</span><span class="sxs-lookup"><span data-stu-id="85122-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="85122-203">整数文本</span><span class="sxs-lookup"><span data-stu-id="85122-203">Integer literals</span></span>

<span data-ttu-id="85122-204">整数文本，用于编写类型的值`int`， `uint`， `long`，和`ulong`。</span><span class="sxs-lookup"><span data-stu-id="85122-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="85122-205">整数文本具有两个可能的形式： 十进制和十六进制。</span><span class="sxs-lookup"><span data-stu-id="85122-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

<span data-ttu-id="85122-206">整数文本的类型确定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="85122-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="85122-207">如果整数没有后缀，它具有第一个类型可以在其中表示其值： `int`， `uint`， `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="85122-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="85122-208">如果文本带有后缀`U`或`u`，它具有第一个类型可以在其中表示其值： `uint`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="85122-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="85122-209">如果文本带有后缀`L`或`l`，它具有第一个类型可以在其中表示其值： `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="85122-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="85122-210">如果文本带有后缀`UL`， `Ul`， `uL`， `ul`， `LU`， `Lu`， `lU`，或`lu`，它是类型`ulong`。</span><span class="sxs-lookup"><span data-stu-id="85122-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="85122-211">如果整数文本所表示的值位于外部的范围`ulong`类型，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="85122-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="85122-212">作为一种样式的但还是建议的"`L`"使用而不是"`l`"写入类型的文字时`long`，因为很容易混淆字母"`l`"与数字"`1`"。</span><span class="sxs-lookup"><span data-stu-id="85122-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="85122-213">若要允许的最小可能`int`和`long`值要写为十进制整数文本，存在以下两个规则：</span><span class="sxs-lookup"><span data-stu-id="85122-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="85122-214">时*decimal_integer_literal*具有值 2147483648 (2 ^31) 和无*integer_type_suffix*显示为紧跟一元负运算符令牌的令牌 ([一元负运算符](expressions.md#unary-minus-operator))，结果是类型的常量`int`值介于-2147483648 (-2 ^31)。</span><span class="sxs-lookup"><span data-stu-id="85122-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="85122-215">在所有其他情况下，此类*decimal_integer_literal*属于类型`uint`。</span><span class="sxs-lookup"><span data-stu-id="85122-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="85122-216">当*decimal_integer_literal*具有值 9223372036854775808 (2 ^63) 和无*integer_type_suffix*或*integer_type_suffix* `L`或`l`显示为紧跟一元负运算符令牌的令牌 ([一元负运算符](expressions.md#unary-minus-operator))，结果是类型的常量`long`值-9223372036854775808 (-2 ^63)。</span><span class="sxs-lookup"><span data-stu-id="85122-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="85122-217">在所有其他情况下，此类*decimal_integer_literal*属于类型`ulong`。</span><span class="sxs-lookup"><span data-stu-id="85122-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="85122-218">实际的文本</span><span class="sxs-lookup"><span data-stu-id="85122-218">Real literals</span></span>

<span data-ttu-id="85122-219">实际文本可用于编写类型的值`float`， `double`，和`decimal`。</span><span class="sxs-lookup"><span data-stu-id="85122-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

<span data-ttu-id="85122-220">如果没有*real_type_suffix*指定实数的文本的类型是`double`。</span><span class="sxs-lookup"><span data-stu-id="85122-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="85122-221">否则，实际类型后缀，如下所示确定实际的文本，类型：</span><span class="sxs-lookup"><span data-stu-id="85122-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="85122-222">Real 文字加上后缀`F`或`f`属于类型`float`。</span><span class="sxs-lookup"><span data-stu-id="85122-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="85122-223">例如，文字`1f`， `1.5f`， `1e10f`，和`123.456F`都是类型的`float`。</span><span class="sxs-lookup"><span data-stu-id="85122-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="85122-224">Real 文字加上后缀`D`或`d`属于类型`double`。</span><span class="sxs-lookup"><span data-stu-id="85122-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="85122-225">例如，文字`1d`， `1.5d`， `1e10d`，和`123.456D`都是类型的`double`。</span><span class="sxs-lookup"><span data-stu-id="85122-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="85122-226">Real 文字加上后缀`M`或`m`属于类型`decimal`。</span><span class="sxs-lookup"><span data-stu-id="85122-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="85122-227">例如，文字`1m`， `1.5m`， `1e10m`，和`123.456M`都是类型的`decimal`。</span><span class="sxs-lookup"><span data-stu-id="85122-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="85122-228">此文本转换为`decimal`通过采用确切的值，并如有必要，则舍入为最接近的可表示的值使用值银行家的舍入 ([十进制类型](types.md#the-decimal-type))。</span><span class="sxs-lookup"><span data-stu-id="85122-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="85122-229">除非舍入值或值为零 （在后一种情况中的登录和小数位数为 0），将保留该实数任何规模。</span><span class="sxs-lookup"><span data-stu-id="85122-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="85122-230">因此，文字`2.900m`将对其进行分析，以形成以符号的十进制`0`，决定系数`2900`，和小数位数`3`。</span><span class="sxs-lookup"><span data-stu-id="85122-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="85122-231">如果指定的文本无法出现在指定的类型，发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="85122-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="85122-232">值类型的实际文字`float`或`double`由使用 IEEE"舍入为最接近"模式。</span><span class="sxs-lookup"><span data-stu-id="85122-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="85122-233">请注意，在实数，小数位数始终需要小数点后。</span><span class="sxs-lookup"><span data-stu-id="85122-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="85122-234">例如，`1.3F`是一个实数文本但`1.F`不是。</span><span class="sxs-lookup"><span data-stu-id="85122-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="85122-235">字符文本</span><span class="sxs-lookup"><span data-stu-id="85122-235">Character literals</span></span>

<span data-ttu-id="85122-236">字符文本表示单个字符，一般包含的字符用引号引起来，如`'a'`。</span><span class="sxs-lookup"><span data-stu-id="85122-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="85122-237">注意:ANTLR 语法表示法实现了以下令人困惑 ！</span><span class="sxs-lookup"><span data-stu-id="85122-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="85122-238">在 ANTLR，当你编写`\'`它代表一个单引号`'`。</span><span class="sxs-lookup"><span data-stu-id="85122-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="85122-239">当你编写和`\\`它代表单个反斜杠`\`。</span><span class="sxs-lookup"><span data-stu-id="85122-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="85122-240">因此字符文本的第一个规则意味着开头单引号，然后是字符，最后一个单引号。</span><span class="sxs-lookup"><span data-stu-id="85122-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="85122-241">和 11 个可能的简单转义序列`\'`， `\"`， `\\`， `\0`， `\a`， `\b`， `\f`， `\n`， `\r`， `\t`， `\v`.</span><span class="sxs-lookup"><span data-stu-id="85122-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

<span data-ttu-id="85122-242">反斜杠字符后面的字符 (`\`) 中*字符*必须是下列字符之一： `'`， `"`， `\`， `0`， `a`， `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="85122-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="85122-243">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="85122-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="85122-244">十六进制转义序列表示单个 Unicode 字符，后接十六进制数的格式正确的值与"`\x`"。</span><span class="sxs-lookup"><span data-stu-id="85122-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="85122-245">如果表示的字符文本的值大于`U+FFFF`，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="85122-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="85122-246">Unicode 字符转义序列 ([Unicode 字符转义序列](lexical-structure.md#unicode-character-escape-sequences)) 中的字符文本必须在范围内`U+0000`到`U+FFFF`。</span><span class="sxs-lookup"><span data-stu-id="85122-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="85122-247">简单转义序列表示 Unicode 字符编码下, 表中所述。</span><span class="sxs-lookup"><span data-stu-id="85122-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="85122-248">__转义序列__</span><span class="sxs-lookup"><span data-stu-id="85122-248">__Escape sequence__</span></span> | <span data-ttu-id="85122-249">__字符名称__</span><span class="sxs-lookup"><span data-stu-id="85122-249">__Character name__</span></span> | <span data-ttu-id="85122-250">__Unicode 编码__</span><span class="sxs-lookup"><span data-stu-id="85122-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="85122-251">单引号</span><span class="sxs-lookup"><span data-stu-id="85122-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="85122-252">双引号</span><span class="sxs-lookup"><span data-stu-id="85122-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="85122-253">反斜杠</span><span class="sxs-lookup"><span data-stu-id="85122-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="85122-254">null</span><span class="sxs-lookup"><span data-stu-id="85122-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="85122-255">警报</span><span class="sxs-lookup"><span data-stu-id="85122-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="85122-256">Backspace</span><span class="sxs-lookup"><span data-stu-id="85122-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="85122-257">换页</span><span class="sxs-lookup"><span data-stu-id="85122-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="85122-258">换行</span><span class="sxs-lookup"><span data-stu-id="85122-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="85122-259">回车</span><span class="sxs-lookup"><span data-stu-id="85122-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="85122-260">水平制表符</span><span class="sxs-lookup"><span data-stu-id="85122-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="85122-261">垂直制表符</span><span class="sxs-lookup"><span data-stu-id="85122-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="85122-262">类型*character_literal*是`char`。</span><span class="sxs-lookup"><span data-stu-id="85122-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="85122-263">字符串文本</span><span class="sxs-lookup"><span data-stu-id="85122-263">String literals</span></span>

<span data-ttu-id="85122-264">C# 支持两种形式的字符串文本：***规则字符串文本***并***逐字字符串文本***。</span><span class="sxs-lookup"><span data-stu-id="85122-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="85122-265">常规字符串括在双引号内，作为中的零个或多个字符组成`"hello"`，并且可能包括这两个简单的转义序列 (如`\t`选项卡上的字符)，十六进制转义序列和 Unicode 转义序列。</span><span class="sxs-lookup"><span data-stu-id="85122-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="85122-266">原义字符串组成`@`字符后跟双引号字符、 零个或多个字符和右双引号字符。</span><span class="sxs-lookup"><span data-stu-id="85122-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="85122-267">一个简单示例是`@"hello"`。</span><span class="sxs-lookup"><span data-stu-id="85122-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="85122-268">在逐字字符串文本中，分隔符之间的字符逐字解释，唯一的异常所*quote_escape_sequence*。</span><span class="sxs-lookup"><span data-stu-id="85122-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="85122-269">具体而言，简单的转义序列和十六进制和 Unicode 转义序列不处理逐字字符串文本中。</span><span class="sxs-lookup"><span data-stu-id="85122-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="85122-270">逐字字符串文本可能跨多个行。</span><span class="sxs-lookup"><span data-stu-id="85122-270">A verbatim string literal may span multiple lines.</span></span>

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

<span data-ttu-id="85122-271">反斜杠字符后面的字符 (`\`) 中*regular_string_literal_character*必须是下列字符之一： `'`， `"`， `\`， `0`， `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="85122-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="85122-272">否则，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="85122-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="85122-273">该示例</span><span class="sxs-lookup"><span data-stu-id="85122-273">The example</span></span>
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
<span data-ttu-id="85122-274">显示了各种不同的字符串。</span><span class="sxs-lookup"><span data-stu-id="85122-274">shows a variety of string literals.</span></span> <span data-ttu-id="85122-275">最后一个字符串文字， `j`，是原义字符串文字跨多个行。</span><span class="sxs-lookup"><span data-stu-id="85122-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="85122-276">将保留原义之间引号，包括新行字符，如空格字符。</span><span class="sxs-lookup"><span data-stu-id="85122-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="85122-277">由于十六进制转义序列可以具有可变数量的十六进制数字，字符串文字`"\x123"`包含单个字符具有十六进制值 123。</span><span class="sxs-lookup"><span data-stu-id="85122-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="85122-278">若要创建包含具有十六进制值后跟字符 3 12 的字符的字符串，其中一个可以编写`"\x00123"`或`"\x12" + "3"`相反。</span><span class="sxs-lookup"><span data-stu-id="85122-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="85122-279">类型*string_literal*是`string`。</span><span class="sxs-lookup"><span data-stu-id="85122-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="85122-280">每个字符串文字不一定产生新的字符串实例。</span><span class="sxs-lookup"><span data-stu-id="85122-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="85122-281">两个或多个字符串的相等时根据字符串的相等运算符 ([字符串是否相等运算符](expressions.md#string-equality-operators)) 出现在同一个程序，这些字符串文字引用相同的字符串实例。</span><span class="sxs-lookup"><span data-stu-id="85122-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="85122-282">例如，生成的输出</span><span class="sxs-lookup"><span data-stu-id="85122-282">For instance, the output produced by</span></span>
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
<span data-ttu-id="85122-283">是`True`因为两个字符串引用相同的字符串实例。</span><span class="sxs-lookup"><span data-stu-id="85122-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="85122-284">内插的字符串文本</span><span class="sxs-lookup"><span data-stu-id="85122-284">Interpolated string literals</span></span>

<span data-ttu-id="85122-285">内插的字符串类似于字符串文本，但包含洞分隔`{`和`}`，其中表达式可以出现。</span><span class="sxs-lookup"><span data-stu-id="85122-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="85122-286">在运行时，使它们代入漏洞的出现位置的位置处的字符串的文本形式计算这些表达式。</span><span class="sxs-lookup"><span data-stu-id="85122-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="85122-287">部分中描述的语法和语义字符串内插 ([内插字符串](expressions.md#interpolated-strings))。</span><span class="sxs-lookup"><span data-stu-id="85122-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="85122-288">字符串文本，如内插的字符串文字可以定期或原义。</span><span class="sxs-lookup"><span data-stu-id="85122-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="85122-289">内插规则字符串文本分隔`$"`并`"`，并由分隔内插逐字字符串文本`$@"`和`"`。</span><span class="sxs-lookup"><span data-stu-id="85122-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="85122-290">像其他文本内, 插字符串文字的词法分析最初导致单个令牌，根据下面的语法。</span><span class="sxs-lookup"><span data-stu-id="85122-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="85122-291">但是之前句法分析, 内, 插字符串文字的单个令牌分解为多个令牌封闭漏洞，该字符串的各部分和的孔内发生的输入的元素从词法上再次分析。</span><span class="sxs-lookup"><span data-stu-id="85122-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="85122-292">反过来，这可能会产生更内插的字符串文本，以便得到处理，但是，如果从词法上更正，最终导致一系列的语法分析，以处理令牌。</span><span class="sxs-lookup"><span data-stu-id="85122-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

    close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

<span data-ttu-id="85122-293">*Interpolated_string_literal*令牌重新解释为多个令牌和其他输入元素，如下所示的中的出现顺序*interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="85122-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="85122-294">以下的匹配项作为单独的各个标记重新解释： 前导`$`的登录， *interpolated_regular_string_whole*， *interpolated_regular_string_start*，*interpolated_regular_string_mid*， *interpolated_regular_string_end*， *interpolated_verbatim_string_whole*， *interpolated_verbatim_string_start*， *interpolated_verbatim_string_mid*并*interpolated_verbatim_string_end*。</span><span class="sxs-lookup"><span data-stu-id="85122-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="85122-295">出现次数*regular_balanced_text*并*verbatim_balanced_text*它们之间重新为处理*input_section* ([词法分析](lexical-structure.md#lexical-analysis)) 和所产生的序列的输入元素为重新解释。</span><span class="sxs-lookup"><span data-stu-id="85122-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="85122-296">这些可能依次包含内插的字符串文本标记被重新解释。</span><span class="sxs-lookup"><span data-stu-id="85122-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="85122-297">句法分析将重新组合到令牌*interpolated_string_expression* ([内插字符串](expressions.md#interpolated-strings))。</span><span class="sxs-lookup"><span data-stu-id="85122-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="85122-298">示例待办事项</span><span class="sxs-lookup"><span data-stu-id="85122-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="85122-299">Null 文本</span><span class="sxs-lookup"><span data-stu-id="85122-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="85122-300">*Null_literal*可以隐式转换为引用类型或可以为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="85122-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="85122-301">运算符和标点符号</span><span class="sxs-lookup"><span data-stu-id="85122-301">Operators and punctuators</span></span>

<span data-ttu-id="85122-302">有几种类型的运算符和标点符号。</span><span class="sxs-lookup"><span data-stu-id="85122-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="85122-303">在表达式中使用的运算符用于描述涉及一个或多个操作数的操作。</span><span class="sxs-lookup"><span data-stu-id="85122-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="85122-304">例如，表达式`a + b`使用`+`运算符将两个操作数`a`和`b`。</span><span class="sxs-lookup"><span data-stu-id="85122-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="85122-305">标点符号是用于分组和分隔。</span><span class="sxs-lookup"><span data-stu-id="85122-305">Punctuators are for grouping and separating.</span></span>

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

<span data-ttu-id="85122-306">中的竖线*right_shift*并*right_shift_assignment*生产用于与不同的语法的语法中，任何类型的任何字符中的其他生产中 （甚至不指示，空格） 允许标记之间。</span><span class="sxs-lookup"><span data-stu-id="85122-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="85122-307">这些生产处理以便正确处理特殊*type_parameter_list*s ([类型参数](classes.md#type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="85122-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="85122-308">预处理指令</span><span class="sxs-lookup"><span data-stu-id="85122-308">Pre-processing directives</span></span>

<span data-ttu-id="85122-309">预处理指令提供有条件地跳过的源代码文件，报告错误和警告条件节以及描绘不同区域的源代码的能力。</span><span class="sxs-lookup"><span data-stu-id="85122-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="85122-310">术语"预处理指令"仅用于与 C 和 c + + 编程语言的一致性。</span><span class="sxs-lookup"><span data-stu-id="85122-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="85122-311">在 C# 中，没有任何单独的预处理步骤;预处理指令处理作为词法分析阶段的一部分。</span><span class="sxs-lookup"><span data-stu-id="85122-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

<span data-ttu-id="85122-312">提供了以下预处理指令：</span><span class="sxs-lookup"><span data-stu-id="85122-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="85122-313">`#define` 并`#undef`，用于定义和未定义，分别条件编译符号 ([声明指令](lexical-structure.md#declaration-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="85122-314">`#if``#elif`， `#else`，并`#endif`，用于有条件地跳过的源代码节 ([条件编译指令](lexical-structure.md#conditional-compilation-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="85122-315">`#line`用于控制发出错误和警告的行号 ([行指令](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="85122-316">`#error` 并`#warning`，用于分别发出错误和警告 ([诊断指令](lexical-structure.md#diagnostic-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="85122-317">`#region` 并`#endregion`，用于将源代码部分显式标记 ([Region 指令](lexical-structure.md#region-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="85122-318">`#pragma`用于指定到编译器的可选上下文信息 ([杂注指令](lexical-structure.md#pragma-directives))。</span><span class="sxs-lookup"><span data-stu-id="85122-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="85122-319">预处理指令总是占用单独的行的源代码，并且始终开头`#`字符和预处理指令名称。</span><span class="sxs-lookup"><span data-stu-id="85122-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="85122-320">空白区域可能会发生之前`#`字符和介于`#`字符和指令的名称。</span><span class="sxs-lookup"><span data-stu-id="85122-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="85122-321">源行包含`#define`， `#undef`， `#if`， `#elif`， `#else`， `#endif`， `#line`，或`#endregion`指令可以其结尾的单行注释。</span><span class="sxs-lookup"><span data-stu-id="85122-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="85122-322">带分隔符的注释 (`/* */`样式的注释) 包含预处理指令的源行上不允许。</span><span class="sxs-lookup"><span data-stu-id="85122-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="85122-323">预处理指令既不是标记，也不是语法的 C# 语法的一部分。</span><span class="sxs-lookup"><span data-stu-id="85122-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="85122-324">但是，预处理指令可用于包含或排除的令牌的序列，可以通过这种方式影响 C# 程序的含义。</span><span class="sxs-lookup"><span data-stu-id="85122-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="85122-325">例如，当编译该程序：</span><span class="sxs-lookup"><span data-stu-id="85122-325">For example, when compiled, the program:</span></span>
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
<span data-ttu-id="85122-326">完全相同的序列中的令牌作为该程序的结果：</span><span class="sxs-lookup"><span data-stu-id="85122-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="85122-327">因此，从词法上，两个程序大不相同，语法上，而它们相同。</span><span class="sxs-lookup"><span data-stu-id="85122-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="85122-328">条件编译符号</span><span class="sxs-lookup"><span data-stu-id="85122-328">Conditional compilation symbols</span></span>

<span data-ttu-id="85122-329">提供的条件编译功能`#if`， `#elif`， `#else`，和`#endif`预处理表达式可以通过控制的指令 ([预处理表达式](lexical-structure.md#pre-processing-expressions))和条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="85122-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="85122-330">条件编译符号有两个可能的状态：***定义***或***未定义***。</span><span class="sxs-lookup"><span data-stu-id="85122-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="85122-331">在源文件的词法处理开始时，条件编译符号未定义，除非已由外部机制 （如命令行编译器选项） 显式定义。</span><span class="sxs-lookup"><span data-stu-id="85122-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="85122-332">当`#define`处理指令，在该指令中指定的条件编译符号成为已在该源文件中定义。</span><span class="sxs-lookup"><span data-stu-id="85122-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="85122-333">符号一直保持定义到`#undef`指令处理该相同的符号时，或直到到达源文件的末尾。</span><span class="sxs-lookup"><span data-stu-id="85122-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="85122-334">这是`#define`和`#undef`一个源文件中的指令不会影响在同一程序中的其他源文件。</span><span class="sxs-lookup"><span data-stu-id="85122-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="85122-335">定义条件编译符号在预处理表达式中引用时, 具有的布尔值`true`，并且未定义的条件编译符号的布尔值`false`。</span><span class="sxs-lookup"><span data-stu-id="85122-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="85122-336">没有任何要求，条件编译符号之前显式声明中预处理表达式引用它们。</span><span class="sxs-lookup"><span data-stu-id="85122-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="85122-337">相反，未声明的符号只是未定义，并因此具有值`false`。</span><span class="sxs-lookup"><span data-stu-id="85122-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="85122-338">条件编译符号在命名空间是不同，独立于 C# 程序中的所有其他命名实体。</span><span class="sxs-lookup"><span data-stu-id="85122-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="85122-339">条件编译符号只能引用在`#define`和`#undef`指令和预处理表达式中。</span><span class="sxs-lookup"><span data-stu-id="85122-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="85122-340">预处理表达式</span><span class="sxs-lookup"><span data-stu-id="85122-340">Pre-processing expressions</span></span>

<span data-ttu-id="85122-341">预处理表达式中可能会发生`#if`和`#elif`指令。</span><span class="sxs-lookup"><span data-stu-id="85122-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="85122-342">运算符`!`， `==`， `!=`，`&&`和`||`允许在预处理表达式，并且可能会使用括号进行分组。</span><span class="sxs-lookup"><span data-stu-id="85122-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

<span data-ttu-id="85122-343">定义条件编译符号在预处理表达式中引用时, 具有的布尔值`true`，并且未定义的条件编译符号的布尔值`false`。</span><span class="sxs-lookup"><span data-stu-id="85122-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="85122-344">始终预处理表达式的计算结果将生成一个布尔值。</span><span class="sxs-lookup"><span data-stu-id="85122-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="85122-345">预处理表达式的求值的规则是相同的常量表达式 ([常量表达式](expressions.md#constant-expressions))，不过，只有用户定义的实体可以引用的是条件编译符号.</span><span class="sxs-lookup"><span data-stu-id="85122-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="85122-346">声明指令</span><span class="sxs-lookup"><span data-stu-id="85122-346">Declaration directives</span></span>

<span data-ttu-id="85122-347">声明指令用于定义或取消定义条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="85122-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="85122-348">在处理`#define`指令会导致给定的条件编译符号成为已定义，跟在指令后面的源代码行开始。</span><span class="sxs-lookup"><span data-stu-id="85122-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="85122-349">同样，在处理`#undef`指令会导致给定的条件编译符号变成未定义，跟在指令后面的源代码行开始。</span><span class="sxs-lookup"><span data-stu-id="85122-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="85122-350">任何`#define`并`#undef`指令在源文件中的必须出现在第一个之前*令牌*([令牌](lexical-structure.md#tokens)) 在源代码文件中; 否则将编译时出现错误。</span><span class="sxs-lookup"><span data-stu-id="85122-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="85122-351">直观地讲，在`#define`和`#undef`指令必须位于任何"实际代码"中的源代码文件之前。</span><span class="sxs-lookup"><span data-stu-id="85122-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="85122-352">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="85122-352">The example:</span></span>
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
<span data-ttu-id="85122-353">无效，因为`#define`指令前加上第一个标记 (`namespace`关键字) 中的源文件。</span><span class="sxs-lookup"><span data-stu-id="85122-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="85122-354">下面的示例导致在编译时错误，因为`#define`遵循真实代码：</span><span class="sxs-lookup"><span data-stu-id="85122-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

<span data-ttu-id="85122-355">一个`#define`可以定义条件编译符号已定义的而无需任何中间那里`#undef`该符号的。</span><span class="sxs-lookup"><span data-stu-id="85122-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="85122-356">下面的示例中定义的条件编译符号`A`，然后定义它。</span><span class="sxs-lookup"><span data-stu-id="85122-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="85122-357">一个`#undef`可能"取消"未定义的条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="85122-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="85122-358">下面的示例中定义的条件编译符号`A`，然后取消定义它两次; 尽管第二个`#undef`不起作用，它是否仍然有效。</span><span class="sxs-lookup"><span data-stu-id="85122-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="85122-359">条件编译指令</span><span class="sxs-lookup"><span data-stu-id="85122-359">Conditional compilation directives</span></span>

<span data-ttu-id="85122-360">条件编译指令用于有条件地包括或排除的源文件部分。</span><span class="sxs-lookup"><span data-stu-id="85122-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

<span data-ttu-id="85122-361">条件编译指令的语法所示，必须编写为集组成，按顺序`#if`指令，零个或多个`#elif`指令、 零个或一个`#else`指令，和一个`#endif`指令。</span><span class="sxs-lookup"><span data-stu-id="85122-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="85122-362">这些指令之间是源代码的条件部分。</span><span class="sxs-lookup"><span data-stu-id="85122-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="85122-363">每个部分由前面紧邻的指令控制。</span><span class="sxs-lookup"><span data-stu-id="85122-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="85122-364">有条件节本身可能包含嵌套的条件编译指令提供这些指令形成完整集。</span><span class="sxs-lookup"><span data-stu-id="85122-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="85122-365">一个*pp_conditional*选择最多包含一个*conditional_section*s 表示正常的词法处理：</span><span class="sxs-lookup"><span data-stu-id="85122-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="85122-366">*Pp_expression*的 s`#if`并`#elif`指令按顺序进行计算一个生成为止`true`。</span><span class="sxs-lookup"><span data-stu-id="85122-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="85122-367">如果表达式的结果`true`，则*conditional_section*选择相应的指令。</span><span class="sxs-lookup"><span data-stu-id="85122-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="85122-368">如果所有*pp_expression*s yield `false`，并且如果`#else`存在，则指令*conditional_section*的`#else`指令处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="85122-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="85122-369">否则为为否*conditional_section*处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="85122-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="85122-370">所选*conditional_section*，如果有的话被处理为普通*input_section*： 词法语法必须遵循的部分中包含的源代码; 从源生成令牌部分; 中的代码预处理指令的部分中具有规定的影响。</span><span class="sxs-lookup"><span data-stu-id="85122-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="85122-371">剩余*conditional_section*s，如果有，作为处理*skipped_section*s： 除预处理指令外的，部分中的源代码不需要符合词法语法; 否从部分; 中的源代码生成令牌和部分中的预处理指令必须在词法上正确但不是否则处理。</span><span class="sxs-lookup"><span data-stu-id="85122-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="85122-372">内*conditional_section* ，为其进行处理作为*skipped_section*、 任何嵌套*conditional_section*s (包含在嵌套`#if`...`#endif`和`#region`...`#endregion`构造) 也作为处理*skipped_section*s。</span><span class="sxs-lookup"><span data-stu-id="85122-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="85122-373">下面的示例演示了如何条件编译指令可以嵌套：</span><span class="sxs-lookup"><span data-stu-id="85122-373">The following example illustrates how conditional compilation directives can nest:</span></span>
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

<span data-ttu-id="85122-374">除了预处理指令，跳过的源代码不遵从语法分析。</span><span class="sxs-lookup"><span data-stu-id="85122-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="85122-375">例如，以下是有效的尽管未终止的注释中`#else`部分：</span><span class="sxs-lookup"><span data-stu-id="85122-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

<span data-ttu-id="85122-376">但是，请注意，预处理指令所需的源代码的已跳过部分中，甚至在词法上正确。</span><span class="sxs-lookup"><span data-stu-id="85122-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="85122-377">预处理指令不会处理时出现在多行输入元素的内部。</span><span class="sxs-lookup"><span data-stu-id="85122-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="85122-378">例如，该计划：</span><span class="sxs-lookup"><span data-stu-id="85122-378">For example, the program:</span></span>
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
<span data-ttu-id="85122-379">在输出中的结果：</span><span class="sxs-lookup"><span data-stu-id="85122-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="85122-380">在特殊情况下，处理的预处理指令集可能依赖于计算*pp_expression*。</span><span class="sxs-lookup"><span data-stu-id="85122-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="85122-381">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="85122-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="85122-382">始终生成相同的令牌流 (`class` `Q` `{` `}`)，无论是否`X`定义。</span><span class="sxs-lookup"><span data-stu-id="85122-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="85122-383">如果`X`是定义，仅处理的指令是`#if`和`#endif`，因为多行注释。</span><span class="sxs-lookup"><span data-stu-id="85122-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="85122-384">如果`X`是未定义，然后三个指令 (`#if`， `#else`， `#endif`) 是指令集的一部分。</span><span class="sxs-lookup"><span data-stu-id="85122-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="85122-385">诊断指令</span><span class="sxs-lookup"><span data-stu-id="85122-385">Diagnostic directives</span></span>

<span data-ttu-id="85122-386">诊断指令用于显式生成错误和警告消息中一样其他编译时错误和警告报告。</span><span class="sxs-lookup"><span data-stu-id="85122-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

<span data-ttu-id="85122-387">下面的示例：</span><span class="sxs-lookup"><span data-stu-id="85122-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="85122-388">始终会生成警告 （"代码评审需要在签入前"），并会生成编译时错误 （"生成不能为调试和发布"） 如果条件符号`Debug`和`Retail`进行定义。</span><span class="sxs-lookup"><span data-stu-id="85122-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="85122-389">请注意， *pp_message*可以包含任意文本; 具体而言，它需要包含格式正确的令牌，如所示在 word 中的单引号`can't`。</span><span class="sxs-lookup"><span data-stu-id="85122-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="85122-390">区域指令</span><span class="sxs-lookup"><span data-stu-id="85122-390">Region directives</span></span>

<span data-ttu-id="85122-391">区域指令用于显式标记的源代码区域。</span><span class="sxs-lookup"><span data-stu-id="85122-391">The region directives are used to explicitly mark regions of source code.</span></span>

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

<span data-ttu-id="85122-392">附加的区域; 没有语义含义区域适用于使用由程序员或通过自动工具将源代码节标记。</span><span class="sxs-lookup"><span data-stu-id="85122-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="85122-393">在指定的消息`#region`或`#endregion`指令同样的语义含义; 它只是用来识别该区域。</span><span class="sxs-lookup"><span data-stu-id="85122-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="85122-394">匹配`#region`并`#endregion`指令可能具有不同*pp_message*s。</span><span class="sxs-lookup"><span data-stu-id="85122-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="85122-395">区域的词法正在处理：</span><span class="sxs-lookup"><span data-stu-id="85122-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="85122-396">完全对应窗体的条件编译指令的词法处理：</span><span class="sxs-lookup"><span data-stu-id="85122-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="85122-397">行指令</span><span class="sxs-lookup"><span data-stu-id="85122-397">Line directives</span></span>

<span data-ttu-id="85122-398">行指令可能用于更改的行号和源文件名的编译器中输出，例如警告和错误，报告的和由调用方信息特性 ([调用方信息特性](attributes.md#caller-info-attributes))。</span><span class="sxs-lookup"><span data-stu-id="85122-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="85122-399">从指定的其他文本输入生成 C# 源代码的元编程工具中最常使用行指令。</span><span class="sxs-lookup"><span data-stu-id="85122-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

<span data-ttu-id="85122-400">如果未`#line`指令是否存在后，编译器会报告真实的行号和源文件名称在其输出中的。</span><span class="sxs-lookup"><span data-stu-id="85122-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="85122-401">处理时`#line`包含的指令*line_indicator*不`default`，编译器将为具有给定的行号 （和文件名称，如果指定） 执行该指令之后的行。</span><span class="sxs-lookup"><span data-stu-id="85122-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="85122-402">一个`#line default`指令反转所有上述 #line 指令的效果。</span><span class="sxs-lookup"><span data-stu-id="85122-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="85122-403">编译器报告后续行的真实行信息就像没有`#line`处理指令一样。</span><span class="sxs-lookup"><span data-stu-id="85122-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="85122-404">一个`#line hidden`指令不起作用的文件和行号错误地报告消息，但会影响源级别的调试。</span><span class="sxs-lookup"><span data-stu-id="85122-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="85122-405">在调试时，所有行之间`#line hidden`指令和后续`#line`指令 (这不是`#line hidden`) 有无行号信息。</span><span class="sxs-lookup"><span data-stu-id="85122-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="85122-406">当逐句通过代码在调试器中，将完全跳过这些行。</span><span class="sxs-lookup"><span data-stu-id="85122-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="85122-407">请注意， *file_name*不同于常规字符串，不会处理转义符;"`\`"字符只需指定一个普通的反斜杠字符内*file_name*.</span><span class="sxs-lookup"><span data-stu-id="85122-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="85122-408">杂注指令</span><span class="sxs-lookup"><span data-stu-id="85122-408">Pragma directives</span></span>

<span data-ttu-id="85122-409">`#pragma`预处理指令用于指定到编译器的可选上下文信息。</span><span class="sxs-lookup"><span data-stu-id="85122-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="85122-410">中提供的信息`#pragma`指令将永远不会更改程序语义。</span><span class="sxs-lookup"><span data-stu-id="85122-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="85122-411">C# 提供`#pragma`指令来控制编译器警告。</span><span class="sxs-lookup"><span data-stu-id="85122-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="85122-412">该语言的未来版本可能包括其他`#pragma`指令。</span><span class="sxs-lookup"><span data-stu-id="85122-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="85122-413">若要确保与其他 C# 编译器互操作性，Microsoft C# 编译器不会发出未知的编译错误`#pragma`指令; 但是生成警告此类指令执行操作。</span><span class="sxs-lookup"><span data-stu-id="85122-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="85122-414">杂注警告</span><span class="sxs-lookup"><span data-stu-id="85122-414">Pragma warning</span></span>

<span data-ttu-id="85122-415">`#pragma warning`指令用于禁用或还原所有或一组特定的警告消息的后续程序文本编译期间。</span><span class="sxs-lookup"><span data-stu-id="85122-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

<span data-ttu-id="85122-416">一个`#pragma warning`忽略该警告列表的指令会影响所有警告。</span><span class="sxs-lookup"><span data-stu-id="85122-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="85122-417">一个`#pragma warning`指令包含警告列表会影响仅在列表中指定这些警告。</span><span class="sxs-lookup"><span data-stu-id="85122-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="85122-418">一个`#pragma warning disable`指令禁用所有或给定的警告集。</span><span class="sxs-lookup"><span data-stu-id="85122-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="85122-419">一个`#pragma warning restore`指令还原所有或将警告记录到有效的编译单元起始处的状态的给定的集。</span><span class="sxs-lookup"><span data-stu-id="85122-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="85122-420">请注意，如果已从外部禁用特定警告， `#pragma warning restore` (是否为所有或特定警告) 不会重新启用该警告。</span><span class="sxs-lookup"><span data-stu-id="85122-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="85122-421">下面的示例演示使用`#pragma warning`暂时禁用该警告时，报告已过时成员被引用时，使用来自 Microsoft C# 编译器的警告编号。</span><span class="sxs-lookup"><span data-stu-id="85122-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
