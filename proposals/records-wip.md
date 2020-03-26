---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281952"
---
# <a name="records-work-in-progress"></a>记录正在进行的工作

不同于其他记录建议，这不是一项提议，而是一项工作，用于记录有关记录功能的共识设计决策。 为了解决问题，将根据需要添加规范详细信息。

建议添加记录的语法，如下所示：

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

`attributes` 非终端还将允许 `data`的新上下文属性。

用参数列表或 `data` 修饰符声明的类（结构）称为记录类（record struct），其中的任何一个是记录类型。

声明不包含参数列表和 `data` 修饰符的记录类型是错误的。

## <a name="members-of-a-record-type"></a>记录类型的成员

除了在类体中声明的成员以外，记录类型还具有下列附加成员：

### <a name="primary-constructor"></a>主构造函数

记录类型具有公共构造函数，该构造函数的签名对应于类型声明的值参数。 这称为类型的主构造函数，并导致禁止显示隐式声明的默认构造函数。 类中已存在具有相同签名的主构造函数和构造函数是错误的。
在运行时，主构造函数 

1. 执行出现在类体中的实例字段初始值设定项;然后调用不带参数的基类构造函数。

1. 为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的属性，请参阅[合成属性](#Synthesized Properties)）


[] TODO：添加基本调用语法和有关通过重载决策选择基构造函数的规范

### <a name="properties"></a>属性

对于记录类型声明的每个记录参数，都有一个对应的公共属性成员，其名称和类型取自值参数声明。 如果没有显式声明或继承具有 get 访问器和此名称和类型的具体（即非抽象）属性，则编译器将生成，如下所示：

对于记录结构或记录类：

* 创建一个公共的仅获取自动属性。 在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。 将重写每个 "匹配" 继承抽象属性的 get 访问器。

### <a name="equality-members"></a>相等成员

记录类型为以下方法生成合成实现：

* 除非已密封或用户提供，否则 `object.GetHashCode()` 重写
* 除非已密封或用户提供，否则 `object.Equals(object)` 重写
* `T Equals(T)` 方法，其中 `T` 是当前类型

指定 `T Equals(T)` 以执行值相等性，将与每个主构造函数参数相同的属性与其他类型的相应属性进行比较。
`object.Equals` 执行等效操作

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>`with` 表达式

`with` 表达式是使用以下语法的新表达式。

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

`with` 表达式可实现 "非破坏性转变"，旨在生成接收方表达式副本，并对 `anonymous_object_initializer`中列出的属性进行修改。

有效的 `with` 表达式具有非 void 类型的接收方。 接收方类型必须包含一个名为 `With` 的可访问实例方法，该方法具有适当的参数和返回类型。 如果有多个非重写 `With` 方法，则是错误的。 如果有多个 `With` 重写，则必须有一个非重写 `With` 方法，该方法是目标方法。 否则，必须只有一个 `With` 方法。

`with` 表达式的右侧是一个具有一系列赋值的 `anonymous_object_initializer`，该分配的左侧带有接收方的一个字段或属性，而右侧的任意表达式都可以隐式转换为左侧的类型，这是一个可隐式转换的类型。

给定目标 `With` 方法时，返回类型必须是接收方表达式类型的类型或其基类型。 对于 `With` 方法的每个参数，对于具有相同名称和相同类型的接收器类型，必须具有可访问的对应实例字段或可读属性。 With 表达式的右侧的每个属性或字段也必须对应于 `With` 方法中具有相同名称的参数。

给定有效 `With` 方法，`with` 表达式的计算等效于使用 `anonymous_object_initializer` 中的表达式调用 `With` 方法，该方法将替换为与左侧属性相同的参数。 如果 `anonymous_object_initializer`中给定参数没有匹配的属性，则该参数是对接收方上的相同名称的字段或属性的计算。

副作用的计算顺序如下所示，每个表达式只计算一次：

1. 接收器表达式

2. `anonymous_object_initializer`中的表达式，按词法顺序排列

3. 与 `With` 方法参数匹配的任何属性的计算，按 `With` 方法参数的定义顺序进行。