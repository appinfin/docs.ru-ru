---
title: Параметры конфигурации сборщика мусора
description: Сведения о параметрах времени выполнения, определяющих, как сборщик мусора управляет памятью для приложений .NET Core.
ms.date: 07/10/2020
ms.topic: reference
ms.openlocfilehash: c4f55124d9f50146ceac1eea52ce60b0dd77ad1d
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875048"
---
# <a name="run-time-configuration-options-for-garbage-collection"></a>Параметры конфигурации времени выполнения для сборки мусора

Эта страница содержит сведения о параметрах сборщика мусора (GC), которые можно изменить во время выполнения. Если вы пытаетесь добиться пиковой производительности запущенных приложений, рекомендуется использовать эти параметры. Однако значения по умолчанию обеспечивают оптимальную производительность для большинства приложений в типичных ситуациях.

На этой странице параметры упорядочены по группам. Параметры в каждой группе обычно используются совместно друг с другом для достижения определенного результата.

> [!NOTE]
>
> - Эти параметры также могут динамически изменяться приложением во время его выполнения, поэтому любые заданные параметры времени выполнения могут быть переопределены.
> - Некоторые параметры, такие как [уровень задержки](../../standard/garbage-collection/latency.md), обычно задаются только через API во время разработки. Такие параметры будут пропущены на этой странице.
> - Для числовых значений используйте десятичную нотацию для параметров в файле *runtimeconfig.json* и шестнадцатеричную нотацию для параметров переменных среды. Шестнадцатеричные значения можно указать с помощью префикса "0x" или без него.

## <a name="flavors-of-garbage-collection"></a>Варианты сборки мусора

Два основных варианта сборки мусора — это сборка мусора рабочей станции и сборка мусора сервера. Дополнительные сведения о различиях между этими двумя вариантами см. в статье [Сборка мусора рабочей станции и сборка мусора сервера](../../standard/garbage-collection/workstation-server-gc.md).

Подвиды сборки мусора представлены фоновым и непараллельным выполнением.

Чтобы выбрать варианты сборки мусора, используйте следующие параметры:

- [Сборка мусора рабочей станции и сервера](#workstation-vs-server)
- [Фоновая сборка мусора](#background-gc)

### <a name="workstation-vs-server"></a>Рабочая станция и сервер

- Указывает, использует ли приложение сборку мусора рабочей станции или сборку мусора сервера.
- По умолчанию: сборка мусора рабочей станции. Это эквивалентно присвоению значения `false`.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.Server` | `false` — рабочая станция<br/>`true` — сервер | .NET Core 1.0 |
| **Свойство MSBuild** | `ServerGarbageCollection` | `false` — рабочая станция<br/>`true` — сервер | .NET Core 1.0 |
| **Переменная среды** | `COMPlus_gcServer` | `0` — рабочая станция<br/>`1` — сервер | .NET Core 1.0 |
| **app.config для .NET Framework** | [GCServer](../../framework/configure-apps/file-schema/runtime/gcserver-element.md) | `false` — рабочая станция<br/>`true` — сервер |  |

#### <a name="examples"></a>Примеры

Файл *runtimeconfig.json*

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Server": true
      }
   }
}
```

Файл проекта:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ServerGarbageCollection>true</ServerGarbageCollection>
  </PropertyGroup>

</Project>
```

### <a name="background-gc"></a>Фоновая сборка мусора

- Указывает, включена ли фоновая (параллельная) сборка мусора.
- По умолчанию: фоновая сборка мусора. Это эквивалентно присвоению значения `true`.
- Дополнительные сведения см. в статье [Фоновая сборка мусора](../../standard/garbage-collection/background-gc.md).

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.Concurrent` | `true` —фоновая сборка мусора<br/>`false` — непараллельная сборка мусора | .NET Core 1.0 |
| **Свойство MSBuild** | `ConcurrentGarbageCollection` | `true` —фоновая сборка мусора<br/>`false` — непараллельная сборка мусора | .NET Core 1.0 |
| **Переменная среды** | `COMPlus_gcConcurrent` | `1` —фоновая сборка мусора<br/>`0` — непараллельная сборка мусора | .NET Core 1.0 |
| **app.config для .NET Framework** | [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) | `true` —фоновая сборка мусора<br/>`false` — непараллельная сборка мусора |  |

#### <a name="examples"></a>Примеры

Файл *runtimeconfig.json*

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Concurrent": false
      }
   }
}
```

Файл проекта:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="manage-resource-usage"></a>Управление использованием ресурсов

Используйте следующие параметры для управления использованием памяти и ЦП в сборщике мусора:

- [Сходство](#affinitize)
- [Сходство маски](#affinitize-mask)
- [Сходство диапазонов](#affinitize-ranges)
- [Группы ЦП](#cpu-groups)
- [Число куч](#heap-count)
- [Ограничение кучи](#heap-limit)
- [Ограничение кучи в процентах](#heap-limit-percent)
- [Потребление большого объема памяти в процентах](#high-memory-percent)
- [Ограничения для отдельных куч объектов](#per-object-heap-limits)
- [Ограничения для отдельных куч объектов в процентах](#per-object-heap-limit-percents)
- [Сохранение виртуальной машины](#retain-vm)

Дополнительные сведения о некоторых из этих параметров см. в записи блога о [компромиссе между сборкой мусора рабочей станции и сервера](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/).

### <a name="heap-count"></a>Число куч

- Ограничивает число куч, создаваемых сборщиком мусора.
- Применяется только к сборке мусора сервера.
- Если включено [сходство процессоров сборки мусора](#affinitize), что используется по умолчанию, параметр счетчика куч реализует сходство `n` куч/потоков сборки мусора с первыми `n` процессорами. (Используйте параметры [сходства маски](#affinitize-mask) или [сходства диапазонов](#affinitize-ranges), чтобы указать, для каких именно процессоров следует реализовать сходство.)
- Если [сходство процессоров сборки мусора](#affinitize) отключено, этот параметр будет ограничивать количество куч сборки мусора.
- Дополнительные сведения см. в [примечаниях по GCHeapCount](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks).

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapCount` | *Десятичное значение* | .NET Core 3.0 |
| **Переменная среды** | `COMPlus_GCHeapCount` | *Шестнадцатеричное значение* | .NET Core 3.0 |
| **app.config для .NET Framework** | [GCHeapCount](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md) | *Десятичное значение* | .NET Framework 4.6.2 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapCount": 16
      }
   }
}
```

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы ограничить число куч значением 16, для JSON-файла нужно указать 16, а для переменной среды — 0x10 или 10.

### <a name="affinitize-mask"></a>Сходство маски

- Указывает конкретные процессоры, которые должны использоваться потоками сборщика мусора.
- Если [сходство процессоров](#affinitize) отключено, этот параметр игнорируется.
- Применяется только к сборке мусора сервера.
- Это значение представляет собой битовую маску, которая определяет доступные процессу процессоры. Например, десятичное значение 1023 (или шестнадцатеричное значение 0x3FF или 3FF, если вы используете переменную среды), равно 0011 1111 1111 в двоичной нотации. При этом будут использоваться первые 10 процессоров. Чтобы указать следующие 10 процессоров, то есть процессоры 10–19, задайте десятичное значение 1047552 (или шестнадцатеричное значение 0xFFC00 или FFC00), которое эквивалентно двоичному значению 1111 1111 1100 0000 0000.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapAffinitizeMask` | *Десятичное значение* | .NET Core 3.0 |
| **Переменная среды** | `COMPlus_GCHeapAffinitizeMask` | *Шестнадцатеричное значение* | .NET Core 3.0 |
| **app.config для .NET Framework** | [GCHeapAffinitizeMask](../../framework/configure-apps/file-schema/runtime/gcheapaffinitizemask-element.md) | *Десятичное значение* | .NET Framework 4.6.2 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapAffinitizeMask": 1023
      }
   }
}
```

### <a name="affinitize-ranges"></a>Сходство диапазонов

- Указывает список процессоров, используемых для потоков сборщика мусора.
- Этот параметр аналогичен [System.GC.HeapAffinitizeMask](#affinitize-mask), за исключением того, что он позволяет указать больше 64 процессоров.
- Для операционных систем Windows добавьте к номеру или диапазону процессоров префикс в виде соответствующей [группы ЦП](/windows/win32/procthread/processor-groups), например "0:1-10,0:12,1:50-52,1:70".
- Если [сходство процессоров](#affinitize) отключено, этот параметр игнорируется.
- Применяется только к сборке мусора сервера.
- Дополнительные сведения см. в записи об [улучшении конфигурации ЦП для сборки мусора на компьютерах, имеющих более 64 ЦП](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/), в блоге Майони Стивенс (Maoni Stephens).

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.GCHeapAffinitizeRanges` | Разделенный запятыми список номеров процессоров или диапазонов номеров процессоров.<br/>Пример для Unix: "1-10,12,50-52,70"<br/>Пример для Windows: "0:1-10,0:12,1:50-52,1:70" | .NET Core 3.0 |
| **Переменная среды** | `COMPlus_GCHeapAffinitizeRanges` | Разделенный запятыми список номеров процессоров или диапазонов номеров процессоров.<br/>Пример для Unix: "1-10,12,50-52,70"<br/>Пример для Windows: "0:1-10,0:12,1:50-52,1:70" | .NET Core 3.0 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.GCHeapAffinitizeRanges": "0:1-10,0:12,1:50-52,1:70"
      }
   }
}
```

### <a name="cpu-groups"></a>Группы ЦП

- Указывает, использует ли сборщик мусора [группы ЦП](/windows/win32/procthread/processor-groups).

  Когда 64-разрядный компьютер с Windows имеет несколько групп ЦП, то есть доступно более 64 процессоров, включение этого элемента расширяет действие сборки мусора на все группы ЦП. Сборщик мусора использует все ядра для создания и балансировки куч.

- Применяется только к сборке мусора сервера в 64-разрядных операционных системах Windows.
- По умолчанию: сборка мусора не распространяется на группы ЦП. Это эквивалентно присвоению значения `0`.
- Дополнительные сведения см. в записи об [улучшении конфигурации ЦП для сборки мусора на компьютерах, имеющих более 64 ЦП](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/), в блоге Майони Стивенс (Maoni Stephens).

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.CpuGroup` | `0` — отключено<br/>`1` — включено | .NET 5.0 |
| **Переменная среды** | `COMPlus_GCCpuGroup` | `0` — отключено<br/>`1` — включено | .NET Core 1.0 |
| **app.config для .NET Framework** | [GCCpuGroup](../../framework/configure-apps/file-schema/runtime/gccpugroup-element.md) | `false` — отключено<br/>`true` — включено |  |

> [!NOTE]
> Чтобы настроить среду CLR для распределения потоков из пула потоков по всем группам ЦП, включите параметр [Элемент Thread_UseAllCpuGroups](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md). Для приложений .NET Core этот параметр можно включить, задав для переменной среды `COMPlus_Thread_UseAllCpuGroups` значение `1`.

### <a name="affinitize"></a>Сходство

- Указывает, следует ли *реализовать сходство* потоков сборки мусора с процессорами. Реализация сходства потока сборки мусора означает, что он может выполняться только на определенном ЦП. Куча создается для каждого потока сборки мусора.
- Применяется только к сборке мусора сервера.
- По умолчанию: реализация сходства потоков сборки мусора с процессорами. Это эквивалентно присвоению значения `false`.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.NoAffinitize` | `false` — реализовать сходство<br/>`true` — не реализовывать сходство | .NET Core 3.0 |
| **Переменная среды** | `COMPlus_GCNoAffinitize` | `0` — реализовать сходство<br/>`1` — не реализовывать сходство | .NET Core 3.0 |
| **app.config для .NET Framework** | [GCNoAffinitize](../../framework/configure-apps/file-schema/runtime/gcnoaffinitize-element.md) | `false` — реализовать сходство<br/>`true` — не реализовывать сходство | .NET Framework 4.6.2 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.NoAffinitize": true
      }
   }
}
```

### <a name="heap-limit"></a>Ограничение кучи

- Указывает максимальный размер фиксации в байтах для кучи сборки мусора и учета сборки мусора.
- Этот параметр применим только к 64-разрядным компьютерам.
- Если настроены [ограничения для отдельных куч объектов](#per-object-heap-limits), этот параметр игнорируется.
- Значение по умолчанию, которое применяется только в определенных случаях, превышает 20 МБ или 75 % предельного объема памяти в контейнере. Значение по умолчанию применяется, если:

  - процесс выполняется в контейнере с заданным предельным объемом памяти;
  - параметр [System.GC.HeapHardLimitPercent](#heap-limit-percent) не задан.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimit` | *Десятичное значение* | .NET Core 3.0 |
| **Переменная среды** | `COMPlus_GCHeapHardLimit` | *Шестнадцатеричное значение* | .NET Core 3.0 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimit": 209715200
      }
   }
}
```

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы задать для куч жесткое ограничение в 200 мебибайт (Миб), для JSON-файла нужно указать значение 209715200, а для переменной среды — значение 0xC800000 или C800000.

### <a name="heap-limit-percent"></a>Ограничение кучи в процентах

- Указывает допустимое использование кучи сборки мусора в процентах от общего объема физической памяти.
- Если также задан параметр [System.GC.HeapHardLimit](#heap-limit), этот параметр игнорируется.
- Этот параметр применим только к 64-разрядным компьютерам.
- Если процесс выполняется в контейнере с заданным предельным объемом памяти, процентная доля вычисляется как процент от этого объема памяти.
- Если настроены [ограничения для отдельных куч объектов](#per-object-heap-limits), этот параметр игнорируется.
- Значение по умолчанию, которое применяется только в определенных случаях, не превышает 20 МБ или 75 % предельного объема памяти в контейнере. Значение по умолчанию применяется, если:

  - процесс выполняется в контейнере с заданным предельным объемом памяти;
  - параметр [System.GC.HeapHardLimit](#heap-limit) не задан.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPercent` | *Десятичное значение* | .NET Core 3.0 |
| **Переменная среды** | `COMPlus_GCHeapHardLimitPercent` | *Шестнадцатеричное значение* | .NET Core 3.0 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimitPercent": 30
      }
   }
}
```

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы ограничить использование кучи значением 30 %, для JSON-файла нужно указать 30, а для переменной среды — 0x1E или 1E.

### <a name="per-object-heap-limits"></a>Ограничения для отдельных куч объектов

Вы можете настраивать допустимый объем использования кучи в ходе сборки мусора для отдельных куч объектов. Это можно сделать для кучи больших (LOH), малых (SOH) и закрепленных (POH) объектов.

- Если задано значение любого из этих параметров (`COMPLUS_GCHeapHardLimitSOH`, `COMPLUS_GCHeapHardLimitLOH` или `COMPLUS_GCHeapHardLimitPOH`), также необходимо указать значения `COMPLUS_GCHeapHardLimitSOH` и `COMPLUS_GCHeapHardLimitLOH`. Если этого не сделать, во время выполнения произойдет сбой инициализации.
- Значение по умолчанию для `COMPLUS_GCHeapHardLimitPOH` равно 0. Параметры `COMPLUS_GCHeapHardLimitSOH` и `COMPLUS_GCHeapHardLimitLOH` не имеют значений по умолчанию.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitSOH` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPLUS_GCHeapHardLimitSOH` | *Шестнадцатеричное значение* | .NET 5.0 |

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitLOH` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPLUS_GCHeapHardLimitLOH` | *Шестнадцатеричное значение* | .NET 5.0 |

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPOH` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPLUS_GCHeapHardLimitPOH` | *Шестнадцатеричное значение* | .NET 5.0 |

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы задать для куч жесткое ограничение в 200 мебибайт (Миб), для JSON-файла нужно указать значение 209715200, а для переменной среды — значение 0xC800000 или C800000.

### <a name="per-object-heap-limit-percents"></a>Ограничения для отдельных куч объектов в процентах

Вы можете настраивать допустимый объем использования кучи в ходе сборки мусора для отдельных куч объектов. Это можно сделать для кучи больших (LOH), малых (SOH) и закрепленных (POH) объектов.

- Если задано значение любого из этих параметров (`COMPLUS_GCHeapHardLimitSOHPercent`, `COMPLUS_GCHeapHardLimitLOHPercent` или `COMPLUS_GCHeapHardLimitPOHPercent`), также необходимо указать значения `COMPLUS_GCHeapHardLimitSOHPercent` и `COMPLUS_GCHeapHardLimitLOHPercent`. Если этого не сделать, во время выполнения произойдет сбой инициализации.
- Эти параметры игнорируются, если заданы значения параметров `COMPLUS_GCHeapHardLimitSOH`, `COMPLUS_GCHeapHardLimitLOH` и `COMPLUS_GCHeapHardLimitPOH`.
- Значение 1 означает, что при сборке мусора используется 1 % от общего объема физической памяти для соответствующей кучи объектов.
- Задаваемое значение должно быть больше нуля и меньше 100. Кроме того, сумма всех трех процентных значений должна быть меньше 100. В противном случае во время выполнения произойдет сбой инициализации.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitSOHPercent` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPLUS_GCHeapHardLimitSOHPercent` | *Шестнадцатеричное значение* | .NET 5.0 |

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitLOHPercent` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPLUS_GCHeapHardLimitLOHPercent` | *Шестнадцатеричное значение* | .NET 5.0 |

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPOHPercent` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPLUS_GCHeapHardLimitPOHPercent` | *Шестнадцатеричное значение* | .NET 5.0 |

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы ограничить использование кучи значением 30 %, для JSON-файла нужно указать 30, а для переменной среды — 0x1E или 1E.

### <a name="high-memory-percent"></a>Потребление большого объема памяти в процентах

Потребление памяти обозначается в процентах использования физической памяти. По умолчанию, когда использование физической памяти достигает **90 %** , сборка мусора начинает работать активнее и становится более компактной, чтобы избежать подкачки. Пока загрузка памяти составляет менее 90 %, сборщик мусора вместо фоновых выполняет полные сборки мусора, паузы при которых короче, зато общий размер кучи намного сокращается. На компьютерах с большим объемом памяти (80 ГБ или более) порог загрузки по умолчанию составляет от 90 до 97 %.

Максимальное пороговое значение загрузки памяти можно изменить с помощью `COMPlus_GCHighMemPercent` переменной среды или параметра конфигурации JSON `System.GC.HighMemoryPercent`. Чтобы изменить размер кучи, скорректируйте пороговое значение. Например, если основной процесс выполняется на компьютере с 64 ГБ памяти, будет разумно, чтобы сборщик мусора начинал реагировать, когда доступной памяти становится 10 %. А вот при небольших процессах, например, когда процесс задействует только 1 ГБ памяти, сборщик мусора может комфортно работать, даже если доступной памяти меньше 10 %. Для таких небольших процессов можно устанавливать более высокое пороговое значение. С другой стороны, если вы хотите уменьшить размер кучи для процессов побольше (даже при достаточном объеме физической памяти), уменьшите это пороговое значение, чтобы сборщик мусора начинал сжимать кучу раньше.

> [!NOTE]
> Для процессов, выполняемых в контейнере, сборщик мусора считает физическую память, исходя из ограничения контейнера.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HighMemoryPercent` | *Десятичное значение* | .NET 5.0 |
| **Переменная среды** | `COMPlus_GCHighMemPercent` | *Шестнадцатеричное значение* | |

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы задать пороговое значение загрузки памяти в 75 %, для JSON-файла нужно указать значение 75, а для переменной среды — 0x4B или 4B.

### <a name="retain-vm"></a>Сохранение виртуальной машины

- Настраивает, будут ли удаляемые сегменты помещаться в список ожидания для будущего использования или возвращаться операционной системе (ОС).
- По умолчанию: возвращение сегментов операционной системе. Это эквивалентно присвоению значения `false`.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.RetainVM` | `false` — возвращение операционной системе<br/>`true` — помещение в режим ожидания | .NET Core 1.0 |
| **Свойство MSBuild** | `RetainVMGarbageCollection` | `false` — возвращение операционной системе<br/>`true` — помещение в режим ожидания | .NET Core 1.0 |
| **Переменная среды** | `COMPlus_GCRetainVM` | `0` — возвращение операционной системе<br/>`1` — помещение в режим ожидания | .NET Core 1.0 |

#### <a name="examples"></a>Примеры

Файл *runtimeconfig.json*

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.RetainVM": true
      }
   }
}
```

Файл проекта:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="large-pages"></a>Большие страницы

- Указывает, следует ли использовать большие страницы, когда задано жесткое ограничение куч.
- По умолчанию: не следует использовать большие страницы, если задан фиксированный предел кучи. Это эквивалентно присвоению значения `0`.
- Это экспериментальный параметр.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | Н/Д | Н/Д | Н/Д |
| **Переменная среды** | `COMPlus_GCLargePages` | `0` — отключено<br/>`1` — включено | .NET Core 3.0 |

## <a name="allow-large-objects"></a>Разрешить большие объекты

- Настраивает для сборщика мусора на 64-разрядных платформах поддержку массивов, размер которых превышает 2 гигабайта (ГБ).
- По умолчанию: сборка мусора поддерживает массивы размером более 2 ГБ. Это эквивалентно присвоению значения `1`.
- В будущей версии .NET этот параметр может быть объявлен устаревшим.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | Н/Д | Н/Д | Н/Д |
| **Переменная среды** | `COMPlus_gcAllowVeryLargeObjects` | `1` — включено<br/> `0` — отключено | .NET Core 1.0 |
| **app.config для .NET Framework** | [gcAllowVeryLargeObjects](../../framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md) | `1` — включено<br/> `0` — отключено | .NET Framework 4,5 |

## <a name="large-object-heap-threshold"></a>Пороговое значение кучи больших объектов

- Указывает пороговый размер (в байтах), при котором объекты попадают в кучу больших объектов (LOH).
- Пороговое значение по умолчанию — 85 000 байт.
- Указанное значение должно быть больше порогового значения по умолчанию.

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.LOHThreshold` | *Десятичное значение* | .NET Core 1.0 |
| **Переменная среды** | `COMPlus_GCLOHThreshold` | *Шестнадцатеричное значение* | .NET Core 1.0 |
| **app.config для .NET Framework** | [GCLOHThreshold](../../framework/configure-apps/file-schema/runtime/gclohthreshold-element.md) | *Десятичное значение* | .NET Framework 4.8 |

Пример.

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.LOHThreshold": 120000
      }
   }
}
```

> [!TIP]
> Если вы задаете параметр в *runtimeconfig.json*, укажите десятичное значение. Если вы задаете параметр в виде переменной среды, укажите шестнадцатеричное значение. Например, чтобы задать порог размера в 120 000 байт, для JSON-файла нужно указать значение 120000, а для переменной среды — значение 0x1D4C0 или 1D4C0.

## <a name="standalone-gc"></a>Автономный сборщик мусора

- Указывает путь к библиотеке, содержащей сборщик мусора, который среда выполнения намеревается загрузить.
- Дополнительные сведения см. в статье [Проектирования загрузчика автономного сборщика мусора](https://github.com/dotnet/runtime/blob/main/docs/design/features/standalone-gc-loading.md).

| | Имя параметра | Значения | Представленная версия |
| - | - | - | - |
| **runtimeconfig.json** | Н/Д | Н/Д | Н/Д |
| **Переменная среды** | `COMPlus_GCName` | *string_path* | .NET Core 2.0; |
