---
ms.openlocfilehash: e29f453eb33e06fb3c84fb542d8b74223af9b975
ms.sourcegitcommit: 7f0c8e4eac7afe75e4f312f54a554614384cd06b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80870977"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="5f75b-101">记录正在进行的工作</span><span class="sxs-lookup"><span data-stu-id="5f75b-101">Records Work-in-Progress</span></span>

<span data-ttu-id="5f75b-102">与其他记录建议不同，这不是建议本身，而是旨在记录记录功能的协商一致设计决策的正在进行的工作。</span><span class="sxs-lookup"><span data-stu-id="5f75b-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="5f75b-103">作为解决问题所必需的，将添加规范详细信息。</span><span class="sxs-lookup"><span data-stu-id="5f75b-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="5f75b-104">建议添加记录的语法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5f75b-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="5f75b-105">非`attributes`终端还将允许新的上下文属性`data`。</span><span class="sxs-lookup"><span data-stu-id="5f75b-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="5f75b-106">使用参数列表或`data`修改器声明的类（结构）称为记录类（记录结构），其中任一为记录类型。</span><span class="sxs-lookup"><span data-stu-id="5f75b-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="5f75b-107">在没有参数列表和`data`修改器的情况下声明记录类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="5f75b-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="5f75b-108">记录类型的成员</span><span class="sxs-lookup"><span data-stu-id="5f75b-108">Members of a record type</span></span>

<span data-ttu-id="5f75b-109">除了在类或结构正文中声明的成员外，记录类型还有以下其他成员：</span><span class="sxs-lookup"><span data-stu-id="5f75b-109">In addition to the members declared in the class or struct body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="5f75b-110">主构造函数</span><span class="sxs-lookup"><span data-stu-id="5f75b-110">Primary Constructor</span></span>

<span data-ttu-id="5f75b-111">记录类型具有公共构造函数，其签名对应于类型声明的值参数。</span><span class="sxs-lookup"><span data-stu-id="5f75b-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="5f75b-112">这称为类型的主构造函数，并导致隐式声明的默认类构造函数（如果存在）被抑制。</span><span class="sxs-lookup"><span data-stu-id="5f75b-112">This is called the primary constructor for the type, and causes the implicitly declared default class constructor, if present, to be suppressed.</span></span> <span data-ttu-id="5f75b-113">在类中具有具有相同签名的主构造函数和构造函数是错误的。</span><span class="sxs-lookup"><span data-stu-id="5f75b-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>

<span data-ttu-id="5f75b-114">在运行时，主构造函数</span><span class="sxs-lookup"><span data-stu-id="5f75b-114">At runtime the primary constructor</span></span>

1. <span data-ttu-id="5f75b-115">执行出现在类正文中的实例字段初始化器;然后调用没有参数的基类构造函数。</span><span class="sxs-lookup"><span data-stu-id="5f75b-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="5f75b-116">初始化与值参数对应的属性的编译器生成的支持字段（如果这些属性是编译器提供的</span><span class="sxs-lookup"><span data-stu-id="5f75b-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided</span></span>

### <a name="properties"></a><span data-ttu-id="5f75b-117">属性</span><span class="sxs-lookup"><span data-stu-id="5f75b-117">Properties</span></span>

<span data-ttu-id="5f75b-118">对于记录类型声明的每个记录参数，都有一个相应的公共属性成员，其名称和类型取自值参数声明。</span><span class="sxs-lookup"><span data-stu-id="5f75b-118">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="5f75b-119">如果没有具有 get 访问器且具有此名称和类型的具体属性（即非抽象）属性显式声明或继承，则编译器会按如下方式生成该属性：</span><span class="sxs-lookup"><span data-stu-id="5f75b-119">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="5f75b-120">对于记录结构或记录类：</span><span class="sxs-lookup"><span data-stu-id="5f75b-120">For a record struct or a record class:</span></span>

* <span data-ttu-id="5f75b-121">将创建公共仅获取自动属性。</span><span class="sxs-lookup"><span data-stu-id="5f75b-121">A public get-only auto-property is created.</span></span> <span data-ttu-id="5f75b-122">其值在构造过程中用相应主构造函数参数的值进行初始化。</span><span class="sxs-lookup"><span data-stu-id="5f75b-122">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="5f75b-123">每个"匹配"继承的抽象属性的获取访问器将被覆盖。</span><span class="sxs-lookup"><span data-stu-id="5f75b-123">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="5f75b-124">平等成员</span><span class="sxs-lookup"><span data-stu-id="5f75b-124">Equality members</span></span>

<span data-ttu-id="5f75b-125">记录类型为以下方法生成合成实现：</span><span class="sxs-lookup"><span data-stu-id="5f75b-125">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="5f75b-126">`object.GetHashCode()`覆盖，除非它是密封的或用户提供的</span><span class="sxs-lookup"><span data-stu-id="5f75b-126">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="5f75b-127">`object.Equals(object)`覆盖，除非它是密封的或用户提供的</span><span class="sxs-lookup"><span data-stu-id="5f75b-127">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="5f75b-128">`T Equals(T)`方法，当前`T`类型在哪里</span><span class="sxs-lookup"><span data-stu-id="5f75b-128">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="5f75b-129">`T Equals(T)`指定执行值相等性，将具有相同名称的属性与每个主构造函数参数与其他类型的相应属性进行比较。</span><span class="sxs-lookup"><span data-stu-id="5f75b-129">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="5f75b-130">`object.Equals`执行等效的</span><span class="sxs-lookup"><span data-stu-id="5f75b-130">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="5f75b-131">`with` 表达式</span><span class="sxs-lookup"><span data-stu-id="5f75b-131">`with` expression</span></span>

<span data-ttu-id="5f75b-132">表达式`with`是使用以下语法的新表达式。</span><span class="sxs-lookup"><span data-stu-id="5f75b-132">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="5f75b-133">表达式`with`允许"非破坏性突变"，旨在生成接收方表达式的副本，并修改 中列出的属性`anonymous_object_initializer`。</span><span class="sxs-lookup"><span data-stu-id="5f75b-133">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="5f75b-134">有效的`with`表达式具有非 void 类型的接收器。</span><span class="sxs-lookup"><span data-stu-id="5f75b-134">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="5f75b-135">接收方类型必须包含使用适当的参数和返回`With`类型调用的可访问实例方法。</span><span class="sxs-lookup"><span data-stu-id="5f75b-135">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="5f75b-136">如果存在多个非重写`With`方法，则为错误。</span><span class="sxs-lookup"><span data-stu-id="5f75b-136">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="5f75b-137">如果有多个`With`重写，则必须有一个非重写`With`方法，即目标方法。</span><span class="sxs-lookup"><span data-stu-id="5f75b-137">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="5f75b-138">否则，必须有一种方法`With`。</span><span class="sxs-lookup"><span data-stu-id="5f75b-138">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="5f75b-139">`with`表达式的右侧是一个`anonymous_object_initializer`赋值序列，赋值左侧具有接收方的字段或属性，右侧的任意表达式隐式转换为左侧的类型。</span><span class="sxs-lookup"><span data-stu-id="5f75b-139">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="5f75b-140">给定目标`With`方法，返回类型必须是接收方表达式类型的类型或其基类型。</span><span class="sxs-lookup"><span data-stu-id="5f75b-140">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="5f75b-141">对于方法的每个参数，`With`必须具有具有相同名称和相同类型的接收方类型上的可访问对应实例字段或可读属性。</span><span class="sxs-lookup"><span data-stu-id="5f75b-141">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="5f75b-142">"给"表达式右侧的每个属性或字段还必须对应于`With`方法中同名的参数。</span><span class="sxs-lookup"><span data-stu-id="5f75b-142">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="5f75b-143">给定一个`With`有效方法，`with`表达式的计算等效于调用`With`方法，用 中的表达式`anonymous_object_initializer`替换与左侧的属性同名的参数。</span><span class="sxs-lookup"><span data-stu-id="5f75b-143">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="5f75b-144">如果 中给定参数`anonymous_object_initializer`没有匹配属性，则参数是计算接收器上同名的字段或属性。</span><span class="sxs-lookup"><span data-stu-id="5f75b-144">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="5f75b-145">副作用评估的顺序如下，每个表达式精确计算一次：</span><span class="sxs-lookup"><span data-stu-id="5f75b-145">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="5f75b-146">接收方表达式</span><span class="sxs-lookup"><span data-stu-id="5f75b-146">Receiver expression</span></span>

2. <span data-ttu-id="5f75b-147">中的`anonymous_object_initializer`表达式，按词法顺序排列</span><span class="sxs-lookup"><span data-stu-id="5f75b-147">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="5f75b-148">按`With``With`方法参数定义顺序计算与方法参数匹配的任何属性。</span><span class="sxs-lookup"><span data-stu-id="5f75b-148">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>
