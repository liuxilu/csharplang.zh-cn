---
ms.openlocfilehash: 1fb1f3b9025d65cb39f675e60bae1cb6415fc184
ms.sourcegitcommit: 88202acd40ca04b6e513ea1e5993625ee26fac3f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/11/2020
ms.locfileid: "81219623"
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

有效的`with`表达式具有非 void 类型的接收器。 接收方类型必须包含一个可访问的无参数实例`Clone`方法，称为其返回类型必须是接收方快递类型的类型或其基类型。

`with`表达式的右侧是具有`anonymous_object_initializer`一系列赋值的赋值，在赋值的左侧具有编译器生成的接收方的记录属性，右侧的任意表达式隐式转换为左侧的类型。

`with`表达式的计算相当于精确调用`Clone`该方法一次，然后使用`Clone`该方法的结果作为接收方，按词法顺序将参数列表中每个记录属性的背衬字段设置为相应的表达式。
