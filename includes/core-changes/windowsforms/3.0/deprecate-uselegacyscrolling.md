---
ms.openlocfilehash: 459e7e1f0b5543f069682dadf60668e94b472377
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643865"
---
### <a name="switchsystemwindowsformsdomainupdownuselegacyscrolling-compatibility-switch-not-supported"></a><span data-ttu-id="ab7cb-101">Параметр совместимости Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling не поддерживается</span><span class="sxs-lookup"><span data-stu-id="ab7cb-101">Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling compatibility switch not supported</span></span>

<span data-ttu-id="ab7cb-102">Параметр совместимости `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling`, появившийся в .NET Framework 4.7.1, не поддерживается в Windows Forms в .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="ab7cb-102">The `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` compatibility switch, which was introduced in .NET Framework 4.7.1, is not supported in Windows Forms on .NET Core 3.0.</span></span>

#### <a name="change-description"></a><span data-ttu-id="ab7cb-103">Описание изменений</span><span class="sxs-lookup"><span data-stu-id="ab7cb-103">Change description</span></span>

<span data-ttu-id="ab7cb-104">Начиная с версии .NET Framework 4.7.1, параметр совместимости `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` позволял разработчикам отказаться от независимых действий <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> и <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="ab7cb-104">Starting with the .NET Framework 4.7.1, the `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` compatibility switch allowed developers to opt-out of independent <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> and <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> actions.</span></span> <span data-ttu-id="ab7cb-105">Параметр восстанавливал прежнее поведение, при котором действие <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> игнорируется, если присутствует текст контекста, и разработчику необходимо использовать действие <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> для элемента управления перед использованием действия <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="ab7cb-105">The switch restored the legacy behavior, in which the <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> is ignored if context text is present, and the developer is required to use <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> action on the control before the <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> action.</span></span> <span data-ttu-id="ab7cb-106">Дополнительные сведения см. в статье [Элемент \<AppContextSwitchOverrides>](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md).</span><span class="sxs-lookup"><span data-stu-id="ab7cb-106">For more information, see [\<AppContextSwitchOverrides> element](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md).</span></span>

<span data-ttu-id="ab7cb-107">В .NET Core параметр `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="ab7cb-107">In .NET Core, the `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` switch is not supported.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="ab7cb-108">Представленная версия</span><span class="sxs-lookup"><span data-stu-id="ab7cb-108">Version introduced</span></span>

<span data-ttu-id="ab7cb-109">3.0, предварительная версия 9</span><span class="sxs-lookup"><span data-stu-id="ab7cb-109">3.0 Preview 9</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="ab7cb-110">Рекомендуемое действие</span><span class="sxs-lookup"><span data-stu-id="ab7cb-110">Recommended action</span></span>

<span data-ttu-id="ab7cb-111">Удалите параметр.</span><span class="sxs-lookup"><span data-stu-id="ab7cb-111">Remove the switch.</span></span> <span data-ttu-id="ab7cb-112">Он не поддерживается, и альтернативного варианта нет.</span><span class="sxs-lookup"><span data-stu-id="ab7cb-112">The switch is not supported, and no alternative functionality is available.</span></span>

#### <a name="category"></a><span data-ttu-id="ab7cb-113">Категория</span><span class="sxs-lookup"><span data-stu-id="ab7cb-113">Category</span></span>

<span data-ttu-id="ab7cb-114">Windows Forms</span><span class="sxs-lookup"><span data-stu-id="ab7cb-114">Windows Forms</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="ab7cb-115">Затронутые API</span><span class="sxs-lookup"><span data-stu-id="ab7cb-115">Affected APIs</span></span>

- <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>

<!-- 

### Affected APIs

- `M:System.Windows.Forms.DomainUpDown.DownButton`
- `M:System.Windows.Forms.DomainUpDown.UpButton`

-->