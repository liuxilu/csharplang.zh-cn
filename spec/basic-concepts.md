---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488492"
---
# <a name="basic-concepts"></a>基本概念

## <a name="application-startup"></a>应用程序启动

具有的程序集***入口点***称为***应用程序***。 应用程序时运行，一个新***应用程序域***创建。 多个不同的实例化的应用程序可能存在同一台计算机上一次，每个都有其自己的应用程序域。

应用程序域启用通过充当应用程序状态的容器应用程序隔离。 应用程序域充当容器和应用程序和它使用类库中定义的类型的边界。 加载到一个应用程序域的类型是不同于相同类型加载到另一个应用程序域，并应用程序域之间不直接共享的对象的实例。 例如，每个应用程序域具有其自己的对于这些类型的静态变量副本，并针对每个应用程序域最多一次运行一种类型的静态构造函数。 实现可以自由地创建和销毁的应用程序域提供特定于实现的策略或机制。

***应用程序启动***发生时执行环境调用指定的方法，这被称为应用程序的入口点。 此入口点方法始终命名为`Main`，并且可以具有以下签名之一：

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

如所示，入口点可能会选择性地返回`int`值。 此返回值用于应用程序终止 ([应用程序终止时](basic-concepts.md#application-termination))。

入口点 （可选） 可能具有一个形参。 该参数可以具有任何名称，但是参数的类型必须为`string[]`。 如果存在的形式参数，则创建执行环境，并将传递`string[]`参数，其中包含了命令行参数指定应用程序启动时。 `string[]`参数永远不会为 null，但它可能具有长度为零，如果未不指定任何命令行参数。

由于 C# 支持重载的方法，类或结构可能包含多个定义的一些方法，提供每个具有不同的签名。 但是，单个程序内没有任何类或结构可能包含多个方法调用`Main`其定义这使得它要用作应用程序入口点。 其他重载的版本`Main`允许使用，但是，只要它们具有多个参数，或者其唯一参数类型不是`string[]`。

应用程序可以组成的多个类或结构。 就可以为多个这些类或结构的以包含一个名为方法`Main`其定义这使得它要用作应用程序入口点。 在这种情况下，必须使用外部机制 （如命令行编译器选项） 来选择其中一种`Main`方法作为入口点。

在 C# 中，每个方法必须定义为类或结构的成员。 通常，声明可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)) 的一种方法由访问修饰符 ([访问修饰符](classes.md#access-modifiers)) 在其声明和与此类似的声明中指定一种类型的可访问性取决于其声明中指定的访问修饰符。 为了可调用给定类型的给定方法，该类型和成员都必须是可访问的。 但是，应用程序入口点是一种特殊情况。 具体而言，执行环境可以访问应用程序的入口点而不考虑其声明的可访问性，而不考虑其封闭类型声明的声明可访问性。

应用程序入口点方法可能不在泛型类声明。

所有其他方面，入口点方法的行为方式类似的不是入口点。

## <a name="application-termination"></a>应用程序终止

***应用程序终止时***将控制权返回给执行环境。

如果在应用程序的返回类型***入口点***方法是`int`，返回的值用作应用程序的***终止状态代码***。 此代码的目的是允许进行通信的成功或失败的执行环境。

如果入口点方法的返回类型是`void`，到达右大括号 (`}`) 终止的方法，或执行`return`没有任何表达式的语句会导致终止状态代码`0`。

在应用程序的终止之前的所有尚未进行垃圾回收的对象的析构函数将调用，除非已取消这类清理 (通过调用库方法`GC.SuppressFinalize`，例如)。

## <a name="declarations"></a>声明

C# 程序中的声明定义该程序的构成元素。 C# 程序的组织结构使用的命名空间 ([命名空间](namespaces.md))，其中包含类型声明和嵌套命名空间声明。 类型声明 ([的类型声明](namespaces.md#type-declarations)) 用于定义类 ([类](classes.md))，结构 ([结构](structs.md))，接口 ([接口](interfaces.md))，枚举 ([枚举](enums.md))，和委托 ([委托](delegates.md))。 类型声明中允许的成员种类取决于类型声明的窗体。 例如，类声明可以包含常量的声明 ([常量](classes.md#constants))，字段 ([字段](classes.md#fields))，方法 ([方法](classes.md#methods))，属性 ([属性](classes.md#properties))，事件 ([事件](classes.md#events))，索引器 ([索引器](classes.md#indexers))，运算符 ([运算符](classes.md#operators))，实例构造函数 ([实例构造函数](classes.md#instance-constructors))，静态构造函数 ([静态构造函数](classes.md#static-constructors))，析构函数 ([析构函数](classes.md#destructors))，和嵌套类型 ([嵌套类型](classes.md#nested-types)).

声明定义中的名称***声明空间***所属声明。 除重载成员 ([签名和超载](basic-concepts.md#signatures-and-overloading))，它会导致编译时错误有两个或多个具有相同的名称声明空间中的成员的声明。 就永远不会可以声明空间，以包含不同类型的成员具有相同的名称。 例如，声明空间可以永远不会有一个字段和方法按相同的名称。

有多种不同类型的声明空间，如以下所述。

*  中的程序的所有源文件*namespace_member_declaration*与没有封闭*namespace_declaration*成员的调用的单个组合的声明空间***全局声明空间***。
*  中的程序的所有源文件*namespace_member_declaration*中的 s *namespace_declaration*具有相同的完全限定的命名空间名称是单个组合声明的成员空间。
*  每个类、 结构或接口声明创建一个新的声明空间。 名称引入到通过此声明空间*class_member_declaration*s， *struct_member_declaration*s， *interface_member_declaration*s，或*type_parameter*s。 除了重载的实例构造函数声明和声明、 类或结构的静态构造函数不能包含具有与类或结构同名的成员声明。 类、 结构或接口允许重载的方法和索引器的声明。 此外，类或结构允许重载的实例构造函数和运算符的声明。 例如，类、 结构或接口可能包含多个方法声明具有相同的名称，前提是这些方法声明其签名中不同 ([签名和超载](basic-concepts.md#signatures-and-overloading))。 请注意，基类，这些类并不影响为声明空间的类的基接口并不参与到接口声明空间。 因此，派生的类或接口被允许声明一个具有相同的名称为继承的成员的成员。 这样的成员可以说负责***隐藏***继承的成员。
*  每个委托声明创建一个新的声明空间。 名称引入到形式参数通过此声明空间 (*fixed_parameter*s 和*parameter_array*s) 和*type_parameter*s。
*  每个枚举声明创建一个新的声明空间。 名称引入到通过此声明空间*enum_member_declarations*。
*  每个方法声明、 索引器声明、 运算符声明、 实例构造函数声明和匿名函数创建一个名为的新声明空间***局部变量声明空间***。 名称引入到形式参数通过此声明空间 (*fixed_parameter*s 和*parameter_array*s) 和*type_parameter*s。 正文的函数成员或匿名函数，如果有的话，被视为嵌套在局部变量声明空间。 它是错误的本地变量声明空间和嵌套的本地变量声明空间包含具有相同名称的元素。 因此，在嵌套的声明空间中不可能在封闭的声明空间中声明本地变量或常量具有相同名称为本地变量或常量。 它是两个声明空间以包含具有相同名称的元素，只要声明空间彼此互不包含。
*  每个*块*或*switch_block* ，以及*有关*， *foreach*并*使用*语句，创建为本地变量和局部常量局部变量声明空间。 名称引入到通过此声明空间*local_variable_declaration*s 和*local_constant_declaration*s。 请注意为或的函数成员或匿名函数体中出现的块均嵌套在其参数为这些函数的声明的局部变量声明空间。 因此，它是错误包含例如具有本地变量与具有相同名称的参数的方法。
*  每个*块*或*switch_block*创建标签的单独声明空间。 名称引入到通过此声明空间*labeled_statement*通过引用和名称*goto_statement*s。 ***标记声明空间***块的包含任何嵌套的块。 因此，在嵌套的块中不可以声明具有相同的名称在封闭块中的标签的标签。

在其中声明名称的文本顺序通常是没有意义。 具体而言，文本顺序并不重要声明和使用的命名空间、 常量、 方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数、 静态构造函数和类型。 按声明顺序非常重要的以下方法：

*  字段声明和局部变量声明的声明顺序确定其初始值设定项 （如果有） 的执行顺序。
*  在使用之前，必须定义本地变量 ([作用域](basic-concepts.md#scopes))。
*  枚举成员声明的声明顺序 ([枚举成员](enums.md#enum-members)) 非常重要*constant_expression*会省略值。

命名空间声明空间为"打开已结束"，和两个命名空间声明具有相同的完全限定名称分配到同一声明空间。 例如
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

上面的两个命名空间声明参与到同一声明空间，在这种情况下声明两个类的完全限定名称`Megacorp.Data.Customer`和`Megacorp.Data.Order`。 由于向同一声明空间分配的两个声明，则将导致编译时错误如果每个包含具有相同名称的类的声明。

以上指定的块声明空间包含任何嵌套的块。 因此，在以下示例中，`F`并`G`方法导致在编译时错误，因为名称`i`在外部块中声明并不能在内部块中重新声明。 但是，`H`并`I`以来这两个方法都有效`i`的在单独的非嵌套块中声明。

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>成员

命名空间和类型具有***成员***。 实体的成员是通常可以通过对实体的引用开头跟限定名"`.`"令牌后, 跟的成员的名称。

或者在类型声明中声明类型的成员或***继承***从类型的基类。 当一个类型继承自基类的基类，但实例构造函数、 析构函数和静态构造函数，所有成员成为所派生的类型的成员。 基类成员的声明可访问性不会控制是否继承成员 — 继承将扩展到任何不是实例构造函数、 静态构造函数或析构函数的成员。 但是，继承的成员可能无法访问中的派生类型，原因是其声明的可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)) 或者是因为它隐藏在类型本身是声明 ([通过隐藏继承](basic-concepts.md#hiding-through-inheritance))。

### <a name="namespace-members"></a>Namespace 成员

命名空间和具有没有封闭命名空间的类型是的成员***全局命名空间***。 这直接对应于全局声明空间中声明的名称。

命名空间和命名空间内声明的类型是该命名空间的成员。 这直接对应于该命名空间的声明空间中声明的名称。

命名空间没有任何访问限制。 不能声明私有、 受保护或内部命名空间和命名空间名称始终是可公开访问。

### <a name="struct-members"></a>结构成员

结构的成员是该结构中声明的成员和继承结构的直接基类的成员`System.ValueType`和间接基类`object`。

简单类型的成员直接对应于由简单类型的结构类型别名的成员：

*  成员`sbyte`成员的`System.SByte`结构。
*  成员`byte`成员的`System.Byte`结构。
*  成员`short`成员的`System.Int16`结构。
*  成员`ushort`成员的`System.UInt16`结构。
*  成员`int`成员的`System.Int32`结构。
*  成员`uint`成员的`System.UInt32`结构。
*  成员`long`成员的`System.Int64`结构。
*  成员`ulong`成员的`System.UInt64`结构。
*  成员`char`成员的`System.Char`结构。
*  成员`float`成员的`System.Single`结构。
*  成员`double`成员的`System.Double`结构。
*  成员`decimal`成员的`System.Decimal`结构。
*  成员`bool`成员的`System.Boolean`结构。

### <a name="enumeration-members"></a>枚举成员

枚举的成员是在枚举中声明的常量和从枚举的直接基类继承的成员`System.Enum`和间接的基类`System.ValueType`和`object`。

### <a name="class-members"></a>类成员

类的成员是在类中声明的成员和继承自的基类的成员 (除了类`object`具有没有基类)。 从基类继承的成员包括常量、 字段、 方法、 属性、 事件、 索引器、 运算符和类型的基类，但不是实例构造函数、 析构函数和静态构造函数的类的基类。 基类成员被继承而不考虑其可访问性。

类声明可以包含常量、 字段、 方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数、 静态构造函数和类型的声明。

成员`object`和`string`直接对应于的类类型成员它们别名：

*  成员`object`成员的`System.Object`类。
*  成员`string`成员的`System.String`类。

### <a name="interface-members"></a>接口成员

接口的成员是在接口和接口的所有基接口中声明的成员。 在类成员`object`不严格地讲，任何接口的成员 ([接口成员](interfaces.md#interface-members))。 但是，在类成员`object`可通过成员查找中任何接口类型 ([成员查找](expressions.md#member-lookup))。

### <a name="array-members"></a>数组成员

数组的成员都是从类继承的成员`System.Array`。

### <a name="delegate-members"></a>委托成员

委托的成员都是从类继承的成员`System.Delegate`。

## <a name="member-access"></a>成员访问

成员的声明允许成员访问控制。 成员的可访问性由声明可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)) 的成员的结合直接包含类型的可访问性如果有的话。

如果允许访问特定成员，则称该成员是***可访问***。 相反，当不被允许访问特定成员，则称该成员为***无法访问***。 当可访问性域中包含文本的位置访问发生时允许访问成员 ([可访问性域](basic-concepts.md#accessibility-domains)) 的成员。

### <a name="declared-accessibility"></a>声明的可访问性

***声明可访问性***的成员中可以是以下之一：

*  公共的其中包括选择`public`成员声明中的修饰符。 直观的含义`public`是"不受限制访问"。
*  受保护，选中此选项通过包括`protected`成员声明中的修饰符。 直观的含义`protected`"访问限于包含类或类型派生自包含类"。
*  内部，通过包含所选`internal`成员声明中的修饰符。 直观的含义`internal`是"访问权限限制为此程序"。
*  受保护的内部 （即受保护或内部），后者通过包括同时选择`protected`和一个`internal`成员声明中的修饰符。 直观的含义`protected internal`是"此程序只能访问或派生自包含类的类型"。
*  通过包含所选的专用`private`成员声明中的修饰符。 直观的含义`private`是"访问限于包含类型"。

具体取决于上下文的成员声明将在其中放置，允许仅特定类型的声明可访问性。 此外，当成员声明中不包括任何访问修饰符，声明发生的上下文确定默认的已声明可访问性。

*  命名空间隐式具有`public`声明可访问性。 没有访问修饰符允许使用命名空间声明。
*  在编译单元或命名空间中声明的类型可以具有`public`或`internal`声明可访问性和默认为`internal`声明可访问性。
*  类的成员可以具有任何声明可访问性的五种和默认为`private`声明可访问性。 (请注意而类型声明为一个命名空间的成员可以只具有一种类型可声明为类的成员可以具有任何种类的声明可访问性，五`public`或`internal`声明可访问性。)
*  结构成员可以具有`public`， `internal`，或`private`声明可访问性和默认为`private`声明可访问性，因为结构隐式密封。 不能具有结构 （即，不会继承该结构） 中引入的结构成员`protected`或`protected internal`声明可访问性。 (请注意类型声明为结构的成员可以具有`public`， `internal`，或`private`声明可访问性，而类型声明为一个命名空间的成员可以只具有`public`或`internal`声明可访问性。)
*  接口的成员隐式具有`public`声明可访问性。 不允许使用任何访问修饰符在接口成员声明。
*  枚举的成员隐式具有`public`声明可访问性。 没有访问修饰符允许使用枚举成员声明。

### <a name="accessibility-domains"></a>可访问性域

***可访问性域***的成员包括的成员的访问权限允许的程序文本 （可能是不连续的） 部分。 为了定义成员的可访问域，说是成员可***顶级***如果它未声明为类型 （） 内，并且成员称为***嵌套***如果它在另一种类型内声明。 此外，***程序文本***的程序的定义如下文本的程序，所有源代码文件中包含的所有程序和一种类型的程序文本定义为所有程序中包含文本*type_declaration*s 的该类型 （包括，也可能嵌套在类型的类型）。

预定义类型的可访问域 (如`object`， `int`，或`double`) 不受限制。

顶级的可访问域之间的绑定类型`T`([绑定和未绑定类型](types.md#bound-and-unbound-types)) 在程序中声明的`P`定义，如下所示：

*  如果声明可访问性`T`是`public`，可访问域`T`的程序文本`P`和任何引用的程序`P`。
*  如果 `T` 的已声明可访问性为 `internal`，则 `T` 的可访问域就是 `P` 的程序文本。

从这些定义可以看出，顶级未绑定类型的可访问域是始终至少声明该类型的程序的程序文本。

构造类型的可访问域`T<A1, ..., An>`是未绑定的泛型类型的可访问域的交集`T`和类型参数的可访问性域`A1, ..., An`。

嵌套成员的可访问域`M`的类型中声明`T`程序内`P`定义，如下所示 (注意的是，`M`本身可能就是一种类型):

*  如果 `M` 的已声明可访问性为 `public`，则 `M` 的可访问域就是 `T` 的可访问域。
*  如果声明可访问性`M`是`protected internal`，可让`D`是联合的程序文本`P`和任何类型的程序文本派生自`T`，这外部声明`P`。 可访问性域`M`是可访问性域的交集`T`与`D`。
*  如果声明可访问性`M`是`protected`，可让`D`是联合的程序文本`T`和任何类型的程序文本派生自`T`。 可访问性域`M`是可访问性域的交集`T`与`D`。
*  如果 `M` 的已声明可访问性为 `internal`，则 `M` 的可访问域就是 `T` 的可访问域与 `P` 的程序文本之间的交集。
*  如果 `M` 的已声明可访问性为 `private`，则 `M` 的可访问域就是 `T` 的程序文本。

从这些定义可以看出的嵌套成员的可访问域是始终至少具有在其中声明成员的类型的程序文本。 此外，它遵循成员的可访问域是永远不会比在其中声明成员的类型的可访问域更广。

直观地讲，在类型或成员时`M`是访问，以下步骤进行计算以确保允许访问：

*  首先，如果`M`内声明了类型而非到编译单元或命名空间），如果该类型不可访问，会发生编译时错误。
*  然后，如果`M`是`public`，允许的访问。
*  否则为如果`M`是`protected internal`，如果它出现在程序中允许的访问`M`声明，或如果它出现在一个类派生自类在其中`M`声明，并且都是通过派生类类型 ([受保护实例访问成员](basic-concepts.md#protected-access-for-instance-members))。
*  否则为如果`M`是`protected`，如果它出现在类中允许的访问`M`声明，或如果它出现在一个类派生自类在其中`M`声明，并且都是通过派生类类型 ([受保护实例访问成员](basic-concepts.md#protected-access-for-instance-members))。
*  否则为如果`M`是`internal`，如果它出现在程序中允许的访问`M`声明。
*  否则为如果`M`是`private`，如果它出现在其中的类型在允许的访问`M`声明。
*  否则为该类型或成员将无法访问，并且将发生编译时错误。

在示例
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
类和成员具有以下可访问性域：

*  可访问性域`A`和`A.X`不受限制。
*  可访问性域`A.Y`， `B`， `B.X`， `B.Y`， `B.C`， `B.C.X`，和`B.C.Y`是包含程序的程序文本。
*  可访问性域`A.Z`的程序文本`A`。
*  可访问性域`B.Z`并`B.D`的程序文本`B`，其中包括的程序文本`B.C`和`B.D`。
*  可访问性域`B.C.Z`的程序文本`B.C`。
*  可访问性域`B.D.X`并`B.D.Y`的程序文本`B`，其中包括的程序文本`B.C`和`B.D`。
*  可访问性域`B.D.Z`的程序文本`B.D`。

如示例所示，成员的可访问域是类型的永远不会超过包含。 例如，即使所有`X`的成员具有公共声明的可访问性，以外的所有`A.X`有包含类型受约束的可访问性域。

如中所述[成员](basic-concepts.md#members)，由派生类型继承的基类，除了实例构造函数、 析构函数和静态构造函数，所有成员。 这包括甚至私有成员的基类。 但是，私有成员的可访问域包含在其中声明成员的类型的程序文本。 在示例
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
`B`类继承私有成员`x`从`A`类。 因为该成员是私有的才可访问内*class_body*的`A`。 因此，对的访问权限`b.x`成功`A.F`方法，但在失败`B.F`方法。

### <a name="protected-access-for-instance-members"></a>受保护的实例成员的访问

当`protected`在其中声明它，类的程序文本外部访问实例成员以及何时`protected internal`实例成员访问声明它的程序的程序文本之外，访问必须发生在从声明它的类派生的类声明。 此外，需要通过该派生的类类型或从其构造的类类型的实例进行的访问。 此限制可防止一个派生的类访问的其他派生类中，受保护的成员，即使从相同的基类继承的成员。

让`B`是声明受保护的实例成员的基类`M`，并让`D`是派生的类`B`。 内*class_body*的`D`，访问`M`可以采用以下形式之一：

*  未限定*type_name*或*primary_expression*窗体的`M`。
*  一个*primary_expression*窗体`E.M`，提供的类型`E`是`T`或从派生的类`T`，其中`T`是类类型`D`，或类类型从构造 `D`
*  一个*primary_expression*窗体的`base.M`。

除了这些形式的访问，派生的类可以访问基类中的受保护的实例构造函数*constructor_initializer* ([构造函数初始值设定项](classes.md#constructor-initializers))。

在示例
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
内`A`，就可以访问`x`通过两个实例`A`并`B`，因为在任一情况下访问都是通过实例`A`派生一个类或`A`。 但是，在`B`，不能访问`x`的实例通过`A`，因为`A`不是派生自`B`。

在示例
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
对三个赋值`x`允许，因为它们都需要通过从泛型类型构造的类类型的实例。

### <a name="accessibility-constraints"></a>可访问性约束

在 C# 语言中的多个构造需要类型为***至少与可访问性***成员，或者另一种类型。 一种类型`T`称其至少与可访问性的成员或类型`M`如果的可访问域`T`是可访问性域中的一个超集`M`。 换而言之，`T`是至少与可访问性`M`如果`T`可在其中的所有上下文中访问`M`可访问。

存在以下的可访问性约束：

*  类类型的直接基类必须至少具有与类类型本身相同的可访问性。
*  接口类型的显式基接口必须至少具有与接口类型本身相同的可访问性。
*  委托类型的返回类型和参数类型必须至少具有与委托类型本身相同的可访问性。
*  常量的类型必须至少具有与常量本身相同的可访问性。
*  字段的类型必须至少具有与字段本身相同的可访问性。
*  方法的返回类型和参数类型必须至少具有与方法本身相同的可访问性。
*  属性的类型必须至少具有与属性本身相同的可访问性。
*  事件的类型必须至少具有与事件本身相同的可访问性。
*  索引器的类型和参数类型必须至少具有与索引器本身相同的可访问性。
*  运算符的类型和参数类型必须至少具有与运算符本身相同的可访问性。
*  实例构造函数的参数类型必须至少与实例构造函数本身相同的可访问性。

在示例
```csharp
class A {...}

public class B: A {...}
```
`B`类会导致编译时错误，因为`A`不是至少与可访问性`B`。

同样，在示例
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
`H`中的方法`B`会导致编译时错误，因为返回类型`A`不是至少与该方法具有同样的可访问性。

## <a name="signatures-and-overloading"></a>签名和超载

方法、 实例构造函数、 索引器和运算符的特征体现在其***签名***:

*  方法签名包含方法、 类型参数的数目和类型和类型 （值、 引用或输出） 的每个从左到右的顺序被视为其正式参数的名称。 出于这些目的，不是按其名称，但该方法的类型实参列表中其序号位置通过标识的方法的形参的类型中发生的任何类型参数。 方法签名特别不包括返回类型，`params`可能最右边的参数，也不可选类型参数约束为指定的修饰符。
*  实例构造函数的签名组成的类型和每个从左到右的顺序被视为其正式参数的类型 （值、 引用或输出）。 实例构造函数的签名专门不包括`params`可能最右边的参数为指定的修饰符。
*  索引器的签名组成的每个从左到右的顺序被视为其正式参数的类型。 索引器的签名专门不包括的元素类型，也不包括`params`可能最右边的参数为指定的修饰符。
*  运算符的签名包含的运算符和每个从左到右的顺序被视为其正式参数的类型名称。 具体而言，运算符的签名不包括的结果类型。

签名是启用的机制，用于***重载***类、 结构和接口中的成员：

*  方法的重载允许类、 结构或接口声明多个具有相同的名称，提供它们的签名的方法是在该类、 结构或接口中是唯一。
*  实例构造函数的重载允许类或结构声明多个实例构造函数提供它们的签名是在该类或结构中是唯一。
*  索引器重载允许类、 结构或接口声明多个索引器，提供它们的签名是在该类、 结构或接口中是唯一。
*  运算符的重载允许类或结构声明多个运算符具有相同的名称，提供它们的签名是在该类或结构中是唯一。

尽管`out`并`ref`参数修饰符被视为一个签名的一部分，在单个类型中声明成员不能在签名中完全由`ref`和`out`。 如果两个成员声明中使用将是相同的签名相同的类型与这两种方法中的所有参数，会发生编译时错误`out`修饰符更改为`ref`修饰符。 用于签名匹配的其他目的 （例如，隐藏或重写），`ref`和`out`被视为签名的一部分，相互不匹配。 (此限制是允许C#程序能够轻松地进行转换，以便运行在公共语言基础结构 (CLI)，其中未提供一种方法来定义仅在不同的方法`ref`并`out`。)

为了进行签名，类型`object`和`dynamic`被视为相同。 在单个类型中声明成员因此不在可能有所不同，签名完全由`object`和`dynamic`。

下面的示例显示了一组重载的方法声明，以及它们的签名。
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

请注意的任何`ref`并`out`参数修饰符 ([方法参数](classes.md#method-parameters)) 是一个签名的一部分。 因此，`F(int)`和`F(ref int)`是唯一的签名。 但是，`F(ref int)`并`F(out int)`不能声明内相同的接口中，因为它们的签名完全由不同`ref`和`out`。 另请注意的返回类型和`params`修饰符不签名的一部分，因此不能重载仅基于返回类型或包含或排除`params`修饰符。 因此，这些方法的声明`F(int)`和`F(params string[])`上述导致编译时错误。

## <a name="scopes"></a>范围

***作用域***的名称是在其中就可以按名称而无需限定名称的声明的实体引用的程序文本的区域。 作用域可以是***嵌套***，并将来自外部范围名称的含义可能会重新内层作用域声明 (这不会但是，删除强加的限制[声明](basic-concepts.md#declarations)嵌套块内不可以通过声明一个局部变量作为封闭块中的本地变量同名）。 从外部作用域的名称然后说可***隐藏***程序的区域中文本涵盖内层作用域，并对外部名称，则仅可能访问的限定名称。

*  通过声明的命名空间成员的作用域*namespace_member_declaration* ([Namespace 成员](namespaces.md#namespace-members)) 与没有封闭*namespace_declaration*是整个程序文本。
*  通过声明的命名空间成员的作用域*namespace_member_declaration*内*namespace_declaration*其完全限定的名称是`N`是*namespace_body*的每个*namespace_declaration*其完全限定的名称是`N`开头或`N`后, 跟一个句点。
*  定义名称的作用域*extern_alias_directive*通过进行了扩展*using_directive*s， *global_attributes*和*namespace_member_声明*s 其直接包含的编译单元或命名空间体。 *Extern_alias_directive*不计为基础的声明空间的任何新成员。 换而言之， *extern_alias_directive*不是可传递的但，而是只影响的编译单元或命名空间体其中发生。
*  名称的作用域定义，或通过导入*using_directive* ([Using 指令](namespaces.md#using-directives)) 通过了扩展*namespace_member_declaration*的*compilation_unit*或*namespace_body*顺序*using_directive*时发生。 一个*using_directive*可能会使零个或多个命名空间、 类型或成员名称可用中特定*compilation_unit*或*namespace_body*，但不支持参与为基础的声明空间的任何新成员。 换而言之， *using_directive*不是可传递，但而不是仅影响*compilation_unit*或*namespace_body*它所在。
*  通过声明的类型参数的作用域*type_parameter_list*上*class_declaration* ([类声明](classes.md#class-declarations)) 是*class_base*， *type_parameter_constraints_clause*s，和*class_body*这些*class_declaration*。
*  通过声明的类型参数的作用域*type_parameter_list*上*struct_declaration* ([结构声明](structs.md#struct-declarations)) 是*struct_interfaces*， *type_parameter_constraints_clause*s，和*struct_body*这些*struct_declaration*。
*  通过声明的类型参数的作用域*type_parameter_list*上*interface_declaration* ([接口声明](interfaces.md#interface-declarations)) 是*interface_base*， *type_parameter_constraints_clause*s，和*interface_body*这些*interface_declaration*。
*  通过声明的类型参数的作用域*type_parameter_list*上*delegate_declaration* ([委托声明](delegates.md#delegate-declarations)) 是*return_type*， *formal_parameter_list*，和*type_parameter_constraints_clause*的 s *delegate_declaration*。
*  通过声明成员的作用域*class_member_declaration* ([类正文](classes.md#class-body)) 是*class_body*中进行声明。 此外，类成员的范围扩展到*class_body*这些派生的可访问性域中包含的类 ([可访问性域](basic-concepts.md#accessibility-domains)) 的成员。
*  通过声明成员的作用域*struct_member_declaration* ([结构成员](structs.md#struct-members)) 是*struct_body*中进行声明。
*  通过声明成员的作用域*enum_member_declaration* ([枚举成员](enums.md#enum-members)) 是*enum_body*中进行声明。
*  参数的作用域中声明*method_declaration* ([方法](classes.md#methods)) 是*method_body*这些*method_declaration*。
*  参数的作用域中声明*indexer_declaration* ([索引器](classes.md#indexers)) 是*accessor_declarations*的*indexer_declaration*.
*  参数的作用域中声明*operator_declaration* ([运算符](classes.md#operators)) 是*块*这些*operator_declaration*。
*  参数的作用域中声明*constructor_declaration* ([实例构造函数](classes.md#instance-constructors)) 是*constructor_initializer*和*块*这些*constructor_declaration*。
*  参数的作用域中声明*lambda_expression* ([匿名函数表达式](expressions.md#anonymous-function-expressions)) 是*anonymous_function_body*的*lambda_表达式*
*  参数的作用域中声明*anonymous_method_expression* ([匿名函数表达式](expressions.md#anonymous-function-expressions)) 是*块*的*anonymous_method_expression*。
*  标签的范围中声明*labeled_statement* ([标记为语句](statements.md#labeled-statements)) 是*块*中进行声明。
*  在中声明本地变量的作用域*local_variable_declaration* ([局部变量声明](statements.md#local-variable-declarations)) 是在块中进行声明。
*  在中声明本地变量的作用域*switch_block*的`switch`语句 ([switch 语句](statements.md#the-switch-statement)) 是*switch_block*。
*  在中声明本地变量的作用域*for_initializer*的`for`语句 ([语句](statements.md#the-for-statement)) 是*for_initializer*，则*for_condition*，则*for_iterator*，和包含*语句*的`for`语句。
*  局部常量的作用域中声明*local_constant_declaration* ([局部常量声明](statements.md#local-constant-declarations)) 是在块中进行声明。 它会导致编译时错误之前的文本位置中的本地常量，请参阅其*constant_declarator*。
*  变量的作用域声明的一部分*foreach_statement*， *using_statement*， *lock_statement*或者*query_expression*是由给定的构造的扩展。

命名空间、 类、 结构或枚举成员的作用域内就可以引用的文本的位置之前的成员的声明的成员。 例如
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
在这里，它是有效的`F`来指代`i`在声明之前。

在本地变量的范围内，它是编译时错误，以引用之前的文本位置中的本地变量*local_variable_declarator*的本地变量。 例如
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

在中`F`方法更高版本中，第一个分配给`i`一定不是指在外部作用域声明的字段。 相反，它是指本地变量和由于文本上位于前面的变量声明导致编译时错误。 在`G`方法中，使用`j`中的声明的初始值设定项`j`是有效的因为使用之前没有*local_variable_declarator*。 在`H`方法，则后续*local_variable_declarator*正确地指的是早期在声明的本地变量*local_variable_declarator*在同一*local_variable_declaration*。

本地变量的范围规则旨在保证的含义的名称，用于在表达式的上下文中始终是块中相同。 如果本地变量的作用域将仅从其声明扩展到块的末尾，然后在上面的示例中，第一个分配将分配给实例变量和第二个分配将分配给本地变量，则可能会导致如果后来重新排列的块的语句的编译时错误。

块中名称的含义可能会因的上下文中使用的名称。 在示例
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
名称`A`表达式上下文中使用来指代本地变量`A`，在类型上下文中来指代类`A`。

### <a name="name-hiding"></a>隐藏名称

实体范围通常包括实体的声明空间比多个程序文本。 具体而言，实体的作用域可能包括引入新声明空间包含具有相同名称的实体的声明。 此类声明会导致原始实体变得***隐藏***。 相反，实体称为***可见***时它不会隐藏。

通过嵌套和通过继承重叠的范围重叠时，会发生名称隐藏。 以下各节所述的隐藏这两种类型的特征。

#### <a name="hiding-through-nesting"></a>通过嵌套隐藏

嵌套命名空间或命名空间、 嵌套类型的类或结构中参数和局部变量声明中的类型的结果会发生名称隐藏通过嵌套。

在示例
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
内`F`方法中，实例变量`i`隐藏由本地变量`i`，但内`G`方法，`i`仍是指实例变量。

如果内层作用域中的名称将隐藏外层作用域中的名称，它会隐藏该名称的所有重载的匹配项。 在示例
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
在调用`F(1)`调用`F`中声明`Inner`因为外部出现的所有`F`隐藏内部声明。 出于相同原因，在调用`F("Hello")`会导致编译时错误。

#### <a name="hiding-through-inheritance"></a>通过继承隐藏

当类或结构重新声明从基类继承的名称，通过继承隐藏名称时发生。 这种类型的隐藏名称采用以下形式之一：

*  常量、 字段、 属性、 事件或在类或结构中引入的类型会隐藏具有相同名称的所有基类成员。
*  在类或结构中引入的方法将隐藏具有相同名称的所有非方法基类成员和具有相同的签名 （方法名称和参数计数、 修饰符和类型） 的所有基类方法。
*  在类或结构中引入的索引器会隐藏具有相同的签名 （参数计数和类型） 的所有基类索引器。

管理运算符声明的规则 ([运算符](classes.md#operators)) 使其不是通过派生类声明为基类中的运算符具有相同签名的运算符。 因此，运算符将永远不会隐藏另一个。

与隐藏来自外部范围的名称，相反隐藏继承范围中的可访问名称会导致报告警告。 在示例
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
声明`F`在`Derived`导致报告警告。 隐藏继承的名称不具体而言是错误，因为那将排除的基类，这些类的单独演变。 例如，如果上述情况可能会发生因为更高版本`Base`引入了`F`类的早期版本中不存在的方法。 具有更高版本的情况是一个错误，然后对单独进行版本控制的类库中的基类进行任何更改可能导致派生的类变得无效。

可以通过使用消除隐藏继承的名称由导致该警告`new`修饰符：
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

`new`修饰符指示`F`中`Derived`是"新建"，，它确实能隐藏继承的成员。

新成员的声明隐藏继承的成员只能在新成员的范围中。

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

在示例中的声明上方`F`中`Derived`隐藏`F`继承自`Base`，但由于新`F`中`Derived`具有私有访问权限，其作用域内不会扩展到`MoreDerived`. 因此，调用`F()`中`MoreDerived.G`有效，并且将调用`Base.F`。

## <a name="namespace-and-type-names"></a>Namespace 和类型名称

中的多个环境C#程序需要 *_ 名称*或*type_name*指定。

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

一个 *_ 名称*是*namespace_or_type_name* ，是指命名空间。 以下解决方法，如下所述*namespace_or_type_name*的 *_ 名称*必须引用一个命名空间，否则发生编译时错误。 没有类型参数 ([的类型实参](types.md#type-arguments)) 可出现在 *_ 名称*（仅类型可以具有类型参数）。

一个*type_name*是*namespace_or_type_name*引用类型。 以下解决方法，如下所述*namespace_or_type_name*的*type_name*必须引用类型，否则发生编译时错误。

如果*namespace_or_type_name*是一个限定的别名的成员，其含义中所述[Namespace 别名限定符](namespaces.md#namespace-alias-qualifiers)。 否则为*namespace_or_type_name*有四种形式之一：

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

其中`I`是单个标识符`N`是*namespace_or_type_name*并`<A1, ..., Ak>`是一个可选*type_argument_list*。 如果未*type_argument_list*是指定，请考虑`k`为零。

含义*namespace_or_type_name* ，如下所示确定：

*   如果*namespace_or_type_name*的形式`I`或窗体的`I<A1, ..., Ak>`:
    * 如果`K`为零， *namespace_or_type_name*出现在泛型方法声明 ([方法](classes.md#methods))，如果该声明包含一个类型参数 ([类型参数](classes.md#type-parameters)) 具有名称 `I`，然后*namespace_or_type_name*引用该类型参数。
    * 否则为如果*namespace_or_type_name*出现在类型声明中，然后，对于每个实例类型 `T`([的实例类型](classes.md#the-instance-type))、 第一页为该类型的实例类型声明，然后继续每个封闭类或结构声明的实例类型 （如果有）：
        * 如果`K`是零的声明`T`包括具有名称的类型参数 `I`，然后*namespace_or_type_name*引用该类型参数。
        * 否则为如果*namespace_or_type_name*将显示在类型声明的主体和`T`或任何其基类型包含具有名称的嵌套可访问类型 `I`和`K`  类型参数，则*namespace_or_type_name*构造具有给定的类型参数的该类型是指。 如果有多个此类类型，被选择派生程度更大的类型中声明的类型。 请注意，非类型成员 （常量、 字段、 方法、 属性、 索引器、 运算符、 实例构造函数、 析构函数和静态构造函数） 和类型成员具有不同数量的类型参数时，将忽略确定的含义*namespace_or_type_name*。
    * 如果前面的步骤已不成功，则每个命名空间 `N`，在其中命名空间从开始*namespace_or_type_name*发生时，继续执行每个封闭命名空间 （如果有），以及结尾全局命名空间中，以下步骤进行计算，直到找到一个实体就是：
        * 如果`K`为零并`I`中的命名空间名称 `N`，然后：
            * 如果位置其中*namespace_or_type_name*发生的命名空间声明括`N`和命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与命名空间或类型，则*namespace_or_type_name*不明确，则发生编译时错误。
            * 否则为*namespace_or_type_name*指的是名为的命名空间`I`中`N`。
        * 否则为如果`N`包含可访问类型具有名称 `I`并`K` 类型形参，则：
            * 如果`K`是零和位置， *namespace_or_type_name*发生括起来的命名空间声明`N`和命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与命名空间或类型，则*namespace_or_type_name*已不明确以及编译时间出现错误。
            * 否则为*namespace_or_type_name*指构造具有给定的类型参数的类型。
        * 否则为如果位置其中*namespace_or_type_name*发生括起来的命名空间声明`N`:
            * 如果`K`为零，并且命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与导入的命名空间或类型，则*namespace_or_type_name*指的是该命名空间或类型。
            * 否则为如果通过导入的命名空间和类型声明*using_namespace_directive*s 和*using_alias_directive*的命名空间声明包含一个可访问类型具有名称 `I`并`K` 类型参数，则*namespace_or_type_name*构造具有给定的类型参数的该类型是指。
            * 否则为如果通过导入的命名空间和类型声明*using_namespace_directive*s 和*using_alias_directive*的命名空间声明包含多个可访问类型具有名称 `I`并`K` 类型参数，则*namespace_or_type_name*不明确并产生一个错误。
    * 否则为*namespace_or_type_name*是未定义，且将发生编译时错误。
*  否则为*namespace_or_type_name*的形式`N.I`或窗体的`N.I<A1, ..., Ak>`。 `N` 作为第一次解决*namespace_or_type_name*。 如果解析`N`不成功，则发生编译时错误。 否则为`N.I`或`N.I<A1, ..., Ak>`解析方式如下：
    * 如果`K`为零并`N`指的是命名空间和`N`包含名称的嵌套命名空间`I`，则*namespace_or_type_name*指的是该嵌套命名空间。
    * 否则为如果`N`指的是命名空间和`N`包含可访问类型具有名称 `I`并`K` 类型参数，则*namespace_or_type_name*引用为给定的类型参数用来构造该类型。
    * 否则为如果`N`指的是一个 （可能是构造） 的类或结构类型和`N`或任何其基类，这些类包含具有名称的嵌套可访问类型 `I`并`K` 类型参数，则*namespace_or_type_name*构造具有给定的类型参数的该类型是指。 如果有多个此类类型，被选择派生程度更大的类型中声明的类型。 请注意，如果的含义`N.I`要确定作为解决的基类规范的一部分`N`然后直接基类`N`被视为对象 ([基类](classes.md#base-classes))。
    * 否则为`N.I`是一个无效*namespace_or_type_name*，并将发生编译时错误。

一个*namespace_or_type_name*允许以引用静态类 ([静态类](classes.md#static-classes)) 仅当

*  *Namespace_or_type_name*是`T`中*namespace_or_type_name*窗体的`T.I`，或
*  *Namespace_or_type_name*是`T`中*typeof_expression* ([自变量列表](expressions.md#argument-lists)1) 的窗体`typeof(T)`。

### <a name="fully-qualified-names"></a>完全限定的名称

每个命名空间和类型有***完全限定的名称***，可唯一标识的命名空间或在所有其他类型。 命名空间或类型的完全限定的名称`N`，如下所示确定：

*  如果`N`成员的全局命名空间，其完全限定的名称是`N`。
*  否则，其完全限定的名称是`S.N`，其中`S`是完全限定的命名空间或在其中的类型名称`N`声明。

换而言之，完全限定的名称`N`是导致的标识符的完整层次结构路径`N`，从全局命名空间。 因为每个命名空间或类型成员必须具有唯一的名称，它遵循的命名空间或类型的完全限定的名称始终是唯一。

下面的示例演示了几个命名空间和类型声明及其关联的完全限定名。
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>自动内存管理

C# 使用自动内存管理，它使开发人员不再需要手动分配和释放对象占用的内存。 实现自动内存管理策略***垃圾回收器***。 对象的内存管理生命周期如下所示：

1. 创建对象后，为其分配内存、 运行时构造函数，和对象被视为实时。
2. 如果不能由执行任何可能继续访问对象或任何部分，以外运行析构函数，该对象被视为无法再在使用中，并成为符合销毁条件。 C# 编译器和垃圾回收器可以选择要分析的代码以确定哪些引用的对象可能在将来使用。 例如，如果在范围内的本地变量是一个对象的唯一现有引用，但该本地变量永远不会引用执行从当前执行的任何后续点过程中，垃圾回收器可能 （但不是为必需） 处理该对象为不再使用。
3. 对象符合销毁条件后，在稍后某个时间析构函数 ([析构函数](classes.md#destructors)) （如果有） 为运行对象。 在正常情况下对象的析构函数是只运行一次，但特定于实现的 Api 可能允许重写此行为。
4. 对象的析构函数运行后，如果不能由任何可能的执行，包括运行析构函数，继续访问该对象或任何部分，该对象被视为无法访问和对象将成为符合回收的条件。
5. 最后，在某个时间，对象将变为符合回收条件后垃圾回收器将释放与该对象相关联的内存。

垃圾回收器会维护有关对象使用情况信息，并使用此信息以使内存管理决策，例如要查找新创建的对象，当重定位对象，以及当对象不再使用或无法访问的内存中的何处。

假定垃圾回收器存在其他语言，如 C# 设计，以便垃圾回收器可能会实现范围广泛的内存管理策略。 例如，C# 不需要析构函数，在运行或对象被回收，只要它们是符合条件，或按任何特定顺序或任何特定线程上运行的析构函数。

可以控制垃圾回收器的行为，某种程度上，通过在类上的静态方法`System.GC`。 此类可用于请求回收发生，析构函数运行 （或不运行），等等。

由于垃圾回收器很大自由度确定何时收集对象和运行析构函数中的，符合标准的实现可能会产生不同于下面的代码所示的输出。 该程序
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
创建类的实例`A`类的实例和`B`。 这些对象会变得可以进行垃圾回收时变量`b`的值赋给`null`，因为在此时间后不可能的任何用户编写的代码以对其进行访问。 输出可以是
```
Destruct instance of A
Destruct instance of B
```
或
```
Destruct instance of B
Destruct instance of A
```
因为该语言没有对实施约束中的对象进行垃圾回收的顺序。

在微妙的情况下，"符合销毁条件"和"符合回收条件"之间的区别非常重要。 例如，应用于对象的
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

在上面的程序，如果垃圾回收器选择要运行的析构函数`A`的析构函数之前`B`，则可能是此程序的输出：
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

请注意，虽然的实例`A`未使用并`A`的析构函数已运行，它表示仍有可能的方法`A`(在这种情况下， `F`) 若要从另一个析构函数调用。 另请注意，析构函数的运行可能会导致再次变得从主线程序使用的对象。 在此情况下，运行`B`的析构函数导致的实例`A`的以前不能在使用变得可从实时引用`Test.RefA`。 在调用`WaitForPendingFinalizers`的实例`B`符合条件的集合，但实例`A`不是，由于引用`Test.RefA`。

若要避免产生混淆和意外的行为，它通常是个好主意析构函数只对它们的对象的字段中存储的数据执行清理并不被引用的对象或静态字段上执行任何操作。

使用析构函数的替代方法是让实现的类`System.IDisposable`接口。 这样的对象的客户端，以确定何时将释放的资源对象，通常为中的资源访问的对象`using`语句 ([using 语句](statements.md#the-using-statement))。

## <a name="execution-order"></a>执行顺序

C# 程序的执行过程，以便在关键执行点处保留的每个执行线程的副作用。 一个***副作用***定义为读取或写入可变字段的、 写入的非易失性变量、 对外部资源，并引发的异常进行写入。 这些副作用的顺序必须保存在其中的关键执行点是对可变字段的引用 ([可变字段](classes.md#volatile-fields))，`lock`语句 ([lock 语句](statements.md#the-lock-statement))，并线程创建和终止。 执行环境可以随意更改 C# 程序，受以下约束的执行顺序：

*  内的执行线程保持数据依赖性。 也就是说，像在原始程序顺序执行线程中的所有语句计算每个变量的值。
*  初始化排序规则将被保留 ([字段初始化](classes.md#field-initialization)并[变量的初始值设定项](classes.md#variable-initializers))。
*  保留的负面影响的顺序相对于易失性读取和写入 ([可变字段](classes.md#volatile-fields))。 此外，执行环境不需要计算表达式的一部分，如果不使用该表达式的值和任何所需的副作用会产生 （包括任何由调用方法或访问可变字段导致），它就可以推断出。 由异步事件 （如由另一个线程引发的异常） 中断程序执行，不保证可观察的副作用是在原始程序顺序中可见。
