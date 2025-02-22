---
title: Разработка с использованием ссылочных типов, допускающих значение null
description: В этом расширенном руководстве содержатся общие сведения о ссылочных типах, допускающих значение null. Вы научитесь выражать проектный замысел относительно того, когда ссылочные значения могут иметь значение null, и используете компилятор, который будет применять соответствующее поведение, когда они не могут иметь значение null.
ms.date: 02/19/2019
ms.technology: csharp-null-safety
ms.custom: mvc
ms.openlocfilehash: 01b05c6fa71c71546109cb25474b703458eb6e22
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878979"
---
# <a name="tutorial-express-your-design-intent-more-clearly-with-nullable-and-non-nullable-reference-types"></a>Учебник. Четкое выражение проектного замысла с помощью ссылочных типов, допускающих и не допускающих значение null

В C# 8.0 вводятся [ссылочные типы, допускающие значение NULL](../../nullable-references.md), которые дополняют ссылочные типы таким же образом, как типы значений, допускающие значение NULL, дополняют другие типы значений. Чтобы объявить переменную как имеющую **ссылочный тип, допускающий значение null**, добавьте `?` к типу. Например, `string?` соответствует типу `string`, допускающему значение null. Эти новые типы можно использовать для более четкого выражения проектного замысла: некоторые переменные *всегда должны содержать значение*, а в других *они могут отсутствовать*.

В этом руководстве вы узнаете, как:

> [!div class="checklist"]
>
> - Внедрять в проекты ссылочные типы, допускающие и не допускающие значение null.
> - Включать в коде проверку ссылочных типов, допускающих значение null.
> - Писать код, в котором компилятор принудительно применяет эти проектные решения.
> - Использовать ссылочный тип, допускающий значение null, в собственных проектах.

## <a name="prerequisites"></a>Предварительные требования

Вам нужно настроить свой компьютер для выполнения .NET Core, включая компилятор C# 8.0. Компилятор C# 8.0 доступен в [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) или [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet/3.0).

В этом руководстве предполагается, что вы знакомы с C# и .NET, включая Visual Studio или .NET Core CLI.

## <a name="incorporate-nullable-reference-types-into-your-designs"></a>Внедрение в проекты ссылочных типов, допускающих значение null

В этом руководстве вы создадите библиотеку, моделирующую выполнение опроса. Код использует оба ссылочных типа (допускающий и не допускающий значение null) для представления реальных понятий. Вопросы в опросе никогда не могут принимать значение null. Респондент может предпочесть не отвечать на вопрос. В этом случае ответы могут принимать значение `null`.

Код, который вы напишете для этого примера, выражает это намерение, а компилятор применяет его.

## <a name="create-the-application-and-enable-nullable-reference-types"></a>Создание приложения и включение ссылочных типов, допускающих значение null

Создайте новое консольное приложение в Visual Studio или из командной строки с помощью `dotnet new console`. Присвойте приложению имя `NullableIntroduction`. Создав приложение, вам нужно указать, что весь проект компилируется c включенным **контекстом заметок, допускающим значение NULL**. Откройте файл *.csproj* и добавьте элемент `Nullable` в элемент `PropertyGroup`. Задайте для него значение `enable`. Необходимо выбрать функцию **ссылочных типов, допускающих значение NULL**, даже в проектах C# 8.0. Это связано с тем, что когда эта функция будет включена, существующие объявления ссылочных переменных становятся **ссылочными типами, допускающими значение null**. Хотя это решение поможет найти проблемы, где существующий код может не иметь правильных проверок нулевых значений, оно может не точно отражать исходное намерение проекта.

```xml
<Nullable>enable</Nullable>
```

### <a name="design-the-types-for-the-application"></a>Проектирование типов для приложения

Для этого приложения опроса требуется создать ряд классов:

- Класс, моделирующий список вопросов.
- Класс, моделирующий список людей, с которыми связались для опроса.
- Класс, моделирующий ответы человека, принявшего участие в опросе.

Эти типы будут использовать ссылочные типы, допускающие значения null и не допускающие значения null, чтобы выразить, какие элементы являются обязательными, а какие — необязательными. Ссылочные типы, допускающие значение null, четко указывают на это намерение проекта:

- Вопросы, входящие в опрос, не могут иметь значение null: Нет смысла задавать пустой вопрос.
- Респонденты никогда не могут принимать значение null. Следует отслеживать людей, с которыми вы связались, включая респондентов, которые отказались участвовать.
- Любой ответ на вопрос может принимать значение null. Респонденты могут отказаться отвечать на некоторые или все вопросы.

Если вы программировали на C#, возможно, вы так привыкли ссылаться на типы, допускающие значение `null`, что упустили другие возможности для объявления экземпляров, не допускающих значение null:

- Набор вопросов не должен допускать значение null.
- Набор ответов не должен допускать значение null.

При написании кода вы увидите, что если ссылочный тип, не допускающий значение null, является типом по умолчанию для ссылок, это позволяет избежать распространенных ошибок, которые могут привести к исключениям из-за пустых ссылок (<xref:System.NullReferenceException>). В этом руководстве описана важность принятия решений о том, какие переменные могут или не могут принимать значение `null`. Язык не предоставлял синтаксис для выражения этих решений, а теперь предоставляет.

Приложение, которое вы создадите, будет выполнять следующие действия:

1. Создание опроса и добавление в него вопросов.
1. Создание псевдослучайного набора респондентов для опроса.
1. Связь с респондентами, пока размер опроса не достигает целевого значения.
1. Запись важных статистических данных на основе ответов на опрос.

## <a name="build-the-survey-with-nullable-and-non-nullable-reference-types"></a>Создание опроса с типами, допускающими и не допускающими значение NULL

Первый код, который вы напишете, создает опрос. Вы напишете классы для моделирования вопроса и выполнения опроса. Опрос состоит из вопросов трех типов, различающихся форматом ответов: да или нет, числовые ответы и текстовые ответы. Создайте класс `public SurveyQuestion`:

```csharp
namespace NullableIntroduction
{
    public class SurveyQuestion
    {
    }
}
```

Компилятор интерпретирует каждое объявление переменной ссылочного типа как ссылочный тип, **не допускающий значение NULL**, для кода во включенном контексте заметок, допускающих значение NULL. Первое предупреждение вы увидите после добавления свойств текста и типа вопроса, как показано в следующем коде:

```csharp
namespace NullableIntroduction
{
    public enum QuestionType
    {
        YesNo,
        Number,
        Text
    }

    public class SurveyQuestion
    {
        public string QuestionText { get; }
        public QuestionType TypeOfQuestion { get; }
    }
}
```

Так как вы еще не инициализировали `QuestionText`, компилятор выдает предупреждение о том, что свойство, не допускающее значение null, не инициализировано. Для проекта требуется, чтобы текст вопроса не принимал значение null, поэтому необходимо добавить конструктор, чтобы инициализировать свойство и значение `QuestionType`. Законченное определение класса выглядит следующим образом:

[!code-csharp[DefineQuestion](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyQuestion.cs)]

После добавления конструктора предупреждение удаляется. Аргумент конструктора также является ссылочным типом, не допускающим значение null, поэтому компилятор не выдает никаких предупреждений.

Затем создайте класс `public` с именем `SurveyRun`. Этот класс содержит список объектов `SurveyQuestion` и методов для добавления вопросов в опрос, как показано в следующем коде:

```csharp
using System.Collections.Generic;

namespace NullableIntroduction
{
    public class SurveyRun
    {
        private List<SurveyQuestion> surveyQuestions = new List<SurveyQuestion>();

        public void AddQuestion(QuestionType type, string question) =>
            AddQuestion(new SurveyQuestion(type, question));
        public void AddQuestion(SurveyQuestion surveyQuestion) => surveyQuestions.Add(surveyQuestion);
    }
}
```

Как и раньше, необходимо инициализировать объект списка с помощью значения, отличного от null, или компилятор выдаст предупреждение. При второй перегрузке `AddQuestion` не будет проверок значений null, так как они не требуются: вы объявили, что переменная не допускает значение null. Это значение не может быть равно `null`.

Переключитесь на *Program.cs* в редакторе и замените содержимое метода `Main` следующими строками кода:

[!code-csharp[AddQuestions](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#AddQuestions)]

Так как весь проект находится во включенном контексте заметок, допускающих значение NULL, при каждой передаче значения `null` в любой метод, который ожидает ссылочный тип, не допускающий значение NULL, будет отображаться предупреждение. Попробуйте добавить следующую строку в `Main`:

```csharp
surveyRun.AddQuestion(QuestionType.Text, default);
```

## <a name="create-respondents-and-get-answers-to-the-survey"></a>Создание респондентов и получение ответов на опрос

Теперь напишите код, который генерирует ответы на опрос. Этот процесс включает в себя несколько мелких задач:

1. Создать метод, генерирующий объекты-респонденты. Они представляют людей, которых просят пройти опрос.
1. Создать логику, чтобы имитировать процесс задания вопросов респонденту и собирать ответы или отмечать, что респондент не ответил.
1. Повторять опрос до тех пор, пока нужное количество респондентов не ответит на вопросы.

Вам понадобится класс для представления ответа на опрос, поэтому добавьте его сейчас. Включите поддержку значений null. Добавьте свойство `Id` и конструктор, который его инициализирует, как показано в следующем коде:

```csharp
namespace NullableIntroduction
{
    public class SurveyResponse
    {
        public int Id { get; }

        public SurveyResponse(int id) => Id = id;
    }
}
```

Затем добавьте метод `static` для создания новых участников путем генерации случайного идентификатора:

[!code-csharp[GenerateRespondents](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#Random)]

Основная задача этого класса состоит в том, чтобы генерировать ответы участников на вопросы в опросе. Эта задача состоит из нескольких шагов:

1. Запросите участие в опросе. Если пользователь не согласен, верните отсутствующий (или null) ответ.
1. Задайте каждый вопрос и запишите ответы. Любой ответ может отсутствовать (или иметь значение null).

Добавьте приведенный ниже код к классу `SurveyResponse`:

[!code-csharp[AnswerSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#AnswerSurvey)]

Хранилищем для ответов на опрос является `Dictionary<int, string>?`, из которого видно, что оно может принимать значение null. Используйте новую языковую функцию, чтобы объявить намерение проекта как компилятору, так и всем, кто будет работать с кодом позже. Если вы когда-нибудь разыменуете `surveyResponses`, не проверив значения `null`, компилятор отобразит предупреждение. В методе `AnswerSurvey` предупреждение не отображается, так как компилятор может определить, что переменной `surveyResponses` ранее было присвоено значение, отличное от null.

Использование значения `null` для отсутствующих ответов указывает на важную особенность при работе с допускающими значение NULL ссылочными типами: перед вами не стоит цель удалить все значения `null` из программы. Скорее, вам нужно привести написанный вами код в соответствие со своим замыслом. Отсутствующие значения — это неотъемлемая часть кода. И значение `null` хорошо подходит для их выражения. Если вы попытаетесь удалить все значения `null`, вам придется определить другой способ, чтобы выразить такие отсутствующие значения без использования `null`.

Далее необходимо написать метод `PerformSurvey` в классе `SurveyRun`. Добавьте в класс `SurveyRun` следующий код:

[!code-csharp[PerformSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#PerformSurvey)]

В этом случае выбор `List<SurveyResponse>?` с возможностью принимать значение null указывает на то, что ответ может отсутствовать. Это значит, что респонденты еще не участвовали в опросе. Обратите внимание, что респонденты добавляются до тех пор, пока не будет достигнуто целевое значение.

Для запуска опроса осталось добавить вызов в конце метода `Main`:

[!code-csharp[RunSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#RunSurvey)]

## <a name="examine-survey-responses"></a>Анализ ответов на опросы

Далее следует отобразить ответы на опрос. Добавьте код во многие из написанных вами классов. Этот код демонстрирует различие ссылочных типов, допускающих и не допускающих значение null. Начните с добавления элементов, воплощающих выражение, в класс `SurveyResponse`:

[!code-csharp[ReportResponses](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#SurveyStatus)]

Так как `surveyResponses` является ссылочным типом, допускающим значение null, прежде чем отменять ссылку на него, укажите, что проверка требуется. Метод `Answer` возвращает строку, не допускающую значения null, поэтому мы должны охватить вариант отсутствующего ответа, используя оператор объединения со значением null.

Затем добавьте эти три элемента, воплощающие выражение, в класс `SurveyRun`:

[!code-csharp[ReportResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#RunReport)]

Элемент `AllParticipants` должен принимать во внимание, что переменная `respondents` может иметь значение null, но возвращаемое значение не может быть равно null. Если изменить это выражение, удалив `??` и следующую пустую последовательность, компилятор выдаст предупреждение о том, что метод может возвращать `null`, а его сигнатура возвращает тип, не допускающий значение null.

Наконец, добавьте следующий цикл в нижней части метода `Main`:

[!code-csharp[DisplaySurveyResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#WriteAnswers)]

В этом коде не требуется никаких проверок `null`, так как вы разработали базовые интерфейсы так, чтобы они возвращали ссылочные типы, не допускающие значения null.

## <a name="get-the-code"></a>Получите код

Вы можете получить код этого руководства из нашего репозитория [samples](https://github.com/dotnet/samples) в папке [csharp/NullableIntroduction](https://github.com/dotnet/samples/tree/main/csharp/NullableIntroduction).

Экспериментируйте, изменяя объявления типов между ссылочными типами, допускающими и не допускающими значение null. Посмотрите, как создаются различные предупреждения, чтобы избежать случайного разыменования `null`.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о переводе существующего приложения на использование допускающих значение NULL ссылочных типов:
> [!div class="nextstepaction"]
> [Перевод приложения на использование ссылочных типов, допускающих значение NULL](upgrade-to-nullable-references.md)

Узнайте, как использовать ссылочный тип, допускающий значение NULL, при использовании Entity Framework:
> [Основы Entity Framework Core. Работа со ссылочными типами, допускающими значение NULL](/ef/core/miscellaneous/nullable-reference-types)
