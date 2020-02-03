---
title: Перенос проектов C++/CLI в .NET Core
description: Сведения о переносе проектов C++/CLI в .NET Core.
author: mjrousos
ms.date: 01/10/2020
ms.openlocfilehash: eb03f2a5ff42e8279fd3ebd6ee6fb6d955f6798d
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964863"
---
# <a name="how-to-port-a-ccli-project-to-net-core"></a><span data-ttu-id="ef2e2-103">Перенос проекта C++/CLI в .NET Core</span><span class="sxs-lookup"><span data-stu-id="ef2e2-103">How to port a C++/CLI project to .NET Core</span></span>

<span data-ttu-id="ef2e2-104">Начиная с .NET Core 3.1 и Visual Studio 2019 версии 16.4 [проекты C++/CLI](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) можно ориентировать на .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-104">Beginning with .NET Core 3.1 and Visual Studio 2019 version 16.4, [C++/CLI projects](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) can target .NET Core.</span></span> <span data-ttu-id="ef2e2-105">Такая поддержка позволяет переносить классические приложения для Windows в .NET Core с помощью уровней взаимодействия C++/CLI.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-105">This support makes it possible to port Windows desktop applications with C++/CLI interop layers to .NET Core.</span></span> <span data-ttu-id="ef2e2-106">В этой статье описывается, как перенести проекты C++/CLI из .NET Framework в .NET Core 3.1.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-106">This article describes how to port C++/CLI projects from .NET Framework to .NET Core 3.1.</span></span>

## <a name="ccli-net-core-limitations"></a><span data-ttu-id="ef2e2-107">Ограничения для C++/CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ef2e2-107">C++/CLI .NET Core limitations</span></span>

<span data-ttu-id="ef2e2-108">Существуют некоторые важные ограничения, касающиеся переноса проектов C++/CLI в .NET Core, в сравнении с другими языками:</span><span class="sxs-lookup"><span data-stu-id="ef2e2-108">There are some important limitations to porting C++/CLI projects to .NET Core compared to other languages:</span></span>

* <span data-ttu-id="ef2e2-109">Поддержка переноса проектов из C++/CLI в .NET Core реализована только для Windows.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-109">C++/CLI support for .NET Core is Windows only.</span></span>
* <span data-ttu-id="ef2e2-110">Проекты C++/CLI невозможно ориентировать на .NET Standard, только на .NET Core (или .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-110">C++/CLI projects can't target .NET Standard, only .NET Core (or .NET Framework).</span></span>
* <span data-ttu-id="ef2e2-111">Проекты C++/CLI не поддерживают новый формат файла проекта в стиле пакета SDK.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-111">C++/CLI projects don't support the new SDK-style project file format.</span></span> <span data-ttu-id="ef2e2-112">Вместо этого даже при ориентировании на .NET Core в проектах C++/CLI используется существующий формат файлов VCXPROJ.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-112">Instead, even when targeting .NET Core, C++/CLI projects use the existing vcxproj file format.</span></span>
* <span data-ttu-id="ef2e2-113">Проекты C++/CLI невозможно ориентировать на несколько платформ .NET.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-113">C++/CLI projects can't multitarget multiple .NET platforms.</span></span> <span data-ttu-id="ef2e2-114">Если необходимо создать проект C++/CLI как для .NET Framework, так и для .NET Core, следует использовать отдельные файлы проекта.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-114">If you need to build a C++/CLI project for both .NET Framework and .NET Core, use separate project files.</span></span>
* <span data-ttu-id="ef2e2-115">.NET Core не поддерживает компиляцию `-clr:pure` или `-clr:safe`, только новый параметр `-clr:netcore` (эквивалентный `-clr` для .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-115">.NET Core doesn't support `-clr:pure` or `-clr:safe` compilation, only the new `-clr:netcore` option (which is equivalent to `-clr` for .NET Framework).</span></span>

## <a name="port-a-ccli-project"></a><span data-ttu-id="ef2e2-116">Перенос проекта C++/CLI</span><span class="sxs-lookup"><span data-stu-id="ef2e2-116">Port a C++/CLI project</span></span>

<span data-ttu-id="ef2e2-117">Чтобы перенести проект C++/CLI в .NET Core, внесите следующие изменения в файл VCXPROJ.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-117">To port a C++/CLI project to .NET Core, make the following changes to the vcxproj file.</span></span> <span data-ttu-id="ef2e2-118">Эти шаги по переносу отличаются от действий, которые необходимо выполнить с проектами других типов, поскольку проекты C++/CLI не используют файлы проектов в стиле пакета SDK.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-118">These migration steps differ from the steps needed for other project types because C++/CLI projects don't use SDK-style project files.</span></span>

1. <span data-ttu-id="ef2e2-119">Замените свойства `<CLRSupport>true</CLRSupport>` на `<CLRSupport>NetCore</CLRSupport>`.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-119">Replace `<CLRSupport>true</CLRSupport>` properties with `<CLRSupport>NetCore</CLRSupport>`.</span></span> <span data-ttu-id="ef2e2-120">Это свойство зачастую находится в группах свойств, зависящих от конфигурации, поэтому его может потребоваться заменить в нескольких местах.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-120">This property is often in configuration-specific property groups, so you may need to replace it in multiple places.</span></span>
2. <span data-ttu-id="ef2e2-121">Замените свойства `<TargetFrameworkVersion>` на `<TargetFramework>netcoreapp3.1</TargetFramework>`.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-121">Replace `<TargetFrameworkVersion>` properties with `<TargetFramework>netcoreapp3.1</TargetFramework>`.</span></span>
3. <span data-ttu-id="ef2e2-122">Удалите все ссылки .NET Framework (например, `<Reference Include="System" />`).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-122">Remove any .NET Framework references (like `<Reference Include="System" />`).</span></span> <span data-ttu-id="ef2e2-123">При использовании `<CLRSupport>NetCore</CLRSupport>` сборки пакета SDK для .NET Core упоминаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-123">.NET Core SDK assemblies are automatically referenced when using `<CLRSupport>NetCore</CLRSupport>`.</span></span>
4. <span data-ttu-id="ef2e2-124">При необходимости обновите использование API в файлах CPP, чтобы удалить API, которые недоступны для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-124">Update API usage in cpp files, as necessary, to remove APIs unavailable to .NET Core.</span></span> <span data-ttu-id="ef2e2-125">Поскольку проекты C++/CLI, как правило, представляют собой очень тонкие уровни взаимодействия, зачастую требуется внести не так много изменений.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-125">Because C++/CLI projects tend to be fairly thin interop layers, there are often not many changes needed.</span></span> <span data-ttu-id="ef2e2-126">Можно использовать [анализатор переносимости .NET](../../standard/analyzers/portability-analyzer.md) для обнаружения неподдерживаемых API .NET, используемых двоичными файлами C++/CLI, так же, как и в случае с полностью управляемыми двоичными файлами.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-126">The [.NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md) can be used to identify unsupported .NET APIs used by C++/CLI binaries just as with purely managed binaries.</span></span> <span data-ttu-id="ef2e2-127">Рекомендации по определению возможности переноса кода и обновлению проектов для работы с API .NET Core представлены в [руководстве по переносу библиотек](./libraries.md#determine-portability).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-127">Guidelines for determining code portability and updating projects to work with .NET Core APIs are available in the [library porting guidance](./libraries.md#determine-portability).</span></span>

### <a name="wpf-and-windows-forms-usage"></a><span data-ttu-id="ef2e2-128">Использование WPF и Windows Forms</span><span class="sxs-lookup"><span data-stu-id="ef2e2-128">WPF and Windows Forms usage</span></span>

<span data-ttu-id="ef2e2-129">Проекты .NET C++Core/CLI могут использовать API Windows Forms и WPF.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-129">.NET Core C++/CLI projects can use Windows Forms and WPF APIs.</span></span> <span data-ttu-id="ef2e2-130">Чтобы использовать эти API Windows Desktop, в библиотеки пользовательского интерфейса необходимо добавить явные ссылки на платформы.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-130">To use these Windows desktop APIs, you need to add explicit framework references to the UI libraries.</span></span> <span data-ttu-id="ef2e2-131">Проекты в стиле пакета SDK, использующие API Windows Desktop, ссылаются на необходимые библиотеки платформы автоматически с помощью пакета SDK для `Microsoft.NET.Sdk.WindowsDesktop`.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-131">SDK-style projects that use Windows desktop APIs reference the necessary framework libraries automatically by using the `Microsoft.NET.Sdk.WindowsDesktop` SDK.</span></span> <span data-ttu-id="ef2e2-132">Поскольку проекты C++/CLI не используют формат проекта в стиле пакета SDK, в них следует добавить явные ссылки на платформы при ориентировании на .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-132">Because C++/CLI projects don't use the SDK-style project format, they need to add explicit framework references when targeting .NET Core.</span></span>

<span data-ttu-id="ef2e2-133">Чтобы использовать API Windows Forms, добавьте эту ссылку в файл VCXPROJ:</span><span class="sxs-lookup"><span data-stu-id="ef2e2-133">To use Windows Forms APIs, add this reference to the vcxproj file:</span></span>

```xml
<!-- Reference all of Windows Forms -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WindowsForms" />
```

<span data-ttu-id="ef2e2-134">Чтобы использовать API WPF, добавьте эту ссылку в файл VCXPROJ:</span><span class="sxs-lookup"><span data-stu-id="ef2e2-134">To use WPF APIs, add this reference to the vcxproj file:</span></span>

```xml
<!-- Reference all of WPF -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App.WPF" />
```

<span data-ttu-id="ef2e2-135">Чтобы использовать как API Windows Forms, так и API WPF, добавьте эту ссылку в файл VCXPROJ:</span><span class="sxs-lookup"><span data-stu-id="ef2e2-135">To use both Windows Forms and WPF APIs, add this reference to the vcxproj file:</span></span>

```xml
<!-- Reference the entirety of the Windows desktop framework:
     Windows Forms, WPF, and the types that provide integration between them -->
<FrameworkReference Include="Microsoft.WindowsDesktop.App" />
```

<span data-ttu-id="ef2e2-136">В настоящее время отсутствует возможность добавить эти ссылки с помощью диспетчера ссылок Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-136">Currently, it's not possible to add these references using Visual Studio's reference manager.</span></span> <span data-ttu-id="ef2e2-137">Вместо этого файл проекта требуется обновить вручную.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-137">Instead, update the project file manually.</span></span> <span data-ttu-id="ef2e2-138">Такое обновление можно выполнить в Visual Studio, выгрузив проект, а затем отредактировав файл проекта.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-138">This update can be done in Visual Studio by unloading the project and then editing the project file.</span></span> <span data-ttu-id="ef2e2-139">Можно также использовать другой редактор, например VS Code.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-139">You can also use another editor like VS Code.</span></span>

## <a name="build-without-msbuild"></a><span data-ttu-id="ef2e2-140">Сборка без MSBuild</span><span class="sxs-lookup"><span data-stu-id="ef2e2-140">Build without MSBuild</span></span>

<span data-ttu-id="ef2e2-141">Кроме того, проекты C++/CLI можно создавать без использования MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-141">It's also possible to build C++/CLI projects without using MSBuild.</span></span> <span data-ttu-id="ef2e2-142">Выполните следующие действия, чтобы создать проект C++/CLI для .NET Core непосредственно с помощью файлов *cl.exe* и *link.exe*:</span><span class="sxs-lookup"><span data-stu-id="ef2e2-142">Follow these steps to build a C++/CLI project for .NET Core directly with *cl.exe* and *link.exe*:</span></span>

1. <span data-ttu-id="ef2e2-143">При компиляции передайте `-clr:netcore` в *cl.exe*.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-143">When compiling, pass `-clr:netcore` to *cl.exe*.</span></span>
2. <span data-ttu-id="ef2e2-144">Создайте ссылки на необходимые базовые сборки .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-144">Reference necessary .NET Core reference assemblies.</span></span>
3. <span data-ttu-id="ef2e2-145">При связывании укажите `LibPath` в качестве основного каталога приложения .NET Core (чтобы можно было найти файл *ijwhost.lib*).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-145">When linking, provide the .NET Core app host directory as a `LibPath` (so that *ijwhost.lib* can be found).</span></span>
4. <span data-ttu-id="ef2e2-146">Скопируйте файл *ijwhost.dll* (из основного каталога приложения .NET Core) в выходной каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-146">Copy *ijwhost.dll* (from the .NET Core app host directory) to the project's output directory.</span></span>
5. <span data-ttu-id="ef2e2-147">Убедитесь, что для первого компонента приложения, который будет выполнять управляемый код, существует файл [runtimeconfig.json](https://github.com/dotnet/cli/blob/master/Documentation/specs/runtime-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-147">Make sure a [runtimeconfig.json](https://github.com/dotnet/cli/blob/master/Documentation/specs/runtime-configuration-file.md) file exists for the first component of the application that will run managed code.</span></span> <span data-ttu-id="ef2e2-148">Если приложение имеет управляемую точку входа, файл `runtime.config` будет создан и скопирован автоматически.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-148">If the application has a managed entry point, a `runtime.config` file will be created and copied automatically.</span></span> <span data-ttu-id="ef2e2-149">Тем не менее, если приложение имеет собственную точку входа, для первой библиотеки C++/CLI необходимо создать файл `runtimeconfig.json`, чтобы использовать среду выполнения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-149">If the application has a native entry point, though, you need to create a `runtimeconfig.json` file for the first C++/CLI library to use the .NET Core runtime.</span></span>

## <a name="known-issues"></a><span data-ttu-id="ef2e2-150">Известные проблемы</span><span class="sxs-lookup"><span data-stu-id="ef2e2-150">Known issues</span></span>

<span data-ttu-id="ef2e2-151">Существует несколько известных проблем, на которые следует обратить внимание при работе с проектами C++/CLI, ориентированными на .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-151">There are a couple known issues to look out for when working with C++/CLI projects targeting .NET Core.</span></span>

* <span data-ttu-id="ef2e2-152">Ссылка на платформу WPF в проектах C++/CLI .NET Core в настоящее время вызывает отображение некоторых внешних предупреждений о том, что не удается импортировать символы.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-152">A WPF framework reference in .NET Core C++/CLI projects currently causes some extraneous warnings about being unable to import symbols.</span></span> <span data-ttu-id="ef2e2-153">Эти предупреждения можно спокойно игнорировать. В скором времени это будет исправлено.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-153">These warnings can be safely ignored and should be fixed soon.</span></span>
* <span data-ttu-id="ef2e2-154">Если приложение имеет собственную точку входа, для библиотеки C++/CLI, которая первой выполняет управляемый код, требуется файл [runtimeconfig.json](https://github.com/dotnet/cli/blob/master/Documentation/specs/runtime-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-154">If the application has a native entry point, the C++/CLI library that first executes managed code needs a [runtimeconfig.json](https://github.com/dotnet/cli/blob/master/Documentation/specs/runtime-configuration-file.md) file.</span></span> <span data-ttu-id="ef2e2-155">Этот файл конфигурации используется при запуске среды выполнения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-155">This config file is used when the .NET Core runtime starts.</span></span> <span data-ttu-id="ef2e2-156">Проекты C++/CLI пока не создают файлы `runtimeconfig.json` автоматически во время сборки, поэтому файл необходимо создавать вручную.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-156">C++/CLI projects don't create `runtimeconfig.json` files automatically at build time yet, so the file must be generated manually.</span></span> <span data-ttu-id="ef2e2-157">Если библиотека C++/CLI вызывается из управляемой точки входа, то библиотеке C++/CLI не требуется файл `runtimeconfig.json` (поскольку сборка точки входа будет иметь имя, используемое при запуске среды выполнения).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-157">If a C++/CLI library is called from a managed entry point, then the C++/CLI library doesn't need a `runtimeconfig.json` file (since the entry point assembly will have one that is used when starting the runtime).</span></span> <span data-ttu-id="ef2e2-158">Ниже приведен пример простого файла `runtimeconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="ef2e2-158">A simple sample `runtimeconfig.json` file is shown below.</span></span> <span data-ttu-id="ef2e2-159">Дополнительные сведения см. в [спецификациях на сайте GitHub](https://github.com/dotnet/cli/blob/master/Documentation/specs/runtime-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="ef2e2-159">For more information, see the [spec on GitHub](https://github.com/dotnet/cli/blob/master/Documentation/specs/runtime-configuration-file.md).</span></span>

    ```json
    {
          "runtimeOptions": {
             "tfm": "netcoreapp3.1",
             "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "3.1.0"
             }
          }
    }
    ```