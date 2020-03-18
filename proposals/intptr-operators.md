---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483546"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="697d1-101">应对 `System.IntPtr` 和 `System.UIntPtr` 应公开运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="697d1-102">[x] 建议</span><span class="sxs-lookup"><span data-stu-id="697d1-102">[x] Proposed</span></span>
* <span data-ttu-id="697d1-103">[] 原型：未启动</span><span class="sxs-lookup"><span data-stu-id="697d1-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="697d1-104">[] 实现：未启动</span><span class="sxs-lookup"><span data-stu-id="697d1-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="697d1-105">[] 规范：未启动</span><span class="sxs-lookup"><span data-stu-id="697d1-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="697d1-106">总结</span><span class="sxs-lookup"><span data-stu-id="697d1-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="697d1-107">CLR 支持 `System.IntPtr` 和 `System.UIntPtr` 类型（`native int`）的一组运算符。</span><span class="sxs-lookup"><span data-stu-id="697d1-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="697d1-108">可以在公共语言基础结构规范（`ECMA-335`） `III.1.5` 中查看这些操作员。</span><span class="sxs-lookup"><span data-stu-id="697d1-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="697d1-109">但是，不支持这些运算符C#。</span><span class="sxs-lookup"><span data-stu-id="697d1-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="697d1-110">应为 `System.IntPtr` 和 `System.UIntPtr`支持的整套运算符提供语言支持。</span><span class="sxs-lookup"><span data-stu-id="697d1-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="697d1-111">这些运算符包括： `Add`、`Divide`、`Multiply`、`Remainder`、`Subtract`、`Negate`、`Equals`、`Compare`、`And`、`Not`、`Or`、`XOr`、`ShiftLeft`、`ShiftRight`。</span><span class="sxs-lookup"><span data-stu-id="697d1-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="697d1-112">动机</span><span class="sxs-lookup"><span data-stu-id="697d1-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="697d1-113">目前，用户可以使用各种C#工具和框架轻松编写面向多个平台的应用程序，例如： `Xamarin`、`.NET Core`、`Mono`等。</span><span class="sxs-lookup"><span data-stu-id="697d1-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="697d1-114">编写跨平台代码时，通常需要以特定的方式编写与特定目标平台交互的互操作代码。</span><span class="sxs-lookup"><span data-stu-id="697d1-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="697d1-115">这可能包括编写图形代码、调用一些系统 API 或与现有本机库交互。</span><span class="sxs-lookup"><span data-stu-id="697d1-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="697d1-116">此互操作代码通常必须处理句柄、非托管内存甚至只是平台特定大小的整数。</span><span class="sxs-lookup"><span data-stu-id="697d1-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="697d1-117">运行时通过定义一组可在 `native int` （`System.IntPtr`）和 `native unsigned int` （`System.UIntPtr`）基元类型上使用的运算符，为此提供支持。</span><span class="sxs-lookup"><span data-stu-id="697d1-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="697d1-118">C#从未支持这些运算符，因此用户必须解决该问题。</span><span class="sxs-lookup"><span data-stu-id="697d1-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="697d1-119">这通常会增加代码的复杂性并降低代码的可维护性。</span><span class="sxs-lookup"><span data-stu-id="697d1-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="697d1-120">因此，该语言应该开始支持这些运算符，以帮助提升语言以更好地支持这些要求。</span><span class="sxs-lookup"><span data-stu-id="697d1-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="697d1-121">详细设计</span><span class="sxs-lookup"><span data-stu-id="697d1-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="697d1-122">支持的整套运算符在公共语言基础结构规范（`ECMA-335`） `III.1.5` 中定义。</span><span class="sxs-lookup"><span data-stu-id="697d1-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="697d1-123">此处提供了该规范： [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf)。</span><span class="sxs-lookup"><span data-stu-id="697d1-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="697d1-124">为方便起见，下面提供了运算符摘要。</span><span class="sxs-lookup"><span data-stu-id="697d1-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="697d1-125">CLI 规范定义的不可验证的操作员并未列出，目前也不是此建议的一部分（尽管可能还需要考虑这些操作）。</span><span class="sxs-lookup"><span data-stu-id="697d1-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="697d1-126">提供一个关键字（例如 `nint` 和 `nuint`），并为 `System.IntPtr` 和 `System.UIntPtr` （如0n）声明的文本提供一种方法，而不是此建议的一部分（尽管可能还需要考虑这种情况）。</span><span class="sxs-lookup"><span data-stu-id="697d1-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="697d1-127">一元加号运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="697d1-128">一元减号运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="697d1-129">按位求补运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="697d1-130">强制转换运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="697d1-131">乘法运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="697d1-132">除法运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="697d1-133">余数运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="697d1-134">加法运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="697d1-135">减法运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="697d1-136">移位运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="697d1-137">整数比较运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="697d1-138">整数逻辑运算符</span><span class="sxs-lookup"><span data-stu-id="697d1-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="697d1-139">缺点</span><span class="sxs-lookup"><span data-stu-id="697d1-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="697d1-140">这些运算符的实际使用可能很小，并且仅限于正在编写较低级别库或互操作代码的最终用户。</span><span class="sxs-lookup"><span data-stu-id="697d1-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="697d1-141">大多数最终用户可能会使用这些较低级别的库，这些库会将本机大小的整数、句柄和互操作代码提取出来。</span><span class="sxs-lookup"><span data-stu-id="697d1-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="697d1-142">因此，他们不需要操作员本身。</span><span class="sxs-lookup"><span data-stu-id="697d1-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="697d1-143">备选项</span><span class="sxs-lookup"><span data-stu-id="697d1-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="697d1-144">让框架通过在 IL 中直接写入来实现所需的运算符。</span><span class="sxs-lookup"><span data-stu-id="697d1-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="697d1-145">此外，运行时还可以提供对框架定义的运算符的内部支持，以便更好地优化最终性能。</span><span class="sxs-lookup"><span data-stu-id="697d1-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="697d1-146">未解决的问题</span><span class="sxs-lookup"><span data-stu-id="697d1-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="697d1-147">设计的哪些部分仍是待定的？</span><span class="sxs-lookup"><span data-stu-id="697d1-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="697d1-148">设计会议</span><span class="sxs-lookup"><span data-stu-id="697d1-148">Design meetings</span></span>

<span data-ttu-id="697d1-149">链接到影响此建议的设计说明，并在一个句子中介绍它们所导致的更改。</span><span class="sxs-lookup"><span data-stu-id="697d1-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>