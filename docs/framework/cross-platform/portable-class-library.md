---
title: Кроссплатформенная разработка с переносной библиотекой классов
description: Быстрое и простое создание кроссплатформенных приложений и библиотек для платформ Майкрософт с помощью типа проекта "Переносимая библиотека классов" в .NET.
ms.date: 09/17/2018
helpviewer_keywords:
- Portable Class Library [.NET Framework]
- targeting multiple platforms
- multiple platforms, targeting
ms.assetid: c31e1663-c164-4e65-b66d-d3aa8750a154
ms.openlocfilehash: ced595f62249609f4524d0b4b7bf734929619bfd
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "102402256"
---
# <a name="cross-platform-development-with-the-portable-class-library"></a>Кроссплатформенная разработка с помощью переносимой библиотеки классов

Тип проекта "Переносимая библиотека классов" в Visual Studio позволяет быстро и легко создавать межплатформенные приложения и библиотеки для платформ Майкрософт.

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

Переносимые библиотеки классов уменьшают временные и трудовые затраты на разработку и тестирования кода. Используйте такой тип проекта для создания переносимых сборок .NET Framework, а затем ссылайтесь на них из приложений, предназначенных для различных платформ, таких как .NET Framework, iOS и Mac.

Даже после создания проекта переносимой библиотеки классов в Visual Studio и начала работы над проектом вы сможете изменить целевые платформы. Visual Studio компилирует вашу библиотеку с новыми сборками, что поможет определить, какие изменения следует внести в код.

## <a name="create-a-portable-class-library-project"></a>Создание проекта переносимой библиотеки классов

Чтобы создать переносимую библиотеку классов, используйте шаблон в Visual Studio. Создайте новый проект (**Файл** > **Создать проект**) и в диалоговом окне **Создание проекта** выберите язык программирования (Visual C# или Visual Basic). Затем выберите шаблон **Библиотека классов (устаревшая переносимость)** . Введите имя проекта и нажмите кнопку **ОК**.

Появится диалоговое окно **Добавление переносимой библиотеки классов**. Выберите два или более целевых объектов и нажмите кнопку **ОК**.

![Добавление целевых платформ переносимой библиотеки классов в Visual Studio](media/add-portable-class-library.png)

## <a name="change-targets"></a>Изменение целевых платформ

Вы можете изменить целевые платформы проекта переносимой библиотеки классов при его создании или после начала разработки. Чтобы изменить целевые платформы после создания проекта, в **обозревателе решений** откройте контекстное меню вашего проекта переносимой библиотеки классов (не решения), а затем выберите **Свойства**. На странице свойств проекта на вкладке **Библиотека** указаны платформы, на которые в данный момент нацелен проект.

![Свойства проекта для переносимой библиотеки классов в Visual Studio](media/pcl-project-properties.png)

Чтобы добавить или удалить целевые платформы, нажмите кнопку **Изменить** и установите или снимите соответствующие флажки.

При изменении целевых платформ API-интерфейсы, доступные для разработки проекта, изменятся в соответствии с выбранными платформами. Visual Studio отображает ошибки и предупреждения после изменения целевых платформ.

Если необходимо оценить переносимость сборок перед внесением изменений в Visual Studio, можно использовать [Анализатор переносимости .NET](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).

## <a name="supported-types-and-members"></a>Поддерживаемые типы и члены

Типы и члены, доступные в проектах переносимой библиотеки классов, ограничены несколькими факторами совместимости:

- они должны быть общими для выбранных целевых платформ;

- они должны вести себя аналогичным образом на всех этих платформах;

- они не должны быть кандидатами на вывод из употребления;

- они должны иметь смысл в переносимой среде, особенно если вспомогательные члены не являются переносимыми.

Если член поддерживается в переносимой библиотеке классов и для выбранных целевых платформ, он будет показан в IntelliSense в вашем проекте. Однако помните, что интерфейс API может поддерживаться в переносимой библиотеки классов, но возможность его использования зависит от выбранных целевых платформ.

## <a name="api-differences-in-the-portable-class-library"></a>Отличия API в переносимой библиотеке классов

Для обеспечения совместимости сборок переносимой библиотеки классов на всех поддерживаемых платформах некоторые члены в переносимой библиотеке классов были немного изменены.

## <a name="use-the-portable-class-library"></a>Использование переносимой библиотеки классов

После построения проекта переносимой библиотеки классов просто добавьте ссылку на нее в других проектах. Можно сделать ссылку на проект или на конкретные сборки, содержащие классы, к которым требуется доступ.

Для запуска приложения, ссылающегося на сборку переносимой библиотеки классов, на компьютере должны быть установлены требуемые (или более поздние) версии целевых платформ. Visual Studio содержит все необходимые платформы, поэтому приложение можно запустить без дальнейших изменений на компьютере, который использовался для его разработки.

### <a name="deploy-a-universal-windows-app"></a>Развертывание универсального приложения для Windows

При создании универсального приложения для Windows, которое ссылается на сборку переносимой библиотеки классов, все необходимые компоненты для развертывания приложения уже включены в пакет приложения, другие действия не требуются.

### <a name="deploy-a-net-framework-app"></a>Развертывание приложения .NET Framework

При развертывании приложения .NET Framework, в котором имеется ссылка на сборку переносимой библиотеки классов, необходимо задать зависимость от нужной версии платформы .NET Framework. Задание этой зависимости обеспечивает установку нужной версии вместе с приложением.

- Чтобы создать зависимость с развертыванием ClickOnce, в окне **Обозреватель решений** выберите узел проекта, который требуется опубликовать. (Это проект, в котором имеется ссылка на проект переносимой библиотеки классов.) В строке меню выберите **Проект** > **Свойства**, а затем перейдите на вкладку **Опубликовать**. На странице **Публикация** выберите **Необходимые компоненты**. Отметьте требуемую версию платформы .NET Framework как необходимый компонент.

- Чтобы создать зависимость с проектом установки, в окне **Обозреватель решений** выберите проект установки. В строке меню щелкните **Проект** > **Свойства** > **Необходимые компоненты**. Отметьте требуемую версию платформы .NET Framework как необходимый компонент.

Дополнительные сведения о развертывании приложений платформы .NET Framework см. в разделе [Руководство по развертыванию для разработчиков](../../framework/deployment/deployment-guide-for-developers.md).

## <a name="see-also"></a>См. также раздел

- [Использование переносимой библиотеки классов с шаблоном MVVM](using-portable-class-library-with-model-view-view-model.md)
- [Ресурсы приложений для библиотек под несколько платформ](app-resources-for-libraries-that-target-multiple-platforms.md)
- [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
- [Поддержка платформы .NET Framework для приложений магазина Windows и среды выполнения Windows](support-for-windows-store-apps-and-windows-runtime.md)
- [Развертывание](../../framework/deployment/net-framework-applications.md)