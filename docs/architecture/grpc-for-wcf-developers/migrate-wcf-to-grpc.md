---
title: Перенос решения WCF в gRPC-gRPC для разработчиков WCF
description: Как перенести различные типы служб WCF в эквивалент в gRPC.
ms.date: 09/02/2019
ms.openlocfilehash: 33823d20e1593323a03da12981c5a4534c4d491a
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73971775"
---
# <a name="migrate-a-wcf-solution-to-grpc"></a><span data-ttu-id="650b0-103">Перенос решения WCF в gRPC</span><span class="sxs-lookup"><span data-stu-id="650b0-103">Migrate a WCF solution to gRPC</span></span>

<span data-ttu-id="650b0-104">В этой главе рассматривается работа с проектами ASP.NET Core 3,0 gRPC и демонстрируется перенос различных типов служб WCF в gRPC эквивалент:</span><span class="sxs-lookup"><span data-stu-id="650b0-104">This chapter will look at how to work with ASP.NET Core 3.0 gRPC projects and demonstrate migrating different types of WCF service to the gRPC equivalent:</span></span>

- <span data-ttu-id="650b0-105">Создайте проект ASP.NET Core 3,0 gRPC.</span><span class="sxs-lookup"><span data-stu-id="650b0-105">Create an ASP.NET Core 3.0 gRPC project.</span></span>
- <span data-ttu-id="650b0-106">Простые операции запроса-ответа для gRPC унарного RPC.</span><span class="sxs-lookup"><span data-stu-id="650b0-106">Simple Request-Reply operations to gRPC unary RPC.</span></span>
- <span data-ttu-id="650b0-107">Односторонние операции для gRPC потоковой передачи RPC клиента.</span><span class="sxs-lookup"><span data-stu-id="650b0-107">One-way operations to gRPC client streaming RPC.</span></span>
- <span data-ttu-id="650b0-108">Полные Дуплексные службы для gRPC двунаправленной потоковой передачи RPC.</span><span class="sxs-lookup"><span data-stu-id="650b0-108">Full Duplex services to gRPC bidirectional streaming RPC.</span></span>

<span data-ttu-id="650b0-109">Кроме того, существует Сравнение использования служб потоковой передачи и повторяющихся полей для возврата наборов данных и использования клиентских библиотек в конце главы.</span><span class="sxs-lookup"><span data-stu-id="650b0-109">There's also a comparison of using streaming services versus repeated fields for returning data sets, and the use of client libraries at the end of the chapter.</span></span>

<span data-ttu-id="650b0-110">Пример приложения WCF — это минимальная заглушка набора служб котировки акции с использованием библиотеки контейнеров инверсии управления (IoC) с открытым исходным кодом, именуемой *Autofac* для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="650b0-110">The sample WCF application is a minimal stub of a set of stock trading services, using the open-source Inversion of Control (IoC) container library called *Autofac* for dependency injection.</span></span> <span data-ttu-id="650b0-111">Он включает три службы — по одному для каждого типа службы WCF.</span><span class="sxs-lookup"><span data-stu-id="650b0-111">It includes three services, one for each WCF service type.</span></span> <span data-ttu-id="650b0-112">Службы будут обсуждаться более подробно в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="650b0-112">The services will be discussed in more detail in the following sections.</span></span> <span data-ttu-id="650b0-113">Решения можно скачать из [DotNet-Architecture/GRPC-for-WCF-Developers](https://github.com/dotnet-architecture/grpc-for-wcf-developers) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="650b0-113">The solutions can be downloaded from [dotnet-architecture/grpc-for-wcf-developers](https://github.com/dotnet-architecture/grpc-for-wcf-developers) on GitHub.</span></span> <span data-ttu-id="650b0-114">Службы используют фиктивные данные для сворачивания внешних зависимостей.</span><span class="sxs-lookup"><span data-stu-id="650b0-114">The services use fake data to minimize external dependencies.</span></span>

<span data-ttu-id="650b0-115">Примеры включают реализации WCF и gRPC для каждой службы.</span><span class="sxs-lookup"><span data-stu-id="650b0-115">The samples include the WCF and gRPC implementations of each service.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="650b0-116">[Назад](ws-protocols.md)
>[Вперед](create-project.md)</span><span class="sxs-lookup"><span data-stu-id="650b0-116">[Previous](ws-protocols.md)
[Next](create-project.md)</span></span>