---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: ae114131069ca76e4a1ec8149b7bab81fce8965c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2020
ms.locfileid: "84228183"
---

# <a name="records-v2"></a><span data-ttu-id="ffdc1-101">记录 v2</span><span class="sxs-lookup"><span data-stu-id="ffdc1-101">Records v2</span></span>

<span data-ttu-id="ffdc1-102">过去，我们已将记录看作一项功能，以便能够处理数据。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="ffdc1-103">"处理数据" 是包含很多方面的大型组，因此，每个方面都可能会有兴趣。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="ffdc1-104">首先，让我们看一看今天的记录及其一些缺点。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="ffdc1-105">例如，按如下所示定义一条简单记录</span><span class="sxs-lookup"><span data-stu-id="ffdc1-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="ffdc1-106">使用情况将读取</span><span class="sxs-lookup"><span data-stu-id="ffdc1-106">and the usage would read</span></span>

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

<span data-ttu-id="ffdc1-107">此代码有明显的优点：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="ffdc1-108">定义是版本复原的，可以轻松地添加或移动属性</span><span class="sxs-lookup"><span data-stu-id="ffdc1-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="ffdc1-109">可以按任意顺序设置属性，并且初始化中的名称始终与访问器匹配</span><span class="sxs-lookup"><span data-stu-id="ffdc1-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="ffdc1-110">只能跳过具有默认值的属性</span><span class="sxs-lookup"><span data-stu-id="ffdc1-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="ffdc1-111">第一个缺陷是属性现在必须是可变的。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="ffdc1-112">可变性</span><span class="sxs-lookup"><span data-stu-id="ffdc1-112">Mutability</span></span>

<span data-ttu-id="ffdc1-113">对于 c #，我们想要提供一种方法来设置 `readonly` 对象初始值设定项中的成员。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="ffdc1-114">由于某些类型可能尚未设计此初始化，因此，我们也喜欢选择加入。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="ffdc1-115">建议的解决方案是新的修饰符， `initonly` 可应用于属性和字段：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="ffdc1-116">这是 codegen 的：我们只是设置了只读字段。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="ffdc1-117">具体而言，降低的属性如下所示：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-117">Specifically, the lowered properties would look like:</span></span>

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

<span data-ttu-id="ffdc1-118">CLR 认为将 readonly 字段设置为不可验证，但不是安全的。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="ffdc1-119">若要支持更高级的验证程序，建议使用以下规则：只读字段只能在方法内修改 `initonly` ，或在 CLR 堆栈上的新对象上进行修改，并且尚未通过存储或方法调用发布。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="ffdc1-120">这应该解决示例中可变性的许多问题 `UserInfo` ，而不需要复杂的或脆弱的发出策略。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="ffdc1-121">但这种情况不会带来新的问题：轻松地使用更改构造对象。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="ffdc1-122">与-ing</span><span class="sxs-lookup"><span data-stu-id="ffdc1-122">With-ing</span></span>

<span data-ttu-id="ffdc1-123">使用不永久性进行编程时，通过使用更改构造副本来完成对对象的更改，而不是直接对对象进行更改。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="ffdc1-124">遗憾的是，即使对于当前样式的记录，也不能方便地在 c # 中执行此操作。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="ffdc1-125">之前已建议为实现该功能的记录提供某种类型的自动生成方法。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="ffdc1-126">如果有这样一种机制，则必须与成员一起使用 `initonly` 。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="ffdc1-127">为此，建议添加一个 `with` 表达式，这与对象初始值设定项类似。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="ffdc1-128">示例用法如下所示：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="ffdc1-129">生成的 `newUserName` 对象将是的副本 `userInfo` ，并 `Username` 将设置为 "angocke"。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="ffdc1-130">表达式上的 codegen `with` 也与对象初始值设定项类似：构造一个新的对象，然后 `initonly` `Username` 在方法体中调用 setter。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="ffdc1-131">当然，两者的区别在于，要构造的新对象不是简单的新对象创建，它是原始对象的副本。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="ffdc1-132">为了提供此功能，我们要求对象提供提供重复对象的 "With 构造函数"。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="ffdc1-133">示例 `With` 构造函数应如下所示：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-133">A sample `With` constructor would look like:</span></span>

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

<span data-ttu-id="ffdc1-134">特别要注意的 `with` 是，表达式将设置 `initonly` 成员，与对象初始值设定项一样，因此，为了支持验证，必须确保在设置成员之前不能发布该对象 `initonly` 。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="ffdc1-135">若要强制执行此 `WithConstructor` 操作，属性（或等效语法）将对方法强制实施新规则：所有 return 语句都必须直接包含对象创建表达式，可能使用对象初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="ffdc1-136">如果 `With` 构造函数要求验证，则用户可能会引入用于执行该验证的构造函数，例如</span><span class="sxs-lookup"><span data-stu-id="ffdc1-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

<span data-ttu-id="ffdc1-137">与关联的最后一种复杂性 `With` 是继承。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="ffdc1-138">如果记录可扩展，则需要为子类提供新 `With` 的。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="ffdc1-139">可按如下所示实现此目的：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-139">This can be achieved as follows:</span></span>

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

<span data-ttu-id="ffdc1-140">请注意，此处有一个额外的复杂性：为了 `With` 用派生类型重写构造函数，语言还需要支持重写中的协变返回类型。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="ffdc1-141">[此处](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)已经有了此功能的单独建议。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="ffdc1-142">**弊端**</span><span class="sxs-lookup"><span data-stu-id="ffdc1-142">**Drawbacks**</span></span>

- <span data-ttu-id="ffdc1-143">使中的所有 return 语句 `WithConstructor` 包含新的对象表达式具有限制性。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="ffdc1-144">这可能会被流分析缓解，这可确保新对象不会转义方法</span><span class="sxs-lookup"><span data-stu-id="ffdc1-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="ffdc1-145">通过编译器技巧支持重写中的变体将需要存根方法，这会将几率与继承深度一起增长。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="ffdc1-146">存根方法的需要是因为运行时要求覆盖签名完全匹配。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="ffdc1-147">如果运行时要求放松，则根本不需要存根方法。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="ffdc1-148">使用窗体的链接构造函数可以 `Type(Type original)` 有效地保留该构造函数，以供使用。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="ffdc1-149">由于构造函数具有唯一语义并且不能重新命名，因此这可能会受到限制。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="ffdc1-150">全部包装：记录</span><span class="sxs-lookup"><span data-stu-id="ffdc1-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="ffdc1-151">上述功能实现了之前非常困难的编程样式。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="ffdc1-152">但即使是在新功能中，它也可能会非常冗长，并容易出错。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="ffdc1-153">还有一些项（如 Equals 和 GetHashCode）现在已经编写，只是太费力了。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="ffdc1-154">此外，在这两个新基元之上实现相等性的一个重要缺陷是，结构相等性是指在添加新数据时，应随数据类型一起更改的内容，但当手动处理数据时，可能会导致这些内容不同步。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="ffdc1-155">因此，建议使用 c # 来支持记录的新语法，而不是提供新功能，而是设置默认值和生成设计为在记录中使用的代码。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="ffdc1-156">示例语法如下所示</span><span class="sxs-lookup"><span data-stu-id="ffdc1-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="ffdc1-157">此类生成的代码会将所有公共字段和自动属性视为记录的结构成员。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="ffdc1-158">可以使用可 `RecordMember(bool)` 用于包括或排除成员的新属性自定义记录成员。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="ffdc1-159">`initonly`默认情况下，记录成员将基于记录成员自动生成类。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="ffdc1-160">在任何时候，只需在源中声明这些成员的行为即可进行自定义。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="ffdc1-161">用户编写的实现将替换所有模式用法中的默认实现。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="ffdc1-162">请注意，继承面临的相等性是很复杂的，但似乎已在[其他记录建议](records.md)中充分解决。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="ffdc1-163">主构造函数</span><span class="sxs-lookup"><span data-stu-id="ffdc1-163">Primary constructors</span></span>

<span data-ttu-id="ffdc1-164">上一条记录提议还为类型本身中的参数列表提供了一个新语法，例如</span><span class="sxs-lookup"><span data-stu-id="ffdc1-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="ffdc1-165">在新设计中，参数列表为正交 c # 功能，可与记录完全集成。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="ffdc1-166">如果记录中包含了主构造函数，则它将具有新的默认值，就像公共字段和自动属性一样：主构造函数中的参数将用于生成具有相同名称的公共记录成员属性。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="ffdc1-167">此外，主构造函数现在可用于自动生成 deconstructor。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="ffdc1-168">例如，具有主构造函数的以下记录</span><span class="sxs-lookup"><span data-stu-id="ffdc1-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="ffdc1-169">将等效于</span><span class="sxs-lookup"><span data-stu-id="ffdc1-169">would be equivalent to</span></span>

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

<span data-ttu-id="ffdc1-170">最后一代将是</span><span class="sxs-lookup"><span data-stu-id="ffdc1-170">and the final generation of the above would be</span></span>

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

<span data-ttu-id="ffdc1-171">请注意，我们使用主构造函数为数据类创建了一个其他信息片段：而不是在生成的受保护的构造函数中设置主字段，而是委托给主构造函数。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="ffdc1-172">如果 Point 类有其他非主要记录成员，例如</span><span class="sxs-lookup"><span data-stu-id="ffdc1-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="ffdc1-173">然后，会更改生成的受保护的构造函数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-173">then that would change the generated protected constructor as follows:</span></span>

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

<span data-ttu-id="ffdc1-174">特别要注意的是，这并不回答与主构造函数的记录继承相关的操作。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="ffdc1-175">例如，</span><span class="sxs-lookup"><span data-stu-id="ffdc1-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="ffdc1-176">更明确的方法可能要求使用基列表提供参数列表，而不是采用任意方式解析，例如</span><span class="sxs-lookup"><span data-stu-id="ffdc1-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="ffdc1-177">然后，基列表中的参数列表将应用到生成的 `base` 主构造函数中的调用：</span><span class="sxs-lookup"><span data-stu-id="ffdc1-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="ffdc1-178">对于主构造函数在记录外的含义，仍可以进一步提出建议。</span><span class="sxs-lookup"><span data-stu-id="ffdc1-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
