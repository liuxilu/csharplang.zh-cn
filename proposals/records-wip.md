---
ms.openlocfilehash: e29f453eb33e06fb3c84fb542d8b74223af9b975
ms.sourcegitcommit: 7f0c8e4eac7afe75e4f312f54a554614384cd06b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80870977"
---
# <a name="records-work-in-progress"></a>记录正在进行的工作

与其他记录建议不同，这不是建议本身，而是旨在记录记录功能的协商一致设计决策的正在进行的工作。 作为解决问题所必需的，将添加规范详细信息。

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

非`attributes`终端还将允许新的上下文属性`data`。

使用参数列表或`data`修改器声明的类（结构）称为记录类（记录结构），其中任一为记录类型。

在没有参数列表和`data`修改器的情况下声明记录类型是错误的。

## <a name="members-of-a-record-type"></a>记录类型的成员

除了在类或结构正文中声明的成员外，记录类型还有以下其他成员：

### <a name="primary-constructor"></a>主构造函数

记录类型具有公共构造函数，其签名对应于类型声明的值参数。 这称为类型的主构造函数，并导致隐式声明的默认类构造函数（如果存在）被抑制。 在类中具有具有相同签名的主构造函数和构造函数是错误的。

在运行时，主构造函数

1. 执行出现在类正文中的实例字段初始化器;然后调用没有参数的基类构造函数。

1. 初始化与值参数对应的属性的编译器生成的支持字段（如果这些属性是编译器提供的

### <a name="properties"></a>属性

对于记录类型声明的每个记录参数，都有一个相应的公共属性成员，其名称和类型取自值参数声明。 如果没有具有 get 访问器且具有此名称和类型的具体属性（即非抽象）属性显式声明或继承，则编译器会按如下方式生成该属性：

对于记录结构或记录类：

* 将创建公共仅获取自动属性。 其值在构造过程中用相应主构造函数参数的值进行初始化。 每个"匹配"继承的抽象属性的获取访问器将被覆盖。

### <a name="equality-members"></a>平等成员

记录类型为以下方法生成合成实现：

* `object.GetHashCode()`覆盖，除非它是密封的或用户提供的
* `object.Equals(object)`覆盖，除非它是密封的或用户提供的
* `T Equals(T)`方法，当前`T`类型在哪里

`T Equals(T)`指定执行值相等性，将具有相同名称的属性与每个主构造函数参数与其他类型的相应属性进行比较。
`object.Equals`执行等效的

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>`with` 表达式

表达式`with`是使用以下语法的新表达式。

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

表达式`with`允许"非破坏性突变"，旨在生成接收方表达式的副本，并修改 中列出的属性`anonymous_object_initializer`。

有效的`with`表达式具有非 void 类型的接收器。 接收方类型必须包含使用适当的参数和返回`With`类型调用的可访问实例方法。 如果存在多个非重写`With`方法，则为错误。 如果有多个`With`重写，则必须有一个非重写`With`方法，即目标方法。 否则，必须有一种方法`With`。

`with`表达式的右侧是一个`anonymous_object_initializer`赋值序列，赋值左侧具有接收方的字段或属性，右侧的任意表达式隐式转换为左侧的类型。

给定目标`With`方法，返回类型必须是接收方表达式类型的类型或其基类型。 对于方法的每个参数，`With`必须具有具有相同名称和相同类型的接收方类型上的可访问对应实例字段或可读属性。 "给"表达式右侧的每个属性或字段还必须对应于`With`方法中同名的参数。

给定一个`With`有效方法，`with`表达式的计算等效于调用`With`方法，用 中的表达式`anonymous_object_initializer`替换与左侧的属性同名的参数。 如果 中给定参数`anonymous_object_initializer`没有匹配属性，则参数是计算接收器上同名的字段或属性。

副作用评估的顺序如下，每个表达式精确计算一次：

1. 接收方表达式

2. 中的`anonymous_object_initializer`表达式，按词法顺序排列

3. 按`With``With`方法参数定义顺序计算与方法参数匹配的任何属性。
