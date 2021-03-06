---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509491"
---
# <a name="attributes-on-local-functions"></a>本地函数的属性

## <a name="attributes"></a>属性

本地函数声明现在允许有[属性](../spec/attributes.md)。 本地函数上的参数和类型参数也允许有属性。

应用于方法、其参数或其类型参数的属性在应用于本地函数、其参数或其类型参数时具有相同的含义。

可以通过使用 `[ConditionalAttribute]`修饰本地函数，使其与[条件方法](../spec/attributes.md#the-conditional-attribute)具有相同的条件。 还必须 `static`条件本地函数。 条件方法的所有限制也适用于条件本地函数，包括返回类型必须 `void`。

## <a name="extern"></a>部

`extern` 修饰符现在允许用于本地函数。 这使本地函数成为外部[方法](../spec/classes.md#external-methods)的外部函数。

与外部方法类似，外部本地函数的*本地函数体*必须是分号。 只允许对外部本地函数使用分号*局部函数体*。 

还必须 `static`外部本地函数。

## <a name="syntax"></a>语法

按如下所示修改[本地函数语法](csharp-7.0/local-functions.md#syntax-grammar)：
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
