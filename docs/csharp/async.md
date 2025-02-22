---
title: Асинхронное программирование на C#
description: Сведения о модели асинхронного программирования на уровне языка C#, которая реализуется в .NET Core.
author: cartermp
ms.date: 05/20/2020
ms.technology: csharp-async
ms.assetid: b878c34c-a78f-419e-a594-a2b44fa521a4
ms.openlocfilehash: c3fbcdec2bf181a5211e89024855967d687388ff
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623737"
---
# <a name="asynchronous-programming"></a>Асинхронное программирование

Для решения задач, связанных с вводом-выводом (например, запрос данных из сети, доступ к базе данных или чтение и запись в файловой системе), желательно использовать асинхронное программирование. Если у вас есть код, ограниченный ресурсами процессора, например выполняющий сложные вычисления, то это также подходящий сценарий для асинхронного программирования.

В C# есть модель асинхронного программирования, реализованная на уровне языка, которая позволяет легко писать асинхронный код, не прибегая к обратным вызовам или библиотекам, которые поддерживают асинхронность. Она строится на принципах [асинхронной модели на основе задач (TAP)](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md).

## <a name="overview-of-the-asynchronous-model"></a>Обзор асинхронной модели

В основе асинхронного программирования лежат объекты `Task` и `Task<T>`, которые моделируют асинхронные операции. Они поддерживаются ключевыми словами `async` и `await`. В большинстве случаев модель достаточно проста.

- В коде, ограниченном производительностью ввода-вывода, выполняйте await для операции, которая возвращает `Task` или `Task<T>`, внутри метода `async`.
- В коде, ограниченном ресурсами процессора, выполняйте await для операции, которая запускается в фоновом потоке методом <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType>.

Именно с помощью ключевого слова `await` творится вся магия. Оно передает управление вызывающему объекту метода, который выполнил `await`, позволяя, таким образом, пользовательскому интерфейсу или службе отвечать на запросы. Хотя [существуют и другие способы](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md) реализации асинхронного кода, кроме `async` и `await`, в этой статье рассматриваются только конструкции уровня языка.

### <a name="io-bound-example-download-data-from-a-web-service"></a>Пример кода, ограниченного производительностью ввода-вывода: скачивание данных из веб-службы

Предположим, вам нужно скачать некоторые данные из веб-службы по нажатию кнопки, не блокируя поток пользовательского интерфейса. Это можно сделать так:

```csharp
private readonly HttpClient _httpClient = new HttpClient();

downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    //
    // The UI thread is now free to perform other work.
    var stringData = await _httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};
```

В этом коде намерение (скачивание данных в асинхронном режиме) выражается без запутанных операций с объектами `Task`.

### <a name="cpu-bound-example-perform-a-calculation-for-a-game"></a>Пример кода, ограниченного ресурсами процессора: выполнение вычислений для игры

Предположим, вы разрабатываете игру для мобильных устройств, в которой при нажатии кнопки может наноситься урон множеству противников на экране. Расчет урона может потреблять много ресурсов. Если производить его в потоке пользовательского интерфейса, то на это время игра может приостанавливаться!

Оптимальный способ — запустить фоновый поток, который выполняет задачу с помощью `Task.Run`, а затем ожидать ее результат с помощью `await`. Это обеспечит плавность работы пользовательского интерфейса в процессе вычисления.

```csharp
private DamageResult CalculateDamageDone()
{
    // Code omitted:
    //
    // Does an expensive calculation and returns
    // the result of that calculation.
}

calculateButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI while CalculateDamageDone()
    // performs its work. The UI thread is free to perform other work.
    var damageResult = await Task.Run(() => CalculateDamageDone());
    DisplayDamage(damageResult);
};
```

В этом коде четко выражается назначение события нажатия кнопки, фоновым потоком не требуется управлять вручную, и он выполняется без блокировки.

### <a name="what-happens-under-the-covers"></a>Что происходит на внутреннем уровне

Выполнение асинхронных операций связано со множеством моментов. Если вы хотите знать внутренние принципы работы объектов `Task` и `Task<T>`, прочтите статью [Подробный обзор асинхронного программирования](../standard/async-in-depth.md).

С точки зрения C#, компилятор преобразовывает код в конечный автомат, который контролирует такие моменты, как передача выполнения при достижении `await` и возобновление выполнения после завершения фонового задания.

Если вас интересует теория, это реализация [модели асинхронности на основе обещаний](https://en.wikipedia.org/wiki/Futures_and_promises).

## <a name="key-pieces-to-understand"></a>Ключевые моменты для понимания

* Асинхронный код можно использовать как при ограниченной производительности ввода-вывода, так и при ограниченных ресурсах процессора, но по-разному в каждом случае.
* В асинхронном коде используются конструкции `Task<T>` и `Task`, которые служат для моделирования задач, выполняемых в фоновом режиме.
* Ключевое слово `async` делает метод асинхронным, что позволяет использовать в его теле ключевое слово `await`.
* Когда применяется ключевое слово `await`, оно приостанавливает выполнение вызывающего метода и передает управление обратно вызывающему объекту, пока не будет завершена ожидаемая задача.
* `await` можно использовать только внутри асинхронного метода.

## <a name="recognize-cpu-bound-and-io-bound-work"></a>Различия задач, ограниченных ресурсами процессора и производительностью ввода-вывода

В первых двух примерах этого руководства было показано, как можно использовать `async` и `await` для выполнения задач, ограниченных производительностью ввода-вывода и ресурсами процессора. Крайне важно уметь идентифицировать такие задачи, так как они могут существенно повлиять на производительность кода и привести к неправильному использованию некоторых конструкций.

Перед написанием любого кода нужно ответить на два вопроса.

1. Будет ли код "ожидать" чего-либо, например данных из базы данных?

   Если ответ утвердительный, то ваша задача **ограничена производительностью ввода-вывода**.

1. Будет ли код выполнять сложные вычисления?

   Если ответ утвердительный, то задача **ограничена ресурсами процессора**.

Если ваша задача **ограничена производительностью ввода-вывода**, используйте `async` и `await` *без* `Task.Run`. Библиотеку параллельных задач использовать *не следует*. Причина этого указана в статье [Подробный обзор асинхронного программирования](../standard/async-in-depth.md).

Если ваша задача **ограничена ресурсами процессора** и вам важна скорость реагирования, используйте `async` и `await`, но перенесите выполнение задачи в дополнительный поток, у которого *есть* `Task.Run`. Если к задаче применим параллелизм, рассмотрите возможность использования [библиотеки параллельных задач](../standard/parallel-programming/task-parallel-library-tpl.md).

Кроме того, всегда следует оценивать выполнение кода. Например, затраты на выполнение задачи, ограниченной ресурсами процессора, могут оказаться не столь высокими, как накладные расходы, связанные с переключениями контекста при многопоточности. Каждый вариант имеет свои недостатки, поэтому следует выбрать наиболее компромиссный вариант в вашей ситуации.

## <a name="more-examples"></a>Дополнительные примеры

В приведенных ниже примерах демонстрируются различные способы написания асинхронного кода на C#. Они охватывают несколько сценариев, с которыми вы можете столкнуться.

### <a name="extract-data-from-a-network"></a>Извлечение данных из сети

Этот фрагмент кода скачивает код HTML с главной страницы сайта <https://dotnetfoundation.org> и подсчитывает в нем число вхождений строки ".NET". С помощью ASP.NET он определяет метод контроллера веб-API, который выполняет эту задачу и возвращает число.

> [!NOTE]
> Если вы планируете проанализировать HTML в рабочем коде, не используйте регулярные выражения. Используйте библиотеку анализа.

```csharp
private readonly HttpClient _httpClient = new HttpClient();

[HttpGet, Route("DotNetCount")]
public async Task<int> GetDotNetCount()
{
    // Suspends GetDotNetCount() to allow the caller (the web server)
    // to accept another request, rather than blocking on this one.
    var html = await _httpClient.GetStringAsync("https://dotnetfoundation.org");

    return Regex.Matches(html, @"\.NET").Count;
}
```

Вот аналогичный код для универсального приложения для Windows, который выполняет ту же задачу при нажатии кнопки:

```csharp
private readonly HttpClient _httpClient = new HttpClient();

private async void OnSeeTheDotNetsButtonClick(object sender, RoutedEventArgs e)
{
    // Capture the task handle here so we can await the background task later.
    var getDotNetFoundationHtmlTask = _httpClient.GetStringAsync("https://dotnetfoundation.org");

    // Any other work on the UI thread can be done here, such as enabling a Progress Bar.
    // This is important to do here, before the "await" call, so that the user
    // sees the progress bar before execution of this method is yielded.
    NetworkProgressBar.IsEnabled = true;
    NetworkProgressBar.Visibility = Visibility.Visible;

    // The await operator suspends OnSeeTheDotNetsButtonClick(), returning control to its caller.
    // This is what allows the app to be responsive and not block the UI thread.
    var html = await getDotNetFoundationHtmlTask;
    int count = Regex.Matches(html, @"\.NET").Count;

    DotNetCountLabel.Text = $"Number of .NETs on dotnetfoundation.org: {count}";

    NetworkProgressBar.IsEnabled = false;
    NetworkProgressBar.Visibility = Visibility.Collapsed;
}
```

### <a name="wait-for-multiple-tasks-to-complete"></a>Ожидание выполнения нескольких задач

Может возникнуть ситуация, когда несколько фрагментов данных должны извлекаться одновременно. API-интерфейс `Task` содержит два метода, <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> и <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>, которые позволяют писать асинхронный код, выполняющий неблокирующее ожидание нескольких фоновых заданий.

В этом примере показано, как можно получить данные `User` для набора `userId`.

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<IEnumerable<User>> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = new List<Task<User>>();
    foreach (int userId in userIds)
    {
        getUserTasks.Add(GetUserAsync(userId));
    }

    return await Task.WhenAll(getUserTasks);
}
```

Вот более лаконичный вариант этого кода, написанный с использованием LINQ:

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<User[]> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = userIds.Select(id => GetUserAsync(id));
    return await Task.WhenAll(getUserTasks);
}
```

Хотя размер кода будет меньше, соблюдайте осторожность при использовании LINQ в сочетании с асинхронным кодом. Так как в LINQ используется отложенное выполнение, асинхронные вызовы будут выполняться не немедленно, как в цикле `foreach`, если только вы не производите принудительную итерацию созданной последовательности с помощью вызова `.ToList()` или `.ToArray()`.

## <a name="important-info-and-advice"></a>Важные сведения и советы

При создании асинхронного кода необходимо учитывать ряд моментов, которые позволят избежать непредвиденного поведения.

* **В методах `async` должно присутствовать ключевое слово** `await` **. В противном случае результат не будет получен.**

  Это важно помнить. Если в теле метода `async` не используется ключевое слово `await`, компилятор C# выдаст предупреждение, но код скомпилируется и будет выполняться, как обычный метод. Это крайне неэффективно, так как созданный компилятором C# конечный автомат для асинхронного метода не будет выполнять никакой работы.

* **К имени каждого создаваемого асинхронного метода следует добавлять суффикс Async.**

Это соглашение применяется в .NET для удобной дифференциации синхронных и асинхронных методов. Это не всегда применимо к некоторым методам, которые не вызываются в коде явным образом (например, к обработчикам событий или методам веб-контроллеров). Так как они не вызываются в коде явно, требования к их именованию не так строги.

* `async void` **следует использовать только для обработчиков событий.**

`async void` — это единственный способ обеспечить работу асинхронных обработчиков событий, так как у событий нет типов возвращаемых значений (поэтому они не могут использовать `Task` и `Task<T>`). Любые иные способы применения `async void` не предусмотрены моделью TAP и могут создавать указанные ниже проблемы.

* Исключения, вызываемые в методе `async void`, невозможно перехватывать вне этого метода.
* Методы `async void` очень трудно тестировать.
* Методы `async void` могут иметь негативные побочные эффекты, если вызывающий объект не ожидает, что они будут асинхронными.

* **Будьте осторожны при использовании асинхронных лямбда-выражений в выражениях LINQ**

Для лямбда-выражений в LINQ применяется отложенное выполнение. Это означает, что код может выполняться в произвольный момент, когда вы этого не ожидаете. Неправильное использование блокирующих задач при этом может привести к взаимоблокировке. Кроме того, вложение такого асинхронного кода может усложнить анализ выполнения кода. Асинхронное выполнение и LINQ — эффективные средства, но использовать их следует с максимальной осторожностью и ясным пониманием того, что вы делаете.

* **При написании кода ожидание задач следует реализовывать без блокирования**

Блокирование текущего потока для ожидания завершения `Task` может привести к взаимоблокировкам и блокированию потоков контекста, что потребует более сложной обработки ошибок. В приведенной ниже таблице даются рекомендации по реализации ожидания задач без блокировки.

| Рекомендуемый способ          | Нерекомендуемый способ           | Задача                 |
|----------------------|------------------------------|--------------------------------------------|
| `await`              | `Task.Wait` или `Task.Result` | Получение результата фоновой задачи |
| `await Task.WhenAny` | `Task.WaitAny`               | Ожидание завершения выполнения любой задачи           |
| `await Task.WhenAll` | `Task.WaitAll`               | Ожидание завершения выполнения всех задач          |
| `await Task.Delay`   | `Thread.Sleep`               | Ожидание в течение заданного времени               |

* **Рекомендуется использовать** `ValueTask` **во всех возможных случаях**

В некоторых случаях возврат объекта `Task` из асинхронных методов может вызывать сложности. `Task` — это тип ссылки, поэтому его применение означает распределение объекта. В случаях, когда метод с модификатором `async` возвращает кэшированный результат или завершается синхронно, лишние распределения могут вызывать серьезные потери времени при выполнении фрагментов кода, зависящих от производительности. Эта проблема встает серьезно, если распределения происходят в коротких циклах. Дополнительные сведения см. в разделе [Обобщенные асинхронные типы возвращаемых значений](whats-new/csharp-7.md#generalized-async-return-types).

* **Рекомендуется использовать** `ConfigureAwait(false)`

Часто возникает вопрос: "Когда же нужно использовать метод <xref:System.Threading.Tasks.Task.ConfigureAwait(System.Boolean)?displayProperty=nameWithType>?" Этот метод позволяет экземпляру `Task` настроить ожидающий объект. Это важный элемент, неправильная настройка которого может привести к снижению производительности и даже к взаимоблокировкам. Дополнительные сведения о `ConfigureAwait` см. в [статье с вопросами и ответами по ConfigureAwait](https://devblogs.microsoft.com/dotnet/configureawait-faq).

* **Пишите код с менее строгим отслеживанием состояния**

Старайтесь, чтобы выполнение кода не зависело от состояния глобальных объектов или выполнения определенных методов. Оно должно зависеть только от возвращаемых методами значений. Почему?

* Код будет проще анализировать.
* Код будет проще тестировать.
* Гораздо проще будет сочетать асинхронный и синхронный код.
* Как правило, можно полностью избежать состояний гонки.
* Зависимость от возвращаемых значений упрощает согласование асинхронного кода.
* Дополнительным преимуществом является то, что такой код хорошо работает с внедрением зависимостей.

Следует стремиться к достижению полной или почти полной [ссылочной прозрачности](https://en.wikipedia.org/wiki/Referential_transparency_%28computer_science%29) в коде. Результатом будет высокий уровень предсказуемости базы кода, а также ее пригодности для тестирования и обслуживания.

## <a name="other-resources"></a>Другие источники

* В статье [Подробный обзор асинхронного программирования](../standard/async-in-depth.md) приводятся дополнительные сведения о принципах работы задач.
* [Асинхронная модель программирования задач (C#)](./programming-guide/concepts/async/task-asynchronous-programming-model.md)
* Эпизод [Six Essential Tips for Async](https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async) (Шесть важных советов по асинхронному программированию) с Лусианом Вышиком (Lucian Wischik) — это отличный ресурс по асинхронному программированию.
