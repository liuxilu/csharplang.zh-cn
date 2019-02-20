# <a name="namespaces"></a>命名空间

C# 程序的组织结构使用的命名空间。 使用命名空间为"内部"组织系统的程序，并作为"外部"组织系统 — 一种方法提供向其他程序公开的程序元素。

Using 指令 ([Using 指令](namespaces.md#using-directives)) 提供，以便命名空间的使用。

## <a name="compilation-units"></a>编译单元

一个*compilation_unit*定义源文件的整体结构。 编译单元包含零个或多*using_directive*s 后跟零个或多*global_attributes*跟零个或多*namespace_member_declaration*s.

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

C# 程序包含一个或多个编译单元，每个包含在单独的源代码文件中。 编译 C# 程序时，所有编译单元一起进行处理。 因此，编译单元可以依赖于彼此，可能是以循环方式。

*Using_directive*秒的编译单元影响*global_attributes*并*namespace_member_declaration*s 该编译单元，但不起任何作用其他编译单元。

*Global_attributes* ([属性](attributes.md)) 编译单元的允许的目标程序集和模块的属性的规范。 程序集和模块作为物理容器的类型。 程序集可能包含多个物理上独立的模块。

*Namespace_member_declaration*s 程序的每个编译单元将成员分配到名为全局命名空间的单个声明空间。 例如：

文件`A.cs`:
```csharp
class A {}
```

文件`B.cs`:
```csharp
class B {}
```

两个编译单元中分配到单一的全局命名空间，在这种情况下声明两个类的完全限定名称`A`和`B`。 因为这两个编译单元中分配到同一声明空间，重要的是成员的一个错误如果每个包含具有相同名称的声明。

## <a name="namespace-declarations"></a>命名空间声明

一个*namespace_declaration*包含关键字`namespace`后, 跟命名空间名称和正文后, 跟分号。

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

一个*namespace_declaration*中的顶级声明中可能产生*compilation_unit*或在另一个的成员声明为*namespace_declaration*。 当*namespace_declaration*作为中的顶级声明发生*compilation_unit*，该命名空间成为全局命名空间的成员。 当*namespace_declaration*出现在另一个*namespace_declaration*，内部命名空间成为外部命名空间的成员。 在任一情况下，命名空间名称必须是包含命名空间内唯一的。

命名空间为隐式`public`和命名空间声明不能包含任何访问修饰符。

内*namespace_body*，可选*using_directive*s 导入其他命名空间、 类型和成员，使他们能够通过限定名而不是直接引用的名称。 可选*namespace_member_declaration*s 将成员分配到的命名空间声明空间。 请注意，所有*using_directive*s 必须出现在之前的任何成员声明。

*Qualified_identifier*的*namespace_declaration*可能是单个标识符或分隔标识符一系列"`.`"令牌。 后一种窗体允许的程序，而不必从词法上嵌套多个命名空间声明中定义的嵌套命名空间。 例如，应用于对象的

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
在语义上等效于
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

命名空间是可扩充的且具有相同的完全限定名称的两个命名空间声明参与到同一声明空间 ([声明](basic-concepts.md#declarations))。 在示例
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
上面的两个命名空间声明参与到同一声明空间，在这种情况下声明两个类的完全限定名称`N1.N2.A`和`N1.N2.B`。 由于向同一声明空间分配的两个声明，它就已出错，如果每个包含一个具有相同名称的成员的声明。

## <a name="extern-aliases"></a>外部别名

*Extern_alias_directive*引入了可作为一个命名空间的别名的标识符。 外部程序的源代码和也适用于嵌套的命名空间的别名的命名空间的别名的命名空间的规范。

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

作用域*extern_alias_directive*通过进行了扩展*using_directive*s， *global_attributes*和*namespace_member_declaration*s 的直接包含编译单元或命名空间正文。

中包含的编译单元或命名空间体*extern_alias_directive*，通过引入的标识符*extern_alias_directive*可以用于引用别名的命名空间。 它是编译时错误*标识符*为单词`global`。

*Extern_alias_directive*使别名可在特定的编译单元或命名空间体，但它不会影响到基础的声明空间的任何新成员。 换而言之， *extern_alias_directive*不是可传递的但，而是只影响的编译单元或命名空间体其中发生。

下面的程序声明，并使用两个外部别名`X`和`Y`，每个表示不同的命名空间层次结构的根的：
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

该程序声明是否存在的外部别名`X`和`Y`，但实际定义的别名是外部的程序。 具有相同名称`N.B`现在可以作为引用类`X.N.B`并`Y.N.B`，或使用的命名空间别名限定符`X::N.B`和`Y::N.B`。 如果程序声明外部别名为其提供任何外部定义，就会出错。

## <a name="using-directives"></a>using 指令

***Using 指令***便于您使用的命名空间和其他命名空间中定义的类型。 使用的名称解析过程的指令影响*namespace_or_type_name*s ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 和*simple_name*s ([简单名称](expressions.md#simple-names))，而与声明中，使用指令并不参与到编译单元或在其中使用它们的命名空间的基础的声明空间的新成员。

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

一个*using_alias_directive* ([Using 别名指令](namespaces.md#using-alias-directives)) 引入了命名空间或类型的别名。

一个*using_namespace_directive* ([使用的命名空间指令](namespaces.md#using-namespace-directives)) 导入的命名空间的类型成员。

一个*using_static_directive* ([Using static 指令](namespaces.md#using-static-directives)) 导入的嵌套的类型和类型的静态成员。

作用域*using_directive*通过进行了扩展*namespace_member_declaration*s 其直接包含的编译单元或命名空间体。 作用域*using_directive*专门不包括其对等方*using_directive*s。 因此，对等互连*using_directive*s 不影响，并且它们编写的顺序并不重要。

### <a name="using-alias-directives"></a>Using 别名指令

一个*using_alias_directive*引入了可作为命名空间或最近的封闭的编译单元或命名空间体中的类型的别名的标识符。

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

中包含的编译单元或命名空间体中的成员声明*using_alias_directive*，通过引入的标识符*using_alias_directive*可引用给定命名空间或类型。 例如：
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

更高版本中的成员声明中`N3`命名空间，`A`是其别名`N1.N2.A`，因此类`N3.B`派生自类`N1.N2.A`。 可以通过创建别名来获取相同的效果`R`有关`N1.N2`然后引用`R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

*标识符*的*using_alias_directive*中的编译单元或立即包含命名空间声明空间必须是唯一*using_alias_directive*. 例如：
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

更高版本，`N3`已有一个成员`A`，因此它是用于编译时错误*using_alias_directive*使用该标识符。 同样，它是两个或多个的编译时错误*using_alias_directive*s 同一编译单元或命名空间体中声明相同的名称的别名。

一个*using_alias_directive*使别名可在特定的编译单元或命名空间体，但它不会影响到基础的声明空间的任何新成员。 换而言之， *using_alias_directive*不可传递，但而不是只影响的编译单元或命名空间体进行的。 在示例
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
作用域*using_alias_directive*引入`R`仅扩展到在其中包含它，命名空间体中的成员声明使`R`未知第二个命名空间声明中。 但是，放置*using_alias_directive*中包含的编译单元会导致为这两个命名空间声明内变为可用的别名：
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

就像常规成员名称引起*using_alias_directive*s 隐藏由嵌套作用域中的同名成员。 在示例
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
对引用`R.A`中的声明`B`导致编译时错误，因为`R`指`N3.R`，而不`N1.N2`。

依据的顺序*using_alias_directive*s 是没有意义，和分辨率*namespace_or_type_name*所引用的*using_alias_directive*不会影响*using_alias_directive*本身或由其他*using_directive*中直接包含的编译单元或命名空间体。 换而言之， *namespace_or_type_name*的*using_alias_directive*得到解决，如同直接包含的编译单元或命名空间体具有无*using_directive*s。 一个*using_alias_directive*但是可能会受*extern_alias_directive*中直接包含的编译单元或命名空间体。 在示例
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
上次*using_alias_directive*导致编译时错误，因为它不受第一个*using_alias_directive*。 第一个*using_alias_directive*不会导致错误以来外部别名的作用域`E`包括*using_alias_directive*。

一个*using_alias_directive*可以创建任何命名空间或类型，包括在其中它将显示的命名空间的别名和任何命名空间或类型嵌套在该命名空间。

通过别名访问命名空间或类型会产生与通过其声明的名称访问该命名空间或类型相同的结果。 例如，给定
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
名称`N1.N2.A`， `R1.N2.A`，并`R2.A`等效项和所有引用的类的完全限定的名称是`N1.N2.A`。

使用别名可以名称封闭式构造类型，但不提供类型实参的情况下无法名称未绑定的泛型类型声明。 例如：
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

### <a name="using-namespace-directives"></a>使用命名空间指令

一个*using_namespace_directive*导入到最近的封闭编译单元或命名空间正文，包含命名空间中启用要使用而无需限定每种类型的标识符的类型。

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

中包含的编译单元或命名空间体中的成员声明*using_namespace_directive*，可以直接引用给定命名空间中包含的类型。 例如：
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

更高版本中的成员声明中`N3`命名空间的类型成员`N1.N2`可直接使用，并因此类`N3.B`派生自类`N1.N2.A`。

一个*using_namespace_directive*导入给定的命名空间中包含的类型，但专门不导入嵌套的命名空间。 在示例
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
*using_namespace_directive*中包含的类型导入`N1`，但命名空间不嵌套在`N1`。 因此，对引用`N2.A`中的声明`B`导致编译时错误，因为没有成员名为`N2`作用域中。

与不同*using_alias_directive*即*using_namespace_directive*可以导入标识符已定义封闭的编译单元或命名空间体中的类型。 实际上，名称是通过导入*using_namespace_directive*封闭的编译单元或命名空间体中的同名成员隐藏的。 例如：
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

此处，在中的成员声明内`N3`命名空间，`A`是指`N3.A`而非`N1.N2.A`。

当多个命名空间或类型由导入*using_namespace_directive*s 或*using_static_directive*同一编译单元或命名空间体中包含相同的名称，对引用类型作为该名称*type_name*被视为不明确。 在示例
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
这两`N1`并`N2`包含成员`A`，并且因为`N3`导入两者，引用`A`中`N3`会导致编译时错误。 在此情况下，冲突可以解决通过对引用的限定`A`，或通过引入*using_alias_directive*选取特定`A`。 例如：
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

此外，当多个命名空间或类型由导入*using_namespace_directive*s 或*using_static_directive*同一编译单元或命名空间体中包含类型或成员的条件对作为该名称引用相同的名称， *simple_name*被视为不明确。 在示例
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
`N1` 包含类型成员`A`，并`C`包含静态字段`A`，并且因为`N2`导入两者，引用`A`作为*simple_name*已不明确以及编译时出现错误。 

像*using_alias_directive*即*using_namespace_directive*不参与到编译单元或命名空间的基础的声明空间的任何新成员，但而不是仅影响它在其中出现的编译单元或命名空间体。

*_ 名称*所引用的*using_namespace_directive*解析为相同的方式*namespace_or_type_name*所引用的*using_alias_directive*。 因此， *using_namespace_directive*同一编译单元或命名空间体中不会影响彼此和可以任何顺序写入。

### <a name="using-static-directives"></a>Using static 指令

一个*using_static_directive*导入的嵌套的类型和静态成员直接到最近的封闭编译单元或命名空间正文，类型声明中包含启用为每个成员和类型的标识符使用而无需限定。

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

中包含的编译单元或命名空间体中的成员声明*using_static_directive*，可以访问嵌套类型和直接的声明中所包含的静态成员 （除了扩展方法）给定的类型可以直接引用。 例如：

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

更高版本中的成员声明中`N2`静态成员和嵌套的类型的命名空间`N1.A`直接可用，因此该方法`N`能够同时引用`B`和`M`的成员`N1.A`.

一个*using_static_directive*专门不导入扩展的方法直接作为静态方法，但使其可用于扩展方法调用 ([扩展方法调用](expressions.md#extension-method-invocations))。 在示例

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
*using_static_directive*导入的扩展方法`M`中包含`N1.A`，但仅作为扩展方法。 因此，对的第一个引用`M`中的正文`B.N`导致编译时错误，因为没有成员名为`M`作用域中。

一个*using_static_directive*只会导入成员并直接在给定类型中声明的类型在基类中声明未成员和类型。

TODO：示例

多个之间存在歧义*using_namespace_directives*并*using_static_directives*中讨论了[使用的命名空间指令](namespaces.md#using-namespace-directives)。

## <a name="namespace-members"></a>Namespace 成员

一个*namespace_member_declaration*可以是*namespace_declaration* ([Namespace 声明](namespaces.md#namespace-declarations)) 或*type_declaration* ([的类型声明](namespaces.md#type-declarations))。

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

编译单元或命名空间体可以包含*namespace_member_declaration*s 和此类声明参与到包含编译单元或命名空间体的基础的声明空间的新成员。

## <a name="type-declarations"></a>类型声明

一个*type_declaration*是*class_declaration* ([类声明](classes.md#class-declarations))， *struct_declaration* ([结构声明](structs.md#struct-declarations))、 一个*interface_declaration* ([接口声明](interfaces.md#interface-declarations))、 一个*enum_declaration* ([枚举声明](enums.md#enum-declarations))，或*delegate_declaration* ([委托声明](delegates.md#delegate-declarations))。

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

一个*type_declaration*为编译单元中的顶级声明或命名空间、 类或结构中的成员声明可以出现。

当一种类型的类型声明`T`发生作为编译单元中的顶级声明的新声明类型的完全限定的名称只是`T`。 当一种类型的类型声明`T`发生的命名空间、 类或结构的新声明类型的完全限定名称是`N.T`，其中`N`是包含命名空间、 类或结构的完全限定的名称。

在类中声明的类型或结构称为嵌套的类型 ([嵌套类型](classes.md#nested-types))。

允许的访问修饰符和类型声明的默认访问权限取决于在其中声明发生处的上下文 ([声明可访问性](basic-concepts.md#declared-accessibility)):

*  在编译单元或命名空间中声明的类型可以具有`public`或`internal`访问。 默认值是`internal`访问。
*  在类中声明的类型可以具有`public`， `protected internal`， `protected`， `internal`，或`private`访问。 默认值是`private`访问。
*  在结构中声明的类型可以具有`public`， `internal`，或`private`访问。 默认值是`private`访问。

## <a name="namespace-alias-qualifiers"></a>Namespace 别名限定符

***命名空间别名限定符***`::`就可保证类型名称查找不受引入新类型和成员。 命名空间别名限定符始终出现在称为左侧和右侧标识符的两个标识符之间。 与普通`.`限定符，左侧标识符`::`查找限定符最多只能作为外部或使用别名。

一个*qualified_alias_member*定义，如下所示：

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

一个*qualified_alias_member*可用作*namespace_or_type_name* ([Namespace 和类型名称](basic-concepts.md#namespace-and-type-names)) 或作为中的左操作数*member_access*([成员访问](expressions.md#member-access))。

一个*qualified_alias_member*有两种形式之一：

*  `N::I<A1, ..., Ak>`其中`N`并`I`表示的标识符，，和`<A1, ..., Ak>`是类型参数列表。 (`K`始终是至少一个。)
*  `N::I`其中`N`和`I`表示标识符。 (在这种情况下，`K`被视为零。)

使用此表示法的含义*qualified_alias_member* ，如下所示确定：

*  如果`N`的标识符`global`，然后搜索全局命名空间`I`:
   * 如果全局命名空间包含名为命名空间 `I`并`K`为零，则*qualified_alias_member*指的是该命名空间。
   * 否则为如果全局命名空间包含一个名为的非泛型类型 `I`并`K`为零，则*qualified_alias_member*引用该类型。
   * 否则为如果全局命名空间包含一个名为类型 `I`，其`K` 类型参数，则*qualified_alias_member*构造具有给定的类型参数的该类型是指。
   * 否则为*qualified_alias_member*是未定义，且将发生编译时错误。

*  否则，从命名空间声明 ([Namespace 声明](namespaces.md#namespace-declarations)) 直接包含*qualified_alias_member* （如果有）、 执行完每个封闭命名空间声明（如果有），并包含在编译单元结尾*qualified_alias_member*，以下步骤进行计算，直到找到一个实体就是：

   * 如果命名空间声明或编译单元中包含*using_alias_directive*将关联`N`类型，则*qualified_alias_member*未定义和编译时间出现错误。
   * 否则为如果命名空间声明或编译单元包含*extern_alias_directive*或*using_alias_directive*将关联`N`与命名空间，然后：
     * 如果与关联的命名空间`N`包含名为的命名空间 `I`并`K`为零，则*qualified_alias_member*指的是该命名空间。
     * 否则为如果与关联的命名空间`N`包含一个名为的非泛型类型 `I`并`K`为零，则*qualified_alias_member*引用该类型。
     * 否则为如果与关联的命名空间`N`包含名为的类型 `I`具有`K` 类型参数，则*qualified_alias_member*指的是使用构造类型给定的类型的参数。
     * 否则为*qualified_alias_member*是未定义，且将发生编译时错误。
*  否则为*qualified_alias_member*是未定义，且将发生编译时错误。

请注意，命名空间别名限定符与引用的类型的别名一起使用会导致编译时错误。 此外请注意，如果标识符 `N`是`global`，然后在全局命名空间中执行查找，即使没有使用别名将关联`global`与类型或命名空间。

### <a name="uniqueness-of-aliases"></a>别名的唯一性

每个编译单元和命名空间正文具有外部别名的单独声明空间和使用别名。 因此，虽然外部别名或使用别名的名称必须是唯一的外部别名集内，且使用别名声明直接包含的编译单元或命名空间体中，别名允许具有相同的名称作为类型或命名空间，只要我t 只能用于`::`限定符。

在示例
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
名称`A`第二个命名空间正文中具有两个可能的含义，因为这两个类`A`和 using 别名`A`作用域中。 出于此原因，利用`A`限定名称中`A.Stream`不明确，会导致编译时错误发生。 但是，利用`A`与`::`限定符不是一个错误，因为`A`查找仅作为命名空间别名。
