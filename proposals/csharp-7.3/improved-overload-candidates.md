---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483576"
---
# <a name="improved-overload-candidates"></a><span data-ttu-id="38829-101">改进了重载候选项</span><span class="sxs-lookup"><span data-stu-id="38829-101">Improved overload candidates</span></span>

## <a name="summary"></a><span data-ttu-id="38829-102">总结</span><span class="sxs-lookup"><span data-stu-id="38829-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="38829-103">几乎每个C#语言更新中都更新了重载决策规则，以提高程序员的体验，避免出现不明确的调用。</span><span class="sxs-lookup"><span data-stu-id="38829-103">The overload resolution rules have been updated in nearly every C# language update to improve the experience for programmers, making ambiguous invocations select the "obvious" choice.</span></span> <span data-ttu-id="38829-104">为了保持向后兼容性，必须小心执行此操作，但由于我们通常会解决错误情况，因此，这些增强功能通常会正常运行。</span><span class="sxs-lookup"><span data-stu-id="38829-104">This has to be done carefully to preserve backward compatibility, but since we are usually resolving what would otherwise be error cases, these enhancements usually work out nicely.</span></span>

1. <span data-ttu-id="38829-105">当方法组同时包含实例和静态成员时，如果在没有实例接收方或上下文的情况下调用，则放弃实例成员，如果使用实例接收方调用，则放弃静态成员。</span><span class="sxs-lookup"><span data-stu-id="38829-105">When a method group contains both instance and static members, we discard the instance members if invoked without an instance receiver or context, and discard the static members if invoked with an instance receiver.</span></span> <span data-ttu-id="38829-106">如果没有接收方，则仅在静态上下文中包含静态成员，否则静态成员和实例成员。</span><span class="sxs-lookup"><span data-stu-id="38829-106">When there is no receiver, we include only static members in a static context, otherwise both static and instance members.</span></span> <span data-ttu-id="38829-107">如果接收方是由于颜色颜色情况而不明确的实例或类型，则可以同时包含这两种情况。</span><span class="sxs-lookup"><span data-stu-id="38829-107">When the receiver is ambiguously an instance or type due to a color-color situation, we include both.</span></span> <span data-ttu-id="38829-108">不能使用隐式此实例接收方的静态上下文包括未定义此实例的成员的主体，如静态成员以及不能使用此实例的位置，例如字段初始值设定项和构造函数初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="38829-108">A static context, where an implicit this instance receiver cannot be used, includes the body of members where no this is defined, such as static members, as well as places where this cannot be used, such as field initializers and constructor-initializers.</span></span>
2. <span data-ttu-id="38829-109">当一个方法组包含类型参数不满足其约束的某些泛型方法时，这些成员将从候选集中移除。</span><span class="sxs-lookup"><span data-stu-id="38829-109">When a method group contains some generic methods whose type arguments do not satisfy their constraints, these members are removed from the candidate set.</span></span>
3. <span data-ttu-id="38829-110">对于方法组转换，返回类型与委托的返回类型不匹配的候选方法将从集中移除。</span><span class="sxs-lookup"><span data-stu-id="38829-110">For a method group conversion, candidate methods whose return type doesn't match up with the delegate's return type are removed from the set.</span></span>
