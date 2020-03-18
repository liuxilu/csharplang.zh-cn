---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484104"
---
# <a name="nullable-reference-types-specification"></a>可以为 null 的引用类型规范

这是一个正在进行的工作-缺少几个部件或这些部件不完整。 ***

## <a name="syntax"></a>语法

### <a name="nullable-reference-types"></a>可为空引用类型

可以为 null 的引用类型与可以为 null 的值类型的缩写形式 `T?` 相同的语法，但没有相应的长格式。

出于规范的目的，当前 `nullable_type` 生产将重命名为 `nullable_value_type`，并添加 `nullable_reference_type` 生产：

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

`nullable_reference_type` 中的 `non_nullable_reference_type` 必须是不可为 null 的引用类型（类、接口、委托或数组），或者是一个被约束为不可为 null 的引用类型的类型参数（通过 `class` 约束或除 `object`以外的类）。

可以为 null 的引用类型不能出现在以下位置：

- 作为基类或接口
- 作为 `member_access` 的接收方
- 作为 `object_creation_expression` 中的 `type`
- 作为 `delegate_creation_expression` 中的 `delegate_type`
- 作为 `is_expression`、`catch_clause` 或 `type_pattern` 中的 `type`
- 作为完全限定的接口成员名称中的 `interface`

在禁用了可为 null 的注释上下文的 `nullable_reference_type` 上给出了警告。

### <a name="nullable-class-constraint"></a>可以为 null 的类约束

`class` 约束具有可为 null 的对应 `class?`：

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>包容性运算符

修复后的 `!` 运算符称为包容性运算符。

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

`primary_expression` 必须是引用类型。  

后缀 `!` 运算符没有运行时效果，其计算结果为基础表达式的结果。 其唯一的作用是更改表达式的 null 状态，并限制其使用时给出的警告。

### <a name="nullable-implicitly-typed-local-variables"></a>可以为 null 的隐式类型的局部变量

`var` 推理引用类型的批注类型。
例如，在 `var s = "";` 中，`var` 将推断为 `string?`。

### <a name="nullable-compiler-directives"></a>可以为 null 的编译器指令

`#nullable` 指令控制可为 null 的批注和警告上下文。

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

将展开 `#pragma warning` 指令，以允许更改可为 null 的警告上下文，并允许在默认情况下启用单个警告：

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

请注意，`pragma_warning_body` 的新形式使用 `nullable_action`，而不是 `warning_action`。

## <a name="nullable-contexts"></a>可为空上下文

源代码的每个行都有*可为 null 的注释上下文*和*可以为 null 的警告上下文*。 这些控件控制是否可为 null 的批注是否有效，以及是否给定了可为空性警告。 给定行的批注上下文已*禁用*或*启用*。 给定行的警告上下文已*禁用*或*启用*。

这两种上下文都可以在项目级别（ C#源代码外）指定，也可以在源文件中的任意位置指定，`#nullable` 预处理器指令。 如果未提供任何项目级别设置，则默认情况下，这两个上下文都是*禁用*的。

`#nullable` 指令控制源文本中的批注和警告上下文，并优先于项目级设置。

指令将它控制的上下文设置为后面的代码行，直到另一个指令重写它，或直到源文件的结尾。

指令的效果如下所示：

- `#nullable disable`：将可为 null 的批注和警告上下文设置为*已禁用*
- `#nullable enable`：将可为 null 的批注和警告上下文设置为*enabled*
- `#nullable restore`：将可为 null 的批注和警告上下文还原到项目设置
- `#nullable disable annotations`：将可为 null 的注释上下文设置为*已禁用*
- `#nullable enable annotations`：将可为 null 的注释上下文设置为*enabled*
- `#nullable restore annotations`：将可为 null 的注释上下文还原到项目设置
- `#nullable disable warnings`：将可为 null 的警告上下文设置为*已禁用*
- `#nullable enable warnings`：将可为 null 的警告上下文设置为 "*已启用*"
- `#nullable restore warnings`：将可为 null 的警告上下文还原到项目设置

## <a name="nullability-of-types"></a>类型为 Null 性

给定的类型可以有以下四个 nullabilities 之一：*在意*、*不可 null*、 *nullable*和*unknown*。 

如果向*不可 null*和*未知*类型分配了潜在 `null` 值，则可能会出现警告。 不过，*在意*和可以*为*null 的类型是 "可*赋值的*"，并且可以为其分配 `null` 值，而不会出现警告。 

可以取消引用或分配*在意*和*不可 null*类型，而不会出现警告。 然而，*可以为*null 的类型和*未知*类型的值都是 "*空*值"，当取消引用或赋值时，如果没有正确的 null 检查，则可能会导致警告。 

Null 生成类型的*默认 null 状态*为 "可能为 null"。 非 null 生成类型的默认 null 状态为 "not null"。

类型的类型和在确定其为 null 性时所发生的可为 null 的注释上下文：

- 不可 null 值类型 `S` 始终为*不可 null*
- `S?` 的可以为 null 的值类型始终*可以为 null*
- *禁用*的批注上下文中 `C` 的批注引用类型为*在意*
- *启用*的批注上下文中 `C` 的批注引用类型为*不可 null*
- `C?` 可以为 null 的引用类型可以为*null* （但在*禁用*的批注上下文中可能生成警告）

类型参数还会考虑它们的约束：

- 一个类型参数 `T` 其中所有约束（如果有）为 null 生成类型（*可为 null*和*未知*）或 `class?` 约束*未知*
- 一个类型参数 `T`，其中至少有一个约束为*在意*或*不可 null* ，或者其中一个 `struct` 或 `class` 约束为
    - *禁用*的注释上下文中的*在意*
    - *已启用*的批注上下文中的*不可 null*
- 一个可以为 null 的类型参数 `T?` 其中至少有一个 `T`的约束是*在意*或*不可 null* ，或者是 `struct` 或 `class` 约束之一。
    - 在*禁用*的注释上下文中*可以为 null* （但生成了警告）
    - 在*启用*的批注上下文中*为 null*

对于类型参数 `T`，仅当已知 `T` 为值类型或已知为引用类型时，才允许使用 `T?`。

### <a name="oblivious-vs-nonnullable"></a>在意 vs 不可 null

当类型的最后一个标记在该上下文中时，将 `type` 视为在给定的批注上下文中发生。

源代码中 `C` 的给定引用类型是解释为在意还是不可 null 依赖于源代码的批注上下文。 但一旦建立，就会将其视为该类型的一部分，并在替换泛型类型参数的过程中将其视为 "随之一起移动"。 如同类型上存在类似 `?` 的批注，但不可见。

## <a name="constraints"></a>约束

可以为 null 的引用类型可用作泛型约束。 此外 `object` 现在作为显式约束有效。 缺少约束现在等效于 `object?` 约束（而不是 `object`），但是（与 `object` 之前） `object?` 不会禁止作为显式约束。

`class?` 是表示 "可能为 null 的引用类型" 的新约束，而 `class` 表示 "不可 null 引用类型"。

类型参数或约束的可为 null 性并不会影响类型是否满足约束，但目前已有的情况除外（可为 null 的值类型不满足 `struct` 约束）。 但是，如果类型参数不满足约束的为 null 性要求，则可以提供警告。

## <a name="null-state-and-null-tracking"></a>Null 状态和 null 跟踪

给定源位置中的每个表达式都具有*null 状态*，指示其是否被认为可能计算为 null。 Null 状态为 "not null" 或 "可能为 null"。 Null 状态用于确定是否应为不安全的转换和取消引用提供警告。

### <a name="null-tracking-for-variables"></a>变量的 Null 跟踪

对于指定变量或属性的某些表达式，将根据对它们的赋值、对它们执行的测试以及它们之间的控制流，在发生之间跟踪 null 状态。 这类似于为变量跟踪明确赋值的方式。 跟踪的表达式如下所示：

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

其中，标识符表示字段或属性。

***描述类似于明确赋值的空状态转换***

### <a name="null-state-for-expressions"></a>表达式为 Null

表达式的 null 状态派生自其形式和类型以及它所涉及的变量的 null 状态。

### <a name="literals"></a>文字

`null` 文本的 null 状态为 "可能为 null"。 正在转换为已知不可 null 值类型的类型的 `default` 文本的 null 状态是 "可能为 null"。 任何其他文本的 null 状态为 "not null"。

### <a name="simple-names"></a>简单名称

如果 `simple_name` 未分类为值，则其 null 状态为 "not null"。 否则为跟踪的表达式，其 null 状态将为其在此源位置跟踪的 null 状态。

### <a name="member-access"></a>成员访问

如果 `member_access` 未分类为值，则其 null 状态为 "not null"。 否则，如果是跟踪的表达式，则其 null 状态将为其在此源位置跟踪的 null 状态。 否则，其 null 状态为其类型的默认 null 状态。

### <a name="invocation-expressions"></a>调用表达式

如果 `invocation_expression` 调用使用一个或多个特性声明的成员以实现特殊的 null 行为，则 null 状态由这些特性决定。 否则，表达式的 null 状态为其类型的默认 null 状态。

### <a name="element-access"></a>元素访问

如果 `element_access` 调用使用一个或多个特性声明的索引器来实现特殊的 null 行为，则 null 状态由这些特性决定。 否则，表达式的 null 状态为其类型的默认 null 状态。

### <a name="base-access"></a>基本访问权限

如果 `B` 表示封闭类型的基类型，则 `base.I` 与 `((B)this).I` 具有相同的 null 状态，`base[E]` 与 `((B)this)[E]`具有相同的 null 状态。

### <a name="default-expressions"></a>默认的表达式

如果已知 `T` 为不可 null 值类型，则 `default(T)` 的状态为 "非 null"。 否则，它的状态为 "可能为 null"。

### <a name="null-conditional-expressions"></a>Null 条件表达式

`null_conditional_expression` 的状态为 "可能为 null"。

### <a name="cast-expressions"></a>强制转换表达式

如果强制转换表达式 `(T)E` 调用用户定义的转换，则表达式的 null 状态为其类型的默认 null 状态。 否则，如果 `T` 为 null （*可为*null 或*未知*），则 null 状态为 "可能为 null"。 否则，null 状态与 `E`的 null 状态相同。

### <a name="await-expressions"></a>Await 表达式

`await E` 的 null 状态为其类型的默认 null 状态。

### <a name="the-as-operator"></a>`as` 运算符

`as` 表达式的状态为 "可能为 null"。

### <a name="the-null-coalescing-operator"></a>Null 合并运算符

`E1 ?? E2` 与 `E2` 具有相同的 null 状态

### <a name="the-conditional-operator"></a>条件运算符

如果 `E2` 和 `E3` 的 null 状态都为 "not null"，则 `E1 ? E2 : E3` 的 null 状态为 "not null"。 否则为 "可能为 null"。

### <a name="query-expressions"></a>查询表达式

查询表达式的 null 状态为其类型的默认 null 状态。

### <a name="assignment-operators"></a>赋值运算符

应用任何隐式转换后，`E1 = E2` 和 `E1 op= E2` 具有与 `E2` 相同的 null 状态。

### <a name="unary-and-binary-operators"></a>一元运算符和二元运算符

如果一元或二元运算符调用了用一个或多个特性声明的用户定义的运算符来实现特殊的 null 行为，则 null 状态由这些特性决定。 否则，表达式的 null 状态为其类型的默认 null 状态。

***对于字符串和委托的二进制 `+` 有特殊的意义吗？***

### <a name="expressions-that-propagate-null-state"></a>传播 null 状态的表达式

`(E)`，`checked(E)` 和 `unchecked(E)` 都与 `E`具有相同的 null 状态。

### <a name="expressions-that-are-never-null"></a>从不为 null 的表达式

以下表达式格式的 null 状态始终为 "not null"：

- `this` access
- 内插字符串
- `new` 表达式（对象、委托、匿名对象和数组创建表达式）
- `typeof` 表达式
- `nameof` 表达式
- 匿名函数（匿名方法和 lambda 表达式）
- 包容性表达式
- `is` 表达式

## <a name="type-inference"></a>类型推理

### <a name="type-inference-for-var"></a>`var` 的类型推理

为使用 `var` 声明的局部变量声明的类型由初始化表达式的 null 状态通知。

```csharp
var x = E;
```

如果 `E` 的类型是可以为 null 的引用类型 `C?` 并且 `E` 的 null 状态为 "not null"，则为 `x` 推断的类型为 `C`。 否则，推理出的类型是 `E`的类型。

根据 `var`的批注上下文确定为 `x` 推断的类型的为空性，就像该位置中显式指定了类型一样。

### <a name="type-inference-for-var"></a>`var?` 的类型推理

为用 `var?` 声明的局部变量推理出的类型与初始化表达式的 null 状态无关。

```csharp
var? x = E;
```

如果 `E` 的类型 `T` 是可以为 null 的值类型或可以为 null 的引用类型，则 `T`为 `x` 推断的类型。 否则，如果 `T` 是不可 null 值类型 `S` 则推断出的类型为 `S?`。 否则，如果 `T` 是不可 null 引用类型 `C` 则 `C?`推理出的类型。 否则，声明是非法的。

为 `x` 推断的类型的为空性始终*可以为 null*。

### <a name="generic-type-inference"></a>泛型类型推理

泛型类型推理经过了增强，可帮助确定推断的引用类型是否可以为 null。 这是一项最大的工作，并且本身不会生成警告，但在将所选重载的推断类型应用于参数时，可能会导致可以为 null 的警告。

类型推断不依赖于传入类型的注释上下文。 相反，`type` 将从其 "可能已在" 中获取其自己的注释上下文（如果已显式表示）。 这会将类型推理的角色作为一种简便方式，方便您自己编写的内容。

更准确地说，推理出的类型实参的注释上下文是后面跟有 `<...>` 类型形参列表的令牌的上下文;例如，要调用的泛型方法的名称。 对于转换为此类调用的查询表达式，上下文取自从中生成调用的查询子句的初始上下文关键字。

### <a name="the-first-phase"></a>第一阶段

可以为 null 的引用类型从初始表达式流入边界，如下所述。 此外，还引入了两种新的界限，即 `null` 和 `default`。 它们的目的是在输入表达式中执行 `null` 或 `default` 的出现，这可能会导致推断出的类型可以为 null，即使在其他情况下也是如此。 这甚至适用于可为 null 的值类型，这些*值*类型得到了增强，可在推断过程中选取 "非 null"。

在第一阶段中，确定要添加的界限如下：

如果参数 `Ei` 具有引用类型，则用于推理的类型 `U` 取决于 `Ei` 的 null 状态及其声明的类型：
- 如果声明的类型是不可 null 引用类型 `U0` 或可以为 null 的引用类型 `U0?` 则
    - 如果 `Ei` 的 null 状态为 "not null"，则 `U` `U0`
    - 如果 `Ei` 的 null 状态为 "可能为 null"，则 `U` 为 `U0?`
- 否则，如果 `Ei` 具有声明的类型，`U` 为该类型
- 否则，如果 `Ei` `null` 则 `U` 为特殊界限 `null`
- 否则，如果 `Ei` `default` 则 `U` 为特殊界限 `default`
- 否则，不进行推理。

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>精确、上限和下限推理

在*从*类型 `U` 推断*到*类型 `V`时，如果 `V` 是 `V0?`的可以为 null 的引用类型，则使用 `V0`，而不是在下面的子句中使用 `V`。
- 如果 `V` 是未固定的类型变量之一，则 `U` 添加为一个精确的、上限或下限，如下所示
- 否则，如果 `U` `null` 或 `default`，则不进行推理
- 否则，如果 `U` 是 `U0?`的可以为 null 的引用类型，则将使用 `U0`，而不是在后续子句中 `U`。

实质上，与某个未固定的类型变量直接相关的可为 null 将保留在其边界内。 另一方面，如果推断将进一步递归到源和目标类型，则会忽略为空性。 它可以或不匹配，但如果不匹配，则会在以后选择并应用重载时发出警告。

### <a name="fixing"></a>更正

如果多个边界彼此之间相互转换，但不同，则该规范并不是一种很好的描述。 这种情况可能发生在 `object` 和 `dynamic`之间、不同于元素名称的元组类型之间、构造的类型之间以及引用类型的 `C` 和 `C?` 之间。

此外，我们还需要将 "非 null" 从输入表达式传播到结果类型。 

为了应对这些情况，我们添加了更多的修复阶段，这现在是：

1. 将所有边界中的所有类型都收集为候选项，同时删除所有属于可为 null 的引用类型的 `?`
2. 根据精确、下限和上限的要求（保留 `null` 和 `default` 界限）来消除候选项
3. 消除没有隐式转换为所有其他候选项的候选项
4. 如果剩余的候选项彼此之间没有标识转换，则类型推理将失败
5. 按如下所述*合并*剩余候选项
6. 如果生成的候选项是引用类型或不可 null 值类型，并且*所有*的完全*边界或下限*都是可以为 null 的值类型、可以为 null 的引用类型、`null` 或 `default`，则 `?` 将添加到生成的候选项，从而使其成为可以为 null 的值类型或引用类型。

两个候选类型之间介绍了*合并*。 它是可传递和可交换的，因此，可按任何顺序将候选项合并，并获得相同的最终结果。 如果两个候选类型不能相互转换，则它是不确定的。

*Merge*函数采用两种候选类型和方向（ *+* 或 *-* ）：

- *Merge*（`T`，`T`， *d*） = t
- *Merge*（`S`、`T?`、 *+* ） = *merge*（`S?`、`T`、 *+* ） = *merge*（`S`、`T`、 *+* ）`?`
- *Merge*（`S`、`T?`、 *-* ） = *merge*（`S?`、`T`、 *-* ） = *merge*（`S`、`T`、 *-* ）
- *Merge*（`C<S1,...,Sn>`、`C<T1,...,Tn>`、 *+* ） = `C<`*merge*（`S1`，`T1`， *d1*）`,...,`*merge*（`Sn`，`Tn`， *dn*）`>`，*其中*
    - 如果 `C<...>` 的第一个 `i`类型参数是协变的，则 `di` =  *+*
    - 如果 `C<...>` 的第一个 `i`类型参数为逆变或固定，则 `di` =  *-*
- *Merge*（`C<S1,...,Sn>`、`C<T1,...,Tn>`、 *-* ） = `C<`*merge*（`S1`，`T1`， *d1*）`,...,`*merge*（`Sn`，`Tn`， *dn*）`>`，*其中*
    - 如果 `C<...>` 的第一个 `i`类型参数是协变的，则 `di` =  *-*
    - 如果 `C<...>` 的第一个 `i`类型参数为逆变或固定，则 `di` =  *+*
- *Merge*（`(S1 s1,..., Sn sn)`、`(T1 t1,..., Tn tn)`、 *d*） = `(`*merge*（`S1`、`T1`、 *d*）`n1,...,`*merge*（`Sn`、`Tn`、 *d*） `nn)`，*其中*
    - 如果 `si` 和 `ti` 不同，或者两者都不存在，则 `ni` 不存在
    - 如果 `si` 和 `ti` 相同，则 `si` `ni`
- *Merge*（`object`、`dynamic`） = *merge*（`dynamic`，`object`） = `dynamic`

## <a name="warnings"></a>警告

### <a name="potential-null-assignment"></a>潜在 null 赋值

### <a name="potential-null-dereference"></a>潜在的空取消引用

### <a name="constraint-nullability-mismatch"></a>约束为空性不匹配

### <a name="nullable-types-in-disabled-annotation-context"></a>禁用的注释上下文中可以为 null 的类型

## <a name="attributes-for-special-null-behavior"></a>特殊 null 行为的特性


