# <a name="namespaces"></a><span data-ttu-id="1318e-101">命名空间</span><span class="sxs-lookup"><span data-stu-id="1318e-101">Namespaces</span></span>

<span data-ttu-id="1318e-102">C# 程序的组织结构使用的命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="1318e-103">使用命名空间为"内部"组织系统的程序，并作为"外部"组织系统 — 一种方法提供向其他程序公开的程序元素。</span><span class="sxs-lookup"><span data-stu-id="1318e-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="1318e-104">Using 指令 ([Using 指令](namespaces.md#using-directives)) 提供，以便命名空间的使用。</span><span class="sxs-lookup"><span data-stu-id="1318e-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="1318e-105">编译单元</span><span class="sxs-lookup"><span data-stu-id="1318e-105">Compilation units</span></span>

<span data-ttu-id="1318e-106">一个*compilation_unit*定义源文件的整体结构。</span><span class="sxs-lookup"><span data-stu-id="1318e-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="1318e-107">编译单元包含零个或多*using_directive*s 后跟零个或多*global_attributes*跟零个或多*namespace_member_declaration*s.</span><span class="sxs-lookup"><span data-stu-id="1318e-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="1318e-108">C# 程序包含一个或多个编译单元，每个包含在单独的源代码文件中。</span><span class="sxs-lookup"><span data-stu-id="1318e-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="1318e-109">编译 C# 程序时，所有编译单元一起进行处理。</span><span class="sxs-lookup"><span data-stu-id="1318e-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="1318e-110">因此，编译单元可以依赖于彼此，可能是以循环方式。</span><span class="sxs-lookup"><span data-stu-id="1318e-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="1318e-111">*Using_directive*秒的编译单元影响*global_attributes*并*namespace_member_declaration*s 该编译单元，但不起任何作用其他编译单元。</span><span class="sxs-lookup"><span data-stu-id="1318e-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="1318e-112">*Global_attributes* ([属性](attributes.md)) 编译单元的允许的目标程序集和模块的属性的规范。</span><span class="sxs-lookup"><span data-stu-id="1318e-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="1318e-113">程序集和模块作为物理容器的类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="1318e-114">程序集可能包含多个物理上独立的模块。</span><span class="sxs-lookup"><span data-stu-id="1318e-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="1318e-115">*Namespace_member_declaration*s 程序的每个编译单元将成员分配到名为全局命名空间的单个声明空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="1318e-116">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-116">For example:</span></span>

<span data-ttu-id="1318e-117">文件`A.cs`:</span><span class="sxs-lookup"><span data-stu-id="1318e-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="1318e-118">文件`B.cs`:</span><span class="sxs-lookup"><span data-stu-id="1318e-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="1318e-119">两个编译单元中分配到单一的全局命名空间，在这种情况下声明两个类的完全限定名称`A`和`B`。</span><span class="sxs-lookup"><span data-stu-id="1318e-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="1318e-120">因为这两个编译单元中分配到同一声明空间，重要的是成员的一个错误如果每个包含具有相同名称的声明。</span><span class="sxs-lookup"><span data-stu-id="1318e-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="1318e-121">命名空间声明</span><span class="sxs-lookup"><span data-stu-id="1318e-121">Namespace declarations</span></span>

<span data-ttu-id="1318e-122">一个*namespace_declaration*包含关键字`namespace`后, 跟命名空间名称和正文后, 跟分号。</span><span class="sxs-lookup"><span data-stu-id="1318e-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="1318e-123">一个*namespace_declaration*中的顶级声明中可能产生*compilation_unit*或在另一个的成员声明为*namespace_declaration*。</span><span class="sxs-lookup"><span data-stu-id="1318e-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="1318e-124">当*namespace_declaration*作为中的顶级声明发生*compilation_unit*，该命名空间成为全局命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="1318e-125">当*namespace_declaration*出现在另一个*namespace_declaration*，内部命名空间成为外部命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="1318e-126">在任一情况下，命名空间名称必须是包含命名空间内唯一的。</span><span class="sxs-lookup"><span data-stu-id="1318e-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="1318e-127">命名空间为隐式`public`和命名空间声明不能包含任何访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="1318e-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="1318e-128">内*namespace_body*，可选*using_directive*s 导入其他命名空间、 类型和成员，使他们能够通过限定名而不是直接引用的名称。</span><span class="sxs-lookup"><span data-stu-id="1318e-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="1318e-129">可选*namespace_member_declaration*s 将成员分配到的命名空间声明空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="1318e-130">请注意，所有*using_directive*s 必须出现在之前的任何成员声明。</span><span class="sxs-lookup"><span data-stu-id="1318e-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="1318e-131">*Qualified_identifier*的*namespace_declaration*可能是单个标识符或分隔标识符一系列"`.`"令牌。</span><span class="sxs-lookup"><span data-stu-id="1318e-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="1318e-132">后一种窗体允许的程序，而不必从词法上嵌套多个命名空间声明中定义的嵌套命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="1318e-133">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="1318e-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="1318e-134">在语义上等效于</span><span class="sxs-lookup"><span data-stu-id="1318e-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="1318e-135">命名空间是可扩充的且具有相同的完全限定名称的两个命名空间声明参与到同一声明空间 ([声明](basic-concepts.md#declarations))。</span><span class="sxs-lookup"><span data-stu-id="1318e-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="1318e-136">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="1318e-137">上面的两个命名空间声明参与到同一声明空间，在这种情况下声明两个类的完全限定名称`N1.N2.A`和`N1.N2.B`。</span><span class="sxs-lookup"><span data-stu-id="1318e-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="1318e-138">由于向同一声明空间分配的两个声明，它就已出错，如果每个包含一个具有相同名称的成员的声明。</span><span class="sxs-lookup"><span data-stu-id="1318e-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="1318e-139">外部别名</span><span class="sxs-lookup"><span data-stu-id="1318e-139">Extern aliases</span></span>

<span data-ttu-id="1318e-140">*Extern_alias_directive*引入了可作为一个命名空间的别名的标识符。</span><span class="sxs-lookup"><span data-stu-id="1318e-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="1318e-141">外部程序的源代码和也适用于嵌套的命名空间的别名的命名空间的别名的命名空间的规范。</span><span class="sxs-lookup"><span data-stu-id="1318e-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="1318e-142">作用域*extern_alias_directive*通过进行了扩展*using_directive*s， *global_attributes*和*namespace_member_declaration*s 的直接包含编译单元或命名空间正文。</span><span class="sxs-lookup"><span data-stu-id="1318e-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="1318e-143">中包含的编译单元或命名空间体*extern_alias_directive*，通过引入的标识符*extern_alias_directive*可以用于引用别名的命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="1318e-144">它是编译时错误*标识符*为单词`global`。</span><span class="sxs-lookup"><span data-stu-id="1318e-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="1318e-145">*Extern_alias_directive*使别名可在特定的编译单元或命名空间体，但它不会影响到基础的声明空间的任何新成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="1318e-146">换而言之， *extern_alias_directive*不是可传递的但，而是只影响的编译单元或命名空间体其中发生。</span><span class="sxs-lookup"><span data-stu-id="1318e-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="1318e-147">下面的程序声明，并使用两个外部别名`X`和`Y`，每个表示不同的命名空间层次结构的根的：</span><span class="sxs-lookup"><span data-stu-id="1318e-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="1318e-148">该程序声明是否存在的外部别名`X`和`Y`，但实际定义的别名是外部的程序。</span><span class="sxs-lookup"><span data-stu-id="1318e-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="1318e-149">具有相同名称`N.B`现在可以作为引用类`X.N.B`并`Y.N.B`，或使用的命名空间别名限定符`X::N.B`和`Y::N.B`。</span><span class="sxs-lookup"><span data-stu-id="1318e-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="1318e-150">如果程序声明外部别名为其提供任何外部定义，就会出错。</span><span class="sxs-lookup"><span data-stu-id="1318e-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="1318e-151">using 指令</span><span class="sxs-lookup"><span data-stu-id="1318e-151">Using directives</span></span>

<span data-ttu-id="1318e-152">***Using 指令***便于您使用的命名空间和其他命名空间中定义的类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="1318e-153">使用的名称解析过程的指令影响*namespace_or_type_name*s ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 和*simple_name*s ([简单名称](expressions.md#simple-names))，而与声明中，使用指令并不参与到编译单元或在其中使用它们的命名空间的基础的声明空间的新成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="1318e-154">一个*using_alias_directive* ([Using 别名指令](namespaces.md#using-alias-directives)) 引入了命名空间或类型的别名。</span><span class="sxs-lookup"><span data-stu-id="1318e-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="1318e-155">一个*using_namespace_directive* ([使用的命名空间指令](namespaces.md#using-namespace-directives)) 导入的命名空间的类型成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="1318e-156">一个*using_static_directive* ([Using static 指令](namespaces.md#using-static-directives)) 导入的嵌套的类型和类型的静态成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="1318e-157">作用域*using_directive*通过进行了扩展*namespace_member_declaration*s 其直接包含的编译单元或命名空间体。</span><span class="sxs-lookup"><span data-stu-id="1318e-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="1318e-158">作用域*using_directive*专门不包括其对等方*using_directive*s。</span><span class="sxs-lookup"><span data-stu-id="1318e-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="1318e-159">因此，对等互连*using_directive*s 不影响，并且它们编写的顺序并不重要。</span><span class="sxs-lookup"><span data-stu-id="1318e-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="1318e-160">Using 别名指令</span><span class="sxs-lookup"><span data-stu-id="1318e-160">Using alias directives</span></span>

<span data-ttu-id="1318e-161">一个*using_alias_directive*引入了可作为命名空间或最近的封闭的编译单元或命名空间体中的类型的别名的标识符。</span><span class="sxs-lookup"><span data-stu-id="1318e-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="1318e-162">中包含的编译单元或命名空间体中的成员声明*using_alias_directive*，通过引入的标识符*using_alias_directive*可引用给定命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="1318e-163">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="1318e-164">更高版本中的成员声明中`N3`命名空间，`A`是其别名`N1.N2.A`，因此类`N3.B`派生自类`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="1318e-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="1318e-165">可以通过创建别名来获取相同的效果`R`有关`N1.N2`然后引用`R.A`:</span><span class="sxs-lookup"><span data-stu-id="1318e-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="1318e-166">*标识符*的*using_alias_directive*中的编译单元或立即包含命名空间声明空间必须是唯一*using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="1318e-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="1318e-167">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="1318e-168">更高版本，`N3`已有一个成员`A`，因此它是用于编译时错误*using_alias_directive*使用该标识符。</span><span class="sxs-lookup"><span data-stu-id="1318e-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="1318e-169">同样，它是两个或多个的编译时错误*using_alias_directive*s 同一编译单元或命名空间体中声明相同的名称的别名。</span><span class="sxs-lookup"><span data-stu-id="1318e-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="1318e-170">一个*using_alias_directive*使别名可在特定的编译单元或命名空间体，但它不会影响到基础的声明空间的任何新成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="1318e-171">换而言之， *using_alias_directive*不可传递，但而不是只影响的编译单元或命名空间体进行的。</span><span class="sxs-lookup"><span data-stu-id="1318e-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="1318e-172">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="1318e-173">作用域*using_alias_directive*引入`R`仅扩展到在其中包含它，命名空间体中的成员声明使`R`未知第二个命名空间声明中。</span><span class="sxs-lookup"><span data-stu-id="1318e-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="1318e-174">但是，放置*using_alias_directive*中包含的编译单元会导致为这两个命名空间声明内变为可用的别名：</span><span class="sxs-lookup"><span data-stu-id="1318e-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="1318e-175">就像常规成员名称引起*using_alias_directive*s 隐藏由嵌套作用域中的同名成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="1318e-176">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="1318e-177">对引用`R.A`中的声明`B`导致编译时错误，因为`R`指`N3.R`，而不`N1.N2`。</span><span class="sxs-lookup"><span data-stu-id="1318e-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="1318e-178">依据的顺序*using_alias_directive*s 是没有意义，和分辨率*namespace_or_type_name*所引用的*using_alias_directive*不会影响*using_alias_directive*本身或由其他*using_directive*中直接包含的编译单元或命名空间体。</span><span class="sxs-lookup"><span data-stu-id="1318e-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="1318e-179">换而言之， *namespace_or_type_name*的*using_alias_directive*得到解决，如同直接包含的编译单元或命名空间体具有无*using_directive*s。</span><span class="sxs-lookup"><span data-stu-id="1318e-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="1318e-180">一个*using_alias_directive*但是可能会受*extern_alias_directive*中直接包含的编译单元或命名空间体。</span><span class="sxs-lookup"><span data-stu-id="1318e-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="1318e-181">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="1318e-182">上次*using_alias_directive*导致编译时错误，因为它不受第一个*using_alias_directive*。</span><span class="sxs-lookup"><span data-stu-id="1318e-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="1318e-183">第一个*using_alias_directive*不会导致错误以来外部别名的作用域`E`包括*using_alias_directive*。</span><span class="sxs-lookup"><span data-stu-id="1318e-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="1318e-184">一个*using_alias_directive*可以创建任何命名空间或类型，包括在其中它将显示的命名空间的别名和任何命名空间或类型嵌套在该命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="1318e-185">通过别名访问命名空间或类型会产生与通过其声明的名称访问该命名空间或类型相同的结果。</span><span class="sxs-lookup"><span data-stu-id="1318e-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="1318e-186">例如，给定</span><span class="sxs-lookup"><span data-stu-id="1318e-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="1318e-187">名称`N1.N2.A`， `R1.N2.A`，并`R2.A`等效项和所有引用的类的完全限定的名称是`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="1318e-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="1318e-188">使用别名可以名称封闭式构造类型，但不提供类型实参的情况下无法名称未绑定的泛型类型声明。</span><span class="sxs-lookup"><span data-stu-id="1318e-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="1318e-189">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="1318e-190">使用命名空间指令</span><span class="sxs-lookup"><span data-stu-id="1318e-190">Using namespace directives</span></span>

<span data-ttu-id="1318e-191">一个*using_namespace_directive*导入到最近的封闭编译单元或命名空间正文，包含命名空间中启用要使用而无需限定每种类型的标识符的类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="1318e-192">中包含的编译单元或命名空间体中的成员声明*using_namespace_directive*，可以直接引用给定命名空间中包含的类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="1318e-193">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="1318e-194">更高版本中的成员声明中`N3`命名空间的类型成员`N1.N2`可直接使用，并因此类`N3.B`派生自类`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="1318e-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="1318e-195">一个*using_namespace_directive*导入给定的命名空间中包含的类型，但专门不导入嵌套的命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="1318e-196">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="1318e-197">*using_namespace_directive*中包含的类型导入`N1`，但命名空间不嵌套在`N1`。</span><span class="sxs-lookup"><span data-stu-id="1318e-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="1318e-198">因此，对引用`N2.A`中的声明`B`导致编译时错误，因为没有成员名为`N2`作用域中。</span><span class="sxs-lookup"><span data-stu-id="1318e-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="1318e-199">与不同*using_alias_directive*即*using_namespace_directive*可以导入标识符已定义封闭的编译单元或命名空间体中的类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="1318e-200">实际上，名称是通过导入*using_namespace_directive*封闭的编译单元或命名空间体中的同名成员隐藏的。</span><span class="sxs-lookup"><span data-stu-id="1318e-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="1318e-201">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="1318e-202">此处，在中的成员声明内`N3`命名空间，`A`是指`N3.A`而非`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="1318e-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="1318e-203">当多个命名空间或类型由导入*using_namespace_directive*s 或*using_static_directive*同一编译单元或命名空间体中包含相同的名称，对引用类型作为该名称*type_name*被视为不明确。</span><span class="sxs-lookup"><span data-stu-id="1318e-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="1318e-204">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="1318e-205">这两`N1`并`N2`包含成员`A`，并且因为`N3`导入两者，引用`A`中`N3`会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="1318e-206">在此情况下，冲突可以解决通过对引用的限定`A`，或通过引入*using_alias_directive*选取特定`A`。</span><span class="sxs-lookup"><span data-stu-id="1318e-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="1318e-207">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="1318e-208">此外，当多个命名空间或类型由导入*using_namespace_directive*s 或*using_static_directive*同一编译单元或命名空间体中包含类型或成员的条件对作为该名称引用相同的名称， *simple_name*被视为不明确。</span><span class="sxs-lookup"><span data-stu-id="1318e-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="1318e-209">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="1318e-210">`N1` 包含类型成员`A`，并`C`包含一个静态方法`A`，因为`N2`导入两者，引用`A`作为*simple_name*已不明确以及编译时出现错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-210">`N1` contains a type member `A`, and `C` contains a static method `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="1318e-211">像*using_alias_directive*即*using_namespace_directive*不参与到编译单元或命名空间的基础的声明空间的任何新成员，但而不是仅影响它在其中出现的编译单元或命名空间体。</span><span class="sxs-lookup"><span data-stu-id="1318e-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="1318e-212">*_ 名称*所引用的*using_namespace_directive*解析为相同的方式*namespace_or_type_name*所引用的*using_alias_directive*。</span><span class="sxs-lookup"><span data-stu-id="1318e-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="1318e-213">因此， *using_namespace_directive*同一编译单元或命名空间体中不会影响彼此和可以任何顺序写入。</span><span class="sxs-lookup"><span data-stu-id="1318e-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="1318e-214">Using static 指令</span><span class="sxs-lookup"><span data-stu-id="1318e-214">Using static directives</span></span>

<span data-ttu-id="1318e-215">一个*using_static_directive*导入的嵌套的类型和静态成员直接到最近的封闭编译单元或命名空间正文，类型声明中包含启用为每个成员和类型的标识符使用而无需限定。</span><span class="sxs-lookup"><span data-stu-id="1318e-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="1318e-216">中包含的编译单元或命名空间体中的成员声明*using_static_directive*，可以访问嵌套类型和直接的声明中所包含的静态成员 （除了扩展方法）给定的类型可以直接引用。</span><span class="sxs-lookup"><span data-stu-id="1318e-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="1318e-217">例如：</span><span class="sxs-lookup"><span data-stu-id="1318e-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="1318e-218">更高版本中的成员声明中`N2`静态成员和嵌套的类型的命名空间`N1.A`直接可用，因此该方法`N`能够同时引用`B`和`M`的成员`N1.A`.</span><span class="sxs-lookup"><span data-stu-id="1318e-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="1318e-219">一个*using_static_directive*专门不导入扩展的方法直接作为静态方法，但使其可用于扩展方法调用 ([扩展方法调用](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="1318e-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="1318e-220">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="1318e-221">*using_static_directive*导入的扩展方法`M`中包含`N1.A`，但仅作为扩展方法。</span><span class="sxs-lookup"><span data-stu-id="1318e-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="1318e-222">因此，对的第一个引用`M`中的正文`B.N`导致编译时错误，因为没有成员名为`M`作用域中。</span><span class="sxs-lookup"><span data-stu-id="1318e-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="1318e-223">一个*using_static_directive*只会导入成员并直接在给定类型中声明的类型在基类中声明未成员和类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="1318e-224">TODO： 示例</span><span class="sxs-lookup"><span data-stu-id="1318e-224">TODO: Example</span></span>

<span data-ttu-id="1318e-225">多个之间存在歧义*using_namespace_directives*并*using_static_directives*中讨论了[使用的命名空间指令](namespaces.md#using-namespace-directives)。</span><span class="sxs-lookup"><span data-stu-id="1318e-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="1318e-226">Namespace 成员</span><span class="sxs-lookup"><span data-stu-id="1318e-226">Namespace members</span></span>

<span data-ttu-id="1318e-227">一个*namespace_member_declaration*可以是*namespace_declaration* ([Namespace 声明](namespaces.md#namespace-declarations)) 或*type_declaration* ([的类型声明](namespaces.md#type-declarations))。</span><span class="sxs-lookup"><span data-stu-id="1318e-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="1318e-228">编译单元或命名空间体可以包含*namespace_member_declaration*s 和此类声明参与到包含编译单元或命名空间体的基础的声明空间的新成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="1318e-229">类型声明</span><span class="sxs-lookup"><span data-stu-id="1318e-229">Type declarations</span></span>

<span data-ttu-id="1318e-230">一个*type_declaration*是*class_declaration* ([类声明](classes.md#class-declarations))， *struct_declaration* ([结构声明](structs.md#struct-declarations))、 一个*interface_declaration* ([接口声明](interfaces.md#interface-declarations))、 一个*enum_declaration* ([枚举声明](enums.md#enum-declarations))，或*delegate_declaration* ([委托声明](delegates.md#delegate-declarations))。</span><span class="sxs-lookup"><span data-stu-id="1318e-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="1318e-231">一个*type_declaration*为编译单元中的顶级声明或命名空间、 类或结构中的成员声明可以出现。</span><span class="sxs-lookup"><span data-stu-id="1318e-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="1318e-232">当一种类型的类型声明`T`发生作为编译单元中的顶级声明的新声明类型的完全限定的名称只是`T`。</span><span class="sxs-lookup"><span data-stu-id="1318e-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="1318e-233">当一种类型的类型声明`T`发生的命名空间、 类或结构的新声明类型的完全限定名称是`N.T`，其中`N`是包含命名空间、 类或结构的完全限定的名称。</span><span class="sxs-lookup"><span data-stu-id="1318e-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="1318e-234">在类中声明的类型或结构称为嵌套的类型 ([嵌套类型](classes.md#nested-types))。</span><span class="sxs-lookup"><span data-stu-id="1318e-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="1318e-235">允许的访问修饰符和类型声明的默认访问权限取决于在其中声明发生处的上下文 ([声明可访问性](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="1318e-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="1318e-236">在编译单元或命名空间中声明的类型可以具有`public`或`internal`访问。</span><span class="sxs-lookup"><span data-stu-id="1318e-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="1318e-237">默认值是`internal`访问。</span><span class="sxs-lookup"><span data-stu-id="1318e-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="1318e-238">在类中声明的类型可以具有`public`， `protected internal`， `protected`， `internal`，或`private`访问。</span><span class="sxs-lookup"><span data-stu-id="1318e-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="1318e-239">默认值是`private`访问。</span><span class="sxs-lookup"><span data-stu-id="1318e-239">The default is `private` access.</span></span>
*  <span data-ttu-id="1318e-240">在结构中声明的类型可以具有`public`， `internal`，或`private`访问。</span><span class="sxs-lookup"><span data-stu-id="1318e-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="1318e-241">默认值是`private`访问。</span><span class="sxs-lookup"><span data-stu-id="1318e-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="1318e-242">Namespace 别名限定符</span><span class="sxs-lookup"><span data-stu-id="1318e-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="1318e-243">***命名空间别名限定符***`::`就可保证类型名称查找不受引入新类型和成员。</span><span class="sxs-lookup"><span data-stu-id="1318e-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="1318e-244">命名空间别名限定符始终出现在称为左侧和右侧标识符的两个标识符之间。</span><span class="sxs-lookup"><span data-stu-id="1318e-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="1318e-245">与普通`.`限定符，左侧标识符`::`查找限定符最多只能作为外部或使用别名。</span><span class="sxs-lookup"><span data-stu-id="1318e-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="1318e-246">一个*qualified_alias_member*定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1318e-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="1318e-247">一个*qualified_alias_member*可用作*namespace_or_type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 或作为中的左操作数*member_access*([成员访问](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="1318e-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="1318e-248">一个*qualified_alias_member*有两种形式之一：</span><span class="sxs-lookup"><span data-stu-id="1318e-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="1318e-249">`N::I<A1, ..., Ak>`其中`N`并`I`表示的标识符，，和`<A1, ..., Ak>`是类型参数列表。</span><span class="sxs-lookup"><span data-stu-id="1318e-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="1318e-250">(`K`始终是至少一个。)</span><span class="sxs-lookup"><span data-stu-id="1318e-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="1318e-251">`N::I`其中`N`和`I`表示标识符。</span><span class="sxs-lookup"><span data-stu-id="1318e-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="1318e-252">(在这种情况下，`K`被视为零。)</span><span class="sxs-lookup"><span data-stu-id="1318e-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="1318e-253">使用此表示法的含义*qualified_alias_member* ，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="1318e-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="1318e-254">如果`N`的标识符`global`，然后搜索全局命名空间`I`:</span><span class="sxs-lookup"><span data-stu-id="1318e-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="1318e-255">如果全局命名空间包含名为命名空间`I`并`K`为零，则*qualified_alias_member*指的是该命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="1318e-256">否则为如果全局命名空间包含一个名为的非泛型类型`I`并`K`为零，则*qualified_alias_member*引用该类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="1318e-257">否则为如果全局命名空间包含一个名为类型`I`，其`K`类型参数，则*qualified_alias_member*构造具有给定的类型参数的该类型是指。</span><span class="sxs-lookup"><span data-stu-id="1318e-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="1318e-258">否则为*qualified_alias_member*是未定义，且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="1318e-259">否则，从命名空间声明 ([Namespace 声明](namespaces.md#namespace-declarations)) 直接包含*qualified_alias_member* （如果有）、 执行完每个封闭命名空间声明（如果有），并包含在编译单元结尾*qualified_alias_member*，以下步骤进行计算，直到找到一个实体就是：</span><span class="sxs-lookup"><span data-stu-id="1318e-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="1318e-260">如果命名空间声明或编译单元中包含*using_alias_directive*将关联`N`类型，则*qualified_alias_member*未定义和编译时间出现错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="1318e-261">否则为如果命名空间声明或编译单元包含*extern_alias_directive*或*using_alias_directive*将关联`N`与命名空间，然后：</span><span class="sxs-lookup"><span data-stu-id="1318e-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="1318e-262">如果与关联的命名空间`N`包含名为的命名空间`I`并`K`为零，则*qualified_alias_member*指的是该命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="1318e-263">否则为如果与关联的命名空间`N`包含一个名为的非泛型类型`I`并`K`为零，则*qualified_alias_member*引用该类型。</span><span class="sxs-lookup"><span data-stu-id="1318e-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="1318e-264">否则为如果与关联的命名空间`N`包含名为的类型`I`具有`K`类型参数，则*qualified_alias_member*构造具有给定类型的该类型是指自变量。</span><span class="sxs-lookup"><span data-stu-id="1318e-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="1318e-265">否则为*qualified_alias_member*是未定义，且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="1318e-266">否则为*qualified_alias_member*是未定义，且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="1318e-267">请注意，命名空间别名限定符与引用的类型的别名一起使用会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="1318e-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="1318e-268">此外请注意，如果标识符`N`是`global`，然后在全局命名空间中执行查找，即使没有使用别名将关联`global`与类型或命名空间。</span><span class="sxs-lookup"><span data-stu-id="1318e-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="1318e-269">别名的唯一性</span><span class="sxs-lookup"><span data-stu-id="1318e-269">Uniqueness of aliases</span></span>

<span data-ttu-id="1318e-270">每个编译单元和命名空间正文具有外部别名的单独声明空间和使用别名。</span><span class="sxs-lookup"><span data-stu-id="1318e-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="1318e-271">因此，虽然外部别名或使用别名的名称必须是唯一的外部别名集内，且使用别名声明直接包含的编译单元或命名空间体中，别名允许具有相同的名称作为类型或命名空间，只要我t 只能用于`::`限定符。</span><span class="sxs-lookup"><span data-stu-id="1318e-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="1318e-272">在示例</span><span class="sxs-lookup"><span data-stu-id="1318e-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="1318e-273">名称`A`第二个命名空间正文中具有两个可能的含义，因为这两个类`A`和 using 别名`A`作用域中。</span><span class="sxs-lookup"><span data-stu-id="1318e-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="1318e-274">出于此原因，利用`A`限定名称中`A.Stream`不明确，会导致编译时错误发生。</span><span class="sxs-lookup"><span data-stu-id="1318e-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="1318e-275">但是，利用`A`与`::`限定符不是一个错误，因为`A`查找仅作为命名空间别名。</span><span class="sxs-lookup"><span data-stu-id="1318e-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
