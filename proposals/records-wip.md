---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/01/2020
ms.locfileid: "79484128"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="83356-101">记录正在进行的工作</span><span class="sxs-lookup"><span data-stu-id="83356-101">Records Work-in-Progress</span></span>

<span data-ttu-id="83356-102">不同于其他记录建议，这不是一项提议，而是一项工作，用于记录有关记录功能的共识设计决策。</span><span class="sxs-lookup"><span data-stu-id="83356-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="83356-103">为了解决问题，将根据需要添加规范详细信息。</span><span class="sxs-lookup"><span data-stu-id="83356-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="83356-104">建议添加记录的语法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="83356-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="83356-105">`attributes` 非终端还将允许 `data`的新上下文属性。</span><span class="sxs-lookup"><span data-stu-id="83356-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="83356-106">用参数列表或 `data` 修饰符声明的类（结构）称为记录类（record struct），其中的任何一个是记录类型。</span><span class="sxs-lookup"><span data-stu-id="83356-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="83356-107">声明不包含参数列表和 `data` 修饰符的记录类型是错误的。</span><span class="sxs-lookup"><span data-stu-id="83356-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="83356-108">记录类型的成员</span><span class="sxs-lookup"><span data-stu-id="83356-108">Members of a record type</span></span>

<span data-ttu-id="83356-109">除了在类体中声明的成员以外，记录类型还具有下列附加成员：</span><span class="sxs-lookup"><span data-stu-id="83356-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="83356-110">主构造函数</span><span class="sxs-lookup"><span data-stu-id="83356-110">Primary Constructor</span></span>

<span data-ttu-id="83356-111">记录类型具有公共构造函数，该构造函数的签名对应于类型声明的值参数。</span><span class="sxs-lookup"><span data-stu-id="83356-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="83356-112">这称为类型的主构造函数，并导致禁止显示隐式声明的默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="83356-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="83356-113">类中已存在具有相同签名的主构造函数和构造函数是错误的。</span><span class="sxs-lookup"><span data-stu-id="83356-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="83356-114">在运行时，主构造函数</span><span class="sxs-lookup"><span data-stu-id="83356-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="83356-115">执行出现在类体中的实例字段初始值设定项;然后调用不带参数的基类构造函数。</span><span class="sxs-lookup"><span data-stu-id="83356-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="83356-116">为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的属性，请参阅[合成属性](#Synthesized Properties)）</span><span class="sxs-lookup"><span data-stu-id="83356-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="83356-117">[] TODO：添加基本调用语法和有关通过重载决策选择基构造函数的规范</span><span class="sxs-lookup"><span data-stu-id="83356-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="83356-118">属性</span><span class="sxs-lookup"><span data-stu-id="83356-118">Properties</span></span>

<span data-ttu-id="83356-119">对于记录类型声明的每个记录参数，都有一个对应的公共属性成员，其名称和类型取自值参数声明。</span><span class="sxs-lookup"><span data-stu-id="83356-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="83356-120">如果没有显式声明或继承具有 get 访问器和此名称和类型的具体（即非抽象）属性，则编译器将生成，如下所示：</span><span class="sxs-lookup"><span data-stu-id="83356-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="83356-121">对于记录结构或记录类：</span><span class="sxs-lookup"><span data-stu-id="83356-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="83356-122">创建一个公共的仅获取自动属性。</span><span class="sxs-lookup"><span data-stu-id="83356-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="83356-123">在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。</span><span class="sxs-lookup"><span data-stu-id="83356-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="83356-124">将重写每个 "匹配" 继承抽象属性的 get 访问器。</span><span class="sxs-lookup"><span data-stu-id="83356-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="83356-125">相等成员</span><span class="sxs-lookup"><span data-stu-id="83356-125">Equality members</span></span>

<span data-ttu-id="83356-126">记录类型为以下方法生成合成实现：</span><span class="sxs-lookup"><span data-stu-id="83356-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="83356-127">除非已密封或用户提供，否则 `object.GetHashCode()` 重写</span><span class="sxs-lookup"><span data-stu-id="83356-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="83356-128">除非已密封或用户提供，否则 `object.Equals(object)` 重写</span><span class="sxs-lookup"><span data-stu-id="83356-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="83356-129">`T Equals(T)` 方法，其中 `T` 是当前类型</span><span class="sxs-lookup"><span data-stu-id="83356-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="83356-130">指定 `T Equals(T)` 以执行值相等性，将与每个主构造函数参数相同的属性与其他类型的相应属性进行比较。</span><span class="sxs-lookup"><span data-stu-id="83356-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="83356-131">`object.Equals` 执行等效操作</span><span class="sxs-lookup"><span data-stu-id="83356-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
