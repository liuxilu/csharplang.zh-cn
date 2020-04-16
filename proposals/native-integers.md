---
ms.openlocfilehash: 42839a8c233468dd0b5ec6dad436dc71f056a6d9
ms.sourcegitcommit: d414836632ba2730545e0b058373a94696bba5e4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81447802"
---
# <a name="native-sized-integers"></a>本机大小整数

## <a name="summary"></a>总结
[summary]: #summary

对本机大小的签名和无符号整数类型的语言支持。

动机是互操作场景和低级别库。

## <a name="design"></a>设计
[design]: #design

标识符`nint`和`nuint`是表示本机签名和无符号整数类型的新的上下文关键字。
仅当名称查找在该程序位置找不到可行结果时，标识符才会被视为关键字。
```C#
nint x = 3;
string y = nameof(nuint);
_ = nint.Equals(x, 3);
```

类型`nint`和`nuint`由基础类型`System.IntPtr`表示，编译器`System.UIntPtr`将这些类型的其他转换和操作作为本机 ints 表示。

### <a name="constants"></a>常量

常量表达式可以是 类型`nint`或`nuint`。
本机 int 文本没有直接语法。 可以改用其他积分常量值的隐式或显式强制转换`const nint i = (nint)42;`：

`nint`常量在 [ ， `int.MinValue` `int.MaxValue` ] 范围内。

`nuint`常量在 [ ， `uint.MinValue` `uint.MaxValue` ] 范围内。

`MinValue`没有`MaxValue`或字段`nint`，或`nuint`因为，除了`nuint.MinValue`，这些值不能作为常量发射。

所有未元`+`运算符 * 、 `-`、 `~` * 和二进制运算符`+` `-`* `*` `/`、 `%` `==`、 `!=` `<`、 `<=` `>`、 `>=` `&`、 `|` `^` `<<` `>>` 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、
无论编译器平台如何，都`Int32`使用`UInt32`和 操作数而不是本机 int 计算恒定的折叠操作，以便进行一致的行为。
如果操作在 32 位中导致恒定值，则在编译时执行恒定折叠。
否则，操作在运行时执行，不被视为常量。

### <a name="conversions"></a>转换
和`nint``IntPtr`之间存在标识转换，和 和`nuint``UIntPtr`之间有标识转换。
复合类型之间存在标识转换，这些类型仅因本机 ints 和基础类型而异：数组、`Nullable<>`构造类型和元组。

下表涵盖了特殊类型之间的转换。
（每个转换的 IL 包括 不同的`unchecked`变体和`checked`上下文（如果不同）。

| 操作数 | 目标 | 转换 | IL |
|:---:|:---:|:---:|:---:|
| `object` | `nint` | 取消装箱 | `unbox` |
| `void*` | `nint` | 指针到虚空 | `conv.i` |
| `sbyte` | `nint` | 隐式数字 | `conv.i` |
| `byte` | `nint` | 隐式数字 | `conv.u` |
| `short` | `nint` | 隐式数字 | `conv.i` |
| `ushort` | `nint` | 隐式数字 | `conv.u` |
| `int` | `nint` | 隐式数字 | `conv.i` |
| `uint` | `nint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `long` | `nint` | 显式数字 | `conv.i` / `conv.ovf.i` |
| `ulong` | `nint` | 显式数字 | `conv.i` / `conv.ovf.i` |
| `char` | `nint` | 隐式数字 | `conv.i` |
| `float` | `nint` | 显式数字 | `conv.i` / `conv.ovf.i` |
| `double` | `nint` | 显式数字 | `conv.i` / `conv.ovf.i` |
| `decimal` | `nint` | 显式数字 | `long decimal.op_Explicit(decimal) conv.i` / `... conv.ovf.i` |
| `IntPtr` | `nint` | 标识 | |
| `UIntPtr` | `nint` | 无 | |
| `object` | `nuint` | 取消装箱 | `unbox` |
| `void*` | `nuint` | 指针到虚空 | `conv.u` |
| `sbyte` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `byte` | `nuint` | 隐式数字 | `conv.u` |
| `short` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `ushort` | `nuint` | 隐式数字 | `conv.u` |
| `int` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `uint` | `nuint` | 隐式数字 | `conv.u` |
| `long` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `ulong` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `char` | `nuint` | 隐式数字 | `conv.u` |
| `float` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `double` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `decimal` | `nuint` | 显式数字 | `ulong decimal.op_Explicit(decimal) conv.u` / `... conv.ovf.u.un`  |
| `IntPtr` | `nuint` | 无 | |
| `UIntPtr` | `nuint` | 标识 | |

| 操作数 | 目标 | 转换 | IL |
|:---:|:---:|:---:|:---:|
| `nint` | `object` | 装箱 | `box` |
| `nint` | `void*` | 指针到虚空 | `conv.i` |
| `nint` | `nuint` | 显式数字 | `conv.u` / `conv.ovf.u` |
| `nint` | `sbyte` | 显式数字 | `conv.i1` / `conv.ovf.i1` |
| `nint` | `byte` | 显式数字 | `conv.u1` / `conv.ovf.u1` |
| `nint` | `short` | 显式数字 | `conv.i2` / `conv.ovf.i2` |
| `nint` | `ushort` | 显式数字 | `conv.u2` / `conv.ovf.u2` |
| `nint` | `int` | 显式数字 | `conv.i4` / `conv.ovf.i4` |
| `nint` | `uint` | 显式数字 | `conv.u4` / `conv.ovf.u4` |
| `nint` | `long` | 隐式数字 | `conv.i8` / `conv.ovf.i8` |
| `nint` | `ulong` | 显式数字 | `conv.i8` / `conv.ovf.i8` |
| `nint` | `char` | 显式数字 | `conv.u2` / `conv.ovf.u2` |
| `nint` | `float` | 隐式数字 | `conv.r4` |
| `nint` | `double` | 隐式数字 | `conv.r8` |
| `nint` | `decimal` | 隐式数字 | `conv.i8 decimal decimal.op_Implicit(long)` |
| `nint` | `IntPtr` | 标识 | |
| `nint` | `UIntPtr` | 无 | |
| `nuint` | `object` | 装箱 | `box` |
| `nuint` | `void*` | 指针到虚空 | `conv.u` |
| `nuint` | `nint` | 显式数字 | `conv.i` / `conv.ovf.i` |
| `nuint` | `sbyte` | 显式数字 | `conv.i1` / `conv.ovf.i1` |
| `nuint` | `byte` | 显式数字 | `conv.u1` / `conv.ovf.u1` |
| `nuint` | `short` | 显式数字 | `conv.i2` / `conv.ovf.i2` |
| `nuint` | `ushort` | 显式数字 | `conv.u2` / `conv.ovf.u2` |
| `nuint` | `int` | 显式数字 | `conv.i4` / `conv.ovf.i4` |
| `nuint` | `uint` | 显式数字 | `conv.u4` / `conv.ovf.u4` |
| `nuint` | `long` | 显式数字 | `conv.i8` / `conv.ovf.i8` |
| `nuint` | `ulong` | 隐式数字 | `conv.u8` / `conv.ovf.u8` |
| `nuint` | `char` | 显式数字 | `conv.u2` / `conv.ovf.u2.un` |
| `nuint` | `float` | 隐式数字 | `conv.r.un conv.r4` |
| `nuint` | `double` | 隐式数字 | `conv.r.un conv.r8` |
| `nuint` | `decimal` | 隐式数字 | `conv.u8 decimal decimal.op_Implicit(ulong)` |
| `nuint` | `IntPtr` | 无 | |
| `nuint` | `UIntPtr` | 标识 | |

转换为`A``Nullable<B>`：
- 如果存在从`A`转换为 的标识转换或隐式转换，则隐`B`式空转换。
- 如果显式转换为`A``B`
- 否则无效。

转换为`Nullable<A>``B`：
- 如果存在从 转换为 的标识转换或隐式或显式数字转换`A`，则显式可`B`取消转换。
- 否则无效。

转换为`Nullable<A>``Nullable<B>`：
- 如果从`A``B`转换为
- 如果从`A``B`
- 否则无效。

### <a name="operators"></a>运算符

预定义的运算符如下所示。
_如果至少一个操作数是类型`nint`或`nuint`，_ 则根据隐式转换的正常规则在重载解析期间考虑这些运算符。

（每个运算符的 IL 包括 的`unchecked`变体和`checked`上下文（如果不同）。

| 一元 | 操作员签名 | IL |
|:---:|:---:|:---:|
| `+` | `nint operator +(nint value)` | `nop` |
| `+` | `nuint operator +(nuint value)` | `nop` |
| `-` | `nint operator -(nint value)` | `neg` |
| `~` | `nint operator ~(nint value)` | `not` |
| `~` | `nuint operator ~(nuint value)` | `not` |

| Binary | 操作员签名 | IL |
|:---:|:---:|:---:|
| `+` | `nint operator +(nint left, nint right)` | `add` / `add.ovf` |
| `+` | `nuint operator +(nuint left, nuint right)` | `add` / `add.ovf.un` |
| `-` | `nint operator -(nint left, nint right)` | `sub` / `sub.ovf` |
| `-` | `nuint operator -(nuint left, nuint right)` | `sub` / `sub.ovf.un` |
| `*` | `nint operator *(nint left, nint right)` | `mul` / `mul.ovf` |
| `*` | `nuint operator *(nuint left, nuint right)` | `mul` / `mul.ovf.un` |
| `/` | `nint operator /(nint left, nint right)` | `div` |
| `/` | `nuint operator /(nuint left, nuint right)` | `div.un` |
| `%` | `nint operator %(nint left, nint right)` | `rem` |
| `%` | `nuint operator %(nuint left, nuint right)` | `rem.un` |
| `==` | `bool operator ==(nint left, nint right)` | `beq` / `ceq` |
| `==` | `bool operator ==(nuint left, nuint right)` | `beq` / `ceq` |
| `!=` | `bool operator !=(nint left, nint right)` | `bne` |
| `!=` | `bool operator !=(nuint left, nuint right)` | `bne` |
| `<` | `bool operator <(nint left, nint right)` | `blt` / `clt` |
| `<` | `bool operator <(nuint left, nuint right)` | `blt.un` / `clt.un` |
| `<=` | `bool operator <=(nint left, nint right)` | `ble` |
| `<=` | `bool operator <=(nuint left, nuint right)` | `ble.un` |
| `>` | `bool operator >(nint left, nint right)` | `bgt` / `cgt` |
| `>` | `bool operator >(nuint left, nuint right)` | `bgt.un` / `cgt.un` |
| `>=` | `bool operator >=(nint left, nint right)` | `bge` |
| `>=` | `bool operator >=(nuint left, nuint right)` | `bge.un` |
| `&` | `nint operator &(nint left, nint right)` | `and` |
| `&` | `nuint operator &(nuint left, nuint right)` | `and` |
| <code>&#124;</code> | <code>nint operator &#124;(nint left, nint right)</code> | `or` |
| <code>&#124;</code> | <code>nuint operator &#124;(nuint left, nuint right)</code> | `or` |
| `^` | `nint operator ^(nint left, nint right)` | `xor` |
| `^` | `nuint operator ^(nuint left, nuint right)` | `xor` |
| `<<` | `nint operator <<(nint left, int right)` | `shl` |
| `<<` | `nuint operator <<(nuint left, int right)` | `shl` |
| `>>` | `nint operator >>(nint left, int right)` | `shr` |
| `>>` | `nuint operator >>(nuint left, int right)` | `shr.un` |

对于某些二进制运算符，IL 运算符支持其他操作数类型（请参阅[ECMA-335](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf) III.1.5 操作数类型表）。
但是，为了简单起见和与语言中现有运算符的一致性，C# 支持的一组操作数类型受到限制。

支持运算符的已提升版本，其中参数和返回类型为`nint?`和`nuint?`。

`x`本机`y`int 的复合赋值操作`x op= y`遵循与具有预定义运算符的其他基元类型相同的规则。
具体来说，`x = (T)(x op y)`表达式绑定为 类型`T``x`和位置`x`仅计算一次。

换档操作员应屏蔽要移动的位数 - 如果`sizeof(nint)`为 4，则为 5 位，如果`sizeof(nint)`为 8，则为 6 位。
（请参阅 C# 规范中的[移位运算符](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#shift-operators)）。

### <a name="dynamic"></a>动态

转换和运算符由编译器合成，不是基础`IntPtr`和`UIntPtr`类型的一部分。
因此，这些转换和运算符从 的运行时活页夹`dynamic`_中不可用_。 

```C#
nint x = 2;
nint y = x + x; // ok
dynamic d = x;
nint z = d + x; // RuntimeBinderException: '+' cannot be applied 'System.IntPtr' and 'System.IntPtr'
```

### <a name="type-members"></a>类型成员

或`nint``nuint`的唯一构造函数是 无参数构造函数。

的以下`System.IntPtr`成员和`System.UIntPtr`_被明确排除_或`nint`： `nuint`
```C#
// constructors
// arithmetic operators
// implicit and explicit conversions
public static readonly IntPtr Zero; // use 0 instead
public static int Size { get; }     // use sizeof() instead
public static IntPtr Add(IntPtr pointer, int offset);
public static IntPtr Subtract(IntPtr pointer, int offset);
public int ToInt32();
public long ToInt64();
public void* ToPointer();
```

的`System.IntPtr``System.UIntPtr`其余成员_被隐式包含在_和`nint``nuint`中。 对于 .NET 框架 4.7.2：
```C#
public override bool Equals(object obj);
public override int GetHashCode();
public override string ToString();
public string ToString(string format);
```

实现的`System.IntPtr``System.UIntPtr`接口_被隐式包含在_和`nint``nuint`中，基础类型的出现被相应的本机整数类型替换。
例如，如果`IntPtr`实现`ISerializable, IEquatable<IntPtr>, IComparable<IntPtr>`，`nint`则`ISerializable, IEquatable<nint>, IComparable<nint>`实现 。

### <a name="overriding-hiding-and-implementing"></a>重写、隐藏和实现

`nint`和`System.IntPtr`和`nuint``System.UIntPtr`和 被视为等效于重写、隐藏和实现。

过载不能单独因`nint``System.IntPtr`和`nuint`和`System.UIntPtr`和 而不同。
重写和实现可能因`nint`和`System.IntPtr`、 或`nuint`和`System.UIntPtr`单独而异。
方法隐藏`nint`其他单独因 和`System.IntPtr`或`nuint``System.UIntPtr`和 而 不同的方法。

### <a name="miscellaneous"></a>杂项

`nint`用作`nuint`数组索引的表达式无需转换即可发出。
```C#
static object GetItem(object[] array, nint index)
{
    return array[index]; // ok
}
```

`nint`并`nuint`可用作`enum`基类型。
```C#
enum E : nint // ok
{
}
```

读取`nint`和写入对于类型`nuint`、 和`enum`基类型`nint`或`nuint`是原子的。

字段可以标记为`volatile`类型`nint`和`nuint`。
[ECMA-334](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf) 15.5.4 不包括`enum`基型`System.IntPtr`或`System.UIntPtr`。但是。

`default(nint)`并`new nint()`等效于`(nint)0`。

`typeof(nint)` 为 `typeof(IntPtr)`。

`sizeof(nint)`受支持，但需要在不安全的上下文中进行编译（同样如此`sizeof(IntPtr)`）。
该值不是编译时间常量。
`sizeof(nint)`作为`sizeof(IntPtr)`而不是`IntPtr.Size`实现 。

涉及`nint`或`nuint`报告`nint`或`nuint`而不是 或 的类型`IntPtr`引用的`UIntPtr`编译器诊断。

### <a name="metadata"></a>元数据

`nint`并在`nuint`元数据中表示为`System.IntPtr``System.UIntPtr`和 。

包含`nint`或`nuint`用 发出的类型引用`System.Runtime.CompilerServices.NativeIntegerAttribute`，以指示类型引用的哪些部分是本机 int。

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(
        AttributeTargets.Class |
        AttributeTargets.Event |
        AttributeTargets.Field |
        AttributeTargets.GenericParameter |
        AttributeTargets.Parameter |
        AttributeTargets.Property |
        AttributeTargets.ReturnValue,
        AllowMultiple = false,
        Inherited = false)]
    public sealed class NativeIntegerAttribute : Attribute
    {
        public NativeIntegerAttribute()
        {
            TransformFlags = new[] { true };
        }
        public NativeIntegerAttribute(bool[] flags)
        {
            TransformFlags = flags;
        }
        public IList<bool> TransformFlags { get; }
    }
}
```

编码使用用于编码`DynamicAttribute`的方法，尽管显然`DynamicAttribute`编码类型引用中的哪些类型`dynamic`是本机 ints，而不是哪些类型是本机 int。
如果编码导致`false`值数组，则不需要。 `NativeIntegerAttribute`
无`NativeIntegerAttribute`参数构造函数生成具有单个`true`值的编码。

```C#
nuint A;                    // [NativeInteger] UIntPtr A
(Stream, nint) B;           // [NativeInteger(new[] { false, false, true })] ValueType<Stream, IntPtr> B
```

## <a name="alternatives"></a>备选项
[alternatives]: #alternatives

与上述"类型擦除"方法的替代方法是引入新类型：`System.NativeInt`和`System.NativeUInt`。
```C#
public readonly struct NativeInt
{
    public IntPtr Value;
}
```
不同的类型将允许重载与 和`IntPtr`不同，允许不同的分析和`ToString()`。
但是，CLR 将有更多的工作能够有效地处理这些类型，从而破坏功能的主要目的 - 效率。
与使用`IntPtr`的现有本机 int 代码进行交互将更加困难。 

另一种方法是在框架`IntPtr`中添加更多本机 int 支持，但没有任何特定的编译器支持。
编译器将自动支持任何新的转换和算术运算。
但是，该语言不会提供关键字、常量或`checked`操作。

## <a name="design-meetings"></a>设计会议

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-26.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-13.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-07-05.md#native-int-and-intptr-operators https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-10-23.md https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md
