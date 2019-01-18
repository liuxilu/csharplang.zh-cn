---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229525"
---
# <a name="basic-concepts"></a><span data-ttu-id="8dc23-101">基本概念</span><span class="sxs-lookup"><span data-stu-id="8dc23-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="8dc23-102">应用程序启动</span><span class="sxs-lookup"><span data-stu-id="8dc23-102">Application Startup</span></span>

<span data-ttu-id="8dc23-103">具有的程序集***入口点***称为***应用程序***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="8dc23-104">应用程序时运行，一个新***应用程序域***创建。</span><span class="sxs-lookup"><span data-stu-id="8dc23-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="8dc23-105">多个不同的实例化的应用程序可能存在同一台计算机上一次，每个都有其自己的应用程序域。</span><span class="sxs-lookup"><span data-stu-id="8dc23-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="8dc23-106">应用程序域启用通过充当应用程序状态的容器应用程序隔离。</span><span class="sxs-lookup"><span data-stu-id="8dc23-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="8dc23-107">应用程序域充当容器和应用程序和它使用类库中定义的类型的边界。</span><span class="sxs-lookup"><span data-stu-id="8dc23-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="8dc23-108">加载到一个应用程序域的类型是不同于相同类型加载到另一个应用程序域，并应用程序域之间不直接共享的对象的实例。</span><span class="sxs-lookup"><span data-stu-id="8dc23-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="8dc23-109">例如，每个应用程序域具有其自己的对于这些类型的静态变量副本，并针对每个应用程序域最多一次运行一种类型的静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="8dc23-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="8dc23-110">实现可以自由地创建和销毁的应用程序域提供特定于实现的策略或机制。</span><span class="sxs-lookup"><span data-stu-id="8dc23-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="8dc23-111">***应用程序启动***发生时执行环境调用指定的方法，这被称为应用程序的入口点。</span><span class="sxs-lookup"><span data-stu-id="8dc23-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="8dc23-112">此入口点方法始终命名为`Main`，并且可以具有以下签名之一：</span><span class="sxs-lookup"><span data-stu-id="8dc23-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="8dc23-113">如所示，入口点可能会选择性地返回`int`值。</span><span class="sxs-lookup"><span data-stu-id="8dc23-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="8dc23-114">此返回值用于应用程序终止 ([应用程序终止时](basic-concepts.md#application-termination))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="8dc23-115">入口点 （可选） 可能具有一个形参。</span><span class="sxs-lookup"><span data-stu-id="8dc23-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="8dc23-116">该参数可以具有任何名称，但是参数的类型必须为`string[]`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="8dc23-117">如果存在的形式参数，则创建执行环境，并将传递`string[]`参数，其中包含了命令行参数指定应用程序启动时。</span><span class="sxs-lookup"><span data-stu-id="8dc23-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="8dc23-118">`string[]`参数永远不会为 null，但它可能具有长度为零，如果未不指定任何命令行参数。</span><span class="sxs-lookup"><span data-stu-id="8dc23-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="8dc23-119">由于 C# 支持重载的方法，类或结构可能包含多个定义的一些方法，提供每个具有不同的签名。</span><span class="sxs-lookup"><span data-stu-id="8dc23-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="8dc23-120">但是，单个程序内没有任何类或结构可能包含多个方法调用`Main`其定义这使得它要用作应用程序入口点。</span><span class="sxs-lookup"><span data-stu-id="8dc23-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="8dc23-121">其他重载的版本`Main`允许使用，但是，只要它们具有多个参数，或者其唯一参数类型不是`string[]`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="8dc23-122">应用程序可以组成的多个类或结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="8dc23-123">就可以为多个这些类或结构的以包含一个名为方法`Main`其定义这使得它要用作应用程序入口点。</span><span class="sxs-lookup"><span data-stu-id="8dc23-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="8dc23-124">在这种情况下，必须使用外部机制 （如命令行编译器选项） 来选择其中一种`Main`方法作为入口点。</span><span class="sxs-lookup"><span data-stu-id="8dc23-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="8dc23-125">在 C# 中，每个方法必须定义为类或结构的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="8dc23-126">通常，声明可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)) 的一种方法由访问修饰符 ([访问修饰符](classes.md#access-modifiers)) 在其声明和与此类似的声明中指定一种类型的可访问性取决于其声明中指定的访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="8dc23-127">为了可调用给定类型的给定方法，该类型和成员都必须是可访问的。</span><span class="sxs-lookup"><span data-stu-id="8dc23-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="8dc23-128">但是，应用程序入口点是一种特殊情况。</span><span class="sxs-lookup"><span data-stu-id="8dc23-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="8dc23-129">具体而言，执行环境可以访问应用程序的入口点而不考虑其声明的可访问性，而不考虑其封闭类型声明的声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="8dc23-130">应用程序入口点方法可能不在泛型类声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="8dc23-131">所有其他方面，入口点方法的行为方式类似的不是入口点。</span><span class="sxs-lookup"><span data-stu-id="8dc23-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="8dc23-132">应用程序终止</span><span class="sxs-lookup"><span data-stu-id="8dc23-132">Application termination</span></span>

<span data-ttu-id="8dc23-133">***应用程序终止时***将控制权返回给执行环境。</span><span class="sxs-lookup"><span data-stu-id="8dc23-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="8dc23-134">如果在应用程序的返回类型***入口点***方法是`int`，返回的值用作应用程序的***终止状态代码***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="8dc23-135">此代码的目的是允许进行通信的成功或失败的执行环境。</span><span class="sxs-lookup"><span data-stu-id="8dc23-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="8dc23-136">如果入口点方法的返回类型是`void`，到达右大括号 (`}`) 终止的方法，或执行`return`没有任何表达式的语句会导致终止状态代码`0`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="8dc23-137">在应用程序的终止之前的所有尚未进行垃圾回收的对象的析构函数将调用，除非已取消这类清理 (通过调用库方法`GC.SuppressFinalize`，例如)。</span><span class="sxs-lookup"><span data-stu-id="8dc23-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="8dc23-138">声明</span><span class="sxs-lookup"><span data-stu-id="8dc23-138">Declarations</span></span>

<span data-ttu-id="8dc23-139">C# 程序中的声明定义该程序的构成元素。</span><span class="sxs-lookup"><span data-stu-id="8dc23-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="8dc23-140">C# 程序的组织结构使用的命名空间 ([命名空间](namespaces.md))，其中包含类型声明和嵌套命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="8dc23-141">类型声明 ([的类型声明](namespaces.md#type-declarations)) 用于定义类 ([类](classes.md))，结构 ([结构](structs.md))，接口 ([接口](interfaces.md))，枚举 ([枚举](enums.md))，和委托 ([委托](delegates.md))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="8dc23-142">类型声明中允许的成员种类取决于类型声明的窗体。</span><span class="sxs-lookup"><span data-stu-id="8dc23-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="8dc23-143">例如，类声明可以包含常量的声明 ([常量](classes.md#constants))，字段 ([字段](classes.md#fields))，方法 ([方法](classes.md#methods))，属性 ([属性](classes.md#properties))，事件 ([事件](classes.md#events))，索引器 ([索引器](classes.md#indexers))，运算符 ([运算符](classes.md#operators))，实例构造函数 ([实例构造函数](classes.md#instance-constructors))，静态构造函数 ([静态构造函数](classes.md#static-constructors))，析构函数 ([析构函数](classes.md#destructors))，和嵌套类型 ([嵌套类型](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="8dc23-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="8dc23-144">声明定义中的名称***声明空间***所属声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="8dc23-145">除重载成员 ([签名和超载](basic-concepts.md#signatures-and-overloading))，它会导致编译时错误有两个或多个具有相同的名称声明空间中的成员的声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="8dc23-146">就永远不会可以声明空间，以包含不同类型的成员具有相同的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="8dc23-147">例如，声明空间可以永远不会有一个字段和方法按相同的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="8dc23-148">有多种不同类型的声明空间，如以下所述。</span><span class="sxs-lookup"><span data-stu-id="8dc23-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="8dc23-149">中的程序的所有源文件*namespace_member_declaration*与没有封闭*namespace_declaration*成员的调用的单个组合的声明空间***全局声明空间***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="8dc23-150">中的程序的所有源文件*namespace_member_declaration*中的 s *namespace_declaration*具有相同的完全限定的命名空间名称是单个组合声明的成员空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="8dc23-151">每个类、 结构或接口声明创建一个新的声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="8dc23-152">名称引入到通过此声明空间*class_member_declaration*s， *struct_member_declaration*s， *interface_member_declaration*s，或*type_parameter*s。</span><span class="sxs-lookup"><span data-stu-id="8dc23-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="8dc23-153">除了重载的实例构造函数声明和声明、 类或结构的静态构造函数不能包含具有与类或结构同名的成员声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="8dc23-154">类、 结构或接口允许重载的方法和索引器的声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="8dc23-155">此外，类或结构允许重载的实例构造函数和运算符的声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="8dc23-156">例如，类、 结构或接口可能包含多个方法声明具有相同的名称，前提是这些方法声明其签名中不同 ([签名和超载](basic-concepts.md#signatures-and-overloading))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="8dc23-157">请注意，基类，这些类并不影响为声明空间的类的基接口并不参与到接口声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="8dc23-158">因此，派生的类或接口被允许声明一个具有相同的名称为继承的成员的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="8dc23-159">这样的成员可以说负责***隐藏***继承的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="8dc23-160">每个委托声明创建一个新的声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="8dc23-161">名称引入到形式参数通过此声明空间 (*fixed_parameter*s 和*parameter_array*s) 和*type_parameter*s。</span><span class="sxs-lookup"><span data-stu-id="8dc23-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="8dc23-162">每个枚举声明创建一个新的声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="8dc23-163">名称引入到通过此声明空间*enum_member_declarations*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="8dc23-164">每个方法声明、 索引器声明、 运算符声明、 实例构造函数声明和匿名函数创建一个名为的新声明空间***局部变量声明空间***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="8dc23-165">名称引入到形式参数通过此声明空间 (*fixed_parameter*s 和*parameter_array*s) 和*type_parameter*s。</span><span class="sxs-lookup"><span data-stu-id="8dc23-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="8dc23-166">正文的函数成员或匿名函数，如果有的话，被视为嵌套在局部变量声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="8dc23-167">它是错误的本地变量声明空间和嵌套的本地变量声明空间包含具有相同名称的元素。</span><span class="sxs-lookup"><span data-stu-id="8dc23-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="8dc23-168">因此，在嵌套的声明空间中不可能在封闭的声明空间中声明本地变量或常量具有相同名称为本地变量或常量。</span><span class="sxs-lookup"><span data-stu-id="8dc23-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="8dc23-169">它是两个声明空间以包含具有相同名称的元素，只要声明空间彼此互不包含。</span><span class="sxs-lookup"><span data-stu-id="8dc23-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="8dc23-170">每个*块*或*switch_block* ，以及*有关*， *foreach*并*使用*语句，创建为本地变量和局部常量局部变量声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="8dc23-171">名称引入到通过此声明空间*local_variable_declaration*s 和*local_constant_declaration*s。</span><span class="sxs-lookup"><span data-stu-id="8dc23-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="8dc23-172">请注意为或的函数成员或匿名函数体中出现的块均嵌套在其参数为这些函数的声明的局部变量声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="8dc23-173">因此，它是错误包含例如具有本地变量与具有相同名称的参数的方法。</span><span class="sxs-lookup"><span data-stu-id="8dc23-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="8dc23-174">每个*块*或*switch_block*创建标签的单独声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="8dc23-175">名称引入到通过此声明空间*labeled_statement*通过引用和名称*goto_statement*s。</span><span class="sxs-lookup"><span data-stu-id="8dc23-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="8dc23-176">***标记声明空间***块的包含任何嵌套的块。</span><span class="sxs-lookup"><span data-stu-id="8dc23-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="8dc23-177">因此，在嵌套的块中不可以声明具有相同的名称在封闭块中的标签的标签。</span><span class="sxs-lookup"><span data-stu-id="8dc23-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="8dc23-178">在其中声明名称的文本顺序通常是没有意义。</span><span class="sxs-lookup"><span data-stu-id="8dc23-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="8dc23-179">具体而言，文本顺序并不重要声明和使用的命名空间、 常量、 方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数、 静态构造函数和类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="8dc23-180">按声明顺序非常重要的以下方法：</span><span class="sxs-lookup"><span data-stu-id="8dc23-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="8dc23-181">字段声明和局部变量声明的声明顺序确定其初始值设定项 （如果有） 的执行顺序。</span><span class="sxs-lookup"><span data-stu-id="8dc23-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="8dc23-182">在使用之前，必须定义本地变量 ([作用域](basic-concepts.md#scopes))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="8dc23-183">枚举成员声明的声明顺序 ([枚举成员](enums.md#enum-members)) 非常重要*constant_expression*会省略值。</span><span class="sxs-lookup"><span data-stu-id="8dc23-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="8dc23-184">命名空间声明空间为"打开已结束"，和两个命名空间声明具有相同的完全限定名称分配到同一声明空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="8dc23-185">例如</span><span class="sxs-lookup"><span data-stu-id="8dc23-185">For example</span></span>
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

<span data-ttu-id="8dc23-186">上面的两个命名空间声明参与到同一声明空间，在这种情况下声明两个类的完全限定名称`Megacorp.Data.Customer`和`Megacorp.Data.Order`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="8dc23-187">由于向同一声明空间分配的两个声明，则将导致编译时错误如果每个包含具有相同名称的类的声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="8dc23-188">以上指定的块声明空间包含任何嵌套的块。</span><span class="sxs-lookup"><span data-stu-id="8dc23-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="8dc23-189">因此，在以下示例中，`F`并`G`方法导致在编译时错误，因为名称`i`在外部块中声明并不能在内部块中重新声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="8dc23-190">但是，`H`并`I`以来这两个方法都有效`i`的在单独的非嵌套块中声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

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

## <a name="members"></a><span data-ttu-id="8dc23-191">成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-191">Members</span></span>

<span data-ttu-id="8dc23-192">命名空间和类型具有***成员***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="8dc23-193">实体的成员是通常可以通过对实体的引用开头跟限定名"`.`"令牌后, 跟的成员的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="8dc23-194">或者在类型声明中声明类型的成员或***继承***从类型的基类。</span><span class="sxs-lookup"><span data-stu-id="8dc23-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="8dc23-195">当一个类型继承自基类的基类，但实例构造函数、 析构函数和静态构造函数，所有成员成为所派生的类型的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="8dc23-196">基类成员的声明可访问性不会控制是否继承成员 — 继承将扩展到任何不是实例构造函数、 静态构造函数或析构函数的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="8dc23-197">但是，继承的成员可能无法访问中的派生类型，原因是其声明的可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)) 或者是因为它隐藏在类型本身是声明 ([通过隐藏继承](basic-concepts.md#hiding-through-inheritance))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="8dc23-198">Namespace 成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-198">Namespace members</span></span>

<span data-ttu-id="8dc23-199">命名空间和具有没有封闭命名空间的类型是的成员***全局命名空间***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="8dc23-200">这直接对应于全局声明空间中声明的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="8dc23-201">命名空间和命名空间内声明的类型是该命名空间的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="8dc23-202">这直接对应于该命名空间的声明空间中声明的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="8dc23-203">命名空间没有任何访问限制。</span><span class="sxs-lookup"><span data-stu-id="8dc23-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="8dc23-204">不能声明私有、 受保护或内部命名空间和命名空间名称始终是可公开访问。</span><span class="sxs-lookup"><span data-stu-id="8dc23-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="8dc23-205">结构成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-205">Struct members</span></span>

<span data-ttu-id="8dc23-206">结构的成员是该结构中声明的成员和继承结构的直接基类的成员`System.ValueType`和间接基类`object`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="8dc23-207">简单类型的成员直接对应于由简单类型的结构类型别名的成员：</span><span class="sxs-lookup"><span data-stu-id="8dc23-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="8dc23-208">成员`sbyte`成员的`System.SByte`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="8dc23-209">成员`byte`成员的`System.Byte`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="8dc23-210">成员`short`成员的`System.Int16`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="8dc23-211">成员`ushort`成员的`System.UInt16`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="8dc23-212">成员`int`成员的`System.Int32`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="8dc23-213">成员`uint`成员的`System.UInt32`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="8dc23-214">成员`long`成员的`System.Int64`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="8dc23-215">成员`ulong`成员的`System.UInt64`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="8dc23-216">成员`char`成员的`System.Char`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="8dc23-217">成员`float`成员的`System.Single`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="8dc23-218">成员`double`成员的`System.Double`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="8dc23-219">成员`decimal`成员的`System.Decimal`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="8dc23-220">成员`bool`成员的`System.Boolean`结构。</span><span class="sxs-lookup"><span data-stu-id="8dc23-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="8dc23-221">枚举成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-221">Enumeration members</span></span>

<span data-ttu-id="8dc23-222">枚举的成员是在枚举中声明的常量和从枚举的直接基类继承的成员`System.Enum`和间接的基类`System.ValueType`和`object`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="8dc23-223">类成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-223">Class members</span></span>

<span data-ttu-id="8dc23-224">类的成员是在类中声明的成员和继承自的基类的成员 (除了类`object`具有没有基类)。</span><span class="sxs-lookup"><span data-stu-id="8dc23-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="8dc23-225">从基类继承的成员包括常量、 字段、 方法、 属性、 事件、 索引器、 运算符和类型的基类，但不是实例构造函数、 析构函数和静态构造函数的类的基类。</span><span class="sxs-lookup"><span data-stu-id="8dc23-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="8dc23-226">基类成员被继承而不考虑其可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="8dc23-227">类声明可以包含常量、 字段、 方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数、 静态构造函数和类型的声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="8dc23-228">成员`object`和`string`直接对应于的类类型成员它们别名：</span><span class="sxs-lookup"><span data-stu-id="8dc23-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="8dc23-229">成员`object`成员的`System.Object`类。</span><span class="sxs-lookup"><span data-stu-id="8dc23-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="8dc23-230">成员`string`成员的`System.String`类。</span><span class="sxs-lookup"><span data-stu-id="8dc23-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="8dc23-231">接口成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-231">Interface members</span></span>

<span data-ttu-id="8dc23-232">接口的成员是在接口和接口的所有基接口中声明的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="8dc23-233">在类成员`object`不严格地讲，任何接口的成员 ([接口成员](interfaces.md#interface-members))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="8dc23-234">但是，在类成员`object`可通过成员查找中任何接口类型 ([成员查找](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="8dc23-235">数组成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-235">Array members</span></span>

<span data-ttu-id="8dc23-236">数组的成员都是从类继承的成员`System.Array`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="8dc23-237">委托成员</span><span class="sxs-lookup"><span data-stu-id="8dc23-237">Delegate members</span></span>

<span data-ttu-id="8dc23-238">委托的成员都是从类继承的成员`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="8dc23-239">成员访问</span><span class="sxs-lookup"><span data-stu-id="8dc23-239">Member access</span></span>

<span data-ttu-id="8dc23-240">成员的声明允许成员访问控制。</span><span class="sxs-lookup"><span data-stu-id="8dc23-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="8dc23-241">成员的可访问性由声明可访问性 ([声明可访问性](basic-concepts.md#declared-accessibility)) 的成员的结合直接包含类型的可访问性如果有的话。</span><span class="sxs-lookup"><span data-stu-id="8dc23-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="8dc23-242">如果允许访问特定成员，则称该成员是***可访问***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="8dc23-243">相反，当不被允许访问特定成员，则称该成员为***无法访问***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="8dc23-244">当可访问性域中包含文本的位置访问发生时允许访问成员 ([可访问性域](basic-concepts.md#accessibility-domains)) 的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="8dc23-245">声明的可访问性</span><span class="sxs-lookup"><span data-stu-id="8dc23-245">Declared accessibility</span></span>

<span data-ttu-id="8dc23-246">***声明可访问性***的成员中可以是以下之一：</span><span class="sxs-lookup"><span data-stu-id="8dc23-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="8dc23-247">公共的其中包括选择`public`成员声明中的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="8dc23-248">直观的含义`public`是"不受限制访问"。</span><span class="sxs-lookup"><span data-stu-id="8dc23-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="8dc23-249">受保护，选中此选项通过包括`protected`成员声明中的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="8dc23-250">直观的含义`protected`"访问限于包含类或类型派生自包含类"。</span><span class="sxs-lookup"><span data-stu-id="8dc23-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="8dc23-251">内部，通过包含所选`internal`成员声明中的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="8dc23-252">直观的含义`internal`是"访问权限限制为此程序"。</span><span class="sxs-lookup"><span data-stu-id="8dc23-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="8dc23-253">受保护的内部 （即受保护或内部），后者通过包括同时选择`protected`和一个`internal`成员声明中的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="8dc23-254">直观的含义`protected internal`是"此程序只能访问或派生自包含类的类型"。</span><span class="sxs-lookup"><span data-stu-id="8dc23-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="8dc23-255">通过包含所选的专用`private`成员声明中的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="8dc23-256">直观的含义`private`是"访问限于包含类型"。</span><span class="sxs-lookup"><span data-stu-id="8dc23-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="8dc23-257">具体取决于上下文的成员声明将在其中放置，允许仅特定类型的声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="8dc23-258">此外，当成员声明中不包括任何访问修饰符，声明发生的上下文确定默认的已声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="8dc23-259">命名空间隐式具有`public`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="8dc23-260">没有访问修饰符允许使用命名空间声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="8dc23-261">在编译单元或命名空间中声明的类型可以具有`public`或`internal`声明可访问性和默认为`internal`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="8dc23-262">类的成员可以具有任何声明可访问性的五种和默认为`private`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="8dc23-263">(请注意而类型声明为一个命名空间的成员可以只具有一种类型可声明为类的成员可以具有任何种类的声明可访问性，五`public`或`internal`声明可访问性。)</span><span class="sxs-lookup"><span data-stu-id="8dc23-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="8dc23-264">结构成员可以具有`public`， `internal`，或`private`声明可访问性和默认为`private`声明可访问性，因为结构隐式密封。</span><span class="sxs-lookup"><span data-stu-id="8dc23-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="8dc23-265">不能具有结构 （即，不会继承该结构） 中引入的结构成员`protected`或`protected internal`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="8dc23-266">(请注意类型声明为结构的成员可以具有`public`， `internal`，或`private`声明可访问性，而类型声明为一个命名空间的成员可以只具有`public`或`internal`声明可访问性。)</span><span class="sxs-lookup"><span data-stu-id="8dc23-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="8dc23-267">接口的成员隐式具有`public`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="8dc23-268">不允许使用任何访问修饰符在接口成员声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="8dc23-269">枚举的成员隐式具有`public`声明可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="8dc23-270">没有访问修饰符允许使用枚举成员声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="8dc23-271">可访问性域</span><span class="sxs-lookup"><span data-stu-id="8dc23-271">Accessibility domains</span></span>

<span data-ttu-id="8dc23-272">***可访问性域***的成员包括的成员的访问权限允许的程序文本 （可能是不连续的） 部分。</span><span class="sxs-lookup"><span data-stu-id="8dc23-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="8dc23-273">为了定义成员的可访问域，说是成员可***顶级***如果它未声明为类型 （） 内，并且成员称为***嵌套***如果它在另一种类型内声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="8dc23-274">此外，***程序文本***的程序的定义如下文本的程序，所有源代码文件中包含的所有程序和一种类型的程序文本定义为所有程序中包含文本*type_declaration*s 的该类型 （包括，也可能嵌套在类型的类型）。</span><span class="sxs-lookup"><span data-stu-id="8dc23-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="8dc23-275">预定义类型的可访问域 (如`object`， `int`，或`double`) 不受限制。</span><span class="sxs-lookup"><span data-stu-id="8dc23-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="8dc23-276">顶级的可访问域之间的绑定类型`T`([绑定和未绑定类型](types.md#bound-and-unbound-types)) 在程序中声明的`P`定义，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8dc23-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="8dc23-277">如果声明可访问性`T`是`public`，可访问域`T`的程序文本`P`和任何引用的程序`P`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="8dc23-278">如果 `T` 的已声明可访问性为 `internal`，则 `T` 的可访问域就是 `P` 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="8dc23-279">从这些定义可以看出，顶级未绑定类型的可访问域是始终至少声明该类型的程序的程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="8dc23-280">构造类型的可访问域`T<A1, ..., An>`是未绑定的泛型类型的可访问域的交集`T`和类型参数的可访问性域`A1, ..., An`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="8dc23-281">嵌套成员的可访问域`M`的类型中声明`T`程序内`P`定义，如下所示 (注意的是，`M`本身可能就是一种类型):</span><span class="sxs-lookup"><span data-stu-id="8dc23-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="8dc23-282">如果 `M` 的已声明可访问性为 `public`，则 `M` 的可访问域就是 `T` 的可访问域。</span><span class="sxs-lookup"><span data-stu-id="8dc23-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="8dc23-283">如果声明可访问性`M`是`protected internal`，可让`D`是联合的程序文本`P`和任何类型的程序文本派生自`T`，这外部声明`P`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="8dc23-284">可访问性域`M`是可访问性域的交集`T`与`D`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="8dc23-285">如果声明可访问性`M`是`protected`，可让`D`是联合的程序文本`T`和任何类型的程序文本派生自`T`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="8dc23-286">可访问性域`M`是可访问性域的交集`T`与`D`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="8dc23-287">如果 `M` 的已声明可访问性为 `internal`，则 `M` 的可访问域就是 `T` 的可访问域与 `P` 的程序文本之间的交集。</span><span class="sxs-lookup"><span data-stu-id="8dc23-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="8dc23-288">如果 `M` 的已声明可访问性为 `private`，则 `M` 的可访问域就是 `T` 的程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="8dc23-289">从这些定义可以看出的嵌套成员的可访问域是始终至少具有在其中声明成员的类型的程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="8dc23-290">此外，它遵循成员的可访问域是永远不会比在其中声明成员的类型的可访问域更广。</span><span class="sxs-lookup"><span data-stu-id="8dc23-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="8dc23-291">直观地讲，在类型或成员时`M`是访问，以下步骤进行计算以确保允许访问：</span><span class="sxs-lookup"><span data-stu-id="8dc23-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="8dc23-292">首先，如果`M`内声明了类型而非到编译单元或命名空间），如果该类型不可访问，会发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="8dc23-293">然后，如果`M`是`public`，允许的访问。</span><span class="sxs-lookup"><span data-stu-id="8dc23-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="8dc23-294">否则为如果`M`是`protected internal`，如果它出现在程序中允许的访问`M`声明，或如果它出现在一个类派生自类在其中`M`声明，并且都是通过派生类类型 ([受保护实例访问成员](basic-concepts.md#protected-access-for-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="8dc23-295">否则为如果`M`是`protected`，如果它出现在类中允许的访问`M`声明，或如果它出现在一个类派生自类在其中`M`声明，并且都是通过派生类类型 ([受保护实例访问成员](basic-concepts.md#protected-access-for-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="8dc23-296">否则为如果`M`是`internal`，如果它出现在程序中允许的访问`M`声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="8dc23-297">否则为如果`M`是`private`，如果它出现在其中的类型在允许的访问`M`声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="8dc23-298">否则为该类型或成员将无法访问，并且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="8dc23-299">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-299">In the example</span></span>
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
<span data-ttu-id="8dc23-300">类和成员具有以下可访问性域：</span><span class="sxs-lookup"><span data-stu-id="8dc23-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="8dc23-301">可访问性域`A`和`A.X`不受限制。</span><span class="sxs-lookup"><span data-stu-id="8dc23-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="8dc23-302">可访问性域`A.Y`， `B`， `B.X`， `B.Y`， `B.C`， `B.C.X`，和`B.C.Y`是包含程序的程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="8dc23-303">可访问性域`A.Z`的程序文本`A`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="8dc23-304">可访问性域`B.Z`并`B.D`的程序文本`B`，其中包括的程序文本`B.C`和`B.D`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="8dc23-305">可访问性域`B.C.Z`的程序文本`B.C`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="8dc23-306">可访问性域`B.D.X`并`B.D.Y`的程序文本`B`，其中包括的程序文本`B.C`和`B.D`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="8dc23-307">可访问性域`B.D.Z`的程序文本`B.D`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="8dc23-308">如示例所示，成员的可访问域是类型的永远不会超过包含。</span><span class="sxs-lookup"><span data-stu-id="8dc23-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="8dc23-309">例如，即使所有`X`的成员具有公共声明的可访问性，以外的所有`A.X`有包含类型受约束的可访问性域。</span><span class="sxs-lookup"><span data-stu-id="8dc23-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="8dc23-310">如中所述[成员](basic-concepts.md#members)，由派生类型继承的基类，除了实例构造函数、 析构函数和静态构造函数，所有成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="8dc23-311">这包括甚至私有成员的基类。</span><span class="sxs-lookup"><span data-stu-id="8dc23-311">This includes even private members of a base class.</span></span> <span data-ttu-id="8dc23-312">但是，私有成员的可访问域包含在其中声明成员的类型的程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="8dc23-313">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-313">In the example</span></span>
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
<span data-ttu-id="8dc23-314">`B`类继承私有成员`x`从`A`类。</span><span class="sxs-lookup"><span data-stu-id="8dc23-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="8dc23-315">因为该成员是私有的才可访问内*class_body*的`A`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="8dc23-316">因此，对的访问权限`b.x`成功`A.F`方法，但在失败`B.F`方法。</span><span class="sxs-lookup"><span data-stu-id="8dc23-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="8dc23-317">受保护的实例成员的访问</span><span class="sxs-lookup"><span data-stu-id="8dc23-317">Protected access for instance members</span></span>

<span data-ttu-id="8dc23-318">当`protected`在其中声明它，类的程序文本外部访问实例成员以及何时`protected internal`实例成员访问声明它的程序的程序文本之外，访问必须发生在从声明它的类派生的类声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="8dc23-319">此外，需要通过该派生的类类型或从其构造的类类型的实例进行的访问。</span><span class="sxs-lookup"><span data-stu-id="8dc23-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="8dc23-320">此限制可防止一个派生的类访问的其他派生类中，受保护的成员，即使从相同的基类继承的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="8dc23-321">让`B`是声明受保护的实例成员的基类`M`，并让`D`是派生的类`B`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="8dc23-322">内*class_body*的`D`，访问`M`可以采用以下形式之一：</span><span class="sxs-lookup"><span data-stu-id="8dc23-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="8dc23-323">未限定*type_name*或*primary_expression*窗体的`M`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="8dc23-324">一个*primary_expression*窗体`E.M`，提供的类型`E`是`T`或从派生的类`T`，其中`T`是类类型`D`，或类类型从构造 `D`</span><span class="sxs-lookup"><span data-stu-id="8dc23-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="8dc23-325">一个*primary_expression*窗体的`base.M`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="8dc23-326">除了这些形式的访问，派生的类可以访问基类中的受保护的实例构造函数*constructor_initializer* ([构造函数初始值设定项](classes.md#constructor-initializers))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="8dc23-327">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-327">In the example</span></span>
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
<span data-ttu-id="8dc23-328">内`A`，就可以访问`x`通过两个实例`A`并`B`，因为在任一情况下访问都是通过实例`A`派生一个类或`A`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="8dc23-329">但是，在`B`，不能访问`x`的实例通过`A`，因为`A`不是派生自`B`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="8dc23-330">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-330">In the example</span></span>
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
<span data-ttu-id="8dc23-331">对三个赋值`x`允许，因为它们都需要通过从泛型类型构造的类类型的实例。</span><span class="sxs-lookup"><span data-stu-id="8dc23-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="8dc23-332">可访问性约束</span><span class="sxs-lookup"><span data-stu-id="8dc23-332">Accessibility constraints</span></span>

<span data-ttu-id="8dc23-333">在 C# 语言中的多个构造需要类型为***至少与可访问性***成员，或者另一种类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="8dc23-334">一种类型`T`称其至少与可访问性的成员或类型`M`如果的可访问域`T`是可访问性域中的一个超集`M`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="8dc23-335">换而言之，`T`是至少与可访问性`M`如果`T`可在其中的所有上下文中访问`M`可访问。</span><span class="sxs-lookup"><span data-stu-id="8dc23-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="8dc23-336">存在以下的可访问性约束：</span><span class="sxs-lookup"><span data-stu-id="8dc23-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="8dc23-337">类类型的直接基类必须至少具有与类类型本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="8dc23-338">接口类型的显式基接口必须至少具有与接口类型本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="8dc23-339">委托类型的返回类型和参数类型必须至少具有与委托类型本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="8dc23-340">常量的类型必须至少具有与常量本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="8dc23-341">字段的类型必须至少具有与字段本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="8dc23-342">方法的返回类型和参数类型必须至少具有与方法本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="8dc23-343">属性的类型必须至少具有与属性本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="8dc23-344">事件的类型必须至少具有与事件本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="8dc23-345">索引器的类型和参数类型必须至少具有与索引器本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="8dc23-346">运算符的类型和参数类型必须至少具有与运算符本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="8dc23-347">实例构造函数的参数类型必须至少与实例构造函数本身相同的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="8dc23-348">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="8dc23-349">`B`类会导致编译时错误，因为`A`不是至少与可访问性`B`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="8dc23-350">同样，在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="8dc23-351">`H`中的方法`B`会导致编译时错误，因为返回类型`A`不是至少与该方法具有同样的可访问性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="8dc23-352">签名和超载</span><span class="sxs-lookup"><span data-stu-id="8dc23-352">Signatures and overloading</span></span>

<span data-ttu-id="8dc23-353">方法、 实例构造函数、 索引器和运算符的特征体现在其***签名***:</span><span class="sxs-lookup"><span data-stu-id="8dc23-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="8dc23-354">方法签名包含方法、 类型参数的数目和类型和类型 （值、 引用或输出） 的每个从左到右的顺序被视为其正式参数的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8dc23-355">出于这些目的，不是按其名称，但该方法的类型实参列表中其序号位置通过标识的方法的形参的类型中发生的任何类型参数。</span><span class="sxs-lookup"><span data-stu-id="8dc23-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="8dc23-356">方法签名特别不包括返回类型，`params`可能最右边的参数，也不可选类型参数约束为指定的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="8dc23-357">实例构造函数的签名组成的类型和每个从左到右的顺序被视为其正式参数的类型 （值、 引用或输出）。</span><span class="sxs-lookup"><span data-stu-id="8dc23-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8dc23-358">实例构造函数的签名专门不包括`params`可能最右边的参数为指定的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="8dc23-359">索引器的签名组成的每个从左到右的顺序被视为其正式参数的类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8dc23-360">索引器的签名专门不包括的元素类型，也不包括`params`可能最右边的参数为指定的修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="8dc23-361">运算符的签名包含的运算符和每个从左到右的顺序被视为其正式参数的类型名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="8dc23-362">具体而言，运算符的签名不包括的结果类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="8dc23-363">签名是启用的机制，用于***重载***类、 结构和接口中的成员：</span><span class="sxs-lookup"><span data-stu-id="8dc23-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="8dc23-364">方法的重载允许类、 结构或接口声明多个具有相同的名称，提供它们的签名的方法是在该类、 结构或接口中是唯一。</span><span class="sxs-lookup"><span data-stu-id="8dc23-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="8dc23-365">实例构造函数的重载允许类或结构声明多个实例构造函数提供它们的签名是在该类或结构中是唯一。</span><span class="sxs-lookup"><span data-stu-id="8dc23-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="8dc23-366">索引器重载允许类、 结构或接口声明多个索引器，提供它们的签名是在该类、 结构或接口中是唯一。</span><span class="sxs-lookup"><span data-stu-id="8dc23-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="8dc23-367">运算符的重载允许类或结构声明多个运算符具有相同的名称，提供它们的签名是在该类或结构中是唯一。</span><span class="sxs-lookup"><span data-stu-id="8dc23-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="8dc23-368">尽管`out`并`ref`参数修饰符被视为一个签名的一部分，在单个类型中声明成员不能在签名中完全由`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="8dc23-369">如果两个成员声明中使用将是相同的签名相同的类型与这两种方法中的所有参数，会发生编译时错误`out`修饰符更改为`ref`修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="8dc23-370">用于签名匹配的其他目的 （例如，隐藏或重写），`ref`和`out`被视为签名的一部分，相互不匹配。</span><span class="sxs-lookup"><span data-stu-id="8dc23-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="8dc23-371">(此限制是允许C#程序能够轻松地进行转换，以便运行在公共语言基础结构 (CLI)，其中未提供一种方法来定义仅在不同的方法`ref`并`out`。)</span><span class="sxs-lookup"><span data-stu-id="8dc23-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="8dc23-372">为了进行签名，类型`object`和`dynamic`被视为相同。</span><span class="sxs-lookup"><span data-stu-id="8dc23-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="8dc23-373">在单个类型中声明成员因此不在可能有所不同，签名完全由`object`和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="8dc23-374">下面的示例显示了一组重载的方法声明，以及它们的签名。</span><span class="sxs-lookup"><span data-stu-id="8dc23-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
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

<span data-ttu-id="8dc23-375">请注意的任何`ref`并`out`参数修饰符 ([方法参数](classes.md#method-parameters)) 是一个签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="8dc23-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="8dc23-376">因此，`F(int)`和`F(ref int)`是唯一的签名。</span><span class="sxs-lookup"><span data-stu-id="8dc23-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="8dc23-377">但是，`F(ref int)`并`F(out int)`不能声明内相同的接口中，因为它们的签名完全由不同`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="8dc23-378">另请注意的返回类型和`params`修饰符不签名的一部分，因此不能重载仅基于返回类型或包含或排除`params`修饰符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="8dc23-379">因此，这些方法的声明`F(int)`和`F(params string[])`上述导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="8dc23-380">范围</span><span class="sxs-lookup"><span data-stu-id="8dc23-380">Scopes</span></span>

<span data-ttu-id="8dc23-381">***作用域***的名称是在其中就可以按名称而无需限定名称的声明的实体引用的程序文本的区域。</span><span class="sxs-lookup"><span data-stu-id="8dc23-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="8dc23-382">作用域可以是***嵌套***，并将来自外部范围名称的含义可能会重新内层作用域声明 (这不会但是，删除强加的限制[声明](basic-concepts.md#declarations)嵌套块内不可以通过声明一个局部变量作为封闭块中的本地变量同名）。</span><span class="sxs-lookup"><span data-stu-id="8dc23-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="8dc23-383">从外部作用域的名称然后说可***隐藏***程序的区域中文本涵盖内层作用域，并对外部名称，则仅可能访问的限定名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="8dc23-384">通过声明的命名空间成员的作用域*namespace_member_declaration* ([Namespace 成员](namespaces.md#namespace-members)) 与没有封闭*namespace_declaration*是整个程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="8dc23-385">通过声明的命名空间成员的作用域*namespace_member_declaration*内*namespace_declaration*其完全限定的名称是`N`是*namespace_body*的每个*namespace_declaration*其完全限定的名称是`N`开头或`N`后, 跟一个句点。</span><span class="sxs-lookup"><span data-stu-id="8dc23-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="8dc23-386">定义名称的作用域*extern_alias_directive*通过进行了扩展*using_directive*s， *global_attributes*和*namespace_member_声明*s 其直接包含的编译单元或命名空间体。</span><span class="sxs-lookup"><span data-stu-id="8dc23-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="8dc23-387">*Extern_alias_directive*不计为基础的声明空间的任何新成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="8dc23-388">换而言之， *extern_alias_directive*不是可传递的但，而是只影响的编译单元或命名空间体其中发生。</span><span class="sxs-lookup"><span data-stu-id="8dc23-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="8dc23-389">名称的作用域定义，或通过导入*using_directive* ([Using 指令](namespaces.md#using-directives)) 通过了扩展*namespace_member_declaration*的*compilation_unit*或*namespace_body*顺序*using_directive*时发生。</span><span class="sxs-lookup"><span data-stu-id="8dc23-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="8dc23-390">一个*using_directive*可能会使零个或多个命名空间、 类型或成员名称可用中特定*compilation_unit*或*namespace_body*，但不支持参与为基础的声明空间的任何新成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="8dc23-391">换而言之， *using_directive*不是可传递，但而不是仅影响*compilation_unit*或*namespace_body*它所在。</span><span class="sxs-lookup"><span data-stu-id="8dc23-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="8dc23-392">通过声明的类型参数的作用域*type_parameter_list*上*class_declaration* ([类声明](classes.md#class-declarations)) 是*class_base*， *type_parameter_constraints_clause*s，和*class_body*这些*class_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="8dc23-393">通过声明的类型参数的作用域*type_parameter_list*上*struct_declaration* ([结构声明](structs.md#struct-declarations)) 是*struct_interfaces*， *type_parameter_constraints_clause*s，和*struct_body*这些*struct_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="8dc23-394">通过声明的类型参数的作用域*type_parameter_list*上*interface_declaration* ([接口声明](interfaces.md#interface-declarations)) 是*interface_base*， *type_parameter_constraints_clause*s，和*interface_body*这些*interface_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="8dc23-395">通过声明的类型参数的作用域*type_parameter_list*上*delegate_declaration* ([委托声明](delegates.md#delegate-declarations)) 是*return_type*， *formal_parameter_list*，和*type_parameter_constraints_clause*的 s *delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="8dc23-396">通过声明成员的作用域*class_member_declaration* ([类正文](classes.md#class-body)) 是*class_body*中进行声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="8dc23-397">此外，类成员的范围扩展到*class_body*这些派生的可访问性域中包含的类 ([可访问性域](basic-concepts.md#accessibility-domains)) 的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="8dc23-398">通过声明成员的作用域*struct_member_declaration* ([结构成员](structs.md#struct-members)) 是*struct_body*中进行声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="8dc23-399">通过声明成员的作用域*enum_member_declaration* ([枚举成员](enums.md#enum-members)) 是*enum_body*中进行声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="8dc23-400">参数的作用域中声明*method_declaration* ([方法](classes.md#methods)) 是*method_body*这些*method_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="8dc23-401">参数的作用域中声明*indexer_declaration* ([索引器](classes.md#indexers)) 是*accessor_declarations*的*indexer_declaration*.</span><span class="sxs-lookup"><span data-stu-id="8dc23-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="8dc23-402">参数的作用域中声明*operator_declaration* ([运算符](classes.md#operators)) 是*块*这些*operator_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="8dc23-403">参数的作用域中声明*constructor_declaration* ([实例构造函数](classes.md#instance-constructors)) 是*constructor_initializer*和*块*这些*constructor_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="8dc23-404">参数的作用域中声明*lambda_expression* ([匿名函数表达式](expressions.md#anonymous-function-expressions)) 是*anonymous_function_body*的*lambda_表达式*</span><span class="sxs-lookup"><span data-stu-id="8dc23-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="8dc23-405">参数的作用域中声明*anonymous_method_expression* ([匿名函数表达式](expressions.md#anonymous-function-expressions)) 是*块*的*anonymous_method_expression*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="8dc23-406">标签的范围中声明*labeled_statement* ([标记为语句](statements.md#labeled-statements)) 是*块*中进行声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="8dc23-407">在中声明本地变量的作用域*local_variable_declaration* ([局部变量声明](statements.md#local-variable-declarations)) 是在块中进行声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="8dc23-408">在中声明本地变量的作用域*switch_block*的`switch`语句 ([switch 语句](statements.md#the-switch-statement)) 是*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="8dc23-409">在中声明本地变量的作用域*for_initializer*的`for`语句 ([语句](statements.md#the-for-statement)) 是*for_initializer*，则*for_condition*，则*for_iterator*，和包含*语句*的`for`语句。</span><span class="sxs-lookup"><span data-stu-id="8dc23-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="8dc23-410">局部常量的作用域中声明*local_constant_declaration* ([局部常量声明](statements.md#local-constant-declarations)) 是在块中进行声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="8dc23-411">它会导致编译时错误之前的文本位置中的本地常量，请参阅其*constant_declarator*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="8dc23-412">变量的作用域声明的一部分*foreach_statement*， *using_statement*， *lock_statement*或者*query_expression*是由给定的构造的扩展。</span><span class="sxs-lookup"><span data-stu-id="8dc23-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="8dc23-413">命名空间、 类、 结构或枚举成员的作用域内就可以引用的文本的位置之前的成员的声明的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="8dc23-414">例如</span><span class="sxs-lookup"><span data-stu-id="8dc23-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="8dc23-415">在这里，它是有效的`F`来指代`i`在声明之前。</span><span class="sxs-lookup"><span data-stu-id="8dc23-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="8dc23-416">在本地变量的范围内，它是编译时错误，以引用之前的文本位置中的本地变量*local_variable_declarator*的本地变量。</span><span class="sxs-lookup"><span data-stu-id="8dc23-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="8dc23-417">例如</span><span class="sxs-lookup"><span data-stu-id="8dc23-417">For example</span></span>
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

<span data-ttu-id="8dc23-418">在中`F`方法更高版本中，第一个分配给`i`一定不是指在外部作用域声明的字段。</span><span class="sxs-lookup"><span data-stu-id="8dc23-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="8dc23-419">相反，它是指本地变量和由于文本上位于前面的变量声明导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="8dc23-420">在`G`方法中，使用`j`中的声明的初始值设定项`j`是有效的因为使用之前没有*local_variable_declarator*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="8dc23-421">在`H`方法，则后续*local_variable_declarator*正确地指的是早期在声明的本地变量*local_variable_declarator*在同一*local_variable_declaration*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="8dc23-422">本地变量的范围规则旨在保证的含义的名称，用于在表达式的上下文中始终是块中相同。</span><span class="sxs-lookup"><span data-stu-id="8dc23-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="8dc23-423">如果本地变量的作用域将仅从其声明扩展到块的末尾，然后在上面的示例中，第一个分配将分配给实例变量和第二个分配将分配给本地变量，则可能会导致如果后来重新排列的块的语句的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="8dc23-424">块中名称的含义可能会因的上下文中使用的名称。</span><span class="sxs-lookup"><span data-stu-id="8dc23-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="8dc23-425">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-425">In the example</span></span>
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
<span data-ttu-id="8dc23-426">名称`A`表达式上下文中使用来指代本地变量`A`，在类型上下文中来指代类`A`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="8dc23-427">隐藏名称</span><span class="sxs-lookup"><span data-stu-id="8dc23-427">Name hiding</span></span>

<span data-ttu-id="8dc23-428">实体范围通常包括实体的声明空间比多个程序文本。</span><span class="sxs-lookup"><span data-stu-id="8dc23-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="8dc23-429">具体而言，实体的作用域可能包括引入新声明空间包含具有相同名称的实体的声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="8dc23-430">此类声明会导致原始实体变得***隐藏***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="8dc23-431">相反，实体称为***可见***时它不会隐藏。</span><span class="sxs-lookup"><span data-stu-id="8dc23-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="8dc23-432">通过嵌套和通过继承重叠的范围重叠时，会发生名称隐藏。</span><span class="sxs-lookup"><span data-stu-id="8dc23-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="8dc23-433">以下各节所述的隐藏这两种类型的特征。</span><span class="sxs-lookup"><span data-stu-id="8dc23-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="8dc23-434">通过嵌套隐藏</span><span class="sxs-lookup"><span data-stu-id="8dc23-434">Hiding through nesting</span></span>

<span data-ttu-id="8dc23-435">嵌套命名空间或命名空间、 嵌套类型的类或结构中参数和局部变量声明中的类型的结果会发生名称隐藏通过嵌套。</span><span class="sxs-lookup"><span data-stu-id="8dc23-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="8dc23-436">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-436">In the example</span></span>
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
<span data-ttu-id="8dc23-437">内`F`方法中，实例变量`i`隐藏由本地变量`i`，但内`G`方法，`i`仍是指实例变量。</span><span class="sxs-lookup"><span data-stu-id="8dc23-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="8dc23-438">如果内层作用域中的名称将隐藏外层作用域中的名称，它会隐藏该名称的所有重载的匹配项。</span><span class="sxs-lookup"><span data-stu-id="8dc23-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="8dc23-439">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-439">In the example</span></span>
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
<span data-ttu-id="8dc23-440">在调用`F(1)`调用`F`中声明`Inner`因为外部出现的所有`F`隐藏内部声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="8dc23-441">出于相同原因，在调用`F("Hello")`会导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="8dc23-442">通过继承隐藏</span><span class="sxs-lookup"><span data-stu-id="8dc23-442">Hiding through inheritance</span></span>

<span data-ttu-id="8dc23-443">当类或结构重新声明从基类继承的名称，通过继承隐藏名称时发生。</span><span class="sxs-lookup"><span data-stu-id="8dc23-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="8dc23-444">这种类型的隐藏名称采用以下形式之一：</span><span class="sxs-lookup"><span data-stu-id="8dc23-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="8dc23-445">常量、 字段、 属性、 事件或在类或结构中引入的类型会隐藏具有相同名称的所有基类成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="8dc23-446">在类或结构中引入的方法将隐藏具有相同名称的所有非方法基类成员和具有相同的签名 （方法名称和参数计数、 修饰符和类型） 的所有基类方法。</span><span class="sxs-lookup"><span data-stu-id="8dc23-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="8dc23-447">在类或结构中引入的索引器会隐藏具有相同的签名 （参数计数和类型） 的所有基类索引器。</span><span class="sxs-lookup"><span data-stu-id="8dc23-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="8dc23-448">管理运算符声明的规则 ([运算符](classes.md#operators)) 使其不是通过派生类声明为基类中的运算符具有相同签名的运算符。</span><span class="sxs-lookup"><span data-stu-id="8dc23-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="8dc23-449">因此，运算符将永远不会隐藏另一个。</span><span class="sxs-lookup"><span data-stu-id="8dc23-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="8dc23-450">与隐藏来自外部范围的名称，相反隐藏继承范围中的可访问名称会导致报告警告。</span><span class="sxs-lookup"><span data-stu-id="8dc23-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="8dc23-451">在示例</span><span class="sxs-lookup"><span data-stu-id="8dc23-451">In the example</span></span>
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
<span data-ttu-id="8dc23-452">声明`F`在`Derived`导致报告警告。</span><span class="sxs-lookup"><span data-stu-id="8dc23-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="8dc23-453">隐藏继承的名称不具体而言是错误，因为那将排除的基类，这些类的单独演变。</span><span class="sxs-lookup"><span data-stu-id="8dc23-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="8dc23-454">例如，如果上述情况可能会发生因为更高版本`Base`引入了`F`类的早期版本中不存在的方法。</span><span class="sxs-lookup"><span data-stu-id="8dc23-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="8dc23-455">具有更高版本的情况是一个错误，然后对单独进行版本控制的类库中的基类进行任何更改可能导致派生的类变得无效。</span><span class="sxs-lookup"><span data-stu-id="8dc23-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="8dc23-456">可以通过使用消除隐藏继承的名称由导致该警告`new`修饰符：</span><span class="sxs-lookup"><span data-stu-id="8dc23-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
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

<span data-ttu-id="8dc23-457">`new`修饰符指示`F`中`Derived`是"新建"，，它确实能隐藏继承的成员。</span><span class="sxs-lookup"><span data-stu-id="8dc23-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="8dc23-458">新成员的声明隐藏继承的成员只能在新成员的范围中。</span><span class="sxs-lookup"><span data-stu-id="8dc23-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

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

<span data-ttu-id="8dc23-459">在示例中的声明上方`F`中`Derived`隐藏`F`继承自`Base`，但由于新`F`中`Derived`具有私有访问权限，其作用域内不会扩展到`MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="8dc23-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="8dc23-460">因此，调用`F()`中`MoreDerived.G`有效，并且将调用`Base.F`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="8dc23-461">Namespace 和类型名称</span><span class="sxs-lookup"><span data-stu-id="8dc23-461">Namespace and type names</span></span>

<span data-ttu-id="8dc23-462">中的多个环境C#程序需要 *_ 名称*或*type_name*指定。</span><span class="sxs-lookup"><span data-stu-id="8dc23-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

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

<span data-ttu-id="8dc23-463">一个 *_ 名称*是*namespace_or_type_name* ，是指命名空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="8dc23-464">以下解决方法，如下所述*namespace_or_type_name*的 *_ 名称*必须引用一个命名空间，否则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="8dc23-465">没有类型参数 ([的类型实参](types.md#type-arguments)) 可出现在 *_ 名称*（仅类型可以具有类型参数）。</span><span class="sxs-lookup"><span data-stu-id="8dc23-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="8dc23-466">一个*type_name*是*namespace_or_type_name*引用类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="8dc23-467">以下解决方法，如下所述*namespace_or_type_name*的*type_name*必须引用类型，否则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="8dc23-468">如果*namespace_or_type_name*是一个限定的别名的成员，其含义中所述[Namespace 别名限定符](namespaces.md#namespace-alias-qualifiers)。</span><span class="sxs-lookup"><span data-stu-id="8dc23-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="8dc23-469">否则为*namespace_or_type_name*有四种形式之一：</span><span class="sxs-lookup"><span data-stu-id="8dc23-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="8dc23-470">其中`I`是单个标识符`N`是*namespace_or_type_name*并`<A1, ..., Ak>`是一个可选*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="8dc23-471">如果未*type_argument_list*是指定，请考虑`k`为零。</span><span class="sxs-lookup"><span data-stu-id="8dc23-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="8dc23-472">含义*namespace_or_type_name* ，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="8dc23-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="8dc23-473">如果*namespace_or_type_name*的形式`I`或窗体的`I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="8dc23-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="8dc23-474">如果`K`为零， *namespace_or_type_name*出现在泛型方法声明 ([方法](classes.md#methods))，如果该声明包含一个类型参数 ([类型参数](classes.md#type-parameters)) 具有名称 `I`，然后*namespace_or_type_name*引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="8dc23-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="8dc23-475">否则为如果*namespace_or_type_name*出现在类型声明中，然后，对于每个实例类型 `T`([的实例类型](classes.md#the-instance-type))、 第一页为该类型的实例类型声明，然后继续每个封闭类或结构声明的实例类型 （如果有）：</span><span class="sxs-lookup"><span data-stu-id="8dc23-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="8dc23-476">如果`K`是零的声明`T`包括具有名称的类型参数 `I`，然后*namespace_or_type_name*引用该类型参数。</span><span class="sxs-lookup"><span data-stu-id="8dc23-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="8dc23-477">否则为如果*namespace_or_type_name*将显示在类型声明的主体和`T`或任何其基类型包含具有名称的嵌套可访问类型 `I`和`K`  类型参数，则*namespace_or_type_name*构造具有给定的类型参数的该类型是指。</span><span class="sxs-lookup"><span data-stu-id="8dc23-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="8dc23-478">如果有多个此类类型，被选择派生程度更大的类型中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="8dc23-479">请注意，非类型成员 （常量、 字段、 方法、 属性、 索引器、 运算符、 实例构造函数、 析构函数和静态构造函数） 和类型成员具有不同数量的类型参数时，将忽略确定的含义*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="8dc23-480">如果前面的步骤已不成功，则每个命名空间 `N`，在其中命名空间从开始*namespace_or_type_name*发生时，继续执行每个封闭命名空间 （如果有），以及结尾全局命名空间中，以下步骤进行计算，直到找到一个实体就是：</span><span class="sxs-lookup"><span data-stu-id="8dc23-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="8dc23-481">如果`K`为零并`I`中的命名空间名称 `N`，然后：</span><span class="sxs-lookup"><span data-stu-id="8dc23-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="8dc23-482">如果位置其中*namespace_or_type_name*发生的命名空间声明括`N`和命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与命名空间或类型，则*namespace_or_type_name*不明确，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="8dc23-483">否则为*namespace_or_type_name*指的是名为的命名空间`I`中`N`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="8dc23-484">否则为如果`N`包含可访问类型具有名称 `I`并`K` 类型形参，则：</span><span class="sxs-lookup"><span data-stu-id="8dc23-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="8dc23-485">如果`K`是零和位置， *namespace_or_type_name*发生括起来的命名空间声明`N`和命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与命名空间或类型，则*namespace_or_type_name*已不明确以及编译时间出现错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="8dc23-486">否则为*namespace_or_type_name*指构造具有给定的类型参数的类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="8dc23-487">否则为如果位置其中*namespace_or_type_name*发生括起来的命名空间声明`N`:</span><span class="sxs-lookup"><span data-stu-id="8dc23-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="8dc23-488">如果`K`为零，并且命名空间声明中包含*extern_alias_directive*或*using_alias_directive*将关联名称 `I`与导入的命名空间或类型，则*namespace_or_type_name*指的是该命名空间或类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="8dc23-489">否则为如果通过导入的命名空间和类型声明*using_namespace_directive*s 和*using_alias_directive*的命名空间声明包含一个可访问类型具有名称 `I`并`K` 类型参数，则*namespace_or_type_name*构造具有给定的类型参数的该类型是指。</span><span class="sxs-lookup"><span data-stu-id="8dc23-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="8dc23-490">否则为如果通过导入的命名空间和类型声明*using_namespace_directive*s 和*using_alias_directive*的命名空间声明包含多个可访问类型具有名称 `I`并`K` 类型参数，则*namespace_or_type_name*不明确并产生一个错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="8dc23-491">否则为*namespace_or_type_name*是未定义，且将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="8dc23-492">否则为*namespace_or_type_name*的形式`N.I`或窗体的`N.I<A1, ..., Ak>`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="8dc23-493">`N` 作为第一次解决*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="8dc23-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="8dc23-494">如果解析`N`不成功，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="8dc23-495">否则为`N.I`或`N.I<A1, ..., Ak>`解析方式如下：</span><span class="sxs-lookup"><span data-stu-id="8dc23-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="8dc23-496">如果`K`为零并`N`指的是命名空间和`N`包含名称的嵌套命名空间`I`，则*namespace_or_type_name*指的是该嵌套命名空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="8dc23-497">否则为如果`N`指的是命名空间和`N`包含可访问类型具有名称 `I`并`K` 类型参数，则*namespace_or_type_name*引用为给定的类型参数用来构造该类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="8dc23-498">否则为如果`N`指的是一个 （可能是构造） 的类或结构类型和`N`或任何其基类，这些类包含具有名称的嵌套可访问类型 `I`并`K` 类型参数，则*namespace_or_type_name*构造具有给定的类型参数的该类型是指。</span><span class="sxs-lookup"><span data-stu-id="8dc23-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="8dc23-499">如果有多个此类类型，被选择派生程度更大的类型中声明的类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="8dc23-500">请注意，如果的含义`N.I`要确定作为解决的基类规范的一部分`N`然后直接基类`N`被视为对象 ([基类](classes.md#base-classes))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="8dc23-501">否则为`N.I`是一个无效*namespace_or_type_name*，并将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="8dc23-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="8dc23-502">一个*namespace_or_type_name*允许以引用静态类 ([静态类](classes.md#static-classes)) 仅当</span><span class="sxs-lookup"><span data-stu-id="8dc23-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="8dc23-503">*Namespace_or_type_name*是`T`中*namespace_or_type_name*窗体的`T.I`，或</span><span class="sxs-lookup"><span data-stu-id="8dc23-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="8dc23-504">*Namespace_or_type_name*是`T`中*typeof_expression* ([自变量列表](expressions.md#argument-lists)1) 的窗体`typeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="8dc23-505">完全限定的名称</span><span class="sxs-lookup"><span data-stu-id="8dc23-505">Fully qualified names</span></span>

<span data-ttu-id="8dc23-506">每个命名空间和类型有***完全限定的名称***，可唯一标识的命名空间或在所有其他类型。</span><span class="sxs-lookup"><span data-stu-id="8dc23-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="8dc23-507">命名空间或类型的完全限定的名称`N`，如下所示确定：</span><span class="sxs-lookup"><span data-stu-id="8dc23-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="8dc23-508">如果`N`成员的全局命名空间，其完全限定的名称是`N`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="8dc23-509">否则，其完全限定的名称是`S.N`，其中`S`是完全限定的命名空间或在其中的类型名称`N`声明。</span><span class="sxs-lookup"><span data-stu-id="8dc23-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="8dc23-510">换而言之，完全限定的名称`N`是导致的标识符的完整层次结构路径`N`，从全局命名空间。</span><span class="sxs-lookup"><span data-stu-id="8dc23-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="8dc23-511">因为每个命名空间或类型成员必须具有唯一的名称，它遵循的命名空间或类型的完全限定的名称始终是唯一。</span><span class="sxs-lookup"><span data-stu-id="8dc23-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="8dc23-512">下面的示例演示了几个命名空间和类型声明及其关联的完全限定名。</span><span class="sxs-lookup"><span data-stu-id="8dc23-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
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

## <a name="automatic-memory-management"></a><span data-ttu-id="8dc23-513">自动内存管理</span><span class="sxs-lookup"><span data-stu-id="8dc23-513">Automatic memory management</span></span>

<span data-ttu-id="8dc23-514">C# 使用自动内存管理，它使开发人员不再需要手动分配和释放对象占用的内存。</span><span class="sxs-lookup"><span data-stu-id="8dc23-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="8dc23-515">实现自动内存管理策略***垃圾回收器***。</span><span class="sxs-lookup"><span data-stu-id="8dc23-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="8dc23-516">对象的内存管理生命周期如下所示：</span><span class="sxs-lookup"><span data-stu-id="8dc23-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="8dc23-517">创建对象后，为其分配内存、 运行时构造函数，和对象被视为实时。</span><span class="sxs-lookup"><span data-stu-id="8dc23-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="8dc23-518">如果不能由执行任何可能继续访问对象或任何部分，以外运行析构函数，该对象被视为无法再在使用中，并成为符合销毁条件。</span><span class="sxs-lookup"><span data-stu-id="8dc23-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="8dc23-519">C# 编译器和垃圾回收器可以选择要分析的代码以确定哪些引用的对象可能在将来使用。</span><span class="sxs-lookup"><span data-stu-id="8dc23-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="8dc23-520">例如，如果在范围内的本地变量是一个对象的唯一现有引用，但该本地变量永远不会引用执行从当前执行的任何后续点过程中，垃圾回收器可能 （但不是为必需） 处理该对象为不再使用。</span><span class="sxs-lookup"><span data-stu-id="8dc23-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="8dc23-521">对象符合销毁条件后，在稍后某个时间析构函数 ([析构函数](classes.md#destructors)) （如果有） 为运行对象。</span><span class="sxs-lookup"><span data-stu-id="8dc23-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="8dc23-522">在正常情况下对象的析构函数是只运行一次，但特定于实现的 Api 可能允许重写此行为。</span><span class="sxs-lookup"><span data-stu-id="8dc23-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="8dc23-523">对象的析构函数运行后，如果不能由任何可能的执行，包括运行析构函数，继续访问该对象或任何部分，该对象被视为无法访问和对象将成为符合回收的条件。</span><span class="sxs-lookup"><span data-stu-id="8dc23-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="8dc23-524">最后，在某个时间，对象将变为符合回收条件后垃圾回收器将释放与该对象相关联的内存。</span><span class="sxs-lookup"><span data-stu-id="8dc23-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="8dc23-525">垃圾回收器会维护有关对象使用情况信息，并使用此信息以使内存管理决策，例如要查找新创建的对象，当重定位对象，以及当对象不再使用或无法访问的内存中的何处。</span><span class="sxs-lookup"><span data-stu-id="8dc23-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="8dc23-526">假定垃圾回收器存在其他语言，如 C# 设计，以便垃圾回收器可能会实现范围广泛的内存管理策略。</span><span class="sxs-lookup"><span data-stu-id="8dc23-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="8dc23-527">例如，C# 不需要析构函数，在运行或对象被回收，只要它们是符合条件，或按任何特定顺序或任何特定线程上运行的析构函数。</span><span class="sxs-lookup"><span data-stu-id="8dc23-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="8dc23-528">可以控制垃圾回收器的行为，某种程度上，通过在类上的静态方法`System.GC`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="8dc23-529">此类可用于请求回收发生，析构函数运行 （或不运行），等等。</span><span class="sxs-lookup"><span data-stu-id="8dc23-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="8dc23-530">由于垃圾回收器很大自由度确定何时收集对象和运行析构函数中的，符合标准的实现可能会产生不同于下面的代码所示的输出。</span><span class="sxs-lookup"><span data-stu-id="8dc23-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="8dc23-531">该程序</span><span class="sxs-lookup"><span data-stu-id="8dc23-531">The program</span></span>
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
<span data-ttu-id="8dc23-532">创建类的实例`A`类的实例和`B`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="8dc23-533">这些对象会变得可以进行垃圾回收时变量`b`的值赋给`null`，因为在此时间后不可能的任何用户编写的代码以对其进行访问。</span><span class="sxs-lookup"><span data-stu-id="8dc23-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="8dc23-534">输出可以是</span><span class="sxs-lookup"><span data-stu-id="8dc23-534">The output could be either</span></span>
```
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="8dc23-535">或</span><span class="sxs-lookup"><span data-stu-id="8dc23-535">or</span></span>
```
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="8dc23-536">因为该语言没有对实施约束中的对象进行垃圾回收的顺序。</span><span class="sxs-lookup"><span data-stu-id="8dc23-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="8dc23-537">在微妙的情况下，"符合销毁条件"和"符合回收条件"之间的区别非常重要。</span><span class="sxs-lookup"><span data-stu-id="8dc23-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="8dc23-538">例如，应用于对象的</span><span class="sxs-lookup"><span data-stu-id="8dc23-538">For example,</span></span>
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

<span data-ttu-id="8dc23-539">在上面的程序，如果垃圾回收器选择要运行的析构函数`A`的析构函数之前`B`，则可能是此程序的输出：</span><span class="sxs-lookup"><span data-stu-id="8dc23-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="8dc23-540">请注意，虽然的实例`A`未使用并`A`的析构函数已运行，它表示仍有可能的方法`A`(在这种情况下， `F`) 若要从另一个析构函数调用。</span><span class="sxs-lookup"><span data-stu-id="8dc23-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="8dc23-541">另请注意，析构函数的运行可能会导致再次变得从主线程序使用的对象。</span><span class="sxs-lookup"><span data-stu-id="8dc23-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="8dc23-542">在此情况下，运行`B`的析构函数导致的实例`A`的以前不能在使用变得可从实时引用`Test.RefA`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="8dc23-543">在调用`WaitForPendingFinalizers`的实例`B`符合条件的集合，但实例`A`不是，由于引用`Test.RefA`。</span><span class="sxs-lookup"><span data-stu-id="8dc23-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="8dc23-544">若要避免产生混淆和意外的行为，它通常是个好主意析构函数只对它们的对象的字段中存储的数据执行清理并不被引用的对象或静态字段上执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="8dc23-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="8dc23-545">使用析构函数的替代方法是让实现的类`System.IDisposable`接口。</span><span class="sxs-lookup"><span data-stu-id="8dc23-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="8dc23-546">这样的对象的客户端，以确定何时将释放的资源对象，通常为中的资源访问的对象`using`语句 ([using 语句](statements.md#the-using-statement))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="8dc23-547">执行顺序</span><span class="sxs-lookup"><span data-stu-id="8dc23-547">Execution order</span></span>

<span data-ttu-id="8dc23-548">C# 程序的执行过程，以便在关键执行点处保留的每个执行线程的副作用。</span><span class="sxs-lookup"><span data-stu-id="8dc23-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="8dc23-549">一个***副作用***定义为读取或写入可变字段的、 写入的非易失性变量、 对外部资源，并引发的异常进行写入。</span><span class="sxs-lookup"><span data-stu-id="8dc23-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="8dc23-550">这些副作用的顺序必须保存在其中的关键执行点是对可变字段的引用 ([可变字段](classes.md#volatile-fields))，`lock`语句 ([lock 语句](statements.md#the-lock-statement))，并线程创建和终止。</span><span class="sxs-lookup"><span data-stu-id="8dc23-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="8dc23-551">执行环境可以随意更改 C# 程序，受以下约束的执行顺序：</span><span class="sxs-lookup"><span data-stu-id="8dc23-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="8dc23-552">内的执行线程保持数据依赖性。</span><span class="sxs-lookup"><span data-stu-id="8dc23-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="8dc23-553">也就是说，像在原始程序顺序执行线程中的所有语句计算每个变量的值。</span><span class="sxs-lookup"><span data-stu-id="8dc23-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="8dc23-554">初始化排序规则将被保留 ([字段初始化](classes.md#field-initialization)并[变量的初始值设定项](classes.md#variable-initializers))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="8dc23-555">保留的负面影响的顺序相对于易失性读取和写入 ([可变字段](classes.md#volatile-fields))。</span><span class="sxs-lookup"><span data-stu-id="8dc23-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="8dc23-556">此外，执行环境不需要计算表达式的一部分，如果不使用该表达式的值和任何所需的副作用会产生 （包括任何由调用方法或访问可变字段导致），它就可以推断出。</span><span class="sxs-lookup"><span data-stu-id="8dc23-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="8dc23-557">由异步事件 （如由另一个线程引发的异常） 中断程序执行，不保证可观察的副作用是在原始程序顺序中可见。</span><span class="sxs-lookup"><span data-stu-id="8dc23-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
