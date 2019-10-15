---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704008"
---
# <a name="basic-concepts"></a>基本概念

## <a name="application-startup"></a>应用程序启动

具有***入口点***的程序集称为***应用程序***。 当应用程序运行时，将创建一个新的***应用程序域***。 同一台计算机上可以同时存在多个不同的应用程序实例，每个实例都有自己的应用程序域。

应用程序域通过充当应用程序状态的容器来实现应用程序隔离。 应用程序域充当应用程序中定义的类型和它所使用的类库的容器和边界。 加载到一个应用程序域中的类型不同于加载到另一个应用程序域中的相同类型，并且不会在应用程序域之间直接共享对象的实例。 例如，每个应用程序域都有自己的静态变量副本用于这些类型，并且每个应用程序域最多运行一次类型的静态构造函数。 实现可自由提供特定于实现的策略，也可以创建和销毁应用程序域。

当执行环境调用指定方法（称为应用程序的入口点）时，将发生***应用程序启动***。 此入口点方法始终命名为 `Main`，可以具有以下签名之一：

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

如图所示，入口点可以选择返回 @no__t 0 值。 此返回值用于应用程序终止（[应用程序终止](basic-concepts.md#application-termination)）。

入口点还可以有一个形参。 参数可以具有任何名称，但参数的类型必须为 `string[]`。 如果正式参数存在，则执行环境将创建并传递一个 @no__t 的参数，该参数包含在启动应用程序时指定的命令行参数。 @No__t 参数决不会为 null，但如果未指定命令行参数，则其长度可能为零。

由于C#支持方法重载，因此类或结构可能包含某些方法的多个定义，前提是每个定义具有不同的签名。 但是，在一个程序中，任何类或结构都不能包含多个称为 @no__t 的方法，它的定义限定它用作应用程序入口点。 但允许 @no__t 的其他重载版本-0，但前提是它们具有多个参数，或者其唯一参数不是类型 `string[]`。

应用程序可以由多个类或结构组成。 其中的多个类或结构可能包含一个名为 @no__t 的方法，该方法的定义将其限定为作为应用程序入口点使用。 在这种情况下，必须使用外部机制（例如命令行编译器选项）来选择其中一个 `Main` 方法作为入口点。

在C#中，每个方法都必须定义为类或结构的成员。 通常，方法的声明的可访问性（已[声明的可访问性](basic-concepts.md#declared-accessibility)）由其声明中指定的访问修饰符（[访问修饰符](classes.md#access-modifiers)）确定，并且类似于类型的声明的可访问性由在其声明中指定的访问修饰符。 为了使给定类型的给定方法可调用，该类型和成员都必须可访问。 但是，应用程序入口点是一种特殊情况。 具体而言，执行环境可以访问应用程序的入口点，无论其声明的可访问性以及其封闭类型声明的声明的可访问性。

应用程序入口点方法不能在泛型类声明中。

在所有其他方面，入口点方法的行为类似于非入口点的行为。

## <a name="application-termination"></a>应用程序终止

***应用程序终止***会向执行环境返回控制权。

如果应用程序的***入口点***方法的返回类型为 `int`，则返回的值将用作应用程序的***终止状态代码***。 此代码的目的是允许向执行环境进行成功或失败的通信。

如果入口点方法的返回类型为 `void`，则到达右大括号（@no__t 为-1）以终止该方法，或执行没有表达式的 `return` 语句会导致终止状态代码 @no__t 为3。

在应用程序终止之前，将调用其所有尚未进行垃圾回收的对象的析构函数，除非已取消此类清理（例如，通过调用库方法 `GC.SuppressFinalize`）。

## <a name="declarations"></a>声明

C#程序中的声明定义程序的构成元素。 C#使用命名空间（[命名空间](namespaces.md)）对程序进行组织，其中可以包含类型声明和嵌套命名空间声明。 类型声明（[类型声明](namespaces.md#type-declarations)）用于定义类（[类](classes.md)）、结构（[结构](structs.md)）、接口（[接口](interfaces.md)）、枚举（[枚举](enums.md)）和委托（[委托](delegates.md)）。 类型声明中允许的成员种类取决于类型声明的形式。 例如，类声明可以包含常量（[常量](classes.md#constants)）、字段（[字段](classes.md#fields)）、方法（[方法](classes.md#methods)）、属性（[属性](classes.md#properties)）、事件（[事件](classes.md#events)）、索引器（[索引器](classes.md#indexers)）的声明，运算符（[运算符](classes.md#operators)）、实例构造函数（[实例构造](classes.md#instance-constructors)函数）、静态构造函数（[静态构造函数](classes.md#static-constructors)）、析构函数（[析构函数](classes.md#destructors)）和嵌套类型（[嵌套类型](classes.md#nested-types)）。

声明定义声明***空间***中声明所属的名称。 除重载成员（[签名和重载](basic-concepts.md#signatures-and-overloading)）外，具有两个或多个将在声明空间中引入同名成员的声明是编译时错误。 声明空间不能包含具有相同名称的不同类型的成员。 例如，声明空间不能包含具有相同名称的字段和方法。

有几种不同类型的声明空间，如下所述。

*  在程序的所有源文件中，不带封闭*namespace_declaration*的*namespace_member_declaration*是单个合并声明空间的成员，称为***全局声明空间***。
*  在程序的所有源文件中，在具有相同完全限定的命名空间名称的*namespace_declaration*中的*namespace_member_declaration*s 是单个组合声明空间的成员。
*  每个类、结构或接口声明都创建新的声明空间。 通过*class_member_declaration*s、 *struct_member_declaration*s、 *interface_member_declaration*或*type_parameter*将名称引入到此声明空间。 除了重载实例构造函数声明和静态构造函数声明，类或结构不能包含与类或结构同名的成员声明。 类、结构或接口允许声明重载方法和索引器。 此外，类或结构允许声明重载实例构造函数和运算符。 例如，类、结构或接口可能包含多个具有相同名称的方法声明，前提是这些方法声明在其签名中不同（[签名和重载](basic-concepts.md#signatures-and-overloading)）。 请注意，基类不涉及类的声明空间，并且基接口不涉及接口的声明空间。 因此，允许派生类或接口声明与继承成员同名的成员。 这种成员被称为***隐藏***继承成员。
*  每个委托声明都将创建一个新的声明空间。 通过形参（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*将名称引入到此声明空间。
*  每个枚举声明都将创建一个新的声明空间。 通过*enum_member_declarations*将名称引入此声明空间。
*  每个方法声明、索引器声明、运算符声明、实例构造函数声明和匿名函数都会创建一个名为***局部变量声明空间***的新声明空间。 通过形参（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*将名称引入到此声明空间。 函数成员或匿名函数（如果有）的主体被视为嵌套在局部变量声明空间内。 局部变量声明空间和嵌套局部变量声明空间包含具有相同名称的元素是错误的。 因此，在嵌套的声明空间内，不能在封闭声明空间中声明与本地变量或常量同名的局部变量或常数。 可能有两个声明空间包含具有相同名称的元素，前提是两个声明空间都不包含另一个。
*  每个*block*或*switch_block*以及*for*、 *foreach*和*using*语句均为局部变量和局部常量创建一个局部变量声明空间。 通过*local_variable_declaration*s 和*local_constant_declaration*将名称引入此声明空间。 请注意，在函数成员或匿名函数的主体中发生的块嵌套在这些函数为其参数声明的局部变量声明空间内。 因此，例如，具有本地变量和同名参数的方法是错误的。
*  每个*block*或*switch_block*为标签创建一个单独的声明空间。 通过*labeled_statement*将名称引入此声明空间，并通过*goto_statement*引用名称。 块的***标签声明空间***包括任何嵌套块。 因此，在嵌套块中，不能声明与封闭块中的标签具有相同名称的标签。

声明名称的文本顺序通常不重要。 特别是，文本顺序对于命名空间、常量、方法、属性、事件、索引器、运算符、实例构造函数、析构函数、静态构造函数和类型的声明和使用并不重要。 声明顺序在以下方面非常重要：

*  字段声明和局部变量声明的声明顺序决定了其初始值设定项（如果有）的执行顺序。
*  局部变量必须在使用之前定义（[范围](basic-concepts.md#scopes)）。
*  省略*constant_expression*值时，枚举成员声明（[enum 成员](enums.md#enum-members)）的声明顺序很重要。

命名空间的声明空间为 "open 已结束"，两个具有相同完全限定名称的命名空间声明会导致相同的声明空间。 例如
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

上面的两个命名空间声明为同一声明空间提供，在此示例中，声明两个具有完全限定名称的类 `Megacorp.Data.Customer` 和 `Megacorp.Data.Order`。 由于这两个声明涉及到相同的声明空间，因此，如果每个声明都包含具有相同名称的类的声明，则会导致编译时错误。

如上所述，块的声明空间包括任何嵌套块。 因此，在下面的示例中，`F` 和 @no__t 1 方法会导致编译时错误，因为名称 `i` 是在外部块中声明的，不能在内部块中重新声明。 但是，@no__t 的 @no__t 方法有效，因为这两个 `i` 在单独的非嵌套块中声明。

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

命名空间和类型具有***成员***。 实体的成员通常通过使用以实体引用开头的限定名称来提供，后跟一个 "`.`" 标记，后跟成员的名称。

类型成员既可以在类型声明中声明，也可以从类型的基类***继承***。 当类型从基类继承时，基类的所有成员（实例构造函数、析构函数和静态构造函数除外）将成为派生类型的成员。 基类成员的声明的可访问性不会控制是否继承成员，继承扩展到任何不是实例构造函数、静态构造函数或析构函数的成员。 但是，在派生的类型中，可能无法访问继承的成员，这可能是由于其声明的可访问性（已[声明可访问性](basic-concepts.md#declared-accessibility)）或由类型本身中的声明隐藏（[通过继承](basic-concepts.md#hiding-through-inheritance)隐藏）。

### <a name="namespace-members"></a>命名空间成员

没有封闭命名空间的命名空间和类型是***全局命名空间***的成员。 这直接对应于全局声明空间中声明的名称。

命名空间中声明的命名空间和类型是该命名空间的成员。 这直接对应于命名空间声明空间中声明的名称。

命名空间没有任何访问限制。 不能声明私有、受保护的或内部命名空间，并且命名空间名称始终是可公开访问的。

### <a name="struct-members"></a>结构成员

结构的成员是在结构中声明的成员和从该结构的直接基类继承的成员 `System.ValueType` 和间接基类 @no__t 为-1。

简单类型的成员直接与简单类型化名为的结构类型成员相对应：

*  @No__t 的成员是 `System.SByte` 结构的成员。
*  @No__t 的成员是 `System.Byte` 结构的成员。
*  @No__t 的成员是 `System.Int16` 结构的成员。
*  @No__t 的成员是 `System.UInt16` 结构的成员。
*  @No__t 的成员是 `System.Int32` 结构的成员。
*  @No__t 的成员是 `System.UInt32` 结构的成员。
*  @No__t 的成员是 `System.Int64` 结构的成员。
*  @No__t 的成员是 `System.UInt64` 结构的成员。
*  @No__t 的成员是 `System.Char` 结构的成员。
*  @No__t 的成员是 `System.Single` 结构的成员。
*  @No__t 的成员是 `System.Double` 结构的成员。
*  @No__t 的成员是 `System.Decimal` 结构的成员。
*  @No__t 的成员是 `System.Boolean` 结构的成员。

### <a name="enumeration-members"></a>枚举成员

枚举的成员是枚举中声明的常量和从枚举的直接基类继承的成员 `System.Enum` 和间接基类 `System.ValueType` 和 `object`。

### <a name="class-members"></a>类成员

类的成员是在类中声明的成员和从基类继承的成员（没有基类的类 `object` 除外）。 从基类继承的成员包括常数、字段、方法、属性、事件、索引器、运算符和基类的类型，但不包括基类的实例构造函数、析构函数和静态构造函数。 继承基类成员，而不考虑其可访问性。

类声明可以包含常量、字段、方法、属性、事件、索引器、运算符、实例构造函数、析构函数、静态构造函数和类型的声明。

@No__t 和 @no__t 1 的成员直接对应于其别名的类类型的成员：

*  @No__t 的成员是 @no__t 类的成员。
*  @No__t 的成员是 @no__t 类的成员。

### <a name="interface-members"></a>接口成员

接口的成员是在接口和接口的所有基接口中声明的成员。 类 `object` 中的成员并不严格地说是任何接口（[接口成员](interfaces.md#interface-members)）的成员。 但是，可以通过成员查找在任何接口类型中（[成员查找](expressions.md#member-lookup)）使用类 `object` 中的成员。

### <a name="array-members"></a>数组成员

数组成员是从类 `System.Array` 继承的成员。

### <a name="delegate-members"></a>委托成员

委托的成员是从类 `System.Delegate` 继承的成员。

## <a name="member-access"></a>成员访问

成员的声明允许控制成员访问。 成员的可访问性是由成员的声明的可访问性（已[声明可访问性](basic-concepts.md#declared-accessibility)）建立的，该成员与直接包含类型的可访问性（如果有）结合使用。

如果允许访问特定成员，则认为该成员是可***访问***的。 相反，当不允许访问特定成员时，该成员被称为***不可***访问。 如果访问发生的文本位置包含在成员的可访问域（[可访问](basic-concepts.md#accessibility-domains)域）中，则允许访问成员。

### <a name="declared-accessibility"></a>声明的可访问性

成员的***声明可访问性***可以是以下项之一：

*  公共，通过在成员声明中包含 `public` 修饰符来选择。 @No__t-0 的直观含义为 "访问不受限制"。
*  Protected：通过在成员声明中包含 `protected` 修饰符来选择。 @No__t-0 的直观含义为 "访问包含类或派生自包含类的类型"。
*  内部，通过在成员声明中包含 `internal` 修饰符来选择。 @No__t-0 的直观含义为 "访问此程序受限"。
*  受保护的内部（这是受保护的或内部的），通过在成员声明中包括 @no__t 0 和 @no__t 修饰符来选择。 @No__t-0 的直观含义为 "访问此程序或派生自包含类的类型"。
*  Private，通过在成员声明中包含 `private` 修饰符来选择。 @No__t-0 的直观含义为 "访问包含类型"。

根据成员声明发生的上下文，仅允许某些类型的声明的可访问性。 此外，当成员声明中不包含任何访问修饰符时，在其中进行声明的上下文会确定默认的声明的可访问性。

*  命名空间隐式具有 @no__t 的可访问性。 命名空间声明中不允许使用访问修饰符。
*  在编译单元或命名空间中声明的类型可以有 `public` 或 @no__t 1 声明的可访问性，并且默认为 `internal` 声明的可访问性。
*  类成员可以具有五种声明的可访问性，并且默认为 `private` 声明的可访问性。 （请注意，声明为类成员的类型可以具有五种声明可访问性中的任何一种，而声明为命名空间成员的类型只能有 `public` 或 @no__t 1 声明的可访问性。）
*  结构成员可以有 `public`、`internal` 或 @no__t 声明的可访问性和默认值 @no__t，因为结构是隐式密封的。 结构中引入的结构成员（即，不是由该结构继承）不能有 `protected` 或 @no__t 声明的可访问性。 （请注意，声明为结构成员的类型可以有 `public`、`internal` 或 @no__t 声明的可访问性，而声明为命名空间成员的类型只能具有 `public` 或 @no__t 的可访问性。）
*  接口成员隐式具有 @no__t 的可访问性。 接口成员声明中不允许使用访问修饰符。
*  枚举成员隐式具有 @no__t 的可访问性。 枚举成员声明中不允许使用访问修饰符。

### <a name="accessibility-domains"></a>辅助功能域

成员的***可访问域***包含允许访问成员的程序文本（可能不连续）部分。 出于定义成员的可访问性域的目的，如果成员未在类型内声明，则称为***顶级***成员，如果在另一种类型中声明成员，则称***嵌套***成员。 此外，程序的***程序文本***定义为程序的所有源文件中包含的所有程序文本，类型的程序文本定义为该类型的*type_declaration*中包含的所有程序文本（包括、可能是嵌套在类型中的类型）。

预定义类型的可访问域（例如 `object`、`int` 或 `double`）不受限制。

在程序 `P` 中声明的顶级未绑定类型 @no__t 的可访问域（[绑定类型和未绑定类型](types.md#bound-and-unbound-types)）的定义如下：

*  如果 `T` 的声明的可访问性 @no__t 为-1，@no__t 的可访问域就是 @no__t 3 的程序文本和引用 `P` 的任何程序。
*  如果 `T` 的已声明可访问性为 `internal`，则 `T` 的可访问域就是 `P` 的程序文本。

从这些定义开始，顶级未绑定类型的可访问域始终至少是声明该类型的程序的程序文本。

构造类型 `T<A1, ..., An>` 的可访问性域是未绑定的泛型类型的可访问域的交集 `T` 和类型参数 `A1, ..., An` 的可访问性域的交集。

在程序 `P` 中声明的类型 `T` 中声明的嵌套成员 `M` 的可访问域定义如下（请注意，@no__t 本身可能是一个类型）：

*  如果 `M` 的已声明可访问性为 `public`，则 `M` 的可访问域就是 `T` 的可访问域。
*  如果 `M` 的声明的可访问性 @no__t 为-1，则允许 `D` 作为从 @no__t 到 @no__t 之外声明的任何类型的程序 @no__t 文本的联合，该程序文本与 @No__t 的可访问域是 @no__t 的可访问域与 `D` 的交集。
*  如果 `M` 的声明的可访问性 @no__t 为-1，则允许 `D` 作为 @no__t 3 的程序文本和从 @no__t 派生的任何类型的程序文本的联合。 @No__t 的可访问域是 @no__t 的可访问域与 `D` 的交集。
*  如果 `M` 的已声明可访问性为 `internal`，则 `M` 的可访问域就是 `T` 的可访问域与 `P` 的程序文本之间的交集。
*  如果 `M` 的已声明可访问性为 `private`，则 `M` 的可访问域就是 `T` 的程序文本。

从这些定义开始，嵌套成员的可访问域始终至少是声明该成员的类型的程序文本。 此外，它还会将成员的可访问域与声明该成员的类型的可访问性域完全相同。

直观地说，当访问一个类型或成员 `M` 时，将评估以下步骤以确保允许访问：

*  首先，如果在类型（而不是编译单元或命名空间）中声明 `M`，则当该类型不可访问时，将发生编译时错误。
*  如果 `M` @no__t 为-1，则允许访问。
*  否则，如果 `M` @no__t 为-1，则允许访问，如果它出现在声明了 `M` 的程序中，或出现在派生自类的类 @no__t 中，并且该类是通过派生类类型进行的（[受保护实例成员的访问权限](basic-concepts.md#protected-access-for-instance-members)）。
*  否则，如果 `M` @no__t 为-1，则允许访问，如果它出现在声明了 `M` 的类中，或者出现在派生自 @no__t 类的类中，而该类是通过派生类类型（[受保护实例成员的访问权限](basic-concepts.md#protected-access-for-instance-members)）。
*  否则，如果 `M` @no__t 为-1，则允许访问，如果在声明 `M` 的程序中发生。
*  否则，如果 `M` @no__t 为-1，则允许访问，如果在声明 `M` 的类型内发生。
*  否则，将无法访问该类型或成员，并发生编译时错误。

示例中
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
类和成员具有以下可访问域：

*  @No__t 的可访问性域（0）和 `A.X` 无限制。
*  @No__t 的可访问域，`B`，`B.X`，`B.Y`，`B.C`，`B.C.X`，`B.C.Y` 是包含程序的程序文本。
*  @No__t 的可访问域是 @no__t 的程序文本。
*  @No__t 的可访问性域（0）和 `B.D` 是 `B` 的程序文本，包括 `B.C` 和 @no__t 的程序文本。
*  @No__t 的可访问域是 @no__t 的程序文本。
*  @No__t 的可访问性域（0）和 `B.D.Y` 是 `B` 的程序文本，包括 `B.C` 和 @no__t 的程序文本。
*  @No__t 的可访问域是 @no__t 的程序文本。

如示例所示，成员的可访问域绝不会超出包含类型的可访问域。 例如，即使所有 @no__t 0 的成员都具有公开声明的可访问性，但所有 `A.X` 都包含受包含类型约束的可访问域。

如[成员](basic-concepts.md#members)中所述，基类的所有成员（实例构造函数、析构函数和静态构造函数除外）都是由派生类型继承的。 这包括甚至是基类的私有成员。 但是，私有成员的可访问域只包含声明该成员的类型的程序文本。 示例中
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
`B` 类从 `A` 类继承私有成员 @no__t。 由于此成员是私有的，因此只能在 `A` 的*class_body*内访问。 因此，对 @no__t 0 的访问成功于 `A.F` 方法中，但在 `B.F` 方法中失败。

### <a name="protected-access-for-instance-members"></a>实例成员的受保护访问

如果在声明 @no__t 0 实例成员的类的程序文本的外部访问该成员，并且在声明该成员的程序的程序文本的外部访问 @no__t 1 实例成员，则必须在类中进行访问派生自其声明类的声明。 此外，需要通过派生类类型的实例或从其构造的类类型进行访问。 此限制可防止一个派生类访问其他派生类的受保护成员，即使这些成员是从同一个基类继承时也是如此。

让 @no__t 是一个基类，该基类声明受保护的实例成员 `M`，并让 `D` 是派生自 @no__t 的类。 在 `D` 的*class_body*中，对 @no__t 的访问可以采用以下形式之一：

*  格式 @no__t 的非限定的*type_name*或*primary_expression* 。
*  如果 `E` 的类型为 `T` 或从 `T` 派生的类，则为形式 `E.M` 的*primary_expression* ，其中 `T` 是类类型 `D`，或是从 @no__t 构造的类类型
*  形式 `base.M` 的*primary_expression* 。

除这些形式的访问权限外，派生类还可以访问*constructor_initializer*中的基类的受保护实例构造函数（[构造函数初始值设定项](classes.md#constructor-initializers)）。

示例中
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
在 `A` 中，可以通过 @no__t 2 和 `B` 的实例访问 @no__t 1，因为在这两种情况下，都可以通过 @no__t 的实例进行访问，或从 @no__t 5 派生的类进行访问。 但是，在 `B` 中，无法通过 `A` 的实例访问 @no__t，因为 `A` 不从 @no__t 派生。

示例中
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
允许对 @no__t 的三个赋值，因为它们都是通过从泛型类型构造的类类型的实例进行的。

### <a name="accessibility-constraints"></a>辅助功能约束

语言中的C#几个构造要求类型必须至少具有与成员或其他类型***一样的可访问性***。 如果 `T` 的可访问域是 `M` 的可访问域的超集，则将类型 `T` 称为至少可作为成员或类型进行访问 `M`。 换言之，如果 @no__t 可访问的所有 @no__t 上下文中都可访问-2，则 `T` 至少可作为 `M` 可访问。

存在以下可访问性约束：

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
*  实例构造函数的参数类型必须至少具有与实例构造函数本身相同的可访问性。

示例中
```csharp
class A {...}

public class B: A {...}
```
`B` 类会导致编译时错误，因为 `A` 至少不能作为 `B` 的访问。

同样，在示例中
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
`B` 中的 `H` 方法导致编译时错误，因为 `A` 的返回类型并不至少与方法相同。

## <a name="signatures-and-overloading"></a>签名和重载

方法、实例构造函数、索引器和运算符按其***签名***特征：

*  方法的签名由方法的名称、类型参数的数目以及其每个形参的类型和种类（值、引用或输出）组成，按从左到右的顺序排列。 出于这些目的，形参的类型中出现的方法的任何类型形参均由其名称标识，但其在方法的类型实参列表中的序号位置。 具体而言，方法的签名不包括返回类型、可为最右侧参数指定的 `params` 修饰符和可选的类型参数约束。
*  实例构造函数的签名由其每个形参的类型和类型（值、引用或输出）组成，按从左到右的顺序排列。 实例构造函数的签名具体不包括可以为最右侧参数指定的 `params` 修饰符。
*  索引器的签名由其每个形参的类型组成，按从左到右的顺序排列。 索引器的签名具体不包括元素类型，也不包括可以为最右侧参数指定的 `params` 修饰符。
*  运算符的签名由运算符的名称和它的每个形参的类型组成，按从左到右的顺序排列。 运算符的签名具体不包括结果类型。

签名是用于在类、结构和接口中***重载***成员的启用机制：

*  方法的重载允许类、结构或接口声明多个具有相同名称的方法，前提是它们的签名在该类、结构或接口中是唯一的。
*  实例构造函数的重载允许类或结构声明多个实例构造函数，前提是它们的签名在该类或结构中是唯一的。
*  索引器的重载允许类、结构或接口声明多个索引器，前提是这些索引器的签名在该类、结构或接口中是唯一的。
*  运算符的重载允许类或结构声明多个具有相同名称的运算符，前提是它们的签名在该类或结构中是唯一的。

尽管 @no__t 0 和 @no__t 1 参数修饰符被视为签名的一部分，但在单个类型中声明的成员不能仅由 `ref` 和 `out` 中的签名有所不同。 如果两个成员在同一类型中声明，但如果两个 @no__t 方法中的所有参数都已更改为 `ref` 修饰符，则会发生编译时错误。 对于签名匹配的其他目的（例如，隐藏或重写），`ref`，`out` 将被视为签名的一部分，并且彼此之间不匹配。 （这一限制是为了C#使程序可以轻松地翻译为在公共语言基础结构（CLI）上运行，而不提供一种方法来定义仅在 `ref` 和 `out` 中存在差异的方法。）

对于签名，类型 `object` 和 `dynamic` 被视为相同。 因此，在单个类型中声明的成员可以不区分签名，而只是 `object` 和 `dynamic`。

下面的示例演示一组重载方法声明及其签名。
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

请注意，任何 @no__t 0 和 @no__t 参数修饰符（[方法参数](classes.md#method-parameters)）都是签名的一部分。 因此，@no__t 0 和 @no__t 为唯一的签名。 但是，不能在同一接口内声明 `F(ref int)` 和 `F(out int)`，因为它们的签名仅因 `ref` 和 @no__t 而有所不同。 另请注意，返回类型和 `params` 修饰符不是签名的一部分，因此不可能仅基于返回类型或在包含或排除 @no__t 修饰符时重载。 同样，在上面标识的方法的声明 `F(int)` 和 @no__t 1 会导致编译时错误。

## <a name="scopes"></a>范围

名称的***作用域***是程序文本的区域，在该区域中，可以引用名称声明的实体，而无需限定名称。 可以***嵌套***作用域，内部作用域可以从外部作用域中重新声明名称的含义（但这并不能删除嵌套块中的[声明](basic-concepts.md#declarations)施加的限制，不能使用与封闭块中的本地变量相同的名称）。 然后，在内部范围所涵盖的程序文本区域中***隐藏***外部范围的名称，并且只能通过限定名称来访问外部名称。

*  *Namespace_member_declaration* （[命名空间成员](namespaces.md#namespace-members)）声明的命名空间成员的作用域不包含任何封闭*namespace_declaration* ，是整个程序文本。
*  *Namespace_declaration*中*namespace_member_declaration*声明的命名空间成员的作用域，其完全限定名称为 `N` 是每个*namespace_declaration*的*namespace_body* ，其完全限定名称为限定名 `N` 或以 `N` 开始，后跟一个句点。
*  由*extern_alias_directive*定义的名称的作用域超出其立即包含编译单元或命名空间正文的*using_directive*s、 *global_attributes*和*namespace_member_declaration*。 *Extern_alias_directive*不会将任何新成员分配给基础声明空间。 换句话说， *extern_alias_directive*是不可传递的，而是仅影响其出现在其中的编译单元或命名空间正文。
*  *Using_directive* （[使用指令](namespaces.md#using-directives)）定义或导入的名称的作用域扩展到*compilation_unit*或*namespace_body*的*namespace_member_declaration*，其中*using_directive*发生。 *Using_directive*可以使特定*compilation_unit*或*namespace_body*中的零个或多个命名空间、类型或成员名称可用，但不会将任何新成员分配给基础声明空间。 换句话说， *using_directive*是不可传递的，而只会影响发生它的*compilation_unit*或*namespace_body* 。
*  由*class_declaration*上的*type_parameter_list*声明的类型参数的作用域（[类声明](classes.md#class-declarations)）是该的*class_base*、 *type_parameter_constraints_clause*和*class_body* 。 *class_declaration*。
*  由*struct_declaration*上的*type_parameter_list*声明的类型参数的作用域（[结构声明](structs.md#struct-declarations)）是的*struct_interfaces*、 *type_parameter_constraints_clause*和*struct_body*该*struct_declaration*。
*  由*interface_declaration*上的*type_parameter_list*声明的类型参数的作用域（[接口声明](interfaces.md#interface-declarations)）为*interface_base*、 *type_parameter_constraints_clause*和*interface_body*的*interface_declaration*。
*  由*delegate_declaration*上的*type_parameter_list*声明的类型参数的作用域（[委托声明](delegates.md#delegate-declarations)）为*return_type*、 *formal_parameter_list*和*type_parameter_constraints_clause*的*delegate_declaration*。
*  *Class_member_declaration* （[类体](classes.md#class-body)）声明的成员的作用域是在其中进行声明的*class_body* 。 此外，类成员的作用域扩展到包含在该成员的可访问域（[可访问](basic-concepts.md#accessibility-domains)域）中的派生类的*class_body* 。
*  *Struct_member_declaration* （[结构成员](structs.md#struct-members)）声明的成员的作用域是在其中进行声明的*struct_body* 。
*  *Enum_member_declaration* （[enum 成员](enums.md#enum-members)）声明的成员的作用域是在其中进行声明的*enum_body* 。
*  在*method_declaration* （[方法](classes.md#methods)）中声明的参数的作用域是该*method_declaration*的*method_body* 。
*  在*indexer_declaration* （[索引器](classes.md#indexers)）中声明的参数的作用域是该*indexer_declaration*的*accessor_declarations* 。
*  在*operator_declaration* （[运算符](classes.md#operators)）中声明的参数的作用域是该*operator_declaration*的*块*。
*  在*constructor_declaration* （[实例构造函数](classes.md#instance-constructors)）中声明的参数的作用域是该*constructor_declaration*的*constructor_initializer*和*块*。
*  在*lambda_expression* （[匿名函数表达式](expressions.md#anonymous-function-expressions)）中声明的参数的作用域是该*lambda_expression*的*anonymous_function_body*
*  在*anonymous_method_expression* （[匿名函数表达式](expressions.md#anonymous-function-expressions)）中声明的参数的作用域是该*anonymous_method_expression*的*块*。
*  在*labeled_statement* （[标记的语句](statements.md#labeled-statements)）中声明的标签的作用域是在其中进行声明的*块*。
*  在*local_variable_declaration* （[局部变量声明](statements.md#local-variable-declarations)）中声明的局部变量的作用域是在其中进行声明的块。
*  在 `switch` 语句（[switch 语句](statements.md#the-switch-statement)）的*switch_block*中声明的局部变量的范围为*switch_block*。
*  在 `for` 语句（[for 语句](statements.md#the-for-statement)）的*for_initializer*中声明的局部变量的作用域是*for_initializer*、 *for_condition*、 *for_iterator*和包含的*语句*。@no__t 7 语句。
*  在*local_constant_declaration*中声明的局部常量的作用域（[局部常量声明](statements.md#local-constant-declarations)）是在其中进行声明的块。 在其*constant_declarator*之前的文本位置引用本地常量是编译时错误。
*  声明为*foreach_statement*、 *using_statement*、 *lock_statement*或*query_expression*的一部分的变量的作用域由给定构造的扩展确定。

在命名空间、类、结构或枚举成员的作用域内，可以引用成员声明之前的文本位置中的成员。 例如
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
此处，在声明之前，`F` 引用 @no__t。

在局部变量的作用域内，在本地变量的*local_variable_declarator*之前的文本位置引用本地变量是编译时错误。 例如
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

在上面的 `F` 方法中，第一次分配给 @no__t 的第1项并未引用在外部作用域中声明的字段。 相反，它是指局部变量，它会导致编译时错误，因为它在此变量的声明之前。 在 `G` 方法中，在 `j` 的声明的初始值设定项中使用 @no__t 是有效的，因为使用不在*local_variable_declarator*之前。 在 `H` 方法中，后续*local_variable_declarator*正确引用在同一*local_variable_declaration*中的早期*local_variable_declarator*中声明的局部变量。

局部变量的作用域规则旨在确保表达式上下文中使用的名称的含义在块中始终相同。 如果本地变量的作用域只是从其声明扩展到块的末尾，则第一个赋值将分配给实例变量，第二个赋值将分配给局部变量，这可能会导致编译时错误（如果以后要重新排列块的语句）。

块内的名称含义可能因使用该名称的上下文而异。 示例中
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
名称 `A` 用于在表达式上下文中引用局部变量 `A` 和在类型上下文中以引用类 `A`。

### <a name="name-hiding"></a>名称隐藏

实体的作用域通常比实体的声明空间包含更多的程序文本。 具体而言，实体的范围可能包括引入新声明空间的声明，这些声明空间包含具有相同名称的实体。 此类声明会使原始实体处于***隐藏状态***。 相反，如果实体未隐藏，则称该实体***可见***。

如果范围通过嵌套重叠，并且作用域通过继承重叠，则会发生名称隐藏。 以下各节将介绍这两种隐藏类型的特征。

#### <a name="hiding-through-nesting"></a>通过嵌套隐藏

由于嵌套命名空间或命名空间中的类型、类或结构中的嵌套类型以及参数和局部变量声明的结果，因此可以通过嵌套命名空间。

示例中
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
在 `F` 方法中，实例变量 `i` 由局部变量 `i` 隐藏，但在 `G` 方法中，`i` 仍引用实例变量。

当内部范围中的名称隐藏外部范围内的名称时，它将隐藏该名称的所有重载匹配项。 示例中
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
调用 `F(1)` 会调用 `Inner` 中声明的 @no__t，因为内部声明隐藏了 @no__t 的所有外部匹配项。 由于同样的原因，调用 `F("Hello")` 会导致编译时错误。

#### <a name="hiding-through-inheritance"></a>通过继承隐藏

当类或结构重新声明从基类继承的名称时，通过继承进行名称隐藏。 此类名称隐藏采用以下形式之一：

*  类或结构中引入的常数、字段、属性、事件或类型隐藏了具有相同名称的所有基类成员。
*  类或结构中引入的方法会隐藏所有具有相同名称的非方法基类成员，并隐藏具有相同签名的所有基类方法（方法名称和参数计数、修饰符和类型）。
*  类或结构中引入的索引器会隐藏具有相同签名的所有基类索引器（参数计数和类型）。

控制运算符声明（[运算符](classes.md#operators)）的规则使得派生类不可能使用与基类中的运算符相同的签名来声明运算符。 因此，运算符永远不会彼此隐藏。

与隐藏外部作用域中的名称相反，从继承的作用域中隐藏可访问的名称会导致报告警告。 示例中
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
`Derived` 中 @no__t 的声明会导致报告警告。 隐藏继承名称特别不是错误，因为这会阻止基类的单独演化。 例如，上述情况可能已涉及，因为更高版本的 `Base` 引入了类的早期版本中不存在的 @no__t 1 方法。 如果上述情况是错误的，则对单独的版本控制类库中的基类进行的任何更改都可能导致派生类无效。

通过使用 `new` 修饰符，可以消除通过隐藏继承名称导致的警告：
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

@No__t-0 修饰符指示 `Derived` 中的 @no__t 为 "new"，它确实用于隐藏继承的成员。

新成员的声明仅在新成员的范围内隐藏继承的成员。

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

在上面的示例中，`Derived` 中 `F` 的声明隐藏了从 `Base` 继承的 `F`，但由于 @no__t 5 中的新 @no__t 有私有访问权限，因此它的作用域不会扩展到 `MoreDerived`。 因此，`MoreDerived.G` 中的调用 `F()` 有效，将调用 `Base.F`。

## <a name="namespace-and-type-names"></a>命名空间和类型名称

程序中的多个上下文需要指定*namespace_name*或*type_name。* C#

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

*Namespace_name*是引用命名空间的*namespace_or_type_name* 。 按照下面所述的解决方法， *namespace_name*的*namespace_or_type_name*必须引用命名空间，否则将发生编译时错误。 *Namespace_name*中不能存在任何类型参数（[类型参数](types.md#type-arguments)）（类型只能有类型参数）。

*Type_name*是引用类型的*namespace_or_type_name* 。 按照下面所述的解决方法， *type_name*的*namespace_or_type_name*必须引用类型，否则将发生编译时错误。

如果*namespace_or_type_name*是一个限定别名成员，则其含义如[命名空间别名限定符](namespaces.md#namespace-alias-qualifiers)中所述。 否则， *namespace_or_type_name*具有四种形式之一：

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

其中 `I` 是单个标识符，`N` 为*namespace_or_type_name* ，@no__t 为可选的*type_argument_list*。 如果未指定*type_argument_list* ，请考虑 `k` 为零。

确定*namespace_or_type_name*的含义，如下所示：

*   如果*namespace_or_type_name*的格式 @no__t 为-1 或格式为 `I<A1, ..., Ak>`：
    * 如果 @no__t 为零且*namespace_or_type_name*显示在泛型方法声明（[方法](classes.md#methods)）中，并且如果声明包含名称为 @ no__t 的类型参数（[类型参数](classes.md#type-parameters)），则*namespace_or_type_名称*是指该类型参数。
    * 否则，如果*namespace_or_type_name*出现在类型声明中，则对于每个实例类型 @ no__t （[实例类型](classes.md#the-instance-type)），从该类型声明的实例类型开始，并继续使用每个实例的实例类型。封闭类或结构声明（如果有）：
        * 如果 @no__t 为零，而 @no__t 的声明包含名为 @ no__t 的 type 参数，则*namespace_or_type_name*将引用该类型参数。
        * 否则，如果*namespace_or_type_name*出现在类型声明的主体中，`T` 或它的任何基类型包含名为 @ no__t-2 和 `K` @ no__t-4type 参数的嵌套可访问类型，则*namespace_or_type_name*是指用给定类型参数构造的类型。 如果有多个这样的类型，则选择在派生程度较大的类型中声明的类型。 请注意，在确定的含义时，将忽略非类型成员（常数、字段、方法、属性、索引器、运算符、实例构造函数、析构函数和静态构造函数）和具有不同数量的类型参数的类型成员。*namespace_or_type_name*。
    * 如果前面的步骤不成功，则对于每个命名空间 @ no__t，从*namespace_or_type_name*发生的命名空间开始，继续每个封闭命名空间（如果有），并以全局命名空间结束，以下在查找实体之前计算步骤：
        * 如果 @no__t 为零且 @no__t 为 @ no__t 中的命名空间的名称，则：
            * 如果发生*namespace_or_type_name*的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含将名称 @ no 关联的*extern_alias_directive*或*using_alias_directive*__t-4，具有命名空间或类型，则*namespace_or_type_name*不明确，并发生编译时错误。
            * 否则， *namespace_or_type_name*引用 `N` 中名为 `I` 的命名空间。
        * 否则，如果 `N` 包含名为 @ no__t 的可访问类型和 `K` @ no__t-3type 参数，则：
            * 如果 @no__t 为零，且*namespace_or_type_name*的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含*extern_alias_directive*或*using_alias_directive* ，则将名称 @ no__t 与命名空间或类型相关联，然后*namespace_or_type_name*是不明确的，并发生编译时错误。
            * 否则， *namespace_or_type_name*将引用用给定类型参数构造的类型。
        * 否则，如果*namespace_or_type_name*发生的位置由 `N` 的命名空间声明括起来，则为; 否则为。
            * 如果 @no__t 为零，命名空间声明包含*extern_alias_directive*或*using_alias_directive* ，它将名称 @ no__t 与导入的命名空间或类型相关联，则*namespace_or_type_name*引用命名空间或类型。
            * 否则，如果由命名空间声明中的*using_namespace_directive*s 和*using_alias_directive*s 导入的命名空间和类型声明包含一个名称为 @ no__t 的可访问类型和 `K` @ no__t-4type参数，则*namespace_or_type_name*引用以给定类型参数构造的类型。
            * 否则，如果由命名空间声明的*using_namespace_directive*s 和*using_alias_directive*s 导入的命名空间和类型声明包含多个名称为 @ no__t 和 `K` @ no__t-4type 的可访问类型参数，则*namespace_or_type_name*是不明确的，并发生错误。
    * 否则， *namespace_or_type_name*是未定义的，并发生编译时错误。
*  否则， *namespace_or_type_name*的格式 @no__t 为-1 或格式为 `N.I<A1, ..., Ak>`。 @no__t 首先解析为*namespace_or_type_name*。 如果 @no__t 的解析失败，则会发生编译时错误。 否则，将按如下方式解析 `N.I` 或 `N.I<A1, ..., Ak>`：
    * 如果 @no__t 为零，`N` 表示命名空间，`N` 包含名称 @no__t 为3的嵌套命名空间，则*namespace_or_type_name*引用该嵌套命名空间。
    * 否则，如果 `N` 引用命名空间，并且 @no__t 包含名称为 @ no__t 的可访问类型和 `K` @ 4type 参数，则*namespace_or_type_name*引用使用给定类型参数构造的类型。
    * 否则，如果 `N` 引用（可能构造的）类或结构类型，`N` 或它的任何一个基类包含名称为 @ no__t 的嵌套可访问性类型和 @no__t @ 4type 参数，则*namespace_or_type_name*引用用给定的类型参数构造的类型。 如果有多个这样的类型，则选择在派生程度较大的类型中声明的类型。 请注意，如果将 `N.I` 的含义确定为解析 @no__t 的基类规范，则 `N` 的直接基类被视为对象（[基类](classes.md#base-classes)）。
    * 否则，@no__t 为无效的*namespace_or_type_name*，并发生编译时错误。

仅当使用*namespace_or_type_name*时，才能引用静态类（[静态类](classes.md#static-classes)）

*  *Namespace_or_type_name*是 `T.I` 形式的*namespace_or_type_name*中的 `T`，或
*  *Namespace_or_type_name*是*typeof_expression* （[参数列表](expressions.md#argument-lists)1）中格式为 `typeof(T)` 的 @no__t。

### <a name="fully-qualified-names"></a>完全限定的名称

每个命名空间和类型都有一个***完全限定的名称***，该名称唯一地标识命名空间或类型。 按如下方式确定命名空间或类型 `N` 的完全限定名：

*  如果 `N` 是全局命名空间的成员，则其完全限定名称为 `N`。
*  否则，其完全限定名称为 `S.N`，其中 `S` 是声明 `N` 的命名空间或类型的完全限定名称。

换句话说，@no__t 的完全限定名是从全局命名空间开始 @no__t 的标识符的完整层次结构路径。 由于命名空间或类型的每个成员都必须具有唯一的名称，因此命名空间或类型的完全限定名称始终是唯一的。

下面的示例显示了几个命名空间和类型声明及其关联的完全限定名称。
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

C#采用自动内存管理，使开发人员无手动分配和释放由对象占用的内存。 自动内存管理策略是由***垃圾回收器***实现的。 对象的内存管理生命周期如下所示：

1. 创建对象时，将为其分配内存，运行构造函数，并将对象视为实时对象。
2. 如果对象或它的任何部分不能通过任何可能的执行继续进行访问，而不是运行析构函数，则该对象被视为不再使用，并可用于销毁。 C#编译器和垃圾回收器可以选择对代码进行分析，以确定将来可能会使用哪些对对象的引用。 例如，如果作用域中的局部变量是唯一的对象引用，但在过程中从当前执行点到执行的任何可能的继续，则不会引用该局部变量，垃圾回收器可能（但不是需要）将对象视为不再使用。
3. 对象符合析构条件后，在某些以后未指定的情况下，将运行该对象的析构[函数（如果有）。](classes.md#destructors) 正常情况下，对象的析构函数只运行一次，不过实现特定的 Api 可能会允许重写此行为。
4. 一旦运行某个对象的析构函数，如果该对象或它的任何部分不能通过任何可能的执行继续进行访问，包括运行析构函数，则该对象将被视为不可访问，并且该对象便可用于收集。
5. 最后，在对象变为符合集合条件后，垃圾回收器将释放与该对象关联的内存。

垃圾回收器维护有关对象使用情况的信息，并使用此信息进行内存管理决策，如在内存中查找新创建的对象的位置、何时重新定位对象以及对象不再使用或无法访问。

与其他假设存在垃圾回收器的语言一样， C#设计使垃圾回收器可以实现各种内存管理策略。 例如， C#不要求运行析构函数，或在对象符合条件时收集对象，或者在任何特定的线程上以任何特定的顺序运行析构函数。

可以通过类上的静态方法控制垃圾回收器的行为，使其在一定程度上控制 `System.GC`。 此类可用于请求进行集合、运行（或不运行）等操作。

由于垃圾回收器允许广泛的纬度来决定何时收集对象和运行析构函数，因此一致的实现可能产生与下面的代码所示不同的输出。 程序
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
创建类 @no__t 的实例，`B` 的实例。 如果为变量 `b` 赋值 `null`，则这些对象将可以进行垃圾回收，因为在此之后，任何用户编写的代码都无法访问它们。 输出可以是

```console
Destruct instance of A
Destruct instance of B
```
或
```console
Destruct instance of B
Destruct instance of A
```
因为该语言对对象的垃圾回收顺序没有约束。

在微妙的情况下，"符合析构条件" 和 "符合集合条件" 之间的区别非常重要。 例如，应用于对象的
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

在上述程序中，如果垃圾回收器选择在 `B` 的析构函数之前运行 `A` 的析构函数，则该程序的输出可能是：
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

请注意，尽管 @no__t 的实例未被使用，并且运行了 `A` 的析构函数，但仍有可能从另一个析构函数调用 `A` （在本例中为 `F`）的方法。 另外，请注意，运行析构函数可能会导致对象再次可从主线程序使用。 在这种情况下，运行 @no__t 0 的析构函数会导致先前未使用的 @no__t 实例变为可从实时引用 `Test.RefA`。 调用 `WaitForPendingFinalizers` 后，`B` 的实例符合集合条件，但 @no__t 的实例不是，因为引用 @no__t 为3。

为了避免混淆和意外行为，通常情况下，析构函数只对存储在其自身字段中的数据执行清理，而不对引用的对象或静态字段执行任何操作。

使用析构函数的替代方法是让类实现 `System.IDisposable` 接口。 这允许对象的客户端确定何时释放对象的资源，这通常是通过在 `using` 语句（[using 语句](statements.md#the-using-statement)）中以资源的形式访问对象的。

## <a name="execution-order"></a>执行顺序

C#程序的执行将继续，使每个正在执行的线程的副作用被保留在关键执行点上。 ***副作用***定义为可变字段的读取或写入、对非易失性变量的写入、写入外部资源和引发异常。 必须保留这些副作用的顺序的关键执行点是对可变字段的引用（[可变字段](classes.md#volatile-fields)）、`lock` 语句（[lock 语句](statements.md#the-lock-statement)）以及线程创建和终止。 执行环境可随意更改C#程序的执行顺序，但要服从以下限制：

*  数据依赖关系在执行线程中保留。 也就是说，将计算每个变量的值，就好像该线程中的所有语句都是按原程序顺序执行的一样。
*  保留初始化排序规则（[字段初始化](classes.md#field-initialization)和[变量初始值设定项](classes.md#variable-initializers)）。
*  对于易失读写（[可变字段](classes.md#volatile-fields)），会保留副作用的顺序。 此外，如果执行环境可以推断出未使用表达式的值并且不会产生所需的副作用（包括通过调用方法或访问可变字段引起的任何副作用），则执行环境不需要计算表达式的一部分。 当异步事件（如由另一个线程引发的异常）中断程序执行时，不保证可观察到的副作用在原始程序顺序中可见。
