---
title: Операторы верхнего уровня — учебник по C#
description: В этом учебнике показано, как использовать инструкции верхнего уровня для проведения экспериментов и подтверждения концепций при анализе идей.
ms.date: 10/28/2020
ms.openlocfilehash: d3cd089c5681e6c06a0c63cbffcc3cf5935fbeef
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878955"
---
# <a name="tutorial-explore-ideas-using-top-level-statements-to-build-code-as-you-learn"></a>Учебник. Анализ идей с использованием операторов верхнего уровня для создания кода по мере обучения

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
>
> - Ознакомитесь с правилами, касающимися использования операторов верхнего уровня.
> - Используете операторы верхнего уровня для изучения алгоритмов.
> - Выполните рефакторинг результатов исследования в многократно используемые компоненты.

## <a name="prerequisites"></a>Предварительные требования

Вам нужно настроить свой компьютер для выполнения .NET 5, включая компилятор C# 9. Компилятор C# 9 доступен, начиная с [Visual Studio 2019 версии 16.9 preview 1](https://visualstudio.microsoft.com/vs/preview/) или [в пакете SDK .NET 5.0](https://dot.net/get-dotnet5).

В этом руководстве предполагается, что вы знакомы с C# и .NET, включая Visual Studio или .NET Core CLI.

## <a name="start-exploring"></a>Начало знакомства

Операторы верхнего уровня позволяют избежать лишних формальностей благодаря помещению точки входа программы в статический метод в классе. Новое консольное приложение обычно начинается со следующего кода.

```csharp
using System;

namespace Application
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

Приведенный выше код является результатом выполнения команды `dotnet new console` для создания консольного приложения. Из этих одиннадцати строк кода только одна является исполняемой. Эту программу можно упростить с помощью новой функции, которая называется операторами верхнего уровня. Она позволяет удалить из этой программы все строки, кроме двух.

```csharp
using System;

Console.WriteLine("Hello World!");
```

Эта функция упрощает изучение новых идей. Операторы верхнего уровня можно использовать для скриптов или исследования возможностей. Получив работающую базовую структуру, можно перейти к рефакторингу кода и создать методы, классы и другие сборки для многократно используемых компонентов. Операторы верхнего уровня позволяют быстро проводить эксперименты и реализовывать упражнения для начинающих. Они также позволяют легко переходить от экспериментов к полнофункциональным программам.

Операторы верхнего уровня выполняются в том порядке, в котором они следуют в файле. Их можно использовать только в одном исходном файле приложения. Если они используются более чем в одном файле, компилятор выдает ошибку.

## <a name="build-a-magic-net-answer-machine"></a>Создание программы для ответа на вопросы на .NET

В этом учебнике мы создадим консольное приложение, которое отвечает на вопрос, требующий ответа "да" или "нет", случайным образом. Его функциональность будет реализована постепенно. Вы можете сосредоточиться на задаче, а не на формальностях, необходимых для построения структуры обычной программы. Когда нужная функциональность будет получена, вы можете выполнить рефакторинг приложения по своему усмотрению.

Начать можно с записи вопроса обратно в консоль. Для этого можно использовать следующий код.

```csharp
using System;

Console.WriteLine(args);
```

Переменная `args` не объявляется. В случае с одним исходным файлом, содержащим операторы верхнего уровня, компилятор распознает `args` как аргументы командной строки. args имеет тип `string[]`, как во всех программах на C#.

Вы можете протестировать код, выполнив следующую команду `dotnet run`.

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?
```

Аргументы после `--` в командной строке передаются в программу. Вы можете увидеть тип переменной `args`, так как в консоли выводится следующее.

```console
System.String[]
```

Чтобы записать вопрос в консоль, необходимо перечислить аргументы через пробел. Замените вызов `WriteLine` следующим кодом.

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="EchoInput":::

Теперь при запуске программы она будет правильно выводить вопрос в виде строки аргументов.

## <a name="respond-with-a-random-answer"></a>Вывод случайного ответа

После вывода вопроса можно добавить код для выбора случайного ответа. Сначала добавьте массив возможных ответов.

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="Answers":::

Этот массив содержит двенадцать утвердительных ответов: шесть неопределенных и шесть отрицательных. Далее добавьте следующий код для выбора случайного ответа из массива и его вывода.

:::code language="csharp" source="snippets/top-level-statements/ProgramSnippets.cs" ID="GenerateAnswer":::

Вы можете снова запустить приложение, чтобы увидеть результаты. Выходные данные должны быть примерно следующими.

```dotnetcli
dotnet run -- Should I use top level statements in all my programs?

Should I use top level statements in all my programs?
Better not tell you now.
```

Этот код отвечает на вопросы, но давайте добавим еще одну функцию. Вы хотите, чтобы ваше приложение делало вид, будто думает над ответом. Для этого можно добавить ASCII-анимацию и сделать небольшую паузу.  Добавьте следующий код после строки, которая выводит на экран вопрос.

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="AnimationFirstPass":::

Необходимо также добавить оператор `using` в начало исходного файла.

```csharp
using System.Threading.Tasks;
```

Операторы `using` должны находиться перед всеми остальными операторами в файле. В противном случае возникает ошибка компилятора. Вы можете снова запустить программу и посмотреть анимацию. Так уже лучше. Поэкспериментируйте с длительностью задержки и выберите понравившуюся.

Приведенный выше код создает несколько вращающихся линий, разделенных пробелами. Добавление ключевого слова `await` предписывает компилятору создать точку входа программы в виде метода с модификатором `async` и возвращает <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>. Эта программа не возвращает значения, поэтому ее точка входа возвращает `Task`. Если программа возвращала бы целочисленное значение, необходимо было бы добавить оператор return в конце блока операторов верхнего уровня. Он указывал бы возвращаемое целочисленное значение. Если операторы верхнего уровня включают в себя выражение `await`, возвращается тип <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>.

## <a name="refactoring-for-the-future"></a>Рефакторинг кода для дальнейшего использования

Код программы должен выглядеть так.

```csharp
using System;
using System.Threading.Tasks;

Console.WriteLine();
foreach(var s in args)
{
    Console.Write(s);
    Console.Write(' ');
}
Console.WriteLine();

for (int i = 0; i < 20; i++)
{
    Console.Write("| -");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("/ \\");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("- |");
    await Task.Delay(50);
    Console.Write("\b\b\b");
    Console.Write("\\ /");
    await Task.Delay(50);
    Console.Write("\b\b\b");
}
Console.WriteLine();

string[] answers =
{
    "It is certain.",       "Reply hazy, try again.",     "Don't count on it.",
    "It is decidedly so.",  "Ask again later.",           "My reply is no.",
    "Without a doubt.",     "Better not tell you now.",   "My sources say no.",
    "Yes – definitely.",    "Cannot predict now.",        "Outlook not so good.",
    "You may rely on it.",  "Concentrate and ask again.", "Very doubtful.",
    "As I see it, yes.",
    "Most likely.",
    "Outlook good.",
    "Yes.",
    "Signs point to yes.",
};

var index = new Random().Next(answers.Length - 1);
Console.WriteLine(answers[index]);
```

Приведенный выше код имеет смысл. Он работает. Но его нельзя использовать многократно. Теперь, когда вы получили работающее приложение, пора извлечь из него многократно используемые части.

Одним из кандидатов является код, отображающий анимацию во время ожидания. Этот фрагмент кода может стать методом.

Можно начать с создания локальной функции в файле. Замените текущую анимацию следующим кодом.

```csharp
await ShowConsoleAnimation();

static async Task ShowConsoleAnimation()
{
    for (int i = 0; i < 20; i++)
    {
        Console.Write("| -");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("/ \\");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("- |");
        await Task.Delay(50);
        Console.Write("\b\b\b");
        Console.Write("\\ /");
        await Task.Delay(50);
        Console.Write("\b\b\b");
    }
    Console.WriteLine();
}
```

Приведенный выше код создает локальную функцию внутри метода main. Его также нельзя использовать повторно. Поэтому давайте извлечем этот код в класс. Создайте файл с именем *utilities.cs* и добавьте следующий код.

:::code language="csharp" source="snippets/top-level-statements/UtilitiesPassOne.cs" ID="SnippetUtilities":::

Файл, содержащий инструкции верхнего уровня, также может содержать пространства имен и типы в конце файла (после инструкций верхнего уровня). Но при работе с этим учебником метод анимации нужно поместить в отдельный файл, чтобы упростить его повторное использование.

Наконец, можно очистить код анимации, удалив повторяющиеся элементы.

:::code language="csharp" source="snippets/top-level-statements/Utilities.cs" ID="Animation":::

Теперь у вас есть законченное приложение и вы выполнили рефакторинг частей, пригодных для многократного использования. Вы можете вызвать новый служебный метод с помощью инструкций верхнего уровня, как показано ниже в готовой версии основной программы:

:::code language="csharp" source="snippets/top-level-statements/Program.cs":::

Таким образом, будет добавлен вызов `Utilities.ShowConsoleAnimation` и дополнительная инструкция `using`.

## <a name="summary"></a>Сводка

Операторы верхнего уровня упрощают создание простых программ для изучения новых алгоритмов. Вы можете экспериментировать с алгоритмами, используя разные фрагменты кода. Определив наиболее эффективный подход, можно выполнить рефакторинг кода, чтобы его было удобнее обслуживать.

Операторы верхнего уровня упрощают программы, основанные на консоли. К ним относятся функции Azure, действия GitHub и другие небольшие служебные программы. Дополнительные сведения см. в статье [Инструкции верхнего уровня (руководство по программированию на C#)](../../programming-guide/main-and-command-args/top-level-statements.md).
