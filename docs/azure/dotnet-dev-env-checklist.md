---
title: Контрольный список развертывания .NET в конфигурации Azure
description: В этом разделе приводится краткий обзор всех средств, которые требуется установить для разработки приложений .NET с помощью Azure
ms.date: 1/1/2021
ms.topic: conceptual
ms.custom: devx-track-dotnet
ms.author: daberry
author: daberry
ms.openlocfilehash: a2ff9bbf1e6a8790ddc161a1a1c8d14e8fab6e41
ms.sourcegitcommit: 5d9cee27d9ffe8f5670e5f663434511e81b8ac38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98027250"
---
# <a name="net-on-azure-development-environment-checklist"></a><span data-ttu-id="aefa0-103">Контрольный список развертывания .NET в среде разработки Azure</span><span class="sxs-lookup"><span data-stu-id="aefa0-103">.NET on Azure development environment checklist</span></span>

<span data-ttu-id="aefa0-104">С помощью этого контрольного списка вы можете убедиться, что среда разработки правильно настроена для разработки приложений .NET с помощью Azure.</span><span class="sxs-lookup"><span data-stu-id="aefa0-104">This checklist is provided to help you make sure you have your development environment correctly configured for .NET development with Azure</span></span>

## <a name="create-an-azure-account"></a><span data-ttu-id="aefa0-105">Создание учетной записи Azure</span><span class="sxs-lookup"><span data-stu-id="aefa0-105">Create an Azure account</span></span>

<span data-ttu-id="aefa0-106">Для доступа к службам Azure или запуска приложений в Azure потребуется учетная запись Azure.</span><span class="sxs-lookup"><span data-stu-id="aefa0-106">To access Azure services or run applications in Azure, you need an Azure account.</span></span>

* <span data-ttu-id="aefa0-107">Если вы являетесь подписчиком Visual Studio, вы будете получать ежемесячные [бесплатные кредиты Azure](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).</span><span class="sxs-lookup"><span data-stu-id="aefa0-107">If you are a Visual Studio subscriber, you have monthly [free Azure credits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) available to you every month</span></span>
* <span data-ttu-id="aefa0-108">[Создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/dotnet/), получите кредиты в размере $200 и выберите бесплатные службы на 12-месячный период.</span><span class="sxs-lookup"><span data-stu-id="aefa0-108">[Create a free Azure account](https://azure.microsoft.com/free/dotnet/) and receive $200 in credits and select services free for 12 months</span></span>
* <span data-ttu-id="aefa0-109">Используйте учетную запись, назначенную вам администратором Azure вашей компании.</span><span class="sxs-lookup"><span data-stu-id="aefa0-109">Use an account assigned to you by your company's Azure administrator</span></span>

## <a name="configure-your-ide"></a><span data-ttu-id="aefa0-110">Настройка интегрированной среды разработки (IDE)</span><span class="sxs-lookup"><span data-stu-id="aefa0-110">Configure your IDE</span></span>

<span data-ttu-id="aefa0-111">Для популярных средств, таких как Visual Studio и Visual Studio Code, есть расширения, позволяющие работать с Azure прямо из интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="aefa0-111">Popular tools like Visual Studio and Visual Studio Code have extensions available to let you work with Azure right from your IDE.</span></span>

* <span data-ttu-id="aefa0-112">[Настройте Visual Studio](./configure-visual-studio.md) для разработки приложений .NET с помощью Azure.</span><span class="sxs-lookup"><span data-stu-id="aefa0-112">[Configure Visual Studio](./configure-visual-studio.md) for .NET development using Azure</span></span>
* <span data-ttu-id="aefa0-113">[Настройте Visual Studio Code](./configure-vs-code.md) для разработки приложений .NET с помощью Azure.</span><span class="sxs-lookup"><span data-stu-id="aefa0-113">[Configure Visual Studio Code](./configure-vs-code.md) for .NET Development using Azure</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="aefa0-114">Установка Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aefa0-114">Install the Azure CLI</span></span>

<span data-ttu-id="aefa0-115">В дополнение к порталу Azure вам может потребоваться установить Azure CLI для создания ресурсов Azure и управления ими.</span><span class="sxs-lookup"><span data-stu-id="aefa0-115">In addition to using the Azure portal, you will want to install the Azure CLI to create and manage Azure resources.</span></span>

* <span data-ttu-id="aefa0-116">[Установка Azure CLI для Windows](/cli/azure/install-azure-cli-windows?tabs=azure-cli)</span><span class="sxs-lookup"><span data-stu-id="aefa0-116">[Install the Azure CLI for Windows](/cli/azure/install-azure-cli-windows?tabs=azure-cli))</span></span>
* [<span data-ttu-id="aefa0-117">Установка Azure CLI для macOS</span><span class="sxs-lookup"><span data-stu-id="aefa0-117">Install the Azure CLI for macOS</span></span>](/cli/azure/install-azure-cli-macos)
* [<span data-ttu-id="aefa0-118">Установка Azure CLI для Linux</span><span class="sxs-lookup"><span data-stu-id="aefa0-118">Install the Azure CLI for Linux</span></span>](/cli/azure/install-azure-cli-linux)

## <a name="install-additional-tools-and-utilities"></a><span data-ttu-id="aefa0-119">Установка дополнительных инструментов и служебных программ</span><span class="sxs-lookup"><span data-stu-id="aefa0-119">Install additional tools and utilities</span></span>

<span data-ttu-id="aefa0-120">Эти дополнительные средства предназначены для повышения эффективности работы с Azure.</span><span class="sxs-lookup"><span data-stu-id="aefa0-120">These additional tools are designed to make you more productive when working with Azure.</span></span>

* <span data-ttu-id="aefa0-121">[Установите Azure PowerShell](/powershell/azure/install-az-ps), если вы планируете использовать скрипты PowerShell для создания ресурсов Azure и управления ими.</span><span class="sxs-lookup"><span data-stu-id="aefa0-121">[Install Azure PowerShell](/powershell/azure/install-az-ps) if you plan on using PowerShell scripts to create and manage Azure resources</span></span>
* <span data-ttu-id="aefa0-122">[Установите обозреватель службы хранилища Azure](https://azure.microsoft.com/features/storage-explorer/) для отправки и скачивания данных в ресурсах службы хранилища Azure, включая большие двоичные объекты, очереди, таблицы и базы данных CosmosDB, а также для управления такими данными.</span><span class="sxs-lookup"><span data-stu-id="aefa0-122">[Install Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) to upload, download, and manage data in Azure storage resources including blobs, queues, tables, and CosmosDB.</span></span>
* <span data-ttu-id="aefa0-123">[Установите Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) для работы с Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="aefa0-123">[Install Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) if you are working with Azure SQL</span></span>

## <a name="bookmark-the-following-sites"></a><span data-ttu-id="aefa0-124">Полезные сайты</span><span class="sxs-lookup"><span data-stu-id="aefa0-124">Bookmark the following sites</span></span>

<span data-ttu-id="aefa0-125">При разработке с помощью Azure вам могут часто требоваться следующие сайты.</span><span class="sxs-lookup"><span data-stu-id="aefa0-125">You will use these sites frequently when doing Azure development.</span></span>  <span data-ttu-id="aefa0-126">Добавьте их в закладки для быстрого доступа.</span><span class="sxs-lookup"><span data-stu-id="aefa0-126">Bookmark them for quick reference.</span></span>

* <span data-ttu-id="aefa0-127">Портал Azure — [https://portal.azure.com/](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="aefa0-127">Azure Portal - [https://portal.azure.com/](https://portal.azure.com/)</span></span>
* <span data-ttu-id="aefa0-128">Azure Cloud Shell — [https://shell.azure.com/](https://shell.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="aefa0-128">Azure Cloud Shell - [https://shell.azure.com/](https://shell.azure.com/)</span></span>