---
ms.openlocfilehash: 4c676a185ff4a7a825acb059bf0a5842ca3922fc
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72393960"
---
### <a name="target-framework-net-framework-support-dropped"></a><span data-ttu-id="09a71-101">Целевая платформа: прекращена поддержка .NET Framework</span><span class="sxs-lookup"><span data-stu-id="09a71-101">Target framework: .NET Framework support dropped</span></span>

<span data-ttu-id="09a71-102">Начиная с ASP.NET Core 3.0, .NET Framework больше поддерживается как целевая платформа.</span><span class="sxs-lookup"><span data-stu-id="09a71-102">Starting with ASP.NET Core 3.0, .NET Framework is an unsupported target framework.</span></span>

#### <a name="change-description"></a><span data-ttu-id="09a71-103">Описание изменений</span><span class="sxs-lookup"><span data-stu-id="09a71-103">Change description</span></span>

<span data-ttu-id="09a71-104">.NET Framework 4.8 — это последняя основная версия .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="09a71-104">.NET Framework 4.8 is the last major version of .NET Framework.</span></span> <span data-ttu-id="09a71-105">Новые приложения ASP.NET Core следует создавать на основе .NET Core.</span><span class="sxs-lookup"><span data-stu-id="09a71-105">New ASP.NET Core apps should be built on .NET Core.</span></span> <span data-ttu-id="09a71-106">Начиная с выпуска .NET Core 3.0 вы можете считать ASP.NET Core 3.0 частью .NET Core.</span><span class="sxs-lookup"><span data-stu-id="09a71-106">Starting with the .NET Core 3.0 release, you can think of ASP.NET Core 3.0 as being part of .NET Core.</span></span>

<span data-ttu-id="09a71-107">Клиенты, которые используют ASP.NET Core совместно с .NET Framework, сохраняют полноценную поддержку в выпуске [.NET Core 2.1 LTS](https://www.microsoft.com/net/download/dotnet-core/2.1).</span><span class="sxs-lookup"><span data-stu-id="09a71-107">Customers using ASP.NET Core with .NET Framework can continue in a fully supported fashion using the [2.1 LTS release](https://www.microsoft.com/net/download/dotnet-core/2.1).</span></span> <span data-ttu-id="09a71-108">Поддержка и обслуживание для выпуска 2.1 сохранится по меньшей мере до 21 августа 2021 г.</span><span class="sxs-lookup"><span data-stu-id="09a71-108">Support and servicing for 2.1 continues until at least August 21, 2021.</span></span> <span data-ttu-id="09a71-109">Это ровно три года с момента объявления выпуска LTS, в строгом соответствии с [политикой поддержки .NET](https://www.microsoft.com/net/platform/support-policy).</span><span class="sxs-lookup"><span data-stu-id="09a71-109">This date is three years after declaration of the LTS release per the [.NET Support Policy](https://www.microsoft.com/net/platform/support-policy).</span></span> <span data-ttu-id="09a71-110">Поддержка пакетов ASP.NET Core 2.1 **на платформе .NET Framework** будет продолжаться неограниченное время, аналогично [политике обслуживания для других платформ ASP.NET на основе пакетов](https://dotnet.microsoft.com/platform/support/policy/aspnet).</span><span class="sxs-lookup"><span data-stu-id="09a71-110">Support for ASP.NET Core 2.1 packages **on .NET Framework** will extend indefinitely, similar to the [servicing policy for other package-based ASP.NET frameworks](https://dotnet.microsoft.com/platform/support/policy/aspnet).</span></span>

<span data-ttu-id="09a71-111">Дополнительные сведения о переносе кода из .NET Core в .NET Framework см. в [этой статье](~/docs/core/porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="09a71-111">For more information about porting from .NET Framework to .NET Core, see [Porting to .NET Core](~/docs/core/porting/index.md).</span></span>

<span data-ttu-id="09a71-112">Это изменение не коснется пакетов `Microsoft.Extensions` (сюда относятся ведение журнала, внедрение зависимостей и конфигурация) и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="09a71-112">`Microsoft.Extensions` packages (such as logging, dependency injection, and configuration) and Entity Framework Core aren't affected.</span></span> <span data-ttu-id="09a71-113">Для них сохраняется поддержка .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="09a71-113">They'll continue to support .NET Standard.</span></span>

<span data-ttu-id="09a71-114">Дополнительные сведения о том, что привело нас в этим изменениям, см. в [исходной записи блога](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0).</span><span class="sxs-lookup"><span data-stu-id="09a71-114">For more information on the motivation for this change, see [the original blog post](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="09a71-115">Представленная версия</span><span class="sxs-lookup"><span data-stu-id="09a71-115">Version introduced</span></span>

<span data-ttu-id="09a71-116">3.0</span><span class="sxs-lookup"><span data-stu-id="09a71-116">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="09a71-117">Старое поведение</span><span class="sxs-lookup"><span data-stu-id="09a71-117">Old behavior</span></span>

<span data-ttu-id="09a71-118">Приложения ASP.NET Core могли работать на основе .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="09a71-118">ASP.NET Core apps could run on either .NET Core or .NET Framework.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="09a71-119">Новое поведение</span><span class="sxs-lookup"><span data-stu-id="09a71-119">New behavior</span></span>

<span data-ttu-id="09a71-120">Приложения ASP.NET Core версии будут выполняться только в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="09a71-120">ASP.NET Core apps can only be run on .NET Core.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="09a71-121">Рекомендуемое действие</span><span class="sxs-lookup"><span data-stu-id="09a71-121">Recommended action</span></span>

<span data-ttu-id="09a71-122">Выполните одно из указанных ниже действий.</span><span class="sxs-lookup"><span data-stu-id="09a71-122">Take one of the following actions:</span></span>

- <span data-ttu-id="09a71-123">Сохраните приложение в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="09a71-123">Keep your app on ASP.NET Core 2.1.</span></span>
- <span data-ttu-id="09a71-124">Перенесите приложения и все его зависимости на .NET Core.</span><span class="sxs-lookup"><span data-stu-id="09a71-124">Migrate your app and dependencies to .NET Core.</span></span>

#### <a name="category"></a><span data-ttu-id="09a71-125">Категория</span><span class="sxs-lookup"><span data-stu-id="09a71-125">Category</span></span>

<span data-ttu-id="09a71-126">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09a71-126">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="09a71-127">Затронутые API</span><span class="sxs-lookup"><span data-stu-id="09a71-127">Affected APIs</span></span>

<span data-ttu-id="09a71-128">Нет</span><span class="sxs-lookup"><span data-stu-id="09a71-128">None</span></span>

<!-- 

#### Affected APIs

Not detectable via API analysis

-->