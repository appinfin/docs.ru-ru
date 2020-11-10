---
title: Предупреждение SYSLIB0011
description: Сведения об устаревших элементах, которые приводят к появлению предупреждения во время компиляции SYSLIB0011.
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 2b363e565ce1143c162679c6b74621885378d7ff
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92333109"
---
# <a name="syslib0011-binaryformatter-serialization-is-obsolete"></a><span data-ttu-id="2e6b3-103">SYSLIB0011. Сериализация BinaryFormatter устарела</span><span class="sxs-lookup"><span data-stu-id="2e6b3-103">SYSLIB0011: BinaryFormatter serialization is obsolete</span></span>

<span data-ttu-id="2e6b3-104">Из-за [уязвимостей системы безопасности](../../standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities) в <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>, следующие API являются устаревшими, начиная с версии .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="2e6b3-104">Due to [security vulnerabilities](../../standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities) in <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>, the following APIs are marked as obsolete, starting in .NET 5.0.</span></span> <span data-ttu-id="2e6b3-105">При их использовании во время компиляции создается предупреждение `SYSLIB0011`.</span><span class="sxs-lookup"><span data-stu-id="2e6b3-105">Using them in code generates warning `SYSLIB0011` at compile time.</span></span>

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

## <a name="workaround"></a><span data-ttu-id="2e6b3-106">Обходной путь</span><span class="sxs-lookup"><span data-stu-id="2e6b3-106">Workaround</span></span>

<span data-ttu-id="2e6b3-107">Рассмотрите возможность использования <xref:System.Text.Json.JsonSerializer> или <xref:System.Xml.Serialization.XmlSerializer> вместо <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>.</span><span class="sxs-lookup"><span data-stu-id="2e6b3-107">Consider using <xref:System.Text.Json.JsonSerializer> or <xref:System.Xml.Serialization.XmlSerializer> instead of <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>.</span></span>

<span data-ttu-id="2e6b3-108">Дополнительные сведения о рекомендуемых действиях см. в статье [Устранение ошибок, связанных с устареванием и отключением BinaryFormatter](https://aka.ms/binaryformatter).</span><span class="sxs-lookup"><span data-stu-id="2e6b3-108">For more information about recommended actions, see [Resolving BinaryFormatter obsoletion and disablement errors](https://aka.ms/binaryformatter).</span></span>

## <a name="see-also"></a><span data-ttu-id="2e6b3-109">См. также</span><span class="sxs-lookup"><span data-stu-id="2e6b3-109">See also</span></span>

- [<span data-ttu-id="2e6b3-110">Устранение ошибок, связанных с устареванием и отключением BinaryFormatter</span><span class="sxs-lookup"><span data-stu-id="2e6b3-110">Resolving BinaryFormatter obsoletion and disablement errors</span></span>](https://aka.ms/binaryformatter)
- [<span data-ttu-id="2e6b3-111">Методы сериализации BinaryFormatter устарели и запрещены в приложениях ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2e6b3-111">BinaryFormatter serialization methods are obsolete and prohibited in ASP.NET apps</span></span>](corefx.md#binaryformatter-serialization-methods-are-obsolete-and-prohibited-in-aspnet-apps)