---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281952"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="3a98d-101">记录正在进行的工作</span><span class="sxs-lookup"><span data-stu-id="3a98d-101">Records Work-in-Progress</span></span>

<span data-ttu-id="3a98d-102">不同于其他记录建议，这不是一项提议，而是一项工作，用于记录有关记录功能的共识设计决策。</span><span class="sxs-lookup"><span data-stu-id="3a98d-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="3a98d-103">为了解决问题，将根据需要添加规范详细信息。</span><span class="sxs-lookup"><span data-stu-id="3a98d-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="3a98d-104">建议添加记录的语法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3a98d-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="3a98d-105">`attributes` 非终端还将允许 `data`的新上下文属性。</span><span class="sxs-lookup"><span data-stu-id="3a98d-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="3a98d-106">用参数列表或 `data` 修饰符声明的类（结构）称为记录类（record struct），其中的任何一个是记录类型。</span><span class="sxs-lookup"><span data-stu-id="3a98d-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="3a98d-107">声明不包含参数列表和 `data` 修饰符的记录类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="3a98d-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="3a98d-108">记录类型的成员</span><span class="sxs-lookup"><span data-stu-id="3a98d-108">Members of a record type</span></span>

<span data-ttu-id="3a98d-109">除了在类体中声明的成员以外，记录类型还具有下列附加成员：</span><span class="sxs-lookup"><span data-stu-id="3a98d-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="3a98d-110">主构造函数</span><span class="sxs-lookup"><span data-stu-id="3a98d-110">Primary Constructor</span></span>

<span data-ttu-id="3a98d-111">记录类型具有公共构造函数，该构造函数的签名对应于类型声明的值参数。</span><span class="sxs-lookup"><span data-stu-id="3a98d-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="3a98d-112">这称为类型的主构造函数，并导致禁止显示隐式声明的默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="3a98d-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="3a98d-113">类中已存在具有相同签名的主构造函数和构造函数是错误的。</span><span class="sxs-lookup"><span data-stu-id="3a98d-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="3a98d-114">在运行时，主构造函数</span><span class="sxs-lookup"><span data-stu-id="3a98d-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="3a98d-115">执行出现在类体中的实例字段初始值设定项;然后调用不带参数的基类构造函数。</span><span class="sxs-lookup"><span data-stu-id="3a98d-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="3a98d-116">为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的属性，请参阅[合成属性](#Synthesized Properties)）</span><span class="sxs-lookup"><span data-stu-id="3a98d-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="3a98d-117">[] TODO：添加基本调用语法和有关通过重载决策选择基构造函数的规范</span><span class="sxs-lookup"><span data-stu-id="3a98d-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="3a98d-118">属性</span><span class="sxs-lookup"><span data-stu-id="3a98d-118">Properties</span></span>

<span data-ttu-id="3a98d-119">对于记录类型声明的每个记录参数，都有一个对应的公共属性成员，其名称和类型取自值参数声明。</span><span class="sxs-lookup"><span data-stu-id="3a98d-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="3a98d-120">如果没有显式声明或继承具有 get 访问器和此名称和类型的具体（即非抽象）属性，则编译器将生成，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3a98d-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="3a98d-121">对于记录结构或记录类：</span><span class="sxs-lookup"><span data-stu-id="3a98d-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="3a98d-122">创建一个公共的仅获取自动属性。</span><span class="sxs-lookup"><span data-stu-id="3a98d-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="3a98d-123">在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。</span><span class="sxs-lookup"><span data-stu-id="3a98d-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="3a98d-124">将重写每个 "匹配" 继承抽象属性的 get 访问器。</span><span class="sxs-lookup"><span data-stu-id="3a98d-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="3a98d-125">相等成员</span><span class="sxs-lookup"><span data-stu-id="3a98d-125">Equality members</span></span>

<span data-ttu-id="3a98d-126">记录类型为以下方法生成合成实现：</span><span class="sxs-lookup"><span data-stu-id="3a98d-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="3a98d-127">除非已密封或用户提供，否则 `object.GetHashCode()` 重写</span><span class="sxs-lookup"><span data-stu-id="3a98d-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="3a98d-128">除非已密封或用户提供，否则 `object.Equals(object)` 重写</span><span class="sxs-lookup"><span data-stu-id="3a98d-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="3a98d-129">`T Equals(T)` 方法，其中 `T` 是当前类型</span><span class="sxs-lookup"><span data-stu-id="3a98d-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="3a98d-130">指定 `T Equals(T)` 以执行值相等性，将与每个主构造函数参数相同的属性与其他类型的相应属性进行比较。</span><span class="sxs-lookup"><span data-stu-id="3a98d-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="3a98d-131">`object.Equals` 执行等效操作</span><span class="sxs-lookup"><span data-stu-id="3a98d-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="3a98d-132">`with` 表达式</span><span class="sxs-lookup"><span data-stu-id="3a98d-132">`with` expression</span></span>

<span data-ttu-id="3a98d-133">`with` 表达式是使用以下语法的新表达式。</span><span class="sxs-lookup"><span data-stu-id="3a98d-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="3a98d-134">`with` 表达式可实现 "非破坏性转变"，旨在生成接收方表达式副本，并对 `anonymous_object_initializer`中列出的属性进行修改。</span><span class="sxs-lookup"><span data-stu-id="3a98d-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="3a98d-135">有效的 `with` 表达式具有非 void 类型的接收方。</span><span class="sxs-lookup"><span data-stu-id="3a98d-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="3a98d-136">接收方类型必须包含一个名为 `With` 的可访问实例方法，该方法具有适当的参数和返回类型。</span><span class="sxs-lookup"><span data-stu-id="3a98d-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="3a98d-137">如果有多个非重写 `With` 方法，则是错误的。</span><span class="sxs-lookup"><span data-stu-id="3a98d-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="3a98d-138">如果有多个 `With` 重写，则必须有一个非重写 `With` 方法，该方法是目标方法。</span><span class="sxs-lookup"><span data-stu-id="3a98d-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="3a98d-139">否则，必须只有一个 `With` 方法。</span><span class="sxs-lookup"><span data-stu-id="3a98d-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="3a98d-140">`with` 表达式的右侧是一个具有一系列赋值的 `anonymous_object_initializer`，该分配的左侧带有接收方的一个字段或属性，而右侧的任意表达式都可以隐式转换为左侧的类型，这是一个可隐式转换的类型。</span><span class="sxs-lookup"><span data-stu-id="3a98d-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="3a98d-141">给定目标 `With` 方法时，返回类型必须是接收方表达式类型的类型或其基类型。</span><span class="sxs-lookup"><span data-stu-id="3a98d-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="3a98d-142">对于 `With` 方法的每个参数，对于具有相同名称和相同类型的接收器类型，必须具有可访问的对应实例字段或可读属性。</span><span class="sxs-lookup"><span data-stu-id="3a98d-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="3a98d-143">With 表达式的右侧的每个属性或字段也必须对应于 `With` 方法中具有相同名称的参数。</span><span class="sxs-lookup"><span data-stu-id="3a98d-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="3a98d-144">给定有效 `With` 方法，`with` 表达式的计算等效于使用 `anonymous_object_initializer` 中的表达式调用 `With` 方法，该方法将替换为与左侧属性相同的参数。</span><span class="sxs-lookup"><span data-stu-id="3a98d-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="3a98d-145">如果 `anonymous_object_initializer`中给定参数没有匹配的属性，则该参数是对接收方上的相同名称的字段或属性的计算。</span><span class="sxs-lookup"><span data-stu-id="3a98d-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="3a98d-146">副作用的计算顺序如下所示，每个表达式只计算一次：</span><span class="sxs-lookup"><span data-stu-id="3a98d-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="3a98d-147">接收器表达式</span><span class="sxs-lookup"><span data-stu-id="3a98d-147">Receiver expression</span></span>

2. <span data-ttu-id="3a98d-148">`anonymous_object_initializer`中的表达式，按词法顺序排列</span><span class="sxs-lookup"><span data-stu-id="3a98d-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="3a98d-149">与 `With` 方法参数匹配的任何属性的计算，按 `With` 方法参数的定义顺序进行。</span><span class="sxs-lookup"><span data-stu-id="3a98d-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>