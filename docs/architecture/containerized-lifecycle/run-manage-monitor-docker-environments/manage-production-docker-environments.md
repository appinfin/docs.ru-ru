---
title: Управление рабочими средами Docker
description: Сведения о ключевых аспектах управления рабочей средой на базе контейнеров.
ms.date: 02/15/2019
ms.openlocfilehash: 7d10f670745f8bac1084b8c33c5acde67bac6229
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673471"
---
# <a name="manage-production-docker-environments"></a><span data-ttu-id="0c26c-103">Управление рабочими средами Docker</span><span class="sxs-lookup"><span data-stu-id="0c26c-103">Manage production Docker environments</span></span>

<span data-ttu-id="0c26c-104">Управление кластером и оркестрация представляют собой процесс управления группой узлов.</span><span class="sxs-lookup"><span data-stu-id="0c26c-104">Cluster management and orchestration is the process of controlling a group of hosts.</span></span> <span data-ttu-id="0c26c-105">Это может включать в себя добавление и удаление узлов в кластере, получение сведений о текущем состоянии узлов и контейнеров, а также запуск и остановку процессов.</span><span class="sxs-lookup"><span data-stu-id="0c26c-105">This can involve adding and removing hosts from a cluster, getting information about the current state of hosts and containers, and starting and stopping processes.</span></span> <span data-ttu-id="0c26c-106">Управление кластером и оркестрация тесно связаны с планированием, так как планировщику необходим доступ к каждому узлу в кластере, чтобы запланировать работу служб.</span><span class="sxs-lookup"><span data-stu-id="0c26c-106">Cluster management and orchestration are closely tied to scheduling because the scheduler must have access to each host in the cluster in order to schedule services.</span></span> <span data-ttu-id="0c26c-107">Поэтому для обеих задач часто используется одно и то же средство.</span><span class="sxs-lookup"><span data-stu-id="0c26c-107">For this reason, the same tool is often used for both purposes.</span></span>

## <a name="container-service-and-management-tools"></a><span data-ttu-id="0c26c-108">Служба контейнеров и средства управления</span><span class="sxs-lookup"><span data-stu-id="0c26c-108">Container Service and management tools</span></span>

<span data-ttu-id="0c26c-109">Служба контейнеров предусматривает быстрое развертывание популярных решений с открытым кодом для кластеризации и оркестрации контейнеров.</span><span class="sxs-lookup"><span data-stu-id="0c26c-109">Container Service provides rapid deployment of popular open-source container clustering and orchestration solutions.</span></span> <span data-ttu-id="0c26c-110">Она использует образы Docker, чтобы гарантировать полную переносимость контейнеров приложений.</span><span class="sxs-lookup"><span data-stu-id="0c26c-110">It uses Docker images to ensure that your application containers are fully portable.</span></span> <span data-ttu-id="0c26c-111">Служба контейнеров позволяет развертывать кластеры DC/OS (основанные на Mesosphere и Apache Mesos) и Docker Swarm, используя шаблоны Azure Resource Manager или портал Azure, чтобы эти приложения можно было масштабировать до тысяч и даже десятков тысяч контейнеров.</span><span class="sxs-lookup"><span data-stu-id="0c26c-111">By using Container Service, you can deploy DC/OS (powered by Mesosphere and Apache Mesos) and Docker Swarm clusters with Azure Resource Manager templates or the Azure portal to ensure that you can scale these applications to thousands—even tens of thousands—of containers.</span></span>

<span data-ttu-id="0c26c-112">Эти кластеры развертываются с помощью наборов масштабирования виртуальных машин Azure. Они также могут использовать возможности сети и хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="0c26c-112">You deploy these clusters by using Azure Virtual Machine Scale Sets, and the clusters take advantage of Azure networking and storage offerings.</span></span> <span data-ttu-id="0c26c-113">Для доступа к службе контейнеров нужна подписка Azure.</span><span class="sxs-lookup"><span data-stu-id="0c26c-113">To access Container Service, you need an Azure subscription.</span></span> <span data-ttu-id="0c26c-114">Служба контейнеров позволяет пользоваться функциями Azure корпоративного уровня, сохраняя при этом переносимость приложений, в том числе на уровнях оркестрации.</span><span class="sxs-lookup"><span data-stu-id="0c26c-114">With Container Service, you can take advantage of the enterprise-grade features of Azure while still maintaining application portability, including at the orchestration layers.</span></span>

<span data-ttu-id="0c26c-115">В таблице 6-1 перечислены стандартные инструменты управления, связанные с оркестраторами, планировщиками и платформой кластеризации.</span><span class="sxs-lookup"><span data-stu-id="0c26c-115">Table 6-1 lists common management tools related to their orchestrators, schedulers, and clustering platform.</span></span>

<span data-ttu-id="0c26c-116">**Таблица 6-1**.</span><span class="sxs-lookup"><span data-stu-id="0c26c-116">**Table 6-1**.</span></span> <span data-ttu-id="0c26c-117">Средства управления Docker</span><span class="sxs-lookup"><span data-stu-id="0c26c-117">Docker management tools</span></span>

| <span data-ttu-id="0c26c-118">Средства управления</span><span class="sxs-lookup"><span data-stu-id="0c26c-118">Management tools</span></span> | <span data-ttu-id="0c26c-119">ОПИСАНИЕ</span><span class="sxs-lookup"><span data-stu-id="0c26c-119">Description</span></span> | <span data-ttu-id="0c26c-120">Связанные оркестраторы</span><span class="sxs-lookup"><span data-stu-id="0c26c-120">Related orchestrators</span></span> |
|------------------|-------------|-----------------------|
| [<span data-ttu-id="0c26c-121">Azure Monitor для контейнеров</span><span class="sxs-lookup"><span data-stu-id="0c26c-121">Azure Monitor for Containers</span></span>](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview) | <span data-ttu-id="0c26c-122">Выделенное средство управления Kubernetes в Azure</span><span class="sxs-lookup"><span data-stu-id="0c26c-122">Azure dedicated Kubernetes management tool</span></span> | <span data-ttu-id="0c26c-123">Службы Azure Kubernetes (AKS)</span><span class="sxs-lookup"><span data-stu-id="0c26c-123">Azure Kubernetes Services (AKS)</span></span> |
| [<span data-ttu-id="0c26c-124">Пользовательский веб-интерфейс Kubernetes (панель мониторинга)</span><span class="sxs-lookup"><span data-stu-id="0c26c-124">Kubernetes Web UI (dashboard)</span></span>](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) | <span data-ttu-id="0c26c-125">Средство управления Kubernetes, позволяющее отслеживать локальный кластер Kubernetes и управлять им</span><span class="sxs-lookup"><span data-stu-id="0c26c-125">Kubernetes management tool, can monitor and manage local Kubernetes cluster</span></span> | <span data-ttu-id="0c26c-126">Служба Azure Kubernetes (AKS)</span><span class="sxs-lookup"><span data-stu-id="0c26c-126">Azure Kubernetes Service (AKS)</span></span><br/><span data-ttu-id="0c26c-127">Локальные средства Kubernetes</span><span class="sxs-lookup"><span data-stu-id="0c26c-127">Local Kubernetes</span></span> |
| [<span data-ttu-id="0c26c-128">Портал Azure для Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0c26c-128">Azure portal for Service Fabric</span></span>](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal)<br/>[<span data-ttu-id="0c26c-129">Azure Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="0c26c-129">Azure Service Fabric Explorer</span></span>](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) | <span data-ttu-id="0c26c-130">Классическая и интернет-версия для управления кластерами Service Fabric в Azure, локальной среде, локальном развертывании и других облаках</span><span class="sxs-lookup"><span data-stu-id="0c26c-130">Online and desktop version for managing Service Fabric clusters, on Azure, on premises, local development, and other clouds</span></span> | <span data-ttu-id="0c26c-131">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0c26c-131">Azure Service Fabric</span></span> |
| [<span data-ttu-id="0c26c-132">Мониторинг контейнеров (Azure Monitor)</span><span class="sxs-lookup"><span data-stu-id="0c26c-132">Container Monitoring (Azure Monitor)</span></span>](https://docs.microsoft.com/azure/azure-monitor/insights/containers) | <span data-ttu-id="0c26c-133">Решение для общего отслеживания контейнеров и управления ими.</span><span class="sxs-lookup"><span data-stu-id="0c26c-133">General container management y monitoring solution.</span></span> <span data-ttu-id="0c26c-134">Позволяет управлять кластерами Kubernetes через [Azure Monitor для контейнеров](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="0c26c-134">Can manage Kubernetes clusters through [Azure Monitor for Containers](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview).</span></span> | <span data-ttu-id="0c26c-135">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0c26c-135">Azure Service Fabric</span></span><br/><span data-ttu-id="0c26c-136">Служба Azure Kubernetes (AKS)</span><span class="sxs-lookup"><span data-stu-id="0c26c-136">Azure Kubernetes Service (AKS)</span></span><br/><span data-ttu-id="0c26c-137">Mesosphere DC/OS и другие.</span><span class="sxs-lookup"><span data-stu-id="0c26c-137">Mesosphere DC/OS and others.</span></span> |

## <a name="azure-service-fabric"></a><span data-ttu-id="0c26c-138">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0c26c-138">Azure Service Fabric</span></span>

<span data-ttu-id="0c26c-139">Другим вариантом для развертывания кластера и управления им является Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0c26c-139">Another choice for cluster-deployment and management is Azure Service Fabric.</span></span> <span data-ttu-id="0c26c-140">[Service Fabric](https://azure.microsoft.com/services/service-fabric/) — это платформа микрослужб Майкрософт, которая включает в себя оркестрацию контейнеров, а также модели программирования для разработчиков, позволяя создавать приложения микрослужб с высокой степенью масштабируемости.</span><span class="sxs-lookup"><span data-stu-id="0c26c-140">[Service Fabric](https://azure.microsoft.com/services/service-fabric/) is a Microsoft microservices platform that includes container orchestration as well as developer programming models to build highly scalable microservices applications.</span></span> <span data-ttu-id="0c26c-141">Service Fabric поддерживает Docker в контейнерах Windows и Linux и может работать на серверах Windows и Linux.</span><span class="sxs-lookup"><span data-stu-id="0c26c-141">Service Fabric supports Docker in Linux and Windows Containers and can run in Windows and Linux servers.</span></span>

<span data-ttu-id="0c26c-142">Ниже приведены средства управления Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="0c26c-142">Following are Service Fabric management tools:</span></span>

- <span data-ttu-id="0c26c-143">[Портал Azure для Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) служит для выполнения операций с кластером (создание/изменение/удаление) или настройки его инфраструктуры (виртуальных машин, подсистемы балансировки нагрузки, сетевого взаимодействия и т. д.)</span><span class="sxs-lookup"><span data-stu-id="0c26c-143">[Azure portal for Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal) cluster-related operations (create/update/delete) a cluster or configure its infrastructure (VMs, load balancer, networking, etc.)</span></span>

- <span data-ttu-id="0c26c-144">[Azure Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) — это многоплатформенное средство, представленное классическим приложением и пользовательским веб-интерфейсом, которое предоставляет определенные ценные сведения о кластере Service Fabric и позволяет выполнять с ним определенные операции на уровне узлов и виртуальных машин, а также на уровне служб и приложений.</span><span class="sxs-lookup"><span data-stu-id="0c26c-144">[Azure Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-visualizing-your-cluster) is a specialized web UI and desktop multi-platform tool that provides insights and certain operations on the Service Fabric cluster, from the nodes/VMs point of view and from the application and services point of view.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="0c26c-145">[Назад](run-microservices-based-applications-in-production.md)
>[Вперед](monitor-containerized-application-services.md)</span><span class="sxs-lookup"><span data-stu-id="0c26c-145">[Previous](run-microservices-based-applications-in-production.md)
[Next](monitor-containerized-application-services.md)</span></span>