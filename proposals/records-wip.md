---
ms.openlocfilehash: 32682010c79073aabbc71ef9de433a7a76138c3f
ms.sourcegitcommit: c30039481ee8a75c3b3e4ddd369fdf8f84f8945b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596752"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="94fed-101">记录正在进行的工作</span><span class="sxs-lookup"><span data-stu-id="94fed-101">Records Work-in-Progress</span></span>

<span data-ttu-id="94fed-102">不同于其他记录建议，这不是一项提议，而是一项工作，用于记录有关记录功能的共识设计决策。</span><span class="sxs-lookup"><span data-stu-id="94fed-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="94fed-103">为了解决问题，将根据需要添加规范详细信息。</span><span class="sxs-lookup"><span data-stu-id="94fed-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="94fed-104">建议添加记录的语法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="94fed-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="94fed-105">`attributes`非终端还将允许新的上下文属性`data`。</span><span class="sxs-lookup"><span data-stu-id="94fed-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="94fed-106">用参数列表或`data`修饰符声明的类（结构）称为记录类（record struct），其中的任何一个是记录类型。</span><span class="sxs-lookup"><span data-stu-id="94fed-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="94fed-107">声明不包含参数列表和`data`修饰符的记录类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="94fed-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="94fed-108">记录类型的成员</span><span class="sxs-lookup"><span data-stu-id="94fed-108">Members of a record type</span></span>

<span data-ttu-id="94fed-109">除了在类或结构体中声明的成员以外，记录类型还具有下列附加成员：</span><span class="sxs-lookup"><span data-stu-id="94fed-109">In addition to the members declared in the class or struct body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="94fed-110">主构造函数</span><span class="sxs-lookup"><span data-stu-id="94fed-110">Primary Constructor</span></span>

<span data-ttu-id="94fed-111">记录类型具有公共构造函数，该构造函数的签名对应于类型声明的值参数。</span><span class="sxs-lookup"><span data-stu-id="94fed-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="94fed-112">这称为类型的主构造函数，并导致禁止显示隐式声明的默认类构造函数（如果存在）。</span><span class="sxs-lookup"><span data-stu-id="94fed-112">This is called the primary constructor for the type, and causes the implicitly declared default class constructor, if present, to be suppressed.</span></span> <span data-ttu-id="94fed-113">类中已存在具有相同签名的主构造函数和构造函数是错误的。</span><span class="sxs-lookup"><span data-stu-id="94fed-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>

<span data-ttu-id="94fed-114">在运行时，主构造函数</span><span class="sxs-lookup"><span data-stu-id="94fed-114">At runtime the primary constructor</span></span>

1. <span data-ttu-id="94fed-115">执行出现在类体中的实例字段初始值设定项;然后调用不带参数的基类构造函数。</span><span class="sxs-lookup"><span data-stu-id="94fed-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="94fed-116">为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的）</span><span class="sxs-lookup"><span data-stu-id="94fed-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided</span></span>

### <a name="properties"></a><span data-ttu-id="94fed-117">属性</span><span class="sxs-lookup"><span data-stu-id="94fed-117">Properties</span></span>

<span data-ttu-id="94fed-118">对于记录类型声明的每个记录参数，都有一个对应的公共属性成员，其名称和类型取自值参数声明。</span><span class="sxs-lookup"><span data-stu-id="94fed-118">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="94fed-119">如果没有显式声明或继承具有 get 访问器和此名称和类型的具体（即非抽象）属性，则编译器将生成，如下所示：</span><span class="sxs-lookup"><span data-stu-id="94fed-119">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="94fed-120">对于记录结构或记录类：</span><span class="sxs-lookup"><span data-stu-id="94fed-120">For a record struct or a record class:</span></span>

* <span data-ttu-id="94fed-121">创建一个公共的仅获取自动属性。</span><span class="sxs-lookup"><span data-stu-id="94fed-121">A public get-only auto-property is created.</span></span> <span data-ttu-id="94fed-122">在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。</span><span class="sxs-lookup"><span data-stu-id="94fed-122">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="94fed-123">将重写每个 "匹配" 继承抽象属性的 get 访问器。</span><span class="sxs-lookup"><span data-stu-id="94fed-123">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

  * <span data-ttu-id="94fed-124">此属性也`initonly`是，这意味着可以在下面的表达式中修改支持字段，前提是可以访问`get` [相应的访问](#With)器</span><span class="sxs-lookup"><span data-stu-id="94fed-124">This property is also `initonly`, meaning the backing field can be modified in the [with](#With) expression below, if the corresponding `get` accessor is accessible</span></span>

### <a name="equality-members"></a><span data-ttu-id="94fed-125">相等成员</span><span class="sxs-lookup"><span data-stu-id="94fed-125">Equality members</span></span>

<span data-ttu-id="94fed-126">记录类型为以下方法生成合成实现：</span><span class="sxs-lookup"><span data-stu-id="94fed-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="94fed-127">`object.GetHashCode()`重写（除非它是密封的或用户提供的）</span><span class="sxs-lookup"><span data-stu-id="94fed-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="94fed-128">`object.Equals(object)`重写（除非它是密封的或用户提供的）</span><span class="sxs-lookup"><span data-stu-id="94fed-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="94fed-129">`T Equals(T)`方法，其中`T`是当前类型</span><span class="sxs-lookup"><span data-stu-id="94fed-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="94fed-130">`T Equals(T)`指定为执行值相等性，将与每个主构造函数参数相同的属性与其他类型的相应属性进行比较。</span><span class="sxs-lookup"><span data-stu-id="94fed-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="94fed-131">`object.Equals`执行等效于</span><span class="sxs-lookup"><span data-stu-id="94fed-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="94fed-132">`with` 表达式</span><span class="sxs-lookup"><span data-stu-id="94fed-132">`with` expression</span></span>

<span data-ttu-id="94fed-133">`with`表达式是使用以下语法的新表达式。</span><span class="sxs-lookup"><span data-stu-id="94fed-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="94fed-134">`with`表达式允许 "非破坏性转变"，旨在生成接收方表达式的副本，并对中列出的`anonymous_object_initializer`属性进行修改。</span><span class="sxs-lookup"><span data-stu-id="94fed-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="94fed-135">有效`with`的表达式包含具有非 void 类型的接收方。</span><span class="sxs-lookup"><span data-stu-id="94fed-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="94fed-136">接收器类型必须包含一个名`Clone`为的可访问参数实例方法，该方法的返回类型必须是接收方类型或其基类型。</span><span class="sxs-lookup"><span data-stu-id="94fed-136">The receiver type must contain an accessible parameterless instance method called `Clone` whose return type must be the receiver type, or a base type thereof.</span></span>

<span data-ttu-id="94fed-137">`with`表达式的右侧是`anonymous_object_initializer`一个具有赋值序列的，每个都具有赋值的左侧的`initonly` `Clone`返回类型（作为属性调用）的属性（请参见[属性](#Properties)），以及右侧的任意表达式，该表达式可隐式转换为属性的类型。</span><span class="sxs-lookup"><span data-stu-id="94fed-137">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments, each with an `initonly` property (see [Properties](#Properties)) of the `Clone` return type on the left-hand side of the assignment (as a property invocation), and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the property.</span></span>

<span data-ttu-id="94fed-138">`with`表达式的计算只调用`Clone`方法一次，然后使用`Clone`方法调用的结果作为接收方， `initonly`将参数列表中每个属性的支持字段按词法顺序设置为其对应的表达式的值。</span><span class="sxs-lookup"><span data-stu-id="94fed-138">The evaluation of a `with` expression calls the `Clone` method exactly once, and then sets the backing field of each `initonly` property in the argument list to the value of its corresponding expression, in lexical order, using the result of the `Clone` method invocation as the receiver.</span></span> <span data-ttu-id="94fed-139">`with`表达式的类型与该`Clone`方法的返回类型相同。</span><span class="sxs-lookup"><span data-stu-id="94fed-139">The type of the `with` expression is the same as the return type of the `Clone` method.</span></span>

