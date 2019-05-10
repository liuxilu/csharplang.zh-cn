---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488941"
---
# <a name="exceptions"></a>Exceptions

C# 中的异常提供结构化、 统一的和类型安全方式来处理系统级别和应用程序级别的错误条件。 中的异常机制C#是非常类似于C++，有几个重要的区别：

*  在 C# 中，必须由派生自类类型的实例表示的所有异常`System.Exception`。 在C++，可以使用任何类型的任何值，表示该异常。
*  在 C# 中，finally 块 ([try 语句](statements.md#the-try-statement)) 可用于编写在正常执行和异常情况中执行的终止代码。 此类代码都很难编写C++无需复制代码。
*  在 C# 中，系统级别的异常例如溢出、 被零除和 null 取消引用有明确定义的异常类，等同于应用程序级别的错误条件。

## <a name="causes-of-exceptions"></a>引发异常的原因

可以在两种不同方法中引发异常。

*  一个`throw`语句 ([throw 语句](statements.md#the-throw-statement)) 立即无条件地将引发异常。 控件永远不会到达紧跟的语句`throw`。
*  C# 语句和表达式的处理过程中出现特定异常情况时无法正常完成该操作，在某些情况下导致异常。 例如，整数除法运算 ([除法运算符](expressions.md#division-operator)) 将引发`System.DivideByZeroException`如果分母为零。 请参阅[常见的异常类](exceptions.md#common-exception-classes)有关可在这种方式中出现的各种异常情况的列表。

## <a name="the-systemexception-class"></a>System.Exception 类

`System.Exception`类是所有异常的基类型。 此类具有共享的所有异常的几个值得注意的属性：

*  `Message` 是只读的属性类型的`string`，其中包含异常的原因的人工可读说明。
*  `InnerException` 是只读的属性类型的`Exception`。 如果其值为非 null，它引用导致当前异常的异常 — 也就是说，在 catch 块中引发当前异常处理`InnerException`。 否则，其值为 null，指示此异常不由另一个异常。 这种方式中链接在一起的异常对象的数目可以是任意的。

可以对的实例构造函数的调用中指定这些属性的值`System.Exception`。

## <a name="how-exceptions-are-handled"></a>如何处理异常

通过处理异常`try`语句 ([try 语句](statements.md#the-try-statement))。

异常发生时，系统将搜索最近`catch`可以处理该异常，由该异常的运行时类型的子句。 首先，当前方法搜索的词法封闭`try`按顺序考虑语句和 try 语句相关联的 catch 子句。 如果该操作失败，搜索调用当前方法的方法从词法上封闭`try`对当前方法的调用点的语句。 此搜索将继续，直至`catch`子句找到能处理当前异常，通过命名的是同一个类或基类时，所引发的异常的运行时类型的异常类。 一个`catch`子句未命名的异常类可以处理任何异常。

一旦找到匹配的 catch 子句，则系统将准备将控制转移到 catch 子句的第一个语句。 Catch 子句的执行开始之前，系统首先，按顺序执行，任何`finally`子句与 try 语句更多相关联，嵌套的比捕获该异常。

如果不找到任何匹配的 catch 子句，则会发生两个条件之一：

*  如果对匹配的 catch 子句的搜索到达静态构造函数 ([静态构造函数](classes.md#static-constructors)) 或静态字段初始值设定项，则`System.TypeInitializationException`触发调用静态构造函数的时候引发。 内部异常的`System.TypeInitializationException`包含最初引发的异常。
*  如果搜索匹配的 catch 子句达到最初启动该线程的代码，则终止线程的执行。 此类终止的影响是实现定义的。

析构函数执行过程中发生的异常是特别注意。 如果析构函数在执行期间，会发生异常，并且不捕获该异常，然后终止该析构函数的执行并调用 （如果有） 的基类的析构函数。 如果没有基类 (一样的情况下`object`类型) 或如果没有基类的析构函数，则异常将被丢弃。

## <a name="common-exception-classes"></a>常见的异常类

某些 C# 操作通过引发以下异常。

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | 算术运算期间出现的异常的基类，例如 `System.DivideByZeroException` 和 `System.OverflowException`。 | 
| `System.ArrayTypeMismatchException`  | 当为一个数组的存储会失败，因为存储的元素的实际类型与数组的实际类型不兼容时引发。 | 
| `System.DivideByZeroException`       | 当尝试除以零的整数值时引发。 | 
| `System.IndexOutOfRangeException`    | 当尝试通过索引小于零或超出数组界限的数组编制索引时引发。 | 
| `System.InvalidCastException`        | 从基类或接口为派生类型的显式转换在运行时失败时引发。 | 
| `System.NullReferenceException`      | 时引发`null`使引用的对象是所需的方式使用引用。 | 
| `System.OutOfMemoryException`        | 当尝试分配内存时引发 (通过`new`) 失败。 | 
| `System.OverflowException`           | `checked` 上下文中的算术运算溢出时引发。 | 
| `System.StackOverflowException`      | 引发在执行堆栈用尽了的过多挂起的方法调用;通常指明可能出现了非常深度或无限递归。 | 
| `System.TypeInitializationException` | 引发时，静态构造函数引发异常，但没有`catch`子句存在捕获它。 | 
