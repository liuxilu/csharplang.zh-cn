---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704008"
---
# <a name="basic-concepts"></a><span data-ttu-id="e3581-101">基本概念</span><span class="sxs-lookup"><span data-stu-id="e3581-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="e3581-102">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="e3581-102">Application Startup</span></span>

<span data-ttu-id="e3581-103">具有***入口点***的程序集称为***应用程序***。</span><span class="sxs-lookup"><span data-stu-id="e3581-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="e3581-104">当应用程序运行时，将创建一个新的***应用程序域***。</span><span class="sxs-lookup"><span data-stu-id="e3581-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="e3581-105">同一台计算机上可以同时存在多个不同的应用程序实例，每个实例都有自己的应用程序域。</span><span class="sxs-lookup"><span data-stu-id="e3581-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="e3581-106">应用程序域通过充当应用程序状态的容器来实现应用程序隔离。</span><span class="sxs-lookup"><span data-stu-id="e3581-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="e3581-107">应用程序域充当应用程序中定义的类型和它所使用的类库的容器和边界。</span><span class="sxs-lookup"><span data-stu-id="e3581-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="e3581-108">加载到一个应用程序域中的类型不同于加载到另一个应用程序域中的相同类型，并且不会在应用程序域之间直接共享对象的实例。</span><span class="sxs-lookup"><span data-stu-id="e3581-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="e3581-109">例如，每个应用程序域都有自己的静态变量副本用于这些类型，并且每个应用程序域最多运行一次类型的静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="e3581-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="e3581-110">实现可自由提供特定于实现的策略，也可以创建和销毁应用程序域。</span><span class="sxs-lookup"><span data-stu-id="e3581-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="e3581-111">当执行环境调用指定方法（称为应用程序的入口点）时，将发生***应用程序启动***。</span><span class="sxs-lookup"><span data-stu-id="e3581-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="e3581-112">此入口点方法始终命名为 `Main`，可以具有以下签名之一：</span><span class="sxs-lookup"><span data-stu-id="e3581-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="e3581-113">如图所示，入口点可以选择返回 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="e3581-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="e3581-114">此返回值用于应用程序终止（[应用程序终止](basic-concepts.md#application-termination)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="e3581-115">入口点还可以有一个形参。</span><span class="sxs-lookup"><span data-stu-id="e3581-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="e3581-116">参数可以具有任何名称，但参数的类型必须为 `string[]`。</span><span class="sxs-lookup"><span data-stu-id="e3581-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="e3581-117">如果正式参数存在，则执行环境将创建并传递一个 @no__t 的参数，该参数包含在启动应用程序时指定的命令行参数。</span><span class="sxs-lookup"><span data-stu-id="e3581-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="e3581-118">@No__t 参数决不会为 null，但如果未指定命令行参数，则其长度可能为零。</span><span class="sxs-lookup"><span data-stu-id="e3581-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="e3581-119">由于C#支持方法重载，因此类或结构可能包含某些方法的多个定义，前提是每个定义具有不同的签名。</span><span class="sxs-lookup"><span data-stu-id="e3581-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="e3581-120">但是，在一个程序中，任何类或结构都不能包含多个称为 @no__t 的方法，它的定义限定它用作应用程序入口点。</span><span class="sxs-lookup"><span data-stu-id="e3581-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="e3581-121">但允许 @no__t 的其他重载版本-0，但前提是它们具有多个参数，或者其唯一参数不是类型 `string[]`。</span><span class="sxs-lookup"><span data-stu-id="e3581-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="e3581-122">应用程序可以由多个类或结构组成。</span><span class="sxs-lookup"><span data-stu-id="e3581-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="e3581-123">其中的多个类或结构可能包含一个名为 @no__t 的方法，该方法的定义将其限定为作为应用程序入口点使用。</span><span class="sxs-lookup"><span data-stu-id="e3581-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="e3581-124">在这种情况下，必须使用外部机制（例如命令行编译器选项）来选择其中一个 `Main` 方法作为入口点。</span><span class="sxs-lookup"><span data-stu-id="e3581-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="e3581-125">在C#中，每个方法都必须定义为类或结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="e3581-126">通常，方法的声明的可访问性（已[声明的可访问性](basic-concepts.md#declared-accessibility)）由其声明中指定的访问修饰符（[访问修饰符](classes.md#access-modifiers)）确定，并且类似于类型的声明的可访问性由在其声明中指定的访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="e3581-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="e3581-127">为了使给定类型的给定方法可调用，该类型和成员都必须可访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="e3581-128">但是，应用程序入口点是一种特殊情况。</span><span class="sxs-lookup"><span data-stu-id="e3581-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="e3581-129">具体而言，执行环境可以访问应用程序的入口点，无论其声明的可访问性以及其封闭类型声明的声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="e3581-130">应用程序入口点方法不能在泛型类声明中。</span><span class="sxs-lookup"><span data-stu-id="e3581-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="e3581-131">在所有其他方面，入口点方法的行为类似于非入口点的行为。</span><span class="sxs-lookup"><span data-stu-id="e3581-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="e3581-132">应用程序终止</span><span class="sxs-lookup"><span data-stu-id="e3581-132">Application termination</span></span>

<span data-ttu-id="e3581-133">***应用程序终止***会向执行环境返回控制权。</span><span class="sxs-lookup"><span data-stu-id="e3581-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="e3581-134">如果应用程序的***入口点***方法的返回类型为 `int`，则返回的值将用作应用程序的***终止状态代码***。</span><span class="sxs-lookup"><span data-stu-id="e3581-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="e3581-135">此代码的目的是允许向执行环境进行成功或失败的通信。</span><span class="sxs-lookup"><span data-stu-id="e3581-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="e3581-136">如果入口点方法的返回类型为 `void`，则到达右大括号（@no__t 为-1）以终止该方法，或执行没有表达式的 `return` 语句会导致终止状态代码 @no__t 为3。</span><span class="sxs-lookup"><span data-stu-id="e3581-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="e3581-137">在应用程序终止之前，将调用其所有尚未进行垃圾回收的对象的析构函数，除非已取消此类清理（例如，通过调用库方法 `GC.SuppressFinalize`）。</span><span class="sxs-lookup"><span data-stu-id="e3581-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="e3581-138">声明</span><span class="sxs-lookup"><span data-stu-id="e3581-138">Declarations</span></span>

<span data-ttu-id="e3581-139">C#程序中的声明定义程序的构成元素。</span><span class="sxs-lookup"><span data-stu-id="e3581-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="e3581-140">C#使用命名空间（[命名空间](namespaces.md)）对程序进行组织，其中可以包含类型声明和嵌套命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="e3581-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="e3581-141">类型声明（[类型声明](namespaces.md#type-declarations)）用于定义类（[类](classes.md)）、结构（[结构](structs.md)）、接口（[接口](interfaces.md)）、枚举（[枚举](enums.md)）和委托（[委托](delegates.md)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="e3581-142">类型声明中允许的成员种类取决于类型声明的形式。</span><span class="sxs-lookup"><span data-stu-id="e3581-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="e3581-143">例如，类声明可以包含常量（[常量](classes.md#constants)）、字段（[字段](classes.md#fields)）、方法（[方法](classes.md#methods)）、属性（[属性](classes.md#properties)）、事件（[事件](classes.md#events)）、索引器（[索引器](classes.md#indexers)）的声明，运算符（[运算符](classes.md#operators)）、实例构造函数（[实例构造](classes.md#instance-constructors)函数）、静态构造函数（[静态构造函数](classes.md#static-constructors)）、析构函数（[析构函数](classes.md#destructors)）和嵌套类型（[嵌套类型](classes.md#nested-types)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="e3581-144">声明定义声明***空间***中声明所属的名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="e3581-145">除重载成员（[签名和重载](basic-concepts.md#signatures-and-overloading)）外，具有两个或多个将在声明空间中引入同名成员的声明是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="e3581-146">声明空间不能包含具有相同名称的不同类型的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="e3581-147">例如，声明空间不能包含具有相同名称的字段和方法。</span><span class="sxs-lookup"><span data-stu-id="e3581-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="e3581-148">有几种不同类型的声明空间，如下所述。</span><span class="sxs-lookup"><span data-stu-id="e3581-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="e3581-149">在程序的所有源文件中，不带封闭*namespace_declaration*的*namespace_member_declaration*是单个合并声明空间的成员，称为***全局声明空间***。</span><span class="sxs-lookup"><span data-stu-id="e3581-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="e3581-150">在程序的所有源文件中，在具有相同完全限定的命名空间名称的*namespace_declaration*中的*namespace_member_declaration*s 是单个组合声明空间的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="e3581-151">每个类、结构或接口声明都创建新的声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="e3581-152">通过*class_member_declaration*s、 *struct_member_declaration*s、 *interface_member_declaration*或*type_parameter*将名称引入到此声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="e3581-153">除了重载实例构造函数声明和静态构造函数声明，类或结构不能包含与类或结构同名的成员声明。</span><span class="sxs-lookup"><span data-stu-id="e3581-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="e3581-154">类、结构或接口允许声明重载方法和索引器。</span><span class="sxs-lookup"><span data-stu-id="e3581-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="e3581-155">此外，类或结构允许声明重载实例构造函数和运算符。</span><span class="sxs-lookup"><span data-stu-id="e3581-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="e3581-156">例如，类、结构或接口可能包含多个具有相同名称的方法声明，前提是这些方法声明在其签名中不同（[签名和重载](basic-concepts.md#signatures-and-overloading)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="e3581-157">请注意，基类不涉及类的声明空间，并且基接口不涉及接口的声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="e3581-158">因此，允许派生类或接口声明与继承成员同名的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="e3581-159">这种成员被称为***隐藏***继承成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="e3581-160">每个委托声明都将创建一个新的声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="e3581-161">通过形参（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*将名称引入到此声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="e3581-162">每个枚举声明都将创建一个新的声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="e3581-163">通过*enum_member_declarations*将名称引入此声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="e3581-164">每个方法声明、索引器声明、运算符声明、实例构造函数声明和匿名函数都会创建一个名为***局部变量声明空间***的新声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="e3581-165">通过形参（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*将名称引入到此声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="e3581-166">函数成员或匿名函数（如果有）的主体被视为嵌套在局部变量声明空间内。</span><span class="sxs-lookup"><span data-stu-id="e3581-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="e3581-167">局部变量声明空间和嵌套局部变量声明空间包含具有相同名称的元素是错误的。</span><span class="sxs-lookup"><span data-stu-id="e3581-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="e3581-168">因此，在嵌套的声明空间内，不能在封闭声明空间中声明与本地变量或常量同名的局部变量或常数。</span><span class="sxs-lookup"><span data-stu-id="e3581-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="e3581-169">可能有两个声明空间包含具有相同名称的元素，前提是两个声明空间都不包含另一个。</span><span class="sxs-lookup"><span data-stu-id="e3581-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="e3581-170">每个*block*或*switch_block*以及*for*、 *foreach*和*using*语句均为局部变量和局部常量创建一个局部变量声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="e3581-171">通过*local_variable_declaration*s 和*local_constant_declaration*将名称引入此声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="e3581-172">请注意，在函数成员或匿名函数的主体中发生的块嵌套在这些函数为其参数声明的局部变量声明空间内。</span><span class="sxs-lookup"><span data-stu-id="e3581-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="e3581-173">因此，例如，具有本地变量和同名参数的方法是错误的。</span><span class="sxs-lookup"><span data-stu-id="e3581-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="e3581-174">每个*block*或*switch_block*为标签创建一个单独的声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="e3581-175">通过*labeled_statement*将名称引入此声明空间，并通过*goto_statement*引用名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="e3581-176">块的***标签声明空间***包括任何嵌套块。</span><span class="sxs-lookup"><span data-stu-id="e3581-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="e3581-177">因此，在嵌套块中，不能声明与封闭块中的标签具有相同名称的标签。</span><span class="sxs-lookup"><span data-stu-id="e3581-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="e3581-178">声明名称的文本顺序通常不重要。</span><span class="sxs-lookup"><span data-stu-id="e3581-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="e3581-179">特别是，文本顺序对于命名空间、常量、方法、属性、事件、索引器、运算符、实例构造函数、析构函数、静态构造函数和类型的声明和使用并不重要。</span><span class="sxs-lookup"><span data-stu-id="e3581-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="e3581-180">声明顺序在以下方面非常重要：</span><span class="sxs-lookup"><span data-stu-id="e3581-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="e3581-181">字段声明和局部变量声明的声明顺序决定了其初始值设定项（如果有）的执行顺序。</span><span class="sxs-lookup"><span data-stu-id="e3581-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="e3581-182">局部变量必须在使用之前定义（[范围](basic-concepts.md#scopes)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="e3581-183">省略*constant_expression*值时，枚举成员声明（[enum 成员](enums.md#enum-members)）的声明顺序很重要。</span><span class="sxs-lookup"><span data-stu-id="e3581-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="e3581-184">命名空间的声明空间为 "open 已结束"，两个具有相同完全限定名称的命名空间声明会导致相同的声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="e3581-185">例如</span><span class="sxs-lookup"><span data-stu-id="e3581-185">For example</span></span>
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

<span data-ttu-id="e3581-186">上面的两个命名空间声明为同一声明空间提供，在此示例中，声明两个具有完全限定名称的类 `Megacorp.Data.Customer` 和 `Megacorp.Data.Order`。</span><span class="sxs-lookup"><span data-stu-id="e3581-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="e3581-187">由于这两个声明涉及到相同的声明空间，因此，如果每个声明都包含具有相同名称的类的声明，则会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="e3581-188">如上所述，块的声明空间包括任何嵌套块。</span><span class="sxs-lookup"><span data-stu-id="e3581-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="e3581-189">因此，在下面的示例中，`F` 和 @no__t 1 方法会导致编译时错误，因为名称 `i` 是在外部块中声明的，不能在内部块中重新声明。</span><span class="sxs-lookup"><span data-stu-id="e3581-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="e3581-190">但是，@no__t 的 @no__t 方法有效，因为这两个 `i` 在单独的非嵌套块中声明。</span><span class="sxs-lookup"><span data-stu-id="e3581-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

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

## <a name="members"></a><span data-ttu-id="e3581-191">成员</span><span class="sxs-lookup"><span data-stu-id="e3581-191">Members</span></span>

<span data-ttu-id="e3581-192">命名空间和类型具有***成员***。</span><span class="sxs-lookup"><span data-stu-id="e3581-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="e3581-193">实体的成员通常通过使用以实体引用开头的限定名称来提供，后跟一个 "`.`" 标记，后跟成员的名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="e3581-194">类型成员既可以在类型声明中声明，也可以从类型的基类***继承***。</span><span class="sxs-lookup"><span data-stu-id="e3581-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="e3581-195">当类型从基类继承时，基类的所有成员（实例构造函数、析构函数和静态构造函数除外）将成为派生类型的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="e3581-196">基类成员的声明的可访问性不会控制是否继承成员，继承扩展到任何不是实例构造函数、静态构造函数或析构函数的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="e3581-197">但是，在派生的类型中，可能无法访问继承的成员，这可能是由于其声明的可访问性（已[声明可访问性](basic-concepts.md#declared-accessibility)）或由类型本身中的声明隐藏（[通过继承](basic-concepts.md#hiding-through-inheritance)隐藏）。</span><span class="sxs-lookup"><span data-stu-id="e3581-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="e3581-198">命名空间成员</span><span class="sxs-lookup"><span data-stu-id="e3581-198">Namespace members</span></span>

<span data-ttu-id="e3581-199">没有封闭命名空间的命名空间和类型是***全局命名空间***的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="e3581-200">这直接对应于全局声明空间中声明的名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="e3581-201">命名空间中声明的命名空间和类型是该命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="e3581-202">这直接对应于命名空间声明空间中声明的名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="e3581-203">命名空间没有任何访问限制。</span><span class="sxs-lookup"><span data-stu-id="e3581-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="e3581-204">不能声明私有、受保护的或内部命名空间，并且命名空间名称始终是可公开访问的。</span><span class="sxs-lookup"><span data-stu-id="e3581-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="e3581-205">结构成员</span><span class="sxs-lookup"><span data-stu-id="e3581-205">Struct members</span></span>

<span data-ttu-id="e3581-206">结构的成员是在结构中声明的成员和从该结构的直接基类继承的成员 `System.ValueType` 和间接基类 @no__t 为-1。</span><span class="sxs-lookup"><span data-stu-id="e3581-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="e3581-207">简单类型的成员直接与简单类型化名为的结构类型成员相对应：</span><span class="sxs-lookup"><span data-stu-id="e3581-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="e3581-208">@No__t 的成员是 `System.SByte` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="e3581-209">@No__t 的成员是 `System.Byte` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="e3581-210">@No__t 的成员是 `System.Int16` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="e3581-211">@No__t 的成员是 `System.UInt16` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="e3581-212">@No__t 的成员是 `System.Int32` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="e3581-213">@No__t 的成员是 `System.UInt32` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="e3581-214">@No__t 的成员是 `System.Int64` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="e3581-215">@No__t 的成员是 `System.UInt64` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="e3581-216">@No__t 的成员是 `System.Char` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="e3581-217">@No__t 的成员是 `System.Single` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="e3581-218">@No__t 的成员是 `System.Double` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="e3581-219">@No__t 的成员是 `System.Decimal` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="e3581-220">@No__t 的成员是 `System.Boolean` 结构的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="e3581-221">枚举成员</span><span class="sxs-lookup"><span data-stu-id="e3581-221">Enumeration members</span></span>

<span data-ttu-id="e3581-222">枚举的成员是枚举中声明的常量和从枚举的直接基类继承的成员 `System.Enum` 和间接基类 `System.ValueType` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="e3581-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="e3581-223">类成员</span><span class="sxs-lookup"><span data-stu-id="e3581-223">Class members</span></span>

<span data-ttu-id="e3581-224">类的成员是在类中声明的成员和从基类继承的成员（没有基类的类 `object` 除外）。</span><span class="sxs-lookup"><span data-stu-id="e3581-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="e3581-225">从基类继承的成员包括常数、字段、方法、属性、事件、索引器、运算符和基类的类型，但不包括基类的实例构造函数、析构函数和静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="e3581-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="e3581-226">继承基类成员，而不考虑其可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="e3581-227">类声明可以包含常量、字段、方法、属性、事件、索引器、运算符、实例构造函数、析构函数、静态构造函数和类型的声明。</span><span class="sxs-lookup"><span data-stu-id="e3581-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="e3581-228">@No__t 和 @no__t 1 的成员直接对应于其别名的类类型的成员：</span><span class="sxs-lookup"><span data-stu-id="e3581-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="e3581-229">@No__t 的成员是 @no__t 类的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="e3581-230">@No__t 的成员是 @no__t 类的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="e3581-231">接口成员</span><span class="sxs-lookup"><span data-stu-id="e3581-231">Interface members</span></span>

<span data-ttu-id="e3581-232">接口的成员是在接口和接口的所有基接口中声明的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="e3581-233">类 `object` 中的成员并不严格地说是任何接口（[接口成员](interfaces.md#interface-members)）的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="e3581-234">但是，可以通过成员查找在任何接口类型中（[成员查找](expressions.md#member-lookup)）使用类 `object` 中的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="e3581-235">数组成员</span><span class="sxs-lookup"><span data-stu-id="e3581-235">Array members</span></span>

<span data-ttu-id="e3581-236">数组成员是从类 `System.Array` 继承的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="e3581-237">委托成员</span><span class="sxs-lookup"><span data-stu-id="e3581-237">Delegate members</span></span>

<span data-ttu-id="e3581-238">委托的成员是从类 `System.Delegate` 继承的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="e3581-239">成员访问</span><span class="sxs-lookup"><span data-stu-id="e3581-239">Member access</span></span>

<span data-ttu-id="e3581-240">成员的声明允许控制成员访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="e3581-241">成员的可访问性是由成员的声明的可访问性（已[声明可访问性](basic-concepts.md#declared-accessibility)）建立的，该成员与直接包含类型的可访问性（如果有）结合使用。</span><span class="sxs-lookup"><span data-stu-id="e3581-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="e3581-242">如果允许访问特定成员，则认为该成员是可***访问***的。</span><span class="sxs-lookup"><span data-stu-id="e3581-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="e3581-243">相反，当不允许访问特定成员时，该成员被称为***不可***访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="e3581-244">如果访问发生的文本位置包含在成员的可访问域（[可访问](basic-concepts.md#accessibility-domains)域）中，则允许访问成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="e3581-245">声明的可访问性</span><span class="sxs-lookup"><span data-stu-id="e3581-245">Declared accessibility</span></span>

<span data-ttu-id="e3581-246">成员的***声明可访问性***可以是以下项之一：</span><span class="sxs-lookup"><span data-stu-id="e3581-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="e3581-247">公共，通过在成员声明中包含 `public` 修饰符来选择。</span><span class="sxs-lookup"><span data-stu-id="e3581-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="e3581-248">@No__t-0 的直观含义为 "访问不受限制"。</span><span class="sxs-lookup"><span data-stu-id="e3581-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="e3581-249">Protected：通过在成员声明中包含 `protected` 修饰符来选择。</span><span class="sxs-lookup"><span data-stu-id="e3581-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="e3581-250">@No__t-0 的直观含义为 "访问包含类或派生自包含类的类型"。</span><span class="sxs-lookup"><span data-stu-id="e3581-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="e3581-251">内部，通过在成员声明中包含 `internal` 修饰符来选择。</span><span class="sxs-lookup"><span data-stu-id="e3581-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="e3581-252">@No__t-0 的直观含义为 "访问此程序受限"。</span><span class="sxs-lookup"><span data-stu-id="e3581-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="e3581-253">受保护的内部（这是受保护的或内部的），通过在成员声明中包括 @no__t 0 和 @no__t 修饰符来选择。</span><span class="sxs-lookup"><span data-stu-id="e3581-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="e3581-254">@No__t-0 的直观含义为 "访问此程序或派生自包含类的类型"。</span><span class="sxs-lookup"><span data-stu-id="e3581-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="e3581-255">Private，通过在成员声明中包含 `private` 修饰符来选择。</span><span class="sxs-lookup"><span data-stu-id="e3581-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="e3581-256">@No__t-0 的直观含义为 "访问包含类型"。</span><span class="sxs-lookup"><span data-stu-id="e3581-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="e3581-257">根据成员声明发生的上下文，仅允许某些类型的声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="e3581-258">此外，当成员声明中不包含任何访问修饰符时，在其中进行声明的上下文会确定默认的声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="e3581-259">命名空间隐式具有 @no__t 的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="e3581-260">命名空间声明中不允许使用访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="e3581-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="e3581-261">在编译单元或命名空间中声明的类型可以有 `public` 或 @no__t 1 声明的可访问性，并且默认为 `internal` 声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="e3581-262">类成员可以具有五种声明的可访问性，并且默认为 `private` 声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="e3581-263">（请注意，声明为类成员的类型可以具有五种声明可访问性中的任何一种，而声明为命名空间成员的类型只能有 `public` 或 @no__t 1 声明的可访问性。）</span><span class="sxs-lookup"><span data-stu-id="e3581-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="e3581-264">结构成员可以有 `public`、`internal` 或 @no__t 声明的可访问性和默认值 @no__t，因为结构是隐式密封的。</span><span class="sxs-lookup"><span data-stu-id="e3581-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="e3581-265">结构中引入的结构成员（即，不是由该结构继承）不能有 `protected` 或 @no__t 声明的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="e3581-266">（请注意，声明为结构成员的类型可以有 `public`、`internal` 或 @no__t 声明的可访问性，而声明为命名空间成员的类型只能具有 `public` 或 @no__t 的可访问性。）</span><span class="sxs-lookup"><span data-stu-id="e3581-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="e3581-267">接口成员隐式具有 @no__t 的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="e3581-268">接口成员声明中不允许使用访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="e3581-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="e3581-269">枚举成员隐式具有 @no__t 的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="e3581-270">枚举成员声明中不允许使用访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="e3581-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="e3581-271">辅助功能域</span><span class="sxs-lookup"><span data-stu-id="e3581-271">Accessibility domains</span></span>

<span data-ttu-id="e3581-272">成员的***可访问域***包含允许访问成员的程序文本（可能不连续）部分。</span><span class="sxs-lookup"><span data-stu-id="e3581-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="e3581-273">出于定义成员的可访问性域的目的，如果成员未在类型内声明，则称为***顶级***成员，如果在另一种类型中声明成员，则称***嵌套***成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="e3581-274">此外，程序的***程序文本***定义为程序的所有源文件中包含的所有程序文本，类型的程序文本定义为该类型的*type_declaration*中包含的所有程序文本（包括、可能是嵌套在类型中的类型）。</span><span class="sxs-lookup"><span data-stu-id="e3581-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="e3581-275">预定义类型的可访问域（例如 `object`、`int` 或 `double`）不受限制。</span><span class="sxs-lookup"><span data-stu-id="e3581-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="e3581-276">在程序 `P` 中声明的顶级未绑定类型 @no__t 的可访问域（[绑定类型和未绑定类型](types.md#bound-and-unbound-types)）的定义如下：</span><span class="sxs-lookup"><span data-stu-id="e3581-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="e3581-277">如果 `T` 的声明的可访问性 @no__t 为-1，@no__t 的可访问域就是 @no__t 3 的程序文本和引用 `P` 的任何程序。</span><span class="sxs-lookup"><span data-stu-id="e3581-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="e3581-278">如果 `T` 的已声明可访问性为 `internal`，则 `T` 的可访问域就是 `P` 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="e3581-279">从这些定义开始，顶级未绑定类型的可访问域始终至少是声明该类型的程序的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="e3581-280">构造类型 `T<A1, ..., An>` 的可访问性域是未绑定的泛型类型的可访问域的交集 `T` 和类型参数 `A1, ..., An` 的可访问性域的交集。</span><span class="sxs-lookup"><span data-stu-id="e3581-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="e3581-281">在程序 `P` 中声明的类型 `T` 中声明的嵌套成员 `M` 的可访问域定义如下（请注意，@no__t 本身可能是一个类型）：</span><span class="sxs-lookup"><span data-stu-id="e3581-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="e3581-282">如果 `M` 的已声明可访问性为 `public`，则 `M` 的可访问域就是 `T` 的可访问域。</span><span class="sxs-lookup"><span data-stu-id="e3581-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="e3581-283">如果 `M` 的声明的可访问性 @no__t 为-1，则允许 `D` 作为从 @no__t 到 @no__t 之外声明的任何类型的程序 @no__t 文本的联合，该程序文本与</span><span class="sxs-lookup"><span data-stu-id="e3581-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="e3581-284">@No__t 的可访问域是 @no__t 的可访问域与 `D` 的交集。</span><span class="sxs-lookup"><span data-stu-id="e3581-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="e3581-285">如果 `M` 的声明的可访问性 @no__t 为-1，则允许 `D` 作为 @no__t 3 的程序文本和从 @no__t 派生的任何类型的程序文本的联合。</span><span class="sxs-lookup"><span data-stu-id="e3581-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="e3581-286">@No__t 的可访问域是 @no__t 的可访问域与 `D` 的交集。</span><span class="sxs-lookup"><span data-stu-id="e3581-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="e3581-287">如果 `M` 的已声明可访问性为 `internal`，则 `M` 的可访问域就是 `T` 的可访问域与 `P` 的程序文本之间的交集。</span><span class="sxs-lookup"><span data-stu-id="e3581-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="e3581-288">如果 `M` 的已声明可访问性为 `private`，则 `M` 的可访问域就是 `T` 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="e3581-289">从这些定义开始，嵌套成员的可访问域始终至少是声明该成员的类型的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="e3581-290">此外，它还会将成员的可访问域与声明该成员的类型的可访问性域完全相同。</span><span class="sxs-lookup"><span data-stu-id="e3581-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="e3581-291">直观地说，当访问一个类型或成员 `M` 时，将评估以下步骤以确保允许访问：</span><span class="sxs-lookup"><span data-stu-id="e3581-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="e3581-292">首先，如果在类型（而不是编译单元或命名空间）中声明 `M`，则当该类型不可访问时，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="e3581-293">如果 `M` @no__t 为-1，则允许访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="e3581-294">否则，如果 `M` @no__t 为-1，则允许访问，如果它出现在声明了 `M` 的程序中，或出现在派生自类的类 @no__t 中，并且该类是通过派生类类型进行的（[受保护实例成员的访问权限](basic-concepts.md#protected-access-for-instance-members)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="e3581-295">否则，如果 `M` @no__t 为-1，则允许访问，如果它出现在声明了 `M` 的类中，或者出现在派生自 @no__t 类的类中，而该类是通过派生类类型（[受保护实例成员的访问权限](basic-concepts.md#protected-access-for-instance-members)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="e3581-296">否则，如果 `M` @no__t 为-1，则允许访问，如果在声明 `M` 的程序中发生。</span><span class="sxs-lookup"><span data-stu-id="e3581-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="e3581-297">否则，如果 `M` @no__t 为-1，则允许访问，如果在声明 `M` 的类型内发生。</span><span class="sxs-lookup"><span data-stu-id="e3581-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="e3581-298">否则，将无法访问该类型或成员，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="e3581-299">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-299">In the example</span></span>
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
<span data-ttu-id="e3581-300">类和成员具有以下可访问域：</span><span class="sxs-lookup"><span data-stu-id="e3581-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="e3581-301">@No__t 的可访问性域（0）和 `A.X` 无限制。</span><span class="sxs-lookup"><span data-stu-id="e3581-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="e3581-302">@No__t 的可访问域，`B`，`B.X`，`B.Y`，`B.C`，`B.C.X`，`B.C.Y` 是包含程序的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="e3581-303">@No__t 的可访问域是 @no__t 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="e3581-304">@No__t 的可访问性域（0）和 `B.D` 是 `B` 的程序文本，包括 `B.C` 和 @no__t 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="e3581-305">@No__t 的可访问域是 @no__t 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="e3581-306">@No__t 的可访问性域（0）和 `B.D.Y` 是 `B` 的程序文本，包括 `B.C` 和 @no__t 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="e3581-307">@No__t 的可访问域是 @no__t 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="e3581-308">如示例所示，成员的可访问域绝不会超出包含类型的可访问域。</span><span class="sxs-lookup"><span data-stu-id="e3581-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="e3581-309">例如，即使所有 @no__t 0 的成员都具有公开声明的可访问性，但所有 `A.X` 都包含受包含类型约束的可访问域。</span><span class="sxs-lookup"><span data-stu-id="e3581-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="e3581-310">如[成员](basic-concepts.md#members)中所述，基类的所有成员（实例构造函数、析构函数和静态构造函数除外）都是由派生类型继承的。</span><span class="sxs-lookup"><span data-stu-id="e3581-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="e3581-311">这包括甚至是基类的私有成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-311">This includes even private members of a base class.</span></span> <span data-ttu-id="e3581-312">但是，私有成员的可访问域只包含声明该成员的类型的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="e3581-313">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-313">In the example</span></span>
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
<span data-ttu-id="e3581-314">`B` 类从 `A` 类继承私有成员 @no__t。</span><span class="sxs-lookup"><span data-stu-id="e3581-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="e3581-315">由于此成员是私有的，因此只能在 `A` 的*class_body*内访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="e3581-316">因此，对 @no__t 0 的访问成功于 `A.F` 方法中，但在 `B.F` 方法中失败。</span><span class="sxs-lookup"><span data-stu-id="e3581-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="e3581-317">实例成员的受保护访问</span><span class="sxs-lookup"><span data-stu-id="e3581-317">Protected access for instance members</span></span>

<span data-ttu-id="e3581-318">如果在声明 @no__t 0 实例成员的类的程序文本的外部访问该成员，并且在声明该成员的程序的程序文本的外部访问 @no__t 1 实例成员，则必须在类中进行访问派生自其声明类的声明。</span><span class="sxs-lookup"><span data-stu-id="e3581-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="e3581-319">此外，需要通过派生类类型的实例或从其构造的类类型进行访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="e3581-320">此限制可防止一个派生类访问其他派生类的受保护成员，即使这些成员是从同一个基类继承时也是如此。</span><span class="sxs-lookup"><span data-stu-id="e3581-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="e3581-321">让 @no__t 是一个基类，该基类声明受保护的实例成员 `M`，并让 `D` 是派生自 @no__t 的类。</span><span class="sxs-lookup"><span data-stu-id="e3581-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="e3581-322">在 `D` 的*class_body*中，对 @no__t 的访问可以采用以下形式之一：</span><span class="sxs-lookup"><span data-stu-id="e3581-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="e3581-323">格式 @no__t 的非限定的*type_name*或*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="e3581-324">如果 `E` 的类型为 `T` 或从 `T` 派生的类，则为形式 `E.M` 的*primary_expression* ，其中 `T` 是类类型 `D`，或是从 @no__t 构造的类类型</span><span class="sxs-lookup"><span data-stu-id="e3581-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="e3581-325">形式 `base.M` 的*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="e3581-326">除这些形式的访问权限外，派生类还可以访问*constructor_initializer*中的基类的受保护实例构造函数（[构造函数初始值设定项](classes.md#constructor-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="e3581-327">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-327">In the example</span></span>
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
<span data-ttu-id="e3581-328">在 `A` 中，可以通过 @no__t 2 和 `B` 的实例访问 @no__t 1，因为在这两种情况下，都可以通过 @no__t 的实例进行访问，或从 @no__t 5 派生的类进行访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="e3581-329">但是，在 `B` 中，无法通过 `A` 的实例访问 @no__t，因为 `A` 不从 @no__t 派生。</span><span class="sxs-lookup"><span data-stu-id="e3581-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="e3581-330">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-330">In the example</span></span>
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
<span data-ttu-id="e3581-331">允许对 @no__t 的三个赋值，因为它们都是通过从泛型类型构造的类类型的实例进行的。</span><span class="sxs-lookup"><span data-stu-id="e3581-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="e3581-332">辅助功能约束</span><span class="sxs-lookup"><span data-stu-id="e3581-332">Accessibility constraints</span></span>

<span data-ttu-id="e3581-333">语言中的C#几个构造要求类型必须至少具有与成员或其他类型***一样的可访问性***。</span><span class="sxs-lookup"><span data-stu-id="e3581-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="e3581-334">如果 `T` 的可访问域是 `M` 的可访问域的超集，则将类型 `T` 称为至少可作为成员或类型进行访问 `M`。</span><span class="sxs-lookup"><span data-stu-id="e3581-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="e3581-335">换言之，如果 @no__t 可访问的所有 @no__t 上下文中都可访问-2，则 `T` 至少可作为 `M` 可访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="e3581-336">存在以下可访问性约束：</span><span class="sxs-lookup"><span data-stu-id="e3581-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="e3581-337">类类型的直接基类必须至少具有与类类型本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="e3581-338">接口类型的显式基接口必须至少具有与接口类型本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="e3581-339">委托类型的返回类型和参数类型必须至少具有与委托类型本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="e3581-340">常量的类型必须至少具有与常量本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="e3581-341">字段的类型必须至少具有与字段本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="e3581-342">方法的返回类型和参数类型必须至少具有与方法本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="e3581-343">属性的类型必须至少具有与属性本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="e3581-344">事件的类型必须至少具有与事件本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="e3581-345">索引器的类型和参数类型必须至少具有与索引器本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="e3581-346">运算符的类型和参数类型必须至少具有与运算符本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="e3581-347">实例构造函数的参数类型必须至少具有与实例构造函数本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="e3581-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="e3581-348">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="e3581-349">`B` 类会导致编译时错误，因为 `A` 至少不能作为 `B` 的访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="e3581-350">同样，在示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="e3581-351">`B` 中的 `H` 方法导致编译时错误，因为 `A` 的返回类型并不至少与方法相同。</span><span class="sxs-lookup"><span data-stu-id="e3581-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="e3581-352">签名和重载</span><span class="sxs-lookup"><span data-stu-id="e3581-352">Signatures and overloading</span></span>

<span data-ttu-id="e3581-353">方法、实例构造函数、索引器和运算符按其***签名***特征：</span><span class="sxs-lookup"><span data-stu-id="e3581-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="e3581-354">方法的签名由方法的名称、类型参数的数目以及其每个形参的类型和种类（值、引用或输出）组成，按从左到右的顺序排列。</span><span class="sxs-lookup"><span data-stu-id="e3581-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="e3581-355">出于这些目的，形参的类型中出现的方法的任何类型形参均由其名称标识，但其在方法的类型实参列表中的序号位置。</span><span class="sxs-lookup"><span data-stu-id="e3581-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="e3581-356">具体而言，方法的签名不包括返回类型、可为最右侧参数指定的 `params` 修饰符和可选的类型参数约束。</span><span class="sxs-lookup"><span data-stu-id="e3581-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="e3581-357">实例构造函数的签名由其每个形参的类型和类型（值、引用或输出）组成，按从左到右的顺序排列。</span><span class="sxs-lookup"><span data-stu-id="e3581-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="e3581-358">实例构造函数的签名具体不包括可以为最右侧参数指定的 `params` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="e3581-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="e3581-359">索引器的签名由其每个形参的类型组成，按从左到右的顺序排列。</span><span class="sxs-lookup"><span data-stu-id="e3581-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="e3581-360">索引器的签名具体不包括元素类型，也不包括可以为最右侧参数指定的 `params` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="e3581-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="e3581-361">运算符的签名由运算符的名称和它的每个形参的类型组成，按从左到右的顺序排列。</span><span class="sxs-lookup"><span data-stu-id="e3581-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="e3581-362">运算符的签名具体不包括结果类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="e3581-363">签名是用于在类、结构和接口中***重载***成员的启用机制：</span><span class="sxs-lookup"><span data-stu-id="e3581-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="e3581-364">方法的重载允许类、结构或接口声明多个具有相同名称的方法，前提是它们的签名在该类、结构或接口中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="e3581-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="e3581-365">实例构造函数的重载允许类或结构声明多个实例构造函数，前提是它们的签名在该类或结构中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="e3581-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="e3581-366">索引器的重载允许类、结构或接口声明多个索引器，前提是这些索引器的签名在该类、结构或接口中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="e3581-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="e3581-367">运算符的重载允许类或结构声明多个具有相同名称的运算符，前提是它们的签名在该类或结构中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="e3581-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="e3581-368">尽管 @no__t 0 和 @no__t 1 参数修饰符被视为签名的一部分，但在单个类型中声明的成员不能仅由 `ref` 和 `out` 中的签名有所不同。</span><span class="sxs-lookup"><span data-stu-id="e3581-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="e3581-369">如果两个成员在同一类型中声明，但如果两个 @no__t 方法中的所有参数都已更改为 `ref` 修饰符，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="e3581-370">对于签名匹配的其他目的（例如，隐藏或重写），`ref`，`out` 将被视为签名的一部分，并且彼此之间不匹配。</span><span class="sxs-lookup"><span data-stu-id="e3581-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="e3581-371">（这一限制是为了C#使程序可以轻松地翻译为在公共语言基础结构（CLI）上运行，而不提供一种方法来定义仅在 `ref` 和 `out` 中存在差异的方法。）</span><span class="sxs-lookup"><span data-stu-id="e3581-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="e3581-372">对于签名，类型 `object` 和 `dynamic` 被视为相同。</span><span class="sxs-lookup"><span data-stu-id="e3581-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="e3581-373">因此，在单个类型中声明的成员可以不区分签名，而只是 `object` 和 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="e3581-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="e3581-374">下面的示例演示一组重载方法声明及其签名。</span><span class="sxs-lookup"><span data-stu-id="e3581-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
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

<span data-ttu-id="e3581-375">请注意，任何 @no__t 0 和 @no__t 参数修饰符（[方法参数](classes.md#method-parameters)）都是签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="e3581-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="e3581-376">因此，@no__t 0 和 @no__t 为唯一的签名。</span><span class="sxs-lookup"><span data-stu-id="e3581-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="e3581-377">但是，不能在同一接口内声明 `F(ref int)` 和 `F(out int)`，因为它们的签名仅因 `ref` 和 @no__t 而有所不同。</span><span class="sxs-lookup"><span data-stu-id="e3581-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="e3581-378">另请注意，返回类型和 `params` 修饰符不是签名的一部分，因此不可能仅基于返回类型或在包含或排除 @no__t 修饰符时重载。</span><span class="sxs-lookup"><span data-stu-id="e3581-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="e3581-379">同样，在上面标识的方法的声明 `F(int)` 和 @no__t 1 会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="e3581-380">范围</span><span class="sxs-lookup"><span data-stu-id="e3581-380">Scopes</span></span>

<span data-ttu-id="e3581-381">名称的***作用域***是程序文本的区域，在该区域中，可以引用名称声明的实体，而无需限定名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="e3581-382">可以***嵌套***作用域，内部作用域可以从外部作用域中重新声明名称的含义（但这并不能删除嵌套块中的[声明](basic-concepts.md#declarations)施加的限制，不能使用与封闭块中的本地变量相同的名称）。</span><span class="sxs-lookup"><span data-stu-id="e3581-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="e3581-383">然后，在内部范围所涵盖的程序文本区域中***隐藏***外部范围的名称，并且只能通过限定名称来访问外部名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="e3581-384">*Namespace_member_declaration* （[命名空间成员](namespaces.md#namespace-members)）声明的命名空间成员的作用域不包含任何封闭*namespace_declaration* ，是整个程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="e3581-385">*Namespace_declaration*中*namespace_member_declaration*声明的命名空间成员的作用域，其完全限定名称为 `N` 是每个*namespace_declaration*的*namespace_body* ，其完全限定名称为限定名 `N` 或以 `N` 开始，后跟一个句点。</span><span class="sxs-lookup"><span data-stu-id="e3581-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="e3581-386">由*extern_alias_directive*定义的名称的作用域超出其立即包含编译单元或命名空间正文的*using_directive*s、 *global_attributes*和*namespace_member_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e3581-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="e3581-387">*Extern_alias_directive*不会将任何新成员分配给基础声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="e3581-388">换句话说， *extern_alias_directive*是不可传递的，而是仅影响其出现在其中的编译单元或命名空间正文。</span><span class="sxs-lookup"><span data-stu-id="e3581-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="e3581-389">*Using_directive* （[使用指令](namespaces.md#using-directives)）定义或导入的名称的作用域扩展到*compilation_unit*或*namespace_body*的*namespace_member_declaration*，其中*using_directive*发生。</span><span class="sxs-lookup"><span data-stu-id="e3581-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="e3581-390">*Using_directive*可以使特定*compilation_unit*或*namespace_body*中的零个或多个命名空间、类型或成员名称可用，但不会将任何新成员分配给基础声明空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="e3581-391">换句话说， *using_directive*是不可传递的，而只会影响发生它的*compilation_unit*或*namespace_body* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="e3581-392">由*class_declaration*上的*type_parameter_list*声明的类型参数的作用域（[类声明](classes.md#class-declarations)）是该的*class_base*、 *type_parameter_constraints_clause*和*class_body* 。 *class_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e3581-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="e3581-393">由*struct_declaration*上的*type_parameter_list*声明的类型参数的作用域（[结构声明](structs.md#struct-declarations)）是的*struct_interfaces*、 *type_parameter_constraints_clause*和*struct_body*该*struct_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e3581-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="e3581-394">由*interface_declaration*上的*type_parameter_list*声明的类型参数的作用域（[接口声明](interfaces.md#interface-declarations)）为*interface_base*、 *type_parameter_constraints_clause*和*interface_body*的*interface_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e3581-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="e3581-395">由*delegate_declaration*上的*type_parameter_list*声明的类型参数的作用域（[委托声明](delegates.md#delegate-declarations)）为*return_type*、 *formal_parameter_list*和*type_parameter_constraints_clause*的*delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e3581-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="e3581-396">*Class_member_declaration* （[类体](classes.md#class-body)）声明的成员的作用域是在其中进行声明的*class_body* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="e3581-397">此外，类成员的作用域扩展到包含在该成员的可访问域（[可访问](basic-concepts.md#accessibility-domains)域）中的派生类的*class_body* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="e3581-398">*Struct_member_declaration* （[结构成员](structs.md#struct-members)）声明的成员的作用域是在其中进行声明的*struct_body* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="e3581-399">*Enum_member_declaration* （[enum 成员](enums.md#enum-members)）声明的成员的作用域是在其中进行声明的*enum_body* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="e3581-400">在*method_declaration* （[方法](classes.md#methods)）中声明的参数的作用域是该*method_declaration*的*method_body* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="e3581-401">在*indexer_declaration* （[索引器](classes.md#indexers)）中声明的参数的作用域是该*indexer_declaration*的*accessor_declarations* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="e3581-402">在*operator_declaration* （[运算符](classes.md#operators)）中声明的参数的作用域是该*operator_declaration*的*块*。</span><span class="sxs-lookup"><span data-stu-id="e3581-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="e3581-403">在*constructor_declaration* （[实例构造函数](classes.md#instance-constructors)）中声明的参数的作用域是该*constructor_declaration*的*constructor_initializer*和*块*。</span><span class="sxs-lookup"><span data-stu-id="e3581-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="e3581-404">在*lambda_expression* （[匿名函数表达式](expressions.md#anonymous-function-expressions)）中声明的参数的作用域是该*lambda_expression*的*anonymous_function_body*</span><span class="sxs-lookup"><span data-stu-id="e3581-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="e3581-405">在*anonymous_method_expression* （[匿名函数表达式](expressions.md#anonymous-function-expressions)）中声明的参数的作用域是该*anonymous_method_expression*的*块*。</span><span class="sxs-lookup"><span data-stu-id="e3581-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="e3581-406">在*labeled_statement* （[标记的语句](statements.md#labeled-statements)）中声明的标签的作用域是在其中进行声明的*块*。</span><span class="sxs-lookup"><span data-stu-id="e3581-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="e3581-407">在*local_variable_declaration* （[局部变量声明](statements.md#local-variable-declarations)）中声明的局部变量的作用域是在其中进行声明的块。</span><span class="sxs-lookup"><span data-stu-id="e3581-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="e3581-408">在 `switch` 语句（[switch 语句](statements.md#the-switch-statement)）的*switch_block*中声明的局部变量的范围为*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="e3581-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="e3581-409">在 `for` 语句（[for 语句](statements.md#the-for-statement)）的*for_initializer*中声明的局部变量的作用域是*for_initializer*、 *for_condition*、 *for_iterator*和包含的*语句*。@no__t 7 语句。</span><span class="sxs-lookup"><span data-stu-id="e3581-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="e3581-410">在*local_constant_declaration*中声明的局部常量的作用域（[局部常量声明](statements.md#local-constant-declarations)）是在其中进行声明的块。</span><span class="sxs-lookup"><span data-stu-id="e3581-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="e3581-411">在其*constant_declarator*之前的文本位置引用本地常量是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="e3581-412">声明为*foreach_statement*、 *using_statement*、 *lock_statement*或*query_expression*的一部分的变量的作用域由给定构造的扩展确定。</span><span class="sxs-lookup"><span data-stu-id="e3581-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="e3581-413">在命名空间、类、结构或枚举成员的作用域内，可以引用成员声明之前的文本位置中的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="e3581-414">例如</span><span class="sxs-lookup"><span data-stu-id="e3581-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="e3581-415">此处，在声明之前，`F` 引用 @no__t。</span><span class="sxs-lookup"><span data-stu-id="e3581-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="e3581-416">在局部变量的作用域内，在本地变量的*local_variable_declarator*之前的文本位置引用本地变量是编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="e3581-417">例如</span><span class="sxs-lookup"><span data-stu-id="e3581-417">For example</span></span>
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

<span data-ttu-id="e3581-418">在上面的 `F` 方法中，第一次分配给 @no__t 的第1项并未引用在外部作用域中声明的字段。</span><span class="sxs-lookup"><span data-stu-id="e3581-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="e3581-419">相反，它是指局部变量，它会导致编译时错误，因为它在此变量的声明之前。</span><span class="sxs-lookup"><span data-stu-id="e3581-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="e3581-420">在 `G` 方法中，在 `j` 的声明的初始值设定项中使用 @no__t 是有效的，因为使用不在*local_variable_declarator*之前。</span><span class="sxs-lookup"><span data-stu-id="e3581-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="e3581-421">在 `H` 方法中，后续*local_variable_declarator*正确引用在同一*local_variable_declaration*中的早期*local_variable_declarator*中声明的局部变量。</span><span class="sxs-lookup"><span data-stu-id="e3581-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="e3581-422">局部变量的作用域规则旨在确保表达式上下文中使用的名称的含义在块中始终相同。</span><span class="sxs-lookup"><span data-stu-id="e3581-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="e3581-423">如果本地变量的作用域只是从其声明扩展到块的末尾，则第一个赋值将分配给实例变量，第二个赋值将分配给局部变量，这可能会导致编译时错误（如果以后要重新排列块的语句）。</span><span class="sxs-lookup"><span data-stu-id="e3581-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="e3581-424">块内的名称含义可能因使用该名称的上下文而异。</span><span class="sxs-lookup"><span data-stu-id="e3581-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="e3581-425">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-425">In the example</span></span>
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
<span data-ttu-id="e3581-426">名称 `A` 用于在表达式上下文中引用局部变量 `A` 和在类型上下文中以引用类 `A`。</span><span class="sxs-lookup"><span data-stu-id="e3581-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="e3581-427">名称隐藏</span><span class="sxs-lookup"><span data-stu-id="e3581-427">Name hiding</span></span>

<span data-ttu-id="e3581-428">实体的作用域通常比实体的声明空间包含更多的程序文本。</span><span class="sxs-lookup"><span data-stu-id="e3581-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="e3581-429">具体而言，实体的范围可能包括引入新声明空间的声明，这些声明空间包含具有相同名称的实体。</span><span class="sxs-lookup"><span data-stu-id="e3581-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="e3581-430">此类声明会使原始实体处于***隐藏状态***。</span><span class="sxs-lookup"><span data-stu-id="e3581-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="e3581-431">相反，如果实体未隐藏，则称该实体***可见***。</span><span class="sxs-lookup"><span data-stu-id="e3581-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="e3581-432">如果范围通过嵌套重叠，并且作用域通过继承重叠，则会发生名称隐藏。</span><span class="sxs-lookup"><span data-stu-id="e3581-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="e3581-433">以下各节将介绍这两种隐藏类型的特征。</span><span class="sxs-lookup"><span data-stu-id="e3581-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="e3581-434">通过嵌套隐藏</span><span class="sxs-lookup"><span data-stu-id="e3581-434">Hiding through nesting</span></span>

<span data-ttu-id="e3581-435">由于嵌套命名空间或命名空间中的类型、类或结构中的嵌套类型以及参数和局部变量声明的结果，因此可以通过嵌套命名空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="e3581-436">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-436">In the example</span></span>
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
<span data-ttu-id="e3581-437">在 `F` 方法中，实例变量 `i` 由局部变量 `i` 隐藏，但在 `G` 方法中，`i` 仍引用实例变量。</span><span class="sxs-lookup"><span data-stu-id="e3581-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="e3581-438">当内部范围中的名称隐藏外部范围内的名称时，它将隐藏该名称的所有重载匹配项。</span><span class="sxs-lookup"><span data-stu-id="e3581-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="e3581-439">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-439">In the example</span></span>
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
<span data-ttu-id="e3581-440">调用 `F(1)` 会调用 `Inner` 中声明的 @no__t，因为内部声明隐藏了 @no__t 的所有外部匹配项。</span><span class="sxs-lookup"><span data-stu-id="e3581-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="e3581-441">由于同样的原因，调用 `F("Hello")` 会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="e3581-442">通过继承隐藏</span><span class="sxs-lookup"><span data-stu-id="e3581-442">Hiding through inheritance</span></span>

<span data-ttu-id="e3581-443">当类或结构重新声明从基类继承的名称时，通过继承进行名称隐藏。</span><span class="sxs-lookup"><span data-stu-id="e3581-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="e3581-444">此类名称隐藏采用以下形式之一：</span><span class="sxs-lookup"><span data-stu-id="e3581-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="e3581-445">类或结构中引入的常数、字段、属性、事件或类型隐藏了具有相同名称的所有基类成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="e3581-446">类或结构中引入的方法会隐藏所有具有相同名称的非方法基类成员，并隐藏具有相同签名的所有基类方法（方法名称和参数计数、修饰符和类型）。</span><span class="sxs-lookup"><span data-stu-id="e3581-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="e3581-447">类或结构中引入的索引器会隐藏具有相同签名的所有基类索引器（参数计数和类型）。</span><span class="sxs-lookup"><span data-stu-id="e3581-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="e3581-448">控制运算符声明（[运算符](classes.md#operators)）的规则使得派生类不可能使用与基类中的运算符相同的签名来声明运算符。</span><span class="sxs-lookup"><span data-stu-id="e3581-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="e3581-449">因此，运算符永远不会彼此隐藏。</span><span class="sxs-lookup"><span data-stu-id="e3581-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="e3581-450">与隐藏外部作用域中的名称相反，从继承的作用域中隐藏可访问的名称会导致报告警告。</span><span class="sxs-lookup"><span data-stu-id="e3581-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="e3581-451">示例中</span><span class="sxs-lookup"><span data-stu-id="e3581-451">In the example</span></span>
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
<span data-ttu-id="e3581-452">`Derived` 中 @no__t 的声明会导致报告警告。</span><span class="sxs-lookup"><span data-stu-id="e3581-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="e3581-453">隐藏继承名称特别不是错误，因为这会阻止基类的单独演化。</span><span class="sxs-lookup"><span data-stu-id="e3581-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="e3581-454">例如，上述情况可能已涉及，因为更高版本的 `Base` 引入了类的早期版本中不存在的 @no__t 1 方法。</span><span class="sxs-lookup"><span data-stu-id="e3581-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="e3581-455">如果上述情况是错误的，则对单独的版本控制类库中的基类进行的任何更改都可能导致派生类无效。</span><span class="sxs-lookup"><span data-stu-id="e3581-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="e3581-456">通过使用 `new` 修饰符，可以消除通过隐藏继承名称导致的警告：</span><span class="sxs-lookup"><span data-stu-id="e3581-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
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

<span data-ttu-id="e3581-457">@No__t-0 修饰符指示 `Derived` 中的 @no__t 为 "new"，它确实用于隐藏继承的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="e3581-458">新成员的声明仅在新成员的范围内隐藏继承的成员。</span><span class="sxs-lookup"><span data-stu-id="e3581-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

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

<span data-ttu-id="e3581-459">在上面的示例中，`Derived` 中 `F` 的声明隐藏了从 `Base` 继承的 `F`，但由于 @no__t 5 中的新 @no__t 有私有访问权限，因此它的作用域不会扩展到 `MoreDerived`。</span><span class="sxs-lookup"><span data-stu-id="e3581-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="e3581-460">因此，`MoreDerived.G` 中的调用 `F()` 有效，将调用 `Base.F`。</span><span class="sxs-lookup"><span data-stu-id="e3581-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="e3581-461">命名空间和类型名称</span><span class="sxs-lookup"><span data-stu-id="e3581-461">Namespace and type names</span></span>

<span data-ttu-id="e3581-462">程序中的多个上下文需要指定*namespace_name*或*type_name。* C#</span><span class="sxs-lookup"><span data-stu-id="e3581-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

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

<span data-ttu-id="e3581-463">*Namespace_name*是引用命名空间的*namespace_or_type_name* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="e3581-464">按照下面所述的解决方法， *namespace_name*的*namespace_or_type_name*必须引用命名空间，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="e3581-465">*Namespace_name*中不能存在任何类型参数（[类型参数](types.md#type-arguments)）（类型只能有类型参数）。</span><span class="sxs-lookup"><span data-stu-id="e3581-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="e3581-466">*Type_name*是引用类型的*namespace_or_type_name* 。</span><span class="sxs-lookup"><span data-stu-id="e3581-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="e3581-467">按照下面所述的解决方法， *type_name*的*namespace_or_type_name*必须引用类型，否则将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="e3581-468">如果*namespace_or_type_name*是一个限定别名成员，则其含义如[命名空间别名限定符](namespaces.md#namespace-alias-qualifiers)中所述。</span><span class="sxs-lookup"><span data-stu-id="e3581-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="e3581-469">否则， *namespace_or_type_name*具有四种形式之一：</span><span class="sxs-lookup"><span data-stu-id="e3581-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="e3581-470">其中 `I` 是单个标识符，`N` 为*namespace_or_type_name* ，@no__t 为可选的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="e3581-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="e3581-471">如果未指定*type_argument_list* ，请考虑 `k` 为零。</span><span class="sxs-lookup"><span data-stu-id="e3581-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="e3581-472">确定*namespace_or_type_name*的含义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e3581-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="e3581-473">如果*namespace_or_type_name*的格式 @no__t 为-1 或格式为 `I<A1, ..., Ak>`：</span><span class="sxs-lookup"><span data-stu-id="e3581-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="e3581-474">如果 @no__t 为零且*namespace_or_type_name*显示在泛型方法声明（[方法](classes.md#methods)）中，并且如果声明包含名称为 @ no__t 的类型参数（[类型参数](classes.md#type-parameters)），则*namespace_or_type_名称*是指该类型参数。</span><span class="sxs-lookup"><span data-stu-id="e3581-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="e3581-475">否则，如果*namespace_or_type_name*出现在类型声明中，则对于每个实例类型 @ no__t （[实例类型](classes.md#the-instance-type)），从该类型声明的实例类型开始，并继续使用每个实例的实例类型。封闭类或结构声明（如果有）：</span><span class="sxs-lookup"><span data-stu-id="e3581-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="e3581-476">如果 @no__t 为零，而 @no__t 的声明包含名为 @ no__t 的 type 参数，则*namespace_or_type_name*将引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="e3581-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="e3581-477">否则，如果*namespace_or_type_name*出现在类型声明的主体中，`T` 或它的任何基类型包含名为 @ no__t-2 和 `K` @ no__t-4type 参数的嵌套可访问类型，则*namespace_or_type_name*是指用给定类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="e3581-478">如果有多个这样的类型，则选择在派生程度较大的类型中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="e3581-479">请注意，在确定的含义时，将忽略非类型成员（常数、字段、方法、属性、索引器、运算符、实例构造函数、析构函数和静态构造函数）和具有不同数量的类型参数的类型成员。*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="e3581-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="e3581-480">如果前面的步骤不成功，则对于每个命名空间 @ no__t，从*namespace_or_type_name*发生的命名空间开始，继续每个封闭命名空间（如果有），并以全局命名空间结束，以下在查找实体之前计算步骤：</span><span class="sxs-lookup"><span data-stu-id="e3581-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="e3581-481">如果 @no__t 为零且 @no__t 为 @ no__t 中的命名空间的名称，则：</span><span class="sxs-lookup"><span data-stu-id="e3581-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="e3581-482">如果发生*namespace_or_type_name*的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含将名称 @ no 关联的*extern_alias_directive*或*using_alias_directive*__t-4，具有命名空间或类型，则*namespace_or_type_name*不明确，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="e3581-483">否则， *namespace_or_type_name*引用 `N` 中名为 `I` 的命名空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="e3581-484">否则，如果 `N` 包含名为 @ no__t 的可访问类型和 `K` @ no__t-3type 参数，则：</span><span class="sxs-lookup"><span data-stu-id="e3581-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="e3581-485">如果 @no__t 为零，且*namespace_or_type_name*的位置由 `N` 的命名空间声明括起来，并且命名空间声明包含*extern_alias_directive*或*using_alias_directive* ，则将名称 @ no__t 与命名空间或类型相关联，然后*namespace_or_type_name*是不明确的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="e3581-486">否则， *namespace_or_type_name*将引用用给定类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="e3581-487">否则，如果*namespace_or_type_name*发生的位置由 `N` 的命名空间声明括起来，则为; 否则为。</span><span class="sxs-lookup"><span data-stu-id="e3581-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="e3581-488">如果 @no__t 为零，命名空间声明包含*extern_alias_directive*或*using_alias_directive* ，它将名称 @ no__t 与导入的命名空间或类型相关联，则*namespace_or_type_name*引用命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="e3581-489">否则，如果由命名空间声明中的*using_namespace_directive*s 和*using_alias_directive*s 导入的命名空间和类型声明包含一个名称为 @ no__t 的可访问类型和 `K` @ no__t-4type参数，则*namespace_or_type_name*引用以给定类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="e3581-490">否则，如果由命名空间声明的*using_namespace_directive*s 和*using_alias_directive*s 导入的命名空间和类型声明包含多个名称为 @ no__t 和 `K` @ no__t-4type 的可访问类型参数，则*namespace_or_type_name*是不明确的，并发生错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="e3581-491">否则， *namespace_or_type_name*是未定义的，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="e3581-492">否则， *namespace_or_type_name*的格式 @no__t 为-1 或格式为 `N.I<A1, ..., Ak>`。</span><span class="sxs-lookup"><span data-stu-id="e3581-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="e3581-493">@no__t 首先解析为*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="e3581-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="e3581-494">如果 @no__t 的解析失败，则会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="e3581-495">否则，将按如下方式解析 `N.I` 或 `N.I<A1, ..., Ak>`：</span><span class="sxs-lookup"><span data-stu-id="e3581-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="e3581-496">如果 @no__t 为零，`N` 表示命名空间，`N` 包含名称 @no__t 为3的嵌套命名空间，则*namespace_or_type_name*引用该嵌套命名空间。</span><span class="sxs-lookup"><span data-stu-id="e3581-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="e3581-497">否则，如果 `N` 引用命名空间，并且 @no__t 包含名称为 @ no__t 的可访问类型和 `K` @ 4type 参数，则*namespace_or_type_name*引用使用给定类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="e3581-498">否则，如果 `N` 引用（可能构造的）类或结构类型，`N` 或它的任何一个基类包含名称为 @ no__t 的嵌套可访问性类型和 @no__t @ 4type 参数，则*namespace_or_type_name*引用用给定的类型参数构造的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="e3581-499">如果有多个这样的类型，则选择在派生程度较大的类型中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="e3581-500">请注意，如果将 `N.I` 的含义确定为解析 @no__t 的基类规范，则 `N` 的直接基类被视为对象（[基类](classes.md#base-classes)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="e3581-501">否则，@no__t 为无效的*namespace_or_type_name*，并发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3581-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="e3581-502">仅当使用*namespace_or_type_name*时，才能引用静态类（[静态类](classes.md#static-classes)）</span><span class="sxs-lookup"><span data-stu-id="e3581-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="e3581-503">*Namespace_or_type_name*是 `T.I` 形式的*namespace_or_type_name*中的 `T`，或</span><span class="sxs-lookup"><span data-stu-id="e3581-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="e3581-504">*Namespace_or_type_name*是*typeof_expression* （[参数列表](expressions.md#argument-lists)1）中格式为 `typeof(T)` 的 @no__t。</span><span class="sxs-lookup"><span data-stu-id="e3581-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="e3581-505">完全限定的名称</span><span class="sxs-lookup"><span data-stu-id="e3581-505">Fully qualified names</span></span>

<span data-ttu-id="e3581-506">每个命名空间和类型都有一个***完全限定的名称***，该名称唯一地标识命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="e3581-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="e3581-507">按如下方式确定命名空间或类型 `N` 的完全限定名：</span><span class="sxs-lookup"><span data-stu-id="e3581-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="e3581-508">如果 `N` 是全局命名空间的成员，则其完全限定名称为 `N`。</span><span class="sxs-lookup"><span data-stu-id="e3581-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="e3581-509">否则，其完全限定名称为 `S.N`，其中 `S` 是声明 `N` 的命名空间或类型的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="e3581-510">换句话说，@no__t 的完全限定名是从全局命名空间开始 @no__t 的标识符的完整层次结构路径。</span><span class="sxs-lookup"><span data-stu-id="e3581-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="e3581-511">由于命名空间或类型的每个成员都必须具有唯一的名称，因此命名空间或类型的完全限定名称始终是唯一的。</span><span class="sxs-lookup"><span data-stu-id="e3581-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="e3581-512">下面的示例显示了几个命名空间和类型声明及其关联的完全限定名称。</span><span class="sxs-lookup"><span data-stu-id="e3581-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
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

## <a name="automatic-memory-management"></a><span data-ttu-id="e3581-513">自动内存管理</span><span class="sxs-lookup"><span data-stu-id="e3581-513">Automatic memory management</span></span>

<span data-ttu-id="e3581-514">C#采用自动内存管理，使开发人员无手动分配和释放由对象占用的内存。</span><span class="sxs-lookup"><span data-stu-id="e3581-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="e3581-515">自动内存管理策略是由***垃圾回收器***实现的。</span><span class="sxs-lookup"><span data-stu-id="e3581-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="e3581-516">对象的内存管理生命周期如下所示：</span><span class="sxs-lookup"><span data-stu-id="e3581-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="e3581-517">创建对象时，将为其分配内存，运行构造函数，并将对象视为实时对象。</span><span class="sxs-lookup"><span data-stu-id="e3581-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="e3581-518">如果对象或它的任何部分不能通过任何可能的执行继续进行访问，而不是运行析构函数，则该对象被视为不再使用，并可用于销毁。</span><span class="sxs-lookup"><span data-stu-id="e3581-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="e3581-519">C#编译器和垃圾回收器可以选择对代码进行分析，以确定将来可能会使用哪些对对象的引用。</span><span class="sxs-lookup"><span data-stu-id="e3581-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="e3581-520">例如，如果作用域中的局部变量是唯一的对象引用，但在过程中从当前执行点到执行的任何可能的继续，则不会引用该局部变量，垃圾回收器可能（但不是需要）将对象视为不再使用。</span><span class="sxs-lookup"><span data-stu-id="e3581-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="e3581-521">对象符合析构条件后，在某些以后未指定的情况下，将运行该对象的析构[函数（如果有）。](classes.md#destructors)</span><span class="sxs-lookup"><span data-stu-id="e3581-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="e3581-522">正常情况下，对象的析构函数只运行一次，不过实现特定的 Api 可能会允许重写此行为。</span><span class="sxs-lookup"><span data-stu-id="e3581-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="e3581-523">一旦运行某个对象的析构函数，如果该对象或它的任何部分不能通过任何可能的执行继续进行访问，包括运行析构函数，则该对象将被视为不可访问，并且该对象便可用于收集。</span><span class="sxs-lookup"><span data-stu-id="e3581-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="e3581-524">最后，在对象变为符合集合条件后，垃圾回收器将释放与该对象关联的内存。</span><span class="sxs-lookup"><span data-stu-id="e3581-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="e3581-525">垃圾回收器维护有关对象使用情况的信息，并使用此信息进行内存管理决策，如在内存中查找新创建的对象的位置、何时重新定位对象以及对象不再使用或无法访问。</span><span class="sxs-lookup"><span data-stu-id="e3581-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="e3581-526">与其他假设存在垃圾回收器的语言一样， C#设计使垃圾回收器可以实现各种内存管理策略。</span><span class="sxs-lookup"><span data-stu-id="e3581-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="e3581-527">例如， C#不要求运行析构函数，或在对象符合条件时收集对象，或者在任何特定的线程上以任何特定的顺序运行析构函数。</span><span class="sxs-lookup"><span data-stu-id="e3581-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="e3581-528">可以通过类上的静态方法控制垃圾回收器的行为，使其在一定程度上控制 `System.GC`。</span><span class="sxs-lookup"><span data-stu-id="e3581-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="e3581-529">此类可用于请求进行集合、运行（或不运行）等操作。</span><span class="sxs-lookup"><span data-stu-id="e3581-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="e3581-530">由于垃圾回收器允许广泛的纬度来决定何时收集对象和运行析构函数，因此一致的实现可能产生与下面的代码所示不同的输出。</span><span class="sxs-lookup"><span data-stu-id="e3581-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="e3581-531">程序</span><span class="sxs-lookup"><span data-stu-id="e3581-531">The program</span></span>
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
<span data-ttu-id="e3581-532">创建类 @no__t 的实例，`B` 的实例。</span><span class="sxs-lookup"><span data-stu-id="e3581-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="e3581-533">如果为变量 `b` 赋值 `null`，则这些对象将可以进行垃圾回收，因为在此之后，任何用户编写的代码都无法访问它们。</span><span class="sxs-lookup"><span data-stu-id="e3581-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="e3581-534">输出可以是</span><span class="sxs-lookup"><span data-stu-id="e3581-534">The output could be either</span></span>

```console
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="e3581-535">或</span><span class="sxs-lookup"><span data-stu-id="e3581-535">or</span></span>
```console
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="e3581-536">因为该语言对对象的垃圾回收顺序没有约束。</span><span class="sxs-lookup"><span data-stu-id="e3581-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="e3581-537">在微妙的情况下，"符合析构条件" 和 "符合集合条件" 之间的区别非常重要。</span><span class="sxs-lookup"><span data-stu-id="e3581-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="e3581-538">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="e3581-538">For example,</span></span>
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

<span data-ttu-id="e3581-539">在上述程序中，如果垃圾回收器选择在 `B` 的析构函数之前运行 `A` 的析构函数，则该程序的输出可能是：</span><span class="sxs-lookup"><span data-stu-id="e3581-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="e3581-540">请注意，尽管 @no__t 的实例未被使用，并且运行了 `A` 的析构函数，但仍有可能从另一个析构函数调用 `A` （在本例中为 `F`）的方法。</span><span class="sxs-lookup"><span data-stu-id="e3581-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="e3581-541">另外，请注意，运行析构函数可能会导致对象再次可从主线程序使用。</span><span class="sxs-lookup"><span data-stu-id="e3581-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="e3581-542">在这种情况下，运行 @no__t 0 的析构函数会导致先前未使用的 @no__t 实例变为可从实时引用 `Test.RefA`。</span><span class="sxs-lookup"><span data-stu-id="e3581-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="e3581-543">调用 `WaitForPendingFinalizers` 后，`B` 的实例符合集合条件，但 @no__t 的实例不是，因为引用 @no__t 为3。</span><span class="sxs-lookup"><span data-stu-id="e3581-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="e3581-544">为了避免混淆和意外行为，通常情况下，析构函数只对存储在其自身字段中的数据执行清理，而不对引用的对象或静态字段执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="e3581-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="e3581-545">使用析构函数的替代方法是让类实现 `System.IDisposable` 接口。</span><span class="sxs-lookup"><span data-stu-id="e3581-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="e3581-546">这允许对象的客户端确定何时释放对象的资源，这通常是通过在 `using` 语句（[using 语句](statements.md#the-using-statement)）中以资源的形式访问对象的。</span><span class="sxs-lookup"><span data-stu-id="e3581-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="e3581-547">执行顺序</span><span class="sxs-lookup"><span data-stu-id="e3581-547">Execution order</span></span>

<span data-ttu-id="e3581-548">C#程序的执行将继续，使每个正在执行的线程的副作用被保留在关键执行点上。</span><span class="sxs-lookup"><span data-stu-id="e3581-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="e3581-549">***副作用***定义为可变字段的读取或写入、对非易失性变量的写入、写入外部资源和引发异常。</span><span class="sxs-lookup"><span data-stu-id="e3581-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="e3581-550">必须保留这些副作用的顺序的关键执行点是对可变字段的引用（[可变字段](classes.md#volatile-fields)）、`lock` 语句（[lock 语句](statements.md#the-lock-statement)）以及线程创建和终止。</span><span class="sxs-lookup"><span data-stu-id="e3581-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="e3581-551">执行环境可随意更改C#程序的执行顺序，但要服从以下限制：</span><span class="sxs-lookup"><span data-stu-id="e3581-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="e3581-552">数据依赖关系在执行线程中保留。</span><span class="sxs-lookup"><span data-stu-id="e3581-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="e3581-553">也就是说，将计算每个变量的值，就好像该线程中的所有语句都是按原程序顺序执行的一样。</span><span class="sxs-lookup"><span data-stu-id="e3581-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="e3581-554">保留初始化排序规则（[字段初始化](classes.md#field-initialization)和[变量初始值设定项](classes.md#variable-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="e3581-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="e3581-555">对于易失读写（[可变字段](classes.md#volatile-fields)），会保留副作用的顺序。</span><span class="sxs-lookup"><span data-stu-id="e3581-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="e3581-556">此外，如果执行环境可以推断出未使用表达式的值并且不会产生所需的副作用（包括通过调用方法或访问可变字段引起的任何副作用），则执行环境不需要计算表达式的一部分。</span><span class="sxs-lookup"><span data-stu-id="e3581-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="e3581-557">当异步事件（如由另一个线程引发的异常）中断程序执行时，不保证可观察到的副作用在原始程序顺序中可见。</span><span class="sxs-lookup"><span data-stu-id="e3581-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
