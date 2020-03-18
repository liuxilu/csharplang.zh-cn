---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483552"
---
# <a name="compiler-intrinsics"></a>编译器内部函数

## <a name="summary"></a>总结

此建议提供的语言构造公开了当前无法有效访问的低级别 IL 操作码，或者根本不能访问： `ldftn`、`ldvirtftn`、`ldtoken` 和 `calli`。 这些低级别操作码在高性能代码中很重要，开发人员需要一种高效的访问方式。

## <a name="motivation"></a>动机

以下问题中介绍了此功能的动机和背景（这是此功能的一个潜在实现方式）： 

https://github.com/dotnet/csharplang/issues/191

这一备用设计建议在查看原提议的原型实现后，还会通过 @msjabby，以及在整个重要的基本代码中使用。 此设计是通过 @mjsabby、@tmat 和 @jkotas的重要输入来完成的。

## <a name="detailed-design"></a>详细设计 

### <a name="allow-address-of-to-target-methods"></a>允许目标方法的地址

现在将允许方法组作为表达式的参数。 此类表达式的类型将 `void*`。 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

如果此处没有委托转换，则筛选方法组中的成员的唯一机制是通过静态/实例访问。 如果无法区分成员，则会发生编译时错误。

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

此上下文中的 addressof 表达式将通过以下方式实现：

- ldftn：当方法是非虚拟时。
- ldvirtftn：当方法为虚拟时。

此功能的限制：

- 仅当对值使用调用表达式时，才能指定实例方法
- 无法在 `&`中使用本地函数。 这些方法的实现细节特意不由语言指定。 这包括它们是静态的还是实例的，或者是否与它们一起发出的签名完全相同。

### <a name="handleof"></a>handleof

`handleof` 上下文关键字将使用 `ldtoken` 指令将字段、成员或类型转换为其等效的 `RuntimeHandle` 类型。 表达式的确切类型将取决于 `handleof`中的名称类型：

- 字段： `RuntimeFieldHandle`
- 类型： `RuntimeTypeHandle`
- 方法： `RuntimeMethodHandle`

`handleof` 的参数与 `nameof`相同。 它必须是简单名称、限定名称、成员访问、指定成员的基访问或指定成员的此访问权限。 参数表达式标识代码定义，但从不进行计算。

`handleof` 表达式是在运行时计算的，且返回类型为 `RuntimeHandle`。 这可以在安全代码中执行，也可以不安全执行。 

``` 
RuntimeHandle stringHandle = handleof(string);
```

此功能的限制：

- 在 `handleof` 表达式中不能使用属性。
- 当作用域中存在现有 `handleof` 名称时，不能使用 `handleof` 表达式。 例如，类型、命名空间等。

### <a name="calli"></a>calli

编译器将添加对有效转换为 `.calli` 指令的一种新 `extern` 函数的支持。 Extern 属性将使用以下形状的属性进行标记：

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

这允许开发人员以以下形式定义方法：

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

应用了 `CallIndirect` 特性的方法的限制：

- 不能有 `DllImport` 特性。
- 不能为泛型。

## <a name="open-issues"></a>打开的问题

### <a name="callingconvention"></a>CallingConvention

所设计的 `CallIndirectAttribute` 使用的是缺少托管调用约定条目的 `CallingConvention` 枚举。 枚举需要进行扩展以包含此调用约定，否则该属性需要采用不同的方法。

## <a name="considerations"></a>注意事项

### <a name="disambiguating-method-groups"></a>歧义方法组

有一些围绕功能的讨论，可以更轻松地消除传递到表达式的方法组的歧义。 例如，可能会将签名元素添加到语法：

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

此操作被拒绝，因为不能做出极具说服力的情况，也不会在此处构想到简单的语法。 此外，还有一个相当直接的方法：简单地定义其他明确方法，并使用C#代码调入所需的函数。 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

如果 `static` 本地函数进入语言，则此功能将变得更简单。 然后，可以在使用不明确的操作地址的同一函数中定义解决方法：

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

允许在编译时将元数据令牌作为 `int` 值加载的原始提议。 实质上具有与 `handleof` 具有相同参数的 `tokenof`，但在编译时计算到 `int` 常数。 

此操作被拒绝，因为它会导致 IL 重写（.NET 具有多个）出现重大问题。 此类 rewriters 通常以可以使这些值无效的方式操作元数据表。 如果将这些值存储为简单 `int` 值，则无法合理地更新这些值。

运行时团队将继续探讨具有元数据条目的不透明句柄的基本概念。 

## <a name="future-considerations"></a>未来的注意事项

### <a name="static-local-functions"></a>静态本地函数

这是指允许对本地函数进行 `static` 修饰符的[提议](https://github.com/dotnet/csharplang/issues/1565)。 此类函数将保证作为 `static` 和在源代码中指定的确切签名发出。 此类函数应为 `&` 的有效参数，因为它不包含本地函数当前所拥有的任何问题。

### <a name="nativecallableattribute"></a>NativeCallableAttribute

CLR 具有一项功能，该功能允许以这样一种方式发出托管方法：直接从本机代码中调用这些方法。 这是通过将 `NativeCallableAttribute` 添加到方法来完成的。 此类方法只能从本机代码调用，因此必须仅包含签名中的本机类型。 从托管代码调用会导致运行时错误。 

此功能可与此建议一起进行模式处理，因为这样可以：

- 将托管代码中定义的函数作为函数指针（通过地址）传递给本机代码，而不会在托管代码或本机代码中产生开销。 
- 运行时可在托管代码中引入此类函数的使用站点错误，以防在编译时调用这些函数。




