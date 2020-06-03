---
title: Порядок модульных тестов
description: Сведения о том, как упорядочить модульные тесты в .NET Core.
author: IEvangelist
ms.date: 05/18/2020
zone_pivot_groups: unit-testing-framework-set-one
ms.openlocfilehash: ce0d01c924075ffcc9ad49ef8aca49222c10c921
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83704535"
---
# <a name="order-unit-tests"></a><span data-ttu-id="46cbf-103">Порядок модульных тестов</span><span class="sxs-lookup"><span data-stu-id="46cbf-103">Order unit tests</span></span>

<span data-ttu-id="46cbf-104">Иногда нужно выполнять модульные тесты в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="46cbf-104">Occasionally, you may want to have unit tests run in a specific order.</span></span> <span data-ttu-id="46cbf-105">В идеальном случае порядок выполнения модульных тестов _не должен_ иметь значение. Мы [настоятельно рекомендуем](unit-testing-best-practices.md) отказаться от упорядочивания модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="46cbf-105">Ideally, the order in which unit tests run should _not_ matter, and it is [best practice](unit-testing-best-practices.md) to avoid ordering unit tests.</span></span> <span data-ttu-id="46cbf-106">Но иногда без этого невозможно обойтись.</span><span class="sxs-lookup"><span data-stu-id="46cbf-106">Regardless, there may be a need to do so.</span></span> <span data-ttu-id="46cbf-107">Если вы столкнулись с такой ситуацией, эта статья поможет вам упорядочить выполнение тестов.</span><span class="sxs-lookup"><span data-stu-id="46cbf-107">In that case, this article demonstrates how to order test runs.</span></span>

<span data-ttu-id="46cbf-108">Если вы предпочитаете изучать исходный код, воспользуйтесь репозиторием примеров [с упорядочением модульных тестов в .NET Core](/samples/dotnet/samples/order-unit-tests-cs).</span><span class="sxs-lookup"><span data-stu-id="46cbf-108">If you prefer to browse the source code, see the [order .NET Core unit tests](/samples/dotnet/samples/order-unit-tests-cs) sample repository.</span></span>

> [!TIP]
> <span data-ttu-id="46cbf-109">Помимо возможностей для упорядочения, которые описаны в этой статье, можно [создать пользовательские списков воспроизведения в Visual Studio](/visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2019#create-custom-playlists).</span><span class="sxs-lookup"><span data-stu-id="46cbf-109">In addition to the ordering capabilities outlined in this article, consider [creating custom playlists with Visual Studio](/visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2019#create-custom-playlists) as an alternative.</span></span>

:::zone pivot="mstest"

## <a name="order-alphabetically"></a><span data-ttu-id="46cbf-110">Упорядочение по алфавиту</span><span class="sxs-lookup"><span data-stu-id="46cbf-110">Order alphabetically</span></span>

<span data-ttu-id="46cbf-111">В MSTest тесты автоматически упорядочиваются по именам.</span><span class="sxs-lookup"><span data-stu-id="46cbf-111">With MSTest, tests are automatically ordered by their test name.</span></span>

> [!NOTE]
> <span data-ttu-id="46cbf-112">Тест с именем `Test14` всегда будет выполняться раньше `Test2`, хотя числовое значение `2` меньше `14`.</span><span class="sxs-lookup"><span data-stu-id="46cbf-112">A test named `Test14` will run before `Test2` even though the number  `2` is less than `14`.</span></span> <span data-ttu-id="46cbf-113">Это связано с тем, что для упорядочения используются текстовые значения имен тестов.</span><span class="sxs-lookup"><span data-stu-id="46cbf-113">This is because, test name ordering uses the text name of the test.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/MSTest.Project/ByAlphabeticalOrder.cs":::

:::zone-end
:::zone pivot="xunit"

<span data-ttu-id="46cbf-114">Платформа тестирования xUnit предоставляет больше возможностей, а также более высокую точность и уровень контроля порядка выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="46cbf-114">The xUnit test framework allows for more granularity and control of test run order.</span></span> <span data-ttu-id="46cbf-115">Для управления порядком выполнения тестовых случаев в классе следует реализовать интерфейсы `ITestCaseOrderer` и `ITestCollectionOrderer` (коллекции тестов).</span><span class="sxs-lookup"><span data-stu-id="46cbf-115">You implement the `ITestCaseOrderer` and `ITestCollectionOrderer` interfaces to control the order of test cases for a class, or test collections.</span></span>

## <a name="order-by-test-case-alphabetically"></a><span data-ttu-id="46cbf-116">Упорядочение тестовых случае по алфавиту</span><span class="sxs-lookup"><span data-stu-id="46cbf-116">Order by test case alphabetically</span></span>

<span data-ttu-id="46cbf-117">Чтобы упорядочить тестовые случаи по имени метода, следует реализовать `ITestCaseOrderer` и предоставить механизм упорядочения.</span><span class="sxs-lookup"><span data-stu-id="46cbf-117">To order test cases by their method name, you implement the `ITestCaseOrderer` and provide an ordering mechanism.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/AlphabeticalOrderer.cs":::

<span data-ttu-id="46cbf-118">Затем задайте порядок тестовых случаев в тестовом классе с помощью `TestCaseOrdererAttribute`.</span><span class="sxs-lookup"><span data-stu-id="46cbf-118">Then in a test class you set the test case order with the `TestCaseOrdererAttribute`.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByAlphabeticalOrder.cs":::

## <a name="order-by-collection-alphabetically"></a><span data-ttu-id="46cbf-119">Упорядочение коллекций по алфавиту</span><span class="sxs-lookup"><span data-stu-id="46cbf-119">Order by collection alphabetically</span></span>

<span data-ttu-id="46cbf-120">Чтобы упорядочить коллекции по отображаемому имени, следует реализовать `ITestCollectionOrderer` и предоставить механизм упорядочения.</span><span class="sxs-lookup"><span data-stu-id="46cbf-120">To order test collections by their display name, you implement the `ITestCollectionOrderer` and provide an ordering mechanism.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/DisplayNameOrderer.cs":::

<span data-ttu-id="46cbf-121">Так как коллекции тестов могут выполняться параллельно, явным образом отключите параллелизацию тестов для коллекций с помощью `CollectionBehaviorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="46cbf-121">Since test collections potentially run in parallel, you must explicitly disable test parallelization of the collections with the `CollectionBehaviorAttribute`.</span></span> <span data-ttu-id="46cbf-122">Затем укажите реализацию в `TestCollectionOrdererAttribute`.</span><span class="sxs-lookup"><span data-stu-id="46cbf-122">Then specify the implementation to the `TestCollectionOrdererAttribute`.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByDisplayName.cs":::

## <a name="order-by-custom-attribute"></a><span data-ttu-id="46cbf-123">Упорядочение по настраиваемому атрибуту</span><span class="sxs-lookup"><span data-stu-id="46cbf-123">Order by custom attribute</span></span>

<span data-ttu-id="46cbf-124">Чтобы упорядочить тесты xUnit по пользовательским атрибутам, прежде всего нужно создать для этого атрибут.</span><span class="sxs-lookup"><span data-stu-id="46cbf-124">To order xUnit tests with custom attributes, you first need an attribute to rely on.</span></span> <span data-ttu-id="46cbf-125">Определите `TestPriorityAttribute` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="46cbf-125">Define a `TestPriorityAttribute` as follows:</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Attributes/TestPriorityAttribute.cs":::

<span data-ttu-id="46cbf-126">Далее мы рассмотрим реализацию `PriorityOrderer` интерфейса `ITestCaseOrderer`.</span><span class="sxs-lookup"><span data-stu-id="46cbf-126">Next, consider the following `PriorityOrderer` implementation of the `ITestCaseOrderer` interface.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/Orderers/PriorityOrderer.cs":::

<span data-ttu-id="46cbf-127">Затем задайте порядок тестовых случаев в тестовом классе с помощью `TestCaseOrdererAttribute` для `PriorityOrderer`.</span><span class="sxs-lookup"><span data-stu-id="46cbf-127">Then in a test class you set the test case order with the `TestCaseOrdererAttribute` to the `PriorityOrderer`.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/XUnit.TestProject/ByPriorityOrder.cs":::

:::zone-end
:::zone pivot="nunit"

## <a name="order-by-priority"></a><span data-ttu-id="46cbf-128">Упорядочение по приоритету</span><span class="sxs-lookup"><span data-stu-id="46cbf-128">Order by priority</span></span>

<span data-ttu-id="46cbf-129">Чтобы явным образом упорядочить тесты, в NUnit можно использовать [`OrderAttribute`](https://github.com/nunit/docs/wiki/Order-Attribute).</span><span class="sxs-lookup"><span data-stu-id="46cbf-129">To order tests explicitly, NUnit provides an [`OrderAttribute`](https://github.com/nunit/docs/wiki/Order-Attribute).</span></span> <span data-ttu-id="46cbf-130">Тесты с этим атрибутом всегда выполняются раньше, чем остальные.</span><span class="sxs-lookup"><span data-stu-id="46cbf-130">Tests with this attribute are started before tests without.</span></span> <span data-ttu-id="46cbf-131">Для определения порядка выполнения модульных тестов используется значение order.</span><span class="sxs-lookup"><span data-stu-id="46cbf-131">The order value is used to determined the order to run the unit tests.</span></span>

:::code language="csharp" source="~/dotnet-samples/csharp/unit-testing/NUnit.TestProject/ByOrder.cs":::

:::zone-end

## <a name="next-steps"></a><span data-ttu-id="46cbf-132">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="46cbf-132">Next Steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="46cbf-133">Рекомендации по модульному тестированию</span><span class="sxs-lookup"><span data-stu-id="46cbf-133">Unit testing best practices</span></span>](unit-testing-best-practices.md)