---
title: 'Зарезервированные атрибуты C#: отслеживание сведений о вызывающем объекте'
ms.date: 04/09/2020
description: Эти атрибуты указывают компилятору создавать сведения о коде, который вызывает член. Для предоставления подробных сведений об отслеживании используются аргументы CallerFilePath, CallerLineNumber и CallerMemberName.
ms.openlocfilehash: ee061d4cae35bdcc0b89007e360e94fee8c5f87c
ms.sourcegitcommit: c91110ef6ee3fedb591f3d628dc17739c4a7071e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389826"
---
# <a name="reserved-attributes-determine-caller-information"></a>Зарезервированные атрибуты: определение сведений о вызывающем объекте

С помощью информационных атрибутов можно получить сведения о вызывающем объекте метода. Можно получить путь к файлу исходного кода, номер строки в исходном коде и имя члена вызывающего объекта. Для получения этих сведений используются атрибуты, которые применяются к необязательным параметрам. Каждый необязательный параметр задает значение по умолчанию. В следующей таблице перечислены информационные атрибуты вызывающего объекта, которые определены в пространстве имен <xref:System.Runtime.CompilerServices?displayProperty=nameWithType>:

|Атрибут|Описание|Type|
|---|---|---|
|<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>|Полный путь исходного файла, содержащего вызывающий объект. Это путь во время компиляции.|`String`|
|<xref:System.Runtime.CompilerServices.CallerLineNumberAttribute>|Номер строки в исходном файле, из которого вызывается метод.|`Integer`|
|<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>|Имя свойства или метода вызывающего объекта.|`String`|

Эти сведения полезны для трассировки, отладки и создания средств диагностики. В следующем примере показано, как использовать информационные атрибуты вызывающего объекта. При каждом вызове метода `TraceMessage` сведения о вызывающем объекте подставляются в качестве аргументов необязательных параметров.

```csharp
public void DoProcessing()
{
    TraceMessage("Something happened.");
}

public void TraceMessage(string message,
        [CallerMemberName] string memberName = "",
        [CallerFilePath] string sourceFilePath = "",
        [CallerLineNumber] int sourceLineNumber = 0)
{
    Trace.WriteLine("message: " + message);
    Trace.WriteLine("member name: " + memberName);
    Trace.WriteLine("source file path: " + sourceFilePath);
    Trace.WriteLine("source line number: " + sourceLineNumber);
}

// Sample Output:
//  message: Something happened.
//  member name: DoProcessing
//  source file path: c:\Visual Studio Projects\CallerInfoCS\CallerInfoCS\Form1.cs
//  source line number: 31
```

Для каждого необязательного параметра указывается явное значение по умолчанию. Нельзя применять информационные атрибуты вызывающего объекта к параметрам, которые не были указаны как необязательные. Информационные атрибуты вызывающего объекта не делают параметр необязательным. Вместо этого они влияют на значение по умолчанию, которое передается, если аргумент был опущен. Информационные значения вызывающего объекта передаются как литералы в IL во время компиляции. В отличие от результатов свойства <xref:System.Exception.StackTrace%2A> для исключений, результаты не затрагиваются запутыванием кода. Можно явно передать необязательные аргументы, чтобы управлять сведениями о вызывающем объекте или скрывать сведения о вызывающем объекте.

### <a name="member-names"></a>Имена членов

Можно использовать атрибут `CallerMemberName`, чтобы не указывать имя члена в виде аргумента `String` вызываемому методу. Этот прием позволяет избежать проблемы, заключающейся в том, что **операция рефакторинга и переименования** не изменяет значений `String`. Это особенно полезно при выполнении следующих задач:

- Использование процедур трассировки и диагностики.
- Реализация интерфейса <xref:System.ComponentModel.INotifyPropertyChanged> при привязке данных. Этот интерфейс позволяет свойству объекта уведомлять связанный элемент управления об изменении свойства, чтобы элемент управления мог отображать обновленные сведения. Если атрибут `CallerMemberName` не используется, необходимо указать имя свойства как литерал.

В следующей таблице представлены имена членов, возвращаемых при использовании атрибута `CallerMemberName`.

|Фрагмент кода, в пределах которого происходят вызовы|Результат имени члена|
|-|-|
|Метод, свойство или событие|Имя метода, свойства или события, из которого происходил вызов.|
|Конструктор|Строка ".ctor"|
|Статический конструктор|Строка ".cctor"|
|Деструктор|Строка "Finalize"|
|Определяемые пользователем операторы и преобразования|Созданное имя члена, например, "op_Addition".|
|Конструктора атрибута|Имя метода или свойства, к которому применяется атрибут. Если атрибут — любой элемент внутри члена (например, параметр, возвращаемое значение или параметр универсального типа), то результат — имя члена, который связан с этим элементом.|
|Нет содержащего члена (например, уровень сборки или атрибуты, примененные к типам)|Значение необязательного параметра по умолчанию.|

## <a name="see-also"></a>См. также

- [Именованные и необязательные аргументы](../../programming-guide/classes-and-structs/named-and-optional-arguments.md)
- <xref:System.Reflection>
- <xref:System.Attribute>
- [Атрибуты](../../../standard/attributes/index.md)