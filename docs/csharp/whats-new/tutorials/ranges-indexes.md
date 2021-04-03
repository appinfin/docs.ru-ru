---
title: Анализ диапазонов данных с помощью индексов и диапазонов
description: В этом учебнике подробно рассказывается, как анализировать данные с помощью индексов и диапазонов и, таким образом, изучать непрерывный диапазон последовательного набора данных.
ms.date: 09/11/2020
ms.technology: csharp-fundamentals
ms.custom: mvc
ms.openlocfilehash: 828cc9cb9abc0be44bb146ba121ff625c6542f03
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878972"
---
# <a name="indices-and-ranges"></a>Индексы и диапазоны

Диапазоны и индексы обеспечивают лаконичный синтаксис для доступа к отдельным элементам или диапазонам в последовательности.

В этом руководстве вы узнаете, как:

> [!div class="checklist"]
>
> - Использовать этот синтаксис для диапазонов в последовательности.
> - Проектировать начало и конец каждой последовательности.
> - Составлять сценарии для типов <xref:System.Index> и <xref:System.Range>.

## <a name="language-support-for-indices-and-ranges"></a>Поддержка языков для индексов и диапазонов

Поддержка языков опирается на два новых типа и два новых оператора:

- <xref:System.Index?displayProperty=nameWithType> представляет индекс в последовательности.
- Оператор `^` (индекс с конца), который указывает, что индекс указан относительно конца последовательности.
- <xref:System.Range?displayProperty=nameWithType> представляет вложенный диапазон последовательности.
- Оператор диапазона `..`, который задает начало и конец диапазона в качестве своих операндов.

Начнем с правил для использования в индексах. Рассмотрим массив `sequence`. Индекс `0` совпадает с `sequence[0]`. Индекс `^0` совпадает с `sequence[sequence.Length]`. Выражение `sequence[^0]` создает исключение так же, как и `sequence[sequence.Length]`. Для любого числа `n` индекс `^n` совпадает с `sequence[sequence.Length - n]`.

```csharp
string[] words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

Последнее слово можно получить с помощью индекса `^1`. Добавьте следующий код после инициализации:

[!code-csharp[LastIndex](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastIndex)]

Диапазон указывает *начало* и *конец* диапазона. Диапазоны являются открытыми, то есть *конечное значение* не включается в диапазон. Диапазон `[0..^0]` представляет весь диапазон так же, как `[0..sequence.Length]` представляет весь диапазон.

Следующий код создает поддиапазон со словами "quick", "brown" и "fox". Он включает в себя элементы от `words[1]` до `words[3]`. Элемент `words[4]` в диапазон не входит. Добавьте в тот же метод указанный ниже код. Скопируйте и вставьте этот код в нижней части интерактивного окна.

[!code-csharp[Range](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Range)]

Следующий код возвращает диапазон со словами "lazy" и "dog". Он включает элементы `words[^2]` и `words[^1]`. Конечный индекс `words[^0]` не включен. Добавьте также следующий код:

[!code-csharp[LastRange](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastRange)]

В следующих примерах создаются диапазоны, которые должны быть открыты для начала, конца или в обоих случаях:

[!code-csharp[PartialRange](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_PartialRanges)]

Диапазоны или индексы можно также объявлять как переменные. Впоследствии такую переменную можно использовать внутри символов `[` и `]`:

[!code-csharp[IndexRangeTypes](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_RangeIndexTypes)]

Следующий пример демонстрирует многие возможные причины для таких решений. Измените `x`, `y` и `z` и опробуйте различные комбинации. Поэкспериментируйте со значениями, у которых `x` меньше `y`, а `y` меньше, чем `z` для допустимых сочетаний. Добавьте в новый метод приведенный ниже код. Опробуйте различные комбинации:

[!code-csharp[SemanticsExamples](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Semantics)]

## <a name="type-support-for-indices-and-ranges"></a>Поддержка типов для индексов и диапазонов

Индексы и диапазоны предоставляют четкий и краткий синтаксис для доступа к одному элементу или диапазону элементов последовательности. Выражение индекса обычно возвращает тип элементов последовательности. Выражение диапазона обычно возвращает тот же тип последовательности, что и исходная последовательность.

Любой тип, который предоставляет [индексатор](../../programming-guide/indexers/index.md) с параметром <xref:System.Index> или <xref:System.Range>, явно поддерживает индексы или диапазоны соответственно. Индексатор, который принимает один параметр <xref:System.Range>, может возвращать другой тип последовательности, например <xref:System.Span%601?displayProperty=nameWithType>.

> [!IMPORTANT]
> Производительность кода, использующего оператор range, зависит от типа операнда последовательности.
>
> Временная сложность оператора range зависит от типа последовательности. Например, если последовательность является `string` или массивом, то результатом будет копия указанного раздела входных данных, поэтому временная сложность имеет значение *O(N)* (где N — это длина диапазона). С другой стороны, если это <xref:System.Span%601?displayProperty=nameWithType> или <xref:System.Memory%601?displayProperty=nameWithType>, результат ссылается на то же резервное хранилище, что означает отсутствие копии и использование операции *O(1)* .
>
> Помимо временной сложности, создаются дополнительные выделения и копии, влияющие на производительность. В коде, чувствительном к производительности, рекомендуется использовать `Span<T>` или `Memory<T>` в качестве типа последовательности, так как оператор range не предоставляет им выделений.

Тип является **счетным**, если у него есть свойство с именем `Length` или `Count` с доступным методом получения и типом возвращаемого значения `int`. Счетный тип, который не поддерживает индексы или диапазоны явным образом, может предоставить неявную поддержку для них. Дополнительные сведения см. в разделах о [поддержке неявного индекса](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-index-support) и [неявного диапазона](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-range-support) в [примечании к предлагаемой функции](~/_csharplang/proposals/csharp-8.0/ranges.md). Диапазоны с поддержкой неявных диапазонов возвращают тот же тип последовательности, что и исходная.

Например, следующие типы .NET поддерживают как индексы, так и диапазоны: <xref:System.String>, <xref:System.Span%601> и <xref:System.ReadOnlySpan%601>. <xref:System.Collections.Generic.List%601> поддерживает индексы, но не поддерживает диапазоны.

<xref:System.Array> реализует более сложное поведение. Одномерные массивы поддерживают как индексы, так и диапазоны. Многомерные массивы не поддерживают индексаторы или диапазоны. Индексатор для многомерного массива имеет не один, а несколько параметров. Массивы массивов также поддерживают и диапазоны и индексаторы. В следующем примере показано выполнение итерации для прямоугольного подраздела массива массивов. В этом примере выполняется итерация раздела в центре с исключением трех первых и последних строк, а также двух первых и последних столбцов из каждой выбранной строки:

[!code-csharp[JaggedArrays](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_JaggedArrays)]

Во всех случаях оператор range для <xref:System.Array> выделяет массив для хранения возвращаемых элементов.

## <a name="scenarios-for-indices-and-ranges"></a>Сценарии для индексов и диапазонов

Часто диапазоны и индексы используются для анализа отдельной части последовательности большего размера. В новом синтаксисе легче понять, о какой части последовательности идет речь. Локальная функция `MovingAverage` принимает <xref:System.Range> в качестве аргумента. После этого метод перечисляет только это диапазон и вычисляет минимальное, максимальное и среднее значение. Попробуйте добавить в свой проект следующий код:

[!code-csharp[MovingAverages](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_MovingAverage)]