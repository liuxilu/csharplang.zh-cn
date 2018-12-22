# <a name="unsafe-code"></a><span data-ttu-id="9d863-101">不安全代码</span><span class="sxs-lookup"><span data-stu-id="9d863-101">Unsafe code</span></span>

<span data-ttu-id="9d863-102">Core C# 语言中，在前面的章节中，定义不同值得注意的是与 C 和 c + + 中的指针的数据类型作为其省略。</span><span class="sxs-lookup"><span data-stu-id="9d863-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="9d863-103">相反，C# 提供引用和创建由垃圾回收器管理的对象的功能。</span><span class="sxs-lookup"><span data-stu-id="9d863-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="9d863-104">这种设计，结合其他功能，使 C# 更安全语言比 C 或 c + +。</span><span class="sxs-lookup"><span data-stu-id="9d863-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="9d863-105">在 core C# 语言不只是可能具有未初始化的变量、"无关联"的指针或索引超出其边界的数组的表达式。</span><span class="sxs-lookup"><span data-stu-id="9d863-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="9d863-106">整个类别的 bug 总是烦扰 C 和 c + + 程序这样就消除了。</span><span class="sxs-lookup"><span data-stu-id="9d863-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="9d863-107">虽然在 C 或 c + + 中的几乎每个指针类型构造在 C# 中有引用类型对应，尽管如此，有些情况下指针类型的访问权限，有必要。</span><span class="sxs-lookup"><span data-stu-id="9d863-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="9d863-108">例如，与基础操作系统进行交互，访问内存映射的设备，或实现时间关键型算法可能可能或可行而无需访问指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="9d863-109">为了满足此需求，C# 提供的功能编写***不安全代码***。</span><span class="sxs-lookup"><span data-stu-id="9d863-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="9d863-110">不安全代码中，则可以声明和对指针执行指针和整数类型，可以获取变量的地址之间的转换等等。</span><span class="sxs-lookup"><span data-stu-id="9d863-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="9d863-111">在某种意义上，编写不安全代码非常类似于编写 C# 程序中的 C 代码。</span><span class="sxs-lookup"><span data-stu-id="9d863-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="9d863-112">不安全代码实际上是从开发人员和用户的角度来看"安全"功能。</span><span class="sxs-lookup"><span data-stu-id="9d863-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="9d863-113">不安全代码必须清楚地标记为修饰符`unsafe`，因此，开发人员可能不能使用不安全的功能不小心，并且执行引擎的工作方式以确保不能在不受信任环境中执行不安全代码。</span><span class="sxs-lookup"><span data-stu-id="9d863-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="9d863-114">不安全的上下文</span><span class="sxs-lookup"><span data-stu-id="9d863-114">Unsafe contexts</span></span>

<span data-ttu-id="9d863-115">仅在不安全上下文中可用的 C# 的不安全功能。</span><span class="sxs-lookup"><span data-stu-id="9d863-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="9d863-116">通过包括引入了不安全的上下文`unsafe`中声明的类型或成员，也可以使用修饰符*unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="9d863-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="9d863-117">类、 结构、 接口或委托的声明可能包括`unsafe`修饰符，在这种情况下 （包括类、 结构或接口的正文） 该类型声明的整个文本范围被认为不安全的上下文。</span><span class="sxs-lookup"><span data-stu-id="9d863-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="9d863-118">字段、 方法、 属性、 事件、 索引器、 运算符、 实例构造函数、 析构函数或静态构造函数的声明可能包括`unsafe`修饰符，在这种情况下该成员声明的整个文本范围被认为不安全上下文。</span><span class="sxs-lookup"><span data-stu-id="9d863-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="9d863-119">*Unsafe_statement*允许不安全的上下文中使用*块*。</span><span class="sxs-lookup"><span data-stu-id="9d863-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="9d863-120">关联的整个文本范围*块*被认为是不安全上下文。</span><span class="sxs-lookup"><span data-stu-id="9d863-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="9d863-121">下面显示了关联的语法生产。</span><span class="sxs-lookup"><span data-stu-id="9d863-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="9d863-122">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="9d863-123">`unsafe`结构声明中指定的修饰符将导致该结构声明变得不安全的上下文的整个文本范围。</span><span class="sxs-lookup"><span data-stu-id="9d863-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="9d863-124">因此，它是可以声明`Left`和`Right`字段为指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="9d863-125">此外可以编写上面的示例</span><span class="sxs-lookup"><span data-stu-id="9d863-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="9d863-126">在这里，`unsafe`中的字段声明修饰符导致这些声明被认为是不安全上下文。</span><span class="sxs-lookup"><span data-stu-id="9d863-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="9d863-127">除了建立安全上下文中，从而允许使用指针类型`unsafe`修饰符的类型或成员上无效。</span><span class="sxs-lookup"><span data-stu-id="9d863-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="9d863-128">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="9d863-129">`unsafe`上的修饰符`F`中的方法`A`只需将导致的文本范围`F`变得不安全的上下文可以在其中使用的语言不安全功能。</span><span class="sxs-lookup"><span data-stu-id="9d863-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="9d863-130">重写`F`中`B`，无需重新指定`unsafe`修饰符-除非，当然，`F`中的方法`B`本身需要访问不安全的功能。</span><span class="sxs-lookup"><span data-stu-id="9d863-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="9d863-131">指针类型为方法的一部分时，这种情况是签名的略有不同</span><span class="sxs-lookup"><span data-stu-id="9d863-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="9d863-132">此处，因为`F`的签名包含类型的指针，它可以只编写不安全的上下文中。</span><span class="sxs-lookup"><span data-stu-id="9d863-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="9d863-133">但是，可以通过使整个类不安全，因为这种情况在引入不安全的上下文`A`，或通过包括`unsafe`修饰符在方法声明中，在这种情况`B`。</span><span class="sxs-lookup"><span data-stu-id="9d863-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="9d863-134">指针类型</span><span class="sxs-lookup"><span data-stu-id="9d863-134">Pointer types</span></span>

<span data-ttu-id="9d863-135">在不安全上下文中，*类型*([类型](types.md)) 可能*pointer_type* ，以及*value_type*或*reference_type*.</span><span class="sxs-lookup"><span data-stu-id="9d863-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="9d863-136">但是， *pointer_type*也可能在使用`typeof`表达式 ([匿名对象创建表达式](expressions.md#anonymous-object-creation-expressions)) 不安全上下文之外这种情况下不使用不安全。</span><span class="sxs-lookup"><span data-stu-id="9d863-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="9d863-137">一个*pointer_type*写作*unmanaged_type*或关键字`void`后, 跟`*`令牌：</span><span class="sxs-lookup"><span data-stu-id="9d863-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="9d863-138">之前指定的类型`*`在指针类型称为***宏中类型***指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="9d863-139">它表示指向指针类型的值的变量的类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="9d863-140">与不同的引用 （引用类型的值），指针不会跟踪垃圾回收器--垃圾回收器是未知的指针和指向的数据。</span><span class="sxs-lookup"><span data-stu-id="9d863-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="9d863-141">对于不允许使用指向此原因以指向引用或为结构，其中包含引用，并且的指针的引用类型必须是*unmanaged_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="9d863-142">*Unmanaged_type*是任何类型，不是*reference_type*或构造类型，并且不包含*reference_type*或构造类型的任何级别的字段嵌套。</span><span class="sxs-lookup"><span data-stu-id="9d863-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="9d863-143">换而言之， *unmanaged_type*是以下之一：</span><span class="sxs-lookup"><span data-stu-id="9d863-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="9d863-144">`sbyte``byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`， `decimal`，或`bool`。</span><span class="sxs-lookup"><span data-stu-id="9d863-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="9d863-145">任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="9d863-146">任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="9d863-147">任何用户定义*struct_type*的构造的类型并不包含的字段*unmanaged_type*仅。</span><span class="sxs-lookup"><span data-stu-id="9d863-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="9d863-148">指针和引用的混合的直观规则是，引用 （对象） 的目标可以包含指针，但不是允许包含引用的指针的引用者。</span><span class="sxs-lookup"><span data-stu-id="9d863-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="9d863-149">下表给出了指针类型的一些示例：</span><span class="sxs-lookup"><span data-stu-id="9d863-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="9d863-150">__示例__</span><span class="sxs-lookup"><span data-stu-id="9d863-150">__Example__</span></span> | <span data-ttu-id="9d863-151">__说明__</span><span class="sxs-lookup"><span data-stu-id="9d863-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="9d863-152">指向 `byte`</span><span class="sxs-lookup"><span data-stu-id="9d863-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="9d863-153">指向 `char`</span><span class="sxs-lookup"><span data-stu-id="9d863-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="9d863-154">指针到指向 `int`</span><span class="sxs-lookup"><span data-stu-id="9d863-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="9d863-155">指向的指针的一维数组 `int`</span><span class="sxs-lookup"><span data-stu-id="9d863-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="9d863-156">指向未知类型</span><span class="sxs-lookup"><span data-stu-id="9d863-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="9d863-157">对于给定的实现，所有指针类型必须都具有相同的大小和表示形式。</span><span class="sxs-lookup"><span data-stu-id="9d863-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="9d863-158">与 C 和 c + +，当在同一声明中，在 C# 中声明多个指针时不同`*`编写基础类型，以及不为每个指针名称的前缀标点符号。</span><span class="sxs-lookup"><span data-stu-id="9d863-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="9d863-159">例如</span><span class="sxs-lookup"><span data-stu-id="9d863-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="9d863-160">具有类型的指针的值`T*`表示的类型的变量的地址`T`。</span><span class="sxs-lookup"><span data-stu-id="9d863-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="9d863-161">指针间接寻址运算符`*`([指针间接寻址](unsafe-code.md#pointer-indirection)) 可用于访问此变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="9d863-162">例如，给定变量`P`类型的`int*`，该表达式`*P`表示`int`变量中包含的地址，请参阅`P`。</span><span class="sxs-lookup"><span data-stu-id="9d863-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="9d863-163">对象引用，如指针可能`null`。</span><span class="sxs-lookup"><span data-stu-id="9d863-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="9d863-164">将应用间接寻址运算符到`null`指针会导致实现定义的行为。</span><span class="sxs-lookup"><span data-stu-id="9d863-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="9d863-165">具有值的指针`null`由所有位均为零。</span><span class="sxs-lookup"><span data-stu-id="9d863-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="9d863-166">`void*`类型表示指向未知类型的指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="9d863-167">目标类型是未知的因为间接寻址运算符不能应用于类型的指针`void*`，也不能对这样的指针执行任何算术运算。</span><span class="sxs-lookup"><span data-stu-id="9d863-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="9d863-168">但是，类型的指针`void*`可转换为任何其他指针类型 （反之亦然）。</span><span class="sxs-lookup"><span data-stu-id="9d863-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="9d863-169">指针类型是单独类别的类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="9d863-170">引用类型和值类型，与指针类型不继承自`object`和指针类型之间不存在转换和`object`。</span><span class="sxs-lookup"><span data-stu-id="9d863-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="9d863-171">具体而言，装箱和取消装箱 ([装箱和取消装箱](types.md#boxing-and-unboxing)) 不支持的指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="9d863-172">但是，指针类型和整型类型之间以及不同的指针类型之间允许转换。</span><span class="sxs-lookup"><span data-stu-id="9d863-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="9d863-173">这中所述[指针转换](unsafe-code.md#pointer-conversions)。</span><span class="sxs-lookup"><span data-stu-id="9d863-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="9d863-174">一个*pointer_type*不能用作类型参数 ([构造类型](types.md#constructed-types))，和类型推理 ([类型推理](expressions.md#type-inference)) 上将具有推断的泛型方法调用失败类型实参为指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="9d863-175">一个*pointer_type*可能用作可变字段的类型 ([可变字段](classes.md#volatile-fields))。</span><span class="sxs-lookup"><span data-stu-id="9d863-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="9d863-176">尽管可以作为传递指针`ref`或`out`参数，这样做可能会导致未定义的行为，因为指针可能也会设置为指向本地变量不再存在时被调用的方法返回时或它的固定的对象用于指明，不再固定的。</span><span class="sxs-lookup"><span data-stu-id="9d863-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="9d863-177">例如：</span><span class="sxs-lookup"><span data-stu-id="9d863-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="9d863-178">一种方法可以返回某些类型的值和该类型可以是指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="9d863-179">例如，当的连续序列提供一个指向`int`s，该序列的元素计数和一些其他`int`值，以下方法在该序列中，返回该值的地址，如果出现匹配项; 否则返回`null`:</span><span class="sxs-lookup"><span data-stu-id="9d863-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="9d863-180">在不安全上下文中，多个构造是可用于操作指针：</span><span class="sxs-lookup"><span data-stu-id="9d863-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="9d863-181">`*`可用于执行指针间接寻址运算符 ([指针间接寻址](unsafe-code.md#pointer-indirection))。</span><span class="sxs-lookup"><span data-stu-id="9d863-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="9d863-182">`->`运算符可用于通过指针访问结构成员 ([指针成员访问](unsafe-code.md#pointer-member-access))。</span><span class="sxs-lookup"><span data-stu-id="9d863-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="9d863-183">`[]`运算符可用于索引指针 ([指针元素访问](unsafe-code.md#pointer-element-access))。</span><span class="sxs-lookup"><span data-stu-id="9d863-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="9d863-184">`&`运算符可用于获取变量的地址 ([address-of 运算符](unsafe-code.md#the-address-of-operator))。</span><span class="sxs-lookup"><span data-stu-id="9d863-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="9d863-185">`++`并`--`运算符可以用于递增和递减指针 ([指针递增和递减](unsafe-code.md#pointer-increment-and-decrement))。</span><span class="sxs-lookup"><span data-stu-id="9d863-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="9d863-186">`+`并`-`可用于执行指针算术运算符 ([指针算法](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="9d863-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="9d863-187">`==`， `!=`， `<`， `>`， `<=`，并且`=>`运算符可用于比较指针 ([指针比较](unsafe-code.md#pointer-comparison))。</span><span class="sxs-lookup"><span data-stu-id="9d863-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="9d863-188">`stackalloc`运算符可用于从调用堆栈中分配内存 ([固定大小的缓冲区](unsafe-code.md#fixed-size-buffers))。</span><span class="sxs-lookup"><span data-stu-id="9d863-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="9d863-189">`fixed`语句可用于临时固定变量，因此可以获取其地址 ([fixed 的语句](unsafe-code.md#the-fixed-statement))。</span><span class="sxs-lookup"><span data-stu-id="9d863-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="9d863-190">固定和可移动变量</span><span class="sxs-lookup"><span data-stu-id="9d863-190">Fixed and moveable variables</span></span>

<span data-ttu-id="9d863-191">Address-of 运算符 ([address-of 运算符](unsafe-code.md#the-address-of-operator)) 和`fixed`语句 ([fixed 的语句](unsafe-code.md#the-fixed-statement)) 将变量划分为两个类别：***固定变量***并***可移动变量***。</span><span class="sxs-lookup"><span data-stu-id="9d863-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="9d863-192">固定的变量驻留在不受垃圾回收器的操作的存储位置中。</span><span class="sxs-lookup"><span data-stu-id="9d863-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="9d863-193">（固定变量的示例包括本地变量、 值参数和变量创建的取消引用指针）。另一方面，可移动变量驻留在受到重定位或垃圾回收器处置的存储位置中。</span><span class="sxs-lookup"><span data-stu-id="9d863-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="9d863-194">（可移动的变量的示例中包括字段对象和数组的元素。）</span><span class="sxs-lookup"><span data-stu-id="9d863-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="9d863-195">`&`运算符 ([address-of 运算符](unsafe-code.md#the-address-of-operator)) 允许不受限制地获取为固定变量的地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="9d863-196">但是，由于受重定位或垃圾回收器处置是可移动变量，可移动的变量的地址可以仅使用获取`fixed`语句 ([fixed 的语句](unsafe-code.md#the-fixed-statement))，并且该地址专用于该期间保持有效`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="9d863-197">准确地说，固定的变量是以下值之一：</span><span class="sxs-lookup"><span data-stu-id="9d863-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="9d863-198">从变量从而*simple_name* ([简单名称](expressions.md#simple-names))，是指本地变量或值参数，除非该变量所捕获的匿名函数。</span><span class="sxs-lookup"><span data-stu-id="9d863-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="9d863-199">从变量产生*member_access* ([成员访问](expressions.md#member-access)) 的窗体`V.I`，其中`V`的固定变量*struct_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="9d863-200">从变量从而*pointer_indirection_expression* ([指针间接寻址](unsafe-code.md#pointer-indirection)) 的窗体`*P`、 一个*pointer_member_access* ([指针成员访问](unsafe-code.md#pointer-member-access)) 的窗体`P->I`，或*pointer_element_access* ([指针元素访问](unsafe-code.md#pointer-element-access)) 的窗体`P[E]`。</span><span class="sxs-lookup"><span data-stu-id="9d863-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="9d863-201">所有其他变量被归类为可移动的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="9d863-202">请注意，静态字段归为可移动的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="9d863-203">另请注意，`ref`或`out`参数分类为可移动的变量，即使指定的参数的参数是固定的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="9d863-204">最后，请注意生成的取消引用某个指针的变量始终属于固定变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="9d863-205">指针转换</span><span class="sxs-lookup"><span data-stu-id="9d863-205">Pointer conversions</span></span>

<span data-ttu-id="9d863-206">在不安全上下文中，可隐式转换的集合 ([隐式转换](conversions.md#implicit-conversions)) 扩展为包括以下隐式指针转换：</span><span class="sxs-lookup"><span data-stu-id="9d863-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="9d863-207">从任何*pointer_type*为类型`void*`。</span><span class="sxs-lookup"><span data-stu-id="9d863-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="9d863-208">从`null`任何文字*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="9d863-209">此外，在不安全的上下文，可显式转换的集合 ([显式转换](conversions.md#explicit-conversions)) 扩展为包括以下显式指针转换：</span><span class="sxs-lookup"><span data-stu-id="9d863-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="9d863-210">从任何*pointer_type*到任何其他*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="9d863-211">从`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`到任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="9d863-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="9d863-212">从任何*pointer_type*到`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="9d863-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="9d863-213">最后，在不安全的上下文，标准隐式转换的集合 ([标准隐式转换](conversions.md#standard-implicit-conversions)) 包括下列指针转换：</span><span class="sxs-lookup"><span data-stu-id="9d863-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="9d863-214">从任何*pointer_type*为类型`void*`。</span><span class="sxs-lookup"><span data-stu-id="9d863-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="9d863-215">两个指针类型之间的转换永远不会更改实际指针值。</span><span class="sxs-lookup"><span data-stu-id="9d863-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="9d863-216">换而言之，从一个指针类型转换为另一个具有对指定的指针的基础地址没有影响。</span><span class="sxs-lookup"><span data-stu-id="9d863-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="9d863-217">当一个指针类型转换到另一个，如果生成的指针不指向的类型正确对齐时，则该行为不确定，如果结果取消引用。</span><span class="sxs-lookup"><span data-stu-id="9d863-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="9d863-218">一般情况下，"正确对齐"的概念是可传递： 如果指向类型的指针`A`指向类型的指针正确对齐`B`，而后者又正确对齐指向类型的指针`C`，然后指向类型`A`指向类型的指针正确对齐`C`。</span><span class="sxs-lookup"><span data-stu-id="9d863-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="9d863-219">请考虑以下情况，在其中具有一种类型的变量访问通过指向不同类型的指针：</span><span class="sxs-lookup"><span data-stu-id="9d863-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="9d863-220">当指针类型转换为指向字节，结果将指向的最低寻址字节的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="9d863-221">结果，最大的变量的大小的连续递增值产生指向该变量的剩余字节数。</span><span class="sxs-lookup"><span data-stu-id="9d863-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="9d863-222">例如，以下方法显示中每个 8 个字节的双精度为十六进制值：</span><span class="sxs-lookup"><span data-stu-id="9d863-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="9d863-223">当然，生成的输出取决于字节顺序。</span><span class="sxs-lookup"><span data-stu-id="9d863-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="9d863-224">指针和整数之间的映射是实现定义的。</span><span class="sxs-lookup"><span data-stu-id="9d863-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="9d863-225">但是，在 32 \* 和 64 位的 CPU 体系结构具有线性地址空间的转换的指针的整数类型通常具有的行为完全相同的转换`uint`或`ulong`分别向或从这些整型值。</span><span class="sxs-lookup"><span data-stu-id="9d863-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="9d863-226">指针数组</span><span class="sxs-lookup"><span data-stu-id="9d863-226">Pointer arrays</span></span>

<span data-ttu-id="9d863-227">在不安全上下文中，可以构造的指针的数组。</span><span class="sxs-lookup"><span data-stu-id="9d863-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="9d863-228">只有某些适用于其他数组类型的转换适用于指针数组：</span><span class="sxs-lookup"><span data-stu-id="9d863-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="9d863-229">隐式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)) 从任何*array_type*到`System.Array`并且它还实现的接口适用于指针数组。</span><span class="sxs-lookup"><span data-stu-id="9d863-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="9d863-230">但是，任何尝试访问数组元素通过`System.Array`或其实现的接口会导致在运行时异常，因为指针类型不是可转换为`object`。</span><span class="sxs-lookup"><span data-stu-id="9d863-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="9d863-231">隐式和显式引用转换 ([隐式引用转换](conversions.md#implicit-reference-conversions)，[显式引用转换](conversions.md#explicit-reference-conversions)) 从一维数组类型`S[]`到`System.Collections.Generic.IList<T>`和其通用的基接口永远不会用于指针的数组，因为指针类型不能用作类型参数，并且不存在于非指针类型转换指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="9d863-232">显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 从`System.Array`到任何实现接口*array_type*适用于指针数组。</span><span class="sxs-lookup"><span data-stu-id="9d863-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="9d863-233">显式引用转换 ([显式引用转换](conversions.md#explicit-reference-conversions)) 从`System.Collections.Generic.IList<S>`到一维数组类型及其基接口`T[]`永远不会应用于指针数组，因为指针类型不能为使用作为类型参数，并且没有指针类型转换为非指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="9d863-234">这些限制意味着为扩展`foreach`语句对数组中所述[foreach 语句](statements.md#the-foreach-statement)不能应用于指针数组。</span><span class="sxs-lookup"><span data-stu-id="9d863-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="9d863-235">相反，在窗体的 foreach 语句</span><span class="sxs-lookup"><span data-stu-id="9d863-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="9d863-236">其中的类型`x`是数组类型的窗体`T[,,...,]`，`N`是数减 1 的维度和`T`或`V`是指针类型，将展开使用嵌套的 for 循环，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9d863-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="9d863-237">变量`a`， `i0`， `i1`，...，`iN`不可见或不可以访问`x`或*embedded_statement*或程序的任何其他源代码。</span><span class="sxs-lookup"><span data-stu-id="9d863-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="9d863-238">该变量`v`是嵌入的语句中以只读的。</span><span class="sxs-lookup"><span data-stu-id="9d863-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="9d863-239">如果不存在的显式转换 ([指针转换](unsafe-code.md#pointer-conversions)) 从`T`（元素类型） 到`V`，将会生成错误并采取任何进一步的步骤。</span><span class="sxs-lookup"><span data-stu-id="9d863-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="9d863-240">如果`x`具有值`null`、`System.NullReferenceException`在运行时引发。</span><span class="sxs-lookup"><span data-stu-id="9d863-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="9d863-241">表达式中的指针</span><span class="sxs-lookup"><span data-stu-id="9d863-241">Pointers in expressions</span></span>

<span data-ttu-id="9d863-242">在不安全上下文中，表达式可能产生的结果是指针类型，但它不安全的上下文之外是表达式是指针类型的表达式的编译时错误。</span><span class="sxs-lookup"><span data-stu-id="9d863-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="9d863-243">准确地说，不安全的上下文之外的编译时错误如果会发生任何*simple_name* ([简单名称](expressions.md#simple-names))， *member_access* ([成员访问](expressions.md#member-access))， *invocation_expression* ([调用表达式](expressions.md#invocation-expressions))，或*element_access* ([元素访问](expressions.md#element-access)) 的指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="9d863-244">在不安全上下文中， *primary_no_array_creation_expression* ([主表达式](expressions.md#primary-expressions)) 和*unary_expression* ([一元运算符](expressions.md#unary-operators))生产允许以下其他构造：</span><span class="sxs-lookup"><span data-stu-id="9d863-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="9d863-245">这些构造以下各节所述。</span><span class="sxs-lookup"><span data-stu-id="9d863-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="9d863-246">语法暗示的优先顺序和不安全的运算符的关联性。</span><span class="sxs-lookup"><span data-stu-id="9d863-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="9d863-247">指针间接寻址</span><span class="sxs-lookup"><span data-stu-id="9d863-247">Pointer indirection</span></span>

<span data-ttu-id="9d863-248">一个*pointer_indirection_expression*包含一个星号 (`*`) 后跟*unary_expression*。</span><span class="sxs-lookup"><span data-stu-id="9d863-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="9d863-249">一元`*`运算符表示指针间接寻址和用于获取一个指针指向的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="9d863-250">计算结果`*P`，其中`P`是指针类型的表达式`T*`，类型的变量`T`。</span><span class="sxs-lookup"><span data-stu-id="9d863-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="9d863-251">它会导致编译时错误应用一元`*`类型的表达式的运算符`void*`或不是指针类型的表达式。</span><span class="sxs-lookup"><span data-stu-id="9d863-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="9d863-252">应用一元的效果`*`运算符`null`是实现定义的指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="9d863-253">特别是，则此操作抛出不能保证`System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="9d863-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="9d863-254">如果已经分配了无效的值的指针，一元的行为`*`运算符是不确定。</span><span class="sxs-lookup"><span data-stu-id="9d863-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="9d863-255">无效值时取消指针引用由一元`*`运算符是为指向类型正确对齐的地址 (示例中的，请参阅[指针转换](unsafe-code.md#pointer-conversions))，以及之后的变量的地址其生命周期结束。</span><span class="sxs-lookup"><span data-stu-id="9d863-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="9d863-256">为了明确赋值分析，通过计算形式的表达式将产生的变量`*P`被认为是最初赋值 ([最初分配变量](variables.md#initially-assigned-variables))。</span><span class="sxs-lookup"><span data-stu-id="9d863-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="9d863-257">指针成员访问</span><span class="sxs-lookup"><span data-stu-id="9d863-257">Pointer member access</span></span>

<span data-ttu-id="9d863-258">一个*pointer_member_access*组成*primary_expression*后, 跟"`->`"令牌后, 跟*标识符*和可选*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="9d863-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="9d863-259">在窗体的指针成员访问`P->I`，`P`必须是指针类型的表达式不是`void*`，和`I`必须表示为的类型的可访问成员`P`点。</span><span class="sxs-lookup"><span data-stu-id="9d863-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="9d863-260">指针成员访问窗体`P->I`评估完全为`(*P).I`。</span><span class="sxs-lookup"><span data-stu-id="9d863-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="9d863-261">有关说明的指针间接寻址运算符 (`*`)，请参阅[指针间接寻址](unsafe-code.md#pointer-indirection)。</span><span class="sxs-lookup"><span data-stu-id="9d863-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="9d863-262">有关成员访问运算符的说明 (`.`)，请参阅[成员访问](expressions.md#member-access)。</span><span class="sxs-lookup"><span data-stu-id="9d863-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="9d863-263">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="9d863-264">`->`运算符用于访问字段并调用通过指针结构的方法。</span><span class="sxs-lookup"><span data-stu-id="9d863-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="9d863-265">因为该操作`P->I`恰好等同于`(*P).I`，则`Main`方法可能很有效编写：</span><span class="sxs-lookup"><span data-stu-id="9d863-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="9d863-266">指针元素访问</span><span class="sxs-lookup"><span data-stu-id="9d863-266">Pointer element access</span></span>

<span data-ttu-id="9d863-267">一个*pointer_element_access*组成*primary_no_array_creation_expression*括起来的表达式后跟"`[`"和"`]`"。</span><span class="sxs-lookup"><span data-stu-id="9d863-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="9d863-268">在指针元素访问窗体`P[E]`，`P`必须是指针类型的表达式不是`void*`，和`E`必须可以隐式转换为的表达式`int`， `uint`， `long`，或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="9d863-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="9d863-269">指针元素访问窗体`P[E]`评估完全为`*(P + E)`。</span><span class="sxs-lookup"><span data-stu-id="9d863-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="9d863-270">有关说明的指针间接寻址运算符 (`*`)，请参阅[指针间接寻址](unsafe-code.md#pointer-indirection)。</span><span class="sxs-lookup"><span data-stu-id="9d863-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="9d863-271">有关说明的指针加法运算符 (`+`)，请参阅[指针算法](unsafe-code.md#pointer-arithmetic)。</span><span class="sxs-lookup"><span data-stu-id="9d863-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="9d863-272">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="9d863-273">指针元素访问用来初始化中的字符缓冲区`for`循环。</span><span class="sxs-lookup"><span data-stu-id="9d863-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="9d863-274">因为该操作`P[E]`恰好等同于`*(P + E)`，该示例可以很有效编写：</span><span class="sxs-lookup"><span data-stu-id="9d863-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="9d863-275">指针元素访问运算符不检查越界错误和访问时的行为元素未定义超出边界。</span><span class="sxs-lookup"><span data-stu-id="9d863-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="9d863-276">这是与 C 和 c + + 相同。</span><span class="sxs-lookup"><span data-stu-id="9d863-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="9d863-277">Address-of 运算符</span><span class="sxs-lookup"><span data-stu-id="9d863-277">The address-of operator</span></span>

<span data-ttu-id="9d863-278">*Addressof_expression*包含与号 (`&`) 后跟*unary_expression*。</span><span class="sxs-lookup"><span data-stu-id="9d863-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="9d863-279">提供了一个表达式`E`它属于类型`T`归类为固定变量和 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables))，则构造`&E`计算通过给出的变量的地址`E`.</span><span class="sxs-lookup"><span data-stu-id="9d863-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="9d863-280">结果的类型是`T*`和分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="9d863-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="9d863-281">如果会发生编译时错误`E`未归类为变量中，如果`E`归类为只读的本地变量，或者如果`E`表示可移动的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="9d863-282">在最后一种情况下，fixed 语句 ([fixed 的语句](unsafe-code.md#the-fixed-statement)) 可以用于暂时"修复"该变量，再获取其地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="9d863-283">如中所述[成员访问](expressions.md#member-access)、 实例构造函数或静态构造函数的结构或类，它定义外部`readonly`字段中，该字段被视为一个值，而不是变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="9d863-284">在这种情况下，不能采用其地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="9d863-285">同样，不能采用常量的地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="9d863-286">`&`运算符不要求其自变量明确赋值，但以下`&`操作，该运算符应用到的变量被视为发生该操作的执行路径中明确赋值。</span><span class="sxs-lookup"><span data-stu-id="9d863-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="9d863-287">它是变量的程序员以确保正确初始化的责任实际进行这种情况下。</span><span class="sxs-lookup"><span data-stu-id="9d863-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="9d863-288">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="9d863-289">`i` 明确赋值后被视为`&i`操作用于初始化`p`。</span><span class="sxs-lookup"><span data-stu-id="9d863-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="9d863-290">分配给`*p`有效初始化`i`，但包含此初始化是程序员的责任时，会发生任何编译时错误，如果分配的已删除。</span><span class="sxs-lookup"><span data-stu-id="9d863-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="9d863-291">明确赋值规则`&`运算符存在，这样可以避免多余的本地变量的初始化。</span><span class="sxs-lookup"><span data-stu-id="9d863-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="9d863-292">例如，许多外部 Api 会将指向由 API 填充的结构的指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="9d863-293">对此类 Api 的调用通常会传递本地结构变量的地址，并且如果没有规则，多余的初始化结构变量的需要。</span><span class="sxs-lookup"><span data-stu-id="9d863-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="9d863-294">指针递增和递减</span><span class="sxs-lookup"><span data-stu-id="9d863-294">Pointer increment and decrement</span></span>

<span data-ttu-id="9d863-295">在不安全上下文中，`++`并`--`运算符 ([后缀递增和递减运算符](expressions.md#postfix-increment-and-decrement-operators)并[前缀递增和递减运算符](expressions.md#prefix-increment-and-decrement-operators)) 可以应用于指针除之外的所有类型的变量`void*`。</span><span class="sxs-lookup"><span data-stu-id="9d863-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="9d863-296">因此，对于每个指针类型`T*`，隐式定义了以下运算符：</span><span class="sxs-lookup"><span data-stu-id="9d863-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="9d863-297">运算符将产生与相同的结果`x + 1`并`x - 1`分别 ([指针算法](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="9d863-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="9d863-298">换句话说，对于类型的指针变量`T*`，则`++`运算符将添加`sizeof(T)`的变量中包含的地址和`--`运算符减去`sizeof(T)`从变量中包含的地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="9d863-299">如果指针递增或递减运算溢出指针类型的域，则结果是实现定义的但不会产生异常。</span><span class="sxs-lookup"><span data-stu-id="9d863-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="9d863-300">指针算术</span><span class="sxs-lookup"><span data-stu-id="9d863-300">Pointer arithmetic</span></span>

<span data-ttu-id="9d863-301">在不安全上下文中，`+`并`-`运算符 ([加法运算符](expressions.md#addition-operator)并[减法运算符](expressions.md#subtraction-operator)) 可应用于除之外的所有指针类型的值`void*`。</span><span class="sxs-lookup"><span data-stu-id="9d863-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="9d863-302">因此，对于每个指针类型`T*`，隐式定义了以下运算符：</span><span class="sxs-lookup"><span data-stu-id="9d863-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="9d863-303">提供了一个表达式`P`的指针类型`T*`和表达式`N`类型的`int`， `uint`， `long`，或者`ulong`，表达式`P + N`和`N + P`计算类型的指针值`T*`相加得到`N * sizeof(T)`到由给定的地址`P`。</span><span class="sxs-lookup"><span data-stu-id="9d863-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="9d863-304">同样，表达式`P - N`计算类型的指针值`T*`中时得到的减去`N * sizeof(T)`从给定的地址`P`。</span><span class="sxs-lookup"><span data-stu-id="9d863-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="9d863-305">给定的两个表达式之间`P`和`Q`，指针类型的`T*`，该表达式`P - Q`计算给定的地址之间的差异`P`和`Q`然后将该差值除以`sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="9d863-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="9d863-306">结果的类型始终是`long`。</span><span class="sxs-lookup"><span data-stu-id="9d863-306">The type of the result is always `long`.</span></span> <span data-ttu-id="9d863-307">实际上，`P - Q`计算为`((long)(P) - (long)(Q)) / sizeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="9d863-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="9d863-308">例如：</span><span class="sxs-lookup"><span data-stu-id="9d863-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="9d863-309">这会生成输出：</span><span class="sxs-lookup"><span data-stu-id="9d863-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="9d863-310">如果指针算术运算溢出指针类型的域，将结果截断以实现定义的方式，但不会产生异常。</span><span class="sxs-lookup"><span data-stu-id="9d863-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="9d863-311">指针比较</span><span class="sxs-lookup"><span data-stu-id="9d863-311">Pointer comparison</span></span>

<span data-ttu-id="9d863-312">在不安全上下文中， `==`， `!=`， `<`， `>`， `<=`，并且`=>`运算符 ([关系和类型测试运算符](expressions.md#relational-and-type-testing-operators)) 可应用于所有的值指针类型。</span><span class="sxs-lookup"><span data-stu-id="9d863-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="9d863-313">指针比较运算符为：</span><span class="sxs-lookup"><span data-stu-id="9d863-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="9d863-314">因为存在从到任何指针类型的隐式转换`void*`可以使用这些运算符比较类型，任何指针类型的操作数。</span><span class="sxs-lookup"><span data-stu-id="9d863-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="9d863-315">比较运算符比较两个操作数给定就好像这是无符号的整数的地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="9d863-316">Sizeof 运算符</span><span class="sxs-lookup"><span data-stu-id="9d863-316">The sizeof operator</span></span>

<span data-ttu-id="9d863-317">`sizeof`运算符将返回给定类型的变量所占用的字节数。</span><span class="sxs-lookup"><span data-stu-id="9d863-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="9d863-318">指定为的操作数的类型`sizeof`必须是*unmanaged_type* ([指针类型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="9d863-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="9d863-319">结果`sizeof`运算符是类型的值`int`。</span><span class="sxs-lookup"><span data-stu-id="9d863-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="9d863-320">对于某些预定义的类型，`sizeof`运算符产生的常量值下, 表中所示。</span><span class="sxs-lookup"><span data-stu-id="9d863-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="9d863-321">__表达式__</span><span class="sxs-lookup"><span data-stu-id="9d863-321">__Expression__</span></span>   | <span data-ttu-id="9d863-322">__结果__</span><span class="sxs-lookup"><span data-stu-id="9d863-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="9d863-323">对于所有其他类型，结果的`sizeof`运算符是实现定义的和分类为一个值，不是常量。</span><span class="sxs-lookup"><span data-stu-id="9d863-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="9d863-324">未指定成员打包到结构的顺序。</span><span class="sxs-lookup"><span data-stu-id="9d863-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="9d863-325">出于对齐目的，可能有未命名的填充结构内的结构的开头和末尾的结构。</span><span class="sxs-lookup"><span data-stu-id="9d863-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="9d863-326">填充为所使用的位的内容是不确定的。</span><span class="sxs-lookup"><span data-stu-id="9d863-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="9d863-327">应用于具有结构类型的操作数时，结果将是包括所有填充该类型的变量中的字节总数。</span><span class="sxs-lookup"><span data-stu-id="9d863-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="9d863-328">Fixed 的语句</span><span class="sxs-lookup"><span data-stu-id="9d863-328">The fixed statement</span></span>

<span data-ttu-id="9d863-329">在不安全上下文中， *embedded_statement* ([语句](statements.md)) 生产允许的其他构造`fixed`用于"修复"可移动的变量的语句，以便其语句的持续时间内，地址保持不变。</span><span class="sxs-lookup"><span data-stu-id="9d863-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="9d863-330">每个*fixed_pointer_declarator*声明的局部变量给定*pointer_type* ，并使用计算相应的地址初始化该本地变量*fixed_pointer_initializer*。</span><span class="sxs-lookup"><span data-stu-id="9d863-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="9d863-331">声明中的本地变量`fixed`语句是在任何可访问*fixed_pointer_initializer*出现在该变量的声明，并在右侧*embedded_statement*的`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="9d863-332">声明的局部变量`fixed`语句被视为只读的。</span><span class="sxs-lookup"><span data-stu-id="9d863-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="9d863-333">如果嵌入的语句试图修改此本地变量，会发生编译时错误 (通过分配或`++`并`--`运算符) 或将其作为传递`ref`或`out`参数。</span><span class="sxs-lookup"><span data-stu-id="9d863-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="9d863-334">一个*fixed_pointer_initializer*可以是以下之一：</span><span class="sxs-lookup"><span data-stu-id="9d863-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="9d863-335">令牌"`&`"后跟*variable_reference* ([精确规则，用于确定明确的赋值](variables.md#precise-rules-for-determining-definite-assignment)) 为可移动的变量 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables))非托管类型的`T`，前提是类型`T*`隐式转换为指针类型中给出`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="9d863-336">在这种情况下，初始值设定项计算给定变量的地址，而变量将保证的持续时间内保持在固定地址`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="9d863-337">表达式*array_type*非托管类型的元素`T`，前提是类型`T*`隐式转换为指针类型中给出`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="9d863-338">在这种情况下，初始值设定项计算数组中的第一个元素的地址，并保证整个数组的持续时间内保持在固定地址`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="9d863-339">如果数组表达式为 null 或该数组包含零个元素，该初始值设定项计算地址等于零。</span><span class="sxs-lookup"><span data-stu-id="9d863-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="9d863-340">类型的表达式`string`，前提是类型`char*`隐式转换为指针类型中给出`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="9d863-341">在这种情况下，初始值设定项计算的第一个字符在字符串中的地址，并保证整个字符串的持续时间内保持在固定地址`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="9d863-342">行为`fixed`语句是实现定义的字符串表达式为 null 时。</span><span class="sxs-lookup"><span data-stu-id="9d863-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="9d863-343">一个*simple_name*或*member_access*提供固定的大小缓冲区成员的类型为隐式转换为给定指针类型引用的可移动的变量，固定的大小缓冲区成员在`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="9d863-344">在这种情况下，初始值设定项计算第一个元素的固定的大小缓冲区的指针 ([表达式中固定大小的缓冲区](unsafe-code.md#fixed-size-buffers-in-expressions))，和固定的大小缓冲区被保证期间保持在固定地址`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="9d863-345">通过计算每个地址*fixed_pointer_initializer* `fixed`语句可确保由该地址引用该变量不受重定位或的持续时间由垃圾回收器可供使用`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="9d863-346">例如，如果该地址由计算*fixed_pointer_initializer*引用对象的字段或数组实例，元素`fixed`语句可保证包含的对象实例没有重定位或释放该语句的生存期内。</span><span class="sxs-lookup"><span data-stu-id="9d863-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="9d863-347">它是程序员有责任确保通过创建了指针`fixed`语句执行这些语句之后不再存在。</span><span class="sxs-lookup"><span data-stu-id="9d863-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="9d863-348">例如，指针创建的`fixed`语句传递给外部 Api，它是程序员有责任确保 Api 保留这些指针没有内存。</span><span class="sxs-lookup"><span data-stu-id="9d863-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="9d863-349">固定的对象可能会导致的堆碎片 （因为它们不能移动）。</span><span class="sxs-lookup"><span data-stu-id="9d863-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="9d863-350">因此，仅在绝对必要时才应修复对象，然后仅为最短的时间量。</span><span class="sxs-lookup"><span data-stu-id="9d863-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="9d863-351">该示例</span><span class="sxs-lookup"><span data-stu-id="9d863-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="9d863-352">演示的几种用法`fixed`语句。</span><span class="sxs-lookup"><span data-stu-id="9d863-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="9d863-353">第一条语句修补程序，并获取静态字段的地址、 第二个语句修补程序，并获取实例字段的地址和第三个语句修补程序，并获取数组元素的地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="9d863-354">每种情况下起来可能会使用常规错误`&`运算符，因为这些变量都属于可移动的变量。</span><span class="sxs-lookup"><span data-stu-id="9d863-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="9d863-355">第四个`fixed`语句在上面的示例生成到第三个类似的结果。</span><span class="sxs-lookup"><span data-stu-id="9d863-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="9d863-356">此示例中的`fixed`语句使用`string`:</span><span class="sxs-lookup"><span data-stu-id="9d863-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="9d863-357">在不安全的上下文中的一维数组的数组元素存储在不断增加的索引顺序，从索引中`0`，以索引结束`Length - 1`。</span><span class="sxs-lookup"><span data-stu-id="9d863-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="9d863-358">对于多维数组，数组元素的存储，以便首先，增加的最右边的维度索引然后下一步左边的维度，依此类推左侧。</span><span class="sxs-lookup"><span data-stu-id="9d863-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="9d863-359">内`fixed`获取的指针的语句`p`数组实例`a`中，指针值范围从`p`到`p + a.Length - 1`表示数组中元素的地址。</span><span class="sxs-lookup"><span data-stu-id="9d863-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="9d863-360">同样，从范围的变量`p[0]`到`p[a.Length - 1]`表示实际的数组元素。</span><span class="sxs-lookup"><span data-stu-id="9d863-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="9d863-361">给定数组存储的方式，我们可以处理，就好像它是线性的任何维度的数组。</span><span class="sxs-lookup"><span data-stu-id="9d863-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="9d863-362">例如：</span><span class="sxs-lookup"><span data-stu-id="9d863-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="9d863-363">这会生成输出：</span><span class="sxs-lookup"><span data-stu-id="9d863-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="9d863-364">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="9d863-365">`fixed`语句用于修复一个数组，因此，其地址可以传递给采用指针的方法。</span><span class="sxs-lookup"><span data-stu-id="9d863-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="9d863-366">在下面的示例中：</span><span class="sxs-lookup"><span data-stu-id="9d863-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="9d863-367">fixed 的语句用于修复固定的大小缓冲区的结构，因此，其地址可以用作指针。</span><span class="sxs-lookup"><span data-stu-id="9d863-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="9d863-368">一个`char*`产生的固定字符串实例始终指向以 null 结尾的字符串值。</span><span class="sxs-lookup"><span data-stu-id="9d863-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="9d863-369">获取的指针的固定语句中`p`到字符串实例`s`中，指针值范围从`p`到`p + s.Length - 1`表示字符串和指针值中的字符的地址`p + s.Length`始终指向 null 字符 (具有值的字符`'\0'`)。</span><span class="sxs-lookup"><span data-stu-id="9d863-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="9d863-370">修改对象的托管类型通过固定指针会导致未定义的行为。</span><span class="sxs-lookup"><span data-stu-id="9d863-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="9d863-371">例如，由于字符串是不可变的它是程序员的责任，以确保不会修改等于固定字符串的指针所引用的字符。</span><span class="sxs-lookup"><span data-stu-id="9d863-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="9d863-372">调用都应为"C 样式"字符串的外部 Api 时，自动的 null 终止的字符串是特别方便。</span><span class="sxs-lookup"><span data-stu-id="9d863-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="9d863-373">但是，请注意，字符串实例，允许以包含 null 字符。</span><span class="sxs-lookup"><span data-stu-id="9d863-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="9d863-374">如果存在此类为 null 字符，字符串将出现截断，当被视为 null 值结束`char*`。</span><span class="sxs-lookup"><span data-stu-id="9d863-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="9d863-375">固定的大小缓冲区</span><span class="sxs-lookup"><span data-stu-id="9d863-375">Fixed size buffers</span></span>

<span data-ttu-id="9d863-376">固定的大小的缓冲区用于将"C 样式"行中数组声明为成员的结构，并且主要用于与非托管 Api 进行连接。</span><span class="sxs-lookup"><span data-stu-id="9d863-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="9d863-377">固定的大小缓冲区的声明</span><span class="sxs-lookup"><span data-stu-id="9d863-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="9d863-378">一个***固定大小缓冲区***是表示对于给定类型的变量的固定的长度缓冲区的存储空间的成员。</span><span class="sxs-lookup"><span data-stu-id="9d863-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="9d863-379">固定的大小缓冲区声明引入了给定的元素类型的一个或多个固定的大小的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="9d863-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="9d863-380">固定的大小缓冲区只能在结构声明中使用和只能发生在不安全的上下文 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="9d863-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="9d863-381">固定的大小缓冲区声明可能包括一组属性 ([特性](attributes.md))、 一个`new`修饰符 ([修饰符](classes.md#modifiers))，四种访问修饰符的有效组合 ([类型参数和约束](classes.md#type-parameters-and-constraints)) 和一个`unsafe`修饰符 ([不安全的上下文](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="9d863-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="9d863-382">特性和修饰符适用于所有固定的大小缓冲区声明由声明的成员。</span><span class="sxs-lookup"><span data-stu-id="9d863-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="9d863-383">它是同一修饰符在固定的大小缓冲区声明中多次出现的错误的。</span><span class="sxs-lookup"><span data-stu-id="9d863-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="9d863-384">不允许使用固定的大小缓冲区声明以包括`static`修饰符。</span><span class="sxs-lookup"><span data-stu-id="9d863-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="9d863-385">缓冲区元素类型的固定的大小缓冲区声明指定该声明引入的元素类型的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="9d863-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="9d863-386">缓冲区元素类型必须是一个预定义的类型`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`bool`。</span><span class="sxs-lookup"><span data-stu-id="9d863-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="9d863-387">缓冲区元素类型后跟一系列固定的大小缓冲区的声明符，其中每个引入了新的成员。</span><span class="sxs-lookup"><span data-stu-id="9d863-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="9d863-388">固定的大小缓冲区声明符包含名称的成员后, 跟一个常量表达式括起来的标识符`[`和`]`令牌。</span><span class="sxs-lookup"><span data-stu-id="9d863-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="9d863-389">常量表达式表示引入该固定的大小缓冲区声明符的成员中的元素数。</span><span class="sxs-lookup"><span data-stu-id="9d863-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="9d863-390">常量表达式的类型必须是隐式转换为键入`int`，其值必须为非零的正整数。</span><span class="sxs-lookup"><span data-stu-id="9d863-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="9d863-391">固定的大小缓冲区的元素都保证在内存中顺序依次布局。</span><span class="sxs-lookup"><span data-stu-id="9d863-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="9d863-392">声明多个固定的大小缓冲区的固定的大小缓冲区声明是等效于多个具有相同的属性和元素类型的单个固定的大小缓冲区声明的声明。</span><span class="sxs-lookup"><span data-stu-id="9d863-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="9d863-393">例如</span><span class="sxs-lookup"><span data-stu-id="9d863-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="9d863-394">等效于</span><span class="sxs-lookup"><span data-stu-id="9d863-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="9d863-395">在表达式中的固定的大小缓冲区</span><span class="sxs-lookup"><span data-stu-id="9d863-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="9d863-396">成员查找 ([运算符](expressions.md#operators)) 的固定大小缓冲区成员将继续与成员查找字段的完全相同。</span><span class="sxs-lookup"><span data-stu-id="9d863-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="9d863-397">可以在表达式中使用引用的固定的大小缓冲区*simple_name* ([类型推理](expressions.md#type-inference)) 或*member_access* ([编译时检查动态重载决策](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="9d863-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="9d863-398">当固定的大小缓冲区成员作为简单名称引用时，效果等同于窗体的成员访问`this.I`，其中`I`是固定的大小缓冲区成员。</span><span class="sxs-lookup"><span data-stu-id="9d863-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="9d863-399">在窗体的成员访问`E.I`，如果`E`的结构类型和成员的查找`I`，结构类型标识固定的大小成员，然后`E.I`是评估分类，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9d863-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="9d863-400">如果表达式`E.I`不会出现在不安全上下文中，将发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="9d863-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="9d863-401">如果`E`分类为一个值，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="9d863-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="9d863-402">否则为如果`E`是可移动变量 ([固定和可移动变量](unsafe-code.md#fixed-and-moveable-variables)) 和表达式`E.I`不是*fixed_pointer_initializer* ([固定语句](unsafe-code.md#the-fixed-statement))，则发生编译时错误。</span><span class="sxs-lookup"><span data-stu-id="9d863-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="9d863-403">否则为`E`引用为固定的变量和表达式的结果是指向第一个元素的固定的大小缓冲区成员`I`中`E`。</span><span class="sxs-lookup"><span data-stu-id="9d863-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="9d863-404">结果为类型`S*`，其中`S`的元素类型是`I`，和分类为一个值。</span><span class="sxs-lookup"><span data-stu-id="9d863-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="9d863-405">可以使用从第一个元素的指针操作访问的固定的大小缓冲区的后续元素。</span><span class="sxs-lookup"><span data-stu-id="9d863-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="9d863-406">与不同于数组的访问权限，对固定的大小缓冲区的元素的访问是不安全的操作并不是选中的范围。</span><span class="sxs-lookup"><span data-stu-id="9d863-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="9d863-407">下面的示例声明并使用具有固定的大小缓冲区成员的结构。</span><span class="sxs-lookup"><span data-stu-id="9d863-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="9d863-408">明确赋值检查</span><span class="sxs-lookup"><span data-stu-id="9d863-408">Definite assignment checking</span></span>

<span data-ttu-id="9d863-409">固定的大小的缓冲区将不明确的赋值检查 ([明确赋值](variables.md#definite-assignment))，和固定的大小缓冲区成员被检查的结构类型的变量明确赋值的目的。</span><span class="sxs-lookup"><span data-stu-id="9d863-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="9d863-410">静态变量，一个类实例或数组元素的一个实例变量的固定的大小缓冲区成员包含的最外层结构变量时，固定的大小缓冲区的元素将自动初始化为其默认值 ([默认值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="9d863-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="9d863-411">在所有其他情况下，固定的大小缓冲区的初始内容未定义。</span><span class="sxs-lookup"><span data-stu-id="9d863-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="9d863-412">堆栈分配</span><span class="sxs-lookup"><span data-stu-id="9d863-412">Stack allocation</span></span>

<span data-ttu-id="9d863-413">在不安全上下文中，本地变量声明 ([局部变量声明](statements.md#local-variable-declarations)) 可能包括从调用堆栈中分配内存的堆栈分配初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="9d863-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="9d863-414">*Unmanaged_type*指示将在新分配的位置中，存储的项的类型和*表达式*指示这些项的数目。</span><span class="sxs-lookup"><span data-stu-id="9d863-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="9d863-415">合起来看，这些名称指定的所需的分配大小。</span><span class="sxs-lookup"><span data-stu-id="9d863-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="9d863-416">由于堆栈分配的大小不能为负数，它是编译时错误，以指定的项作为数*constant_expression*的计算结果为负值。</span><span class="sxs-lookup"><span data-stu-id="9d863-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="9d863-417">在窗体的堆栈分配初始值设定项`stackalloc T[E]`需要`T`为非托管的类型 ([指针类型](unsafe-code.md#pointer-types)) 和`E`是类型的表达式`int`。</span><span class="sxs-lookup"><span data-stu-id="9d863-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="9d863-418">构造分配`E * sizeof(T)`个字节从调用堆栈，并返回类型的指针`T*`，为新分配的块。</span><span class="sxs-lookup"><span data-stu-id="9d863-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="9d863-419">如果`E`为负值，则该行为不确定。</span><span class="sxs-lookup"><span data-stu-id="9d863-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="9d863-420">如果`E`是零，则不进行任何分配，并且返回的指针是实现定义的。</span><span class="sxs-lookup"><span data-stu-id="9d863-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="9d863-421">如果没有足够内存可用于分配指定大小的块`System.StackOverflowException`引发。</span><span class="sxs-lookup"><span data-stu-id="9d863-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="9d863-422">新分配的内存的内容未定义。</span><span class="sxs-lookup"><span data-stu-id="9d863-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="9d863-423">中不允许使用堆栈分配初始值设定项`catch`或`finally`块 ([try 语句](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="9d863-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="9d863-424">没有方法来显式释放内存分配使用`stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="9d863-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="9d863-425">该函数成员返回时，会自动丢弃函数成员的执行过程中创建的所有堆栈中分配的内存块。</span><span class="sxs-lookup"><span data-stu-id="9d863-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="9d863-426">这对应于`alloca`函数、 C 和 c + + 实现中常见的扩展。</span><span class="sxs-lookup"><span data-stu-id="9d863-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="9d863-427">在示例</span><span class="sxs-lookup"><span data-stu-id="9d863-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="9d863-428">`stackalloc`初始值设定项中使用`IntToString`方法来分配在堆栈上的 16 个字符的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="9d863-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="9d863-429">该方法返回时，缓冲区将自动被丢弃。</span><span class="sxs-lookup"><span data-stu-id="9d863-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="9d863-430">动态内存分配</span><span class="sxs-lookup"><span data-stu-id="9d863-430">Dynamic memory allocation</span></span>

<span data-ttu-id="9d863-431">除`stackalloc`运算符，C# 提供任何预定义的构造管理非垃圾收集的内存。</span><span class="sxs-lookup"><span data-stu-id="9d863-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="9d863-432">通常，此类服务提供的支持类库或直接从基础操作系统导入。</span><span class="sxs-lookup"><span data-stu-id="9d863-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="9d863-433">例如，`Memory`以下类说明了可以如何从 C# 访问基础操作系统的堆函数：</span><span class="sxs-lookup"><span data-stu-id="9d863-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="9d863-434">使用的示例，`Memory`类如下：</span><span class="sxs-lookup"><span data-stu-id="9d863-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="9d863-435">示例分配 256 个字节的内存通过`Memory.Alloc`和增加从 0 到 255 的值初始化内存块。</span><span class="sxs-lookup"><span data-stu-id="9d863-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="9d863-436">然后会分配 256 个元素字节数组，并使用`Memory.Copy`内存块的内容复制到的字节数组。</span><span class="sxs-lookup"><span data-stu-id="9d863-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="9d863-437">最后，释放内存块时使用`Memory.Free`和字节数组的内容可能会在控制台上的输出。</span><span class="sxs-lookup"><span data-stu-id="9d863-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
