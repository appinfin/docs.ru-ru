---
title: Централизованная конфигурация
description: Централизованная настройка с помощью Azure Key Vault может упростить управление облачными приложениями.
ms.date: 06/30/2019
ms.openlocfilehash: 4c516b6773d913acd71ff06742302e9766a141e2
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "73841141"
---
# <a name="centralized-configuration"></a><span data-ttu-id="b30b0-103">Централизованная конфигурация</span><span class="sxs-lookup"><span data-stu-id="b30b0-103">Centralized configuration</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="b30b0-104">Облачные приложения используют гораздо больше работающих служб, чем традиционные монолитные приложения с одним экземпляром.</span><span class="sxs-lookup"><span data-stu-id="b30b0-104">Cloud-native applications involve many more running services than traditional single-instance monolithic apps.</span></span> <span data-ttu-id="b30b0-105">Управление параметрами конфигурации для десятков взаимозависимых служб может оказаться сложной задачей, поэтому централизованные хранилища конфигурации часто реализуются для распределенных приложений.</span><span class="sxs-lookup"><span data-stu-id="b30b0-105">Managing configuration settings for dozens of interdependent services can be challenging, which is why centralized configuration stores are often implemented for distributed applications.</span></span>

<span data-ttu-id="b30b0-106">Как обсуждалось в [главе 1](introduction.md), для рекомендаций по развертыванию кода и конфигурации требуется четкое разделение.</span><span class="sxs-lookup"><span data-stu-id="b30b0-106">As discussed in [Chapter 1](introduction.md), the Twelve-Factor App recommendations require strict separation between code and configuration.</span></span> <span data-ttu-id="b30b0-107">Это означает, что сохранение параметров конфигурации в виде констант или литеральных значений в коде является нарушением.</span><span class="sxs-lookup"><span data-stu-id="b30b0-107">This means storing configuration settings as constants or literal values in code is a violation.</span></span> <span data-ttu-id="b30b0-108">Эта рекомендация существует, поскольку один и тот же код должен использоваться в нескольких средах, включая разработку, тестирование, промежуточное хранение и рабочую среду.</span><span class="sxs-lookup"><span data-stu-id="b30b0-108">This recommendation exists because the same code should be used across multiple environments, including development, testing, staging, and production.</span></span> <span data-ttu-id="b30b0-109">Однако значения конфигурации, скорее всего, будут различаться в разных средах.</span><span class="sxs-lookup"><span data-stu-id="b30b0-109">However, configuration values are likely to vary between each of these environments.</span></span> <span data-ttu-id="b30b0-110">Таким образом, значения конфигурации должны храниться в самой среде, или среда должна хранить учетные данные в централизованном хранилище конфигураций.</span><span class="sxs-lookup"><span data-stu-id="b30b0-110">So, configuration values should be stored in the environment itself, or the environment should store the credentials to a centralized configuration store.</span></span>

<span data-ttu-id="b30b0-111">Приложение eShopOnContainers содержит файлы параметров локального приложения для каждой микрослужбы.</span><span class="sxs-lookup"><span data-stu-id="b30b0-111">The eShopOnContainers application includes local application settings files with each microservice.</span></span> <span data-ttu-id="b30b0-112">Эти файлы возвращаются в систему управления версиями, но не включают в себя рабочие секреты, такие как строки подключения или ключи API.</span><span class="sxs-lookup"><span data-stu-id="b30b0-112">These files are checked into source control but don't include production secrets such as connection strings or API keys.</span></span> <span data-ttu-id="b30b0-113">В рабочей среде отдельные параметры могут быть перезаписаны с помощью переменных среды для каждой службы.</span><span class="sxs-lookup"><span data-stu-id="b30b0-113">In production, individual settings may be overwritten with per-service environment variables.</span></span> <span data-ttu-id="b30b0-114">Это распространенная практика для размещенных приложений, но не предоставляет централизованное хранилище конфигураций.</span><span class="sxs-lookup"><span data-stu-id="b30b0-114">This is a common practice for hosted applications, but doesn't provide a central configuration store.</span></span> <span data-ttu-id="b30b0-115">Для поддержки централизованного управления параметрами конфигурации каждая микрослужба включает параметр для переключения между использованием локальных параметров или параметров Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="b30b0-115">To support centralized management of configuration settings, each microservice includes a setting to toggle between its use of local settings or Azure Key Vault settings.</span></span>

## <a name="azure-key-vault"></a><span data-ttu-id="b30b0-116">Хранилище ключей Azure;</span><span class="sxs-lookup"><span data-stu-id="b30b0-116">Azure Key Vault</span></span>

<span data-ttu-id="b30b0-117">Azure Key Vault обеспечивает безопасное хранение маркеров, паролей, сертификатов, ключей API и других конфиденциальных секретов.</span><span class="sxs-lookup"><span data-stu-id="b30b0-117">Azure Key Vault provides secure storage of tokens, passwords, certificates, API keys, and other sensitive secrets.</span></span> <span data-ttu-id="b30b0-118">Для доступа к Key Vault требуется правильная проверка подлинности и авторизация вызывающей стороны, которая в случае микрослужб eShopOnContainers означает использование комбинации ClientId/ClientSecret.</span><span class="sxs-lookup"><span data-stu-id="b30b0-118">Access to Key Vault requires proper caller authentication and authorization, which in the case of the eShopOnContainers microservices means the use of a ClientId/ClientSecret combination.</span></span> <span data-ttu-id="b30b0-119">Не возверяйте эти учетные данные в системе управления версиями, а задавайте их в среде приложения.</span><span class="sxs-lookup"><span data-stu-id="b30b0-119">Don't check in these credentials into source control, but instead set in the application's environment.</span></span> <span data-ttu-id="b30b0-120">Прямой доступ к Key Vault из AKS можно достичь с помощью [Key Vault флексволуме](https://github.com/Azure/kubernetes-keyvault-flexvol).</span><span class="sxs-lookup"><span data-stu-id="b30b0-120">Direct access to Key Vault from AKS can be achieved using [Key Vault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol).</span></span>

<span data-ttu-id="b30b0-121">При централизованной настройке параметры, применяемые ко всему приложению, например централизованную конечную точку ведения журнала, можно задать один раз и использовать в каждой части распределенного приложения.</span><span class="sxs-lookup"><span data-stu-id="b30b0-121">With centralized configuration, settings that apply to the entire application, such as the centralized logging endpoint, can be set once and used by every part of the distributed application.</span></span> <span data-ttu-id="b30b0-122">Несмотря на то, что микрослужбы должны быть независимы друг от друга, обычно все еще являются общими зависимостями, сведения о конфигурации которых могут быть полезны из централизованного хранилища конфигураций.</span><span class="sxs-lookup"><span data-stu-id="b30b0-122">Although microservices should be independent of one another, there will typically still be some shared dependencies whose configuration details can benefit from a centralized configuration store.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="b30b0-123">[Назад](deploy-eshoponcontainers-azure.md)
>[Вперед](scale-applications.md)</span><span class="sxs-lookup"><span data-stu-id="b30b0-123">[Previous](deploy-eshoponcontainers-azure.md)
[Next](scale-applications.md)</span></span>