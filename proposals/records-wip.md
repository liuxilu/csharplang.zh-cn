---
ms.openlocfilehash: 758bc3fddf577b118fc50866a683fa96324e5dc3
ms.sourcegitcommit: 300226c009490828d615e86c50df7ac8c836f945
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2020
ms.locfileid: "83843594"
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

`attributes`非终端还将允许新的上下文属性 `data` 。

用参数列表或修饰符声明的类（结构） `data` 称为记录类（record struct），其中的任何一个是记录类型。

声明不包含参数列表和修饰符的记录类型是错误的 `data` 。

## <a name="members-of-a-record-type"></a>记录类型的成员

除了在类或结构体中声明的成员以外，记录类型还具有下列附加成员：

### <a name="primary-constructor"></a>主构造函数

记录类型具有公共构造函数，该构造函数的签名对应于类型声明的值参数。 这称为类型的主构造函数，并导致禁止显示隐式声明的默认类构造函数（如果存在）。 类中已存在具有相同签名的主构造函数和构造函数是错误的。

在运行时，主构造函数

1. 执行出现在类体中的实例字段初始值设定项;然后调用不带参数的基类构造函数。

1. 为对应于值参数的属性初始化编译器生成的支持字段（如果这些属性是编译器提供的）

### <a name="properties"></a>属性

对于记录类型声明的每个记录参数，都有一个对应的公共属性成员，其名称和类型取自值参数声明。 如果没有显式声明或继承具有 get 访问器和此名称和类型的具体（即非抽象）属性，则编译器将生成，如下所示：

对于记录结构或记录类：

* 创建一个公共的仅获取自动属性。 在构造过程中，它的值将与相应的主构造函数参数的值一起初始化。 将重写每个 "匹配" 继承抽象属性的 get 访问器。

  * 此属性也是 `initonly` ，这意味着可以在下面的表达式中修改[with](#With)支持字段，前提是可以访问相应的 `get` 访问器

### <a name="equality-members"></a>相等成员

记录类型为以下方法生成合成实现：

* `object.GetHashCode()`重写（除非它是密封的或用户提供的）
* `object.Equals(object)`重写（除非它是密封的或用户提供的）
* `T Equals(T)`方法，其中 `T` 是当前类型

`T Equals(T)`指定为执行值相等性，这样， `Equals` 当且仅当接收方类型中声明的所有实例字段都等于其他类型的字段时，才为 true。

`object.Equals`执行等效于

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>`with` 表达式

`with`表达式是使用以下语法的新表达式。

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

`with`表达式允许 "非破坏性转变"，旨在生成接收方表达式的副本，并对中列出的属性进行修改 `anonymous_object_initializer` 。

有效的 `with` 表达式包含具有非 void 类型的接收方。 接收器类型必须包含一个名为的可访问参数实例方法，该方法 `Clone` 的返回类型必须是接收方类型或其基类型。

表达式的右侧 `with` 是一个 `anonymous_object_initializer` 具有赋值序列的，每个都具有 `initonly` 赋值的左侧的返回类型（作为属性调用）的属性（请参见[属性](#Properties)） `Clone` ，以及右侧的任意表达式，该表达式可隐式转换为属性的类型。

表达式的计算 `with` `Clone` 只调用方法一次，然后 `initonly` 使用 `Clone` 方法调用的结果作为接收方，将参数列表中每个属性的支持字段按词法顺序设置为其对应的表达式的值。 表达式的类型与该 `with` 方法的返回类型相同 `Clone` 。

