---
title: Lc.exe (компилятор лицензий)
description: Используйте Lc.exe, компилятор лицензий. Это средство считывает текстовые файлы, содержащие сведения о лицензиях, и создает двоичный файл для внедрения в исполняемый файл среды CLR в качестве ресурса.
ms.date: 03/30/2017
helpviewer_keywords:
- Lc.exe
- .licx file
- License Compiler
- control licenses [Windows Forms]
- compiling licenses file
- license file
- .licenses file
- Windows Forms, control licenses
- licensed controls [Windows Forms]
ms.assetid: 2de803b8-495e-4982-b209-19a72aba0460
ms.openlocfilehash: 69e79f4d2a75117f74428fdeee7684048d218e27
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654041"
---
# <a name="lcexe-license-compiler"></a>Lc.exe (компилятор лицензий)

Компилятор лицензий считывает текстовые файлы, содержащие сведения о лицензиях, и создает двоичный файл, который может быть внедрен в исполняемый файл среды CLR в качестве ресурса.  
  
 Конструктор Windows Forms автоматически создает или обновляет текстовый LICX-файл при каждом добавлении лицензированного элемента управления в форму. В процессе компиляции система проектов преобразует текстовый LICX-файл в двоичный LICENSES-файл ресурсов, который обеспечивает лицензирование элементов управления .NET. Двоичный файл ресурсов будет затем внедрен в выходной файл проекта.  
  
 При использовании компилятора лицензий во время создания проекта перекрестная компиляция между 32-разрядными и 64-разрядными системами не поддерживается. Это происходит потому, что компилятор лицензий должен загружать сборки, а загрузка 64-разрядных сборок из 32-разрядного приложения и наоборот не допускается. В этом случае запускайте компилятор лицензий из командной строки, чтобы скомпилировать лицензии вручную, и укажите соответствующую архитектуру.  
  
 Эта программа автоматически устанавливается вместе с Visual Studio. Для запуска этого средства используйте [Командную строку разработчика или PowerShell для разработчиков в Visual Studio](/visualstudio/ide/reference/command-prompt-powershell).  
  
 В командной строке введите следующее.  
  
## <a name="syntax"></a>Синтаксис  
  
```console
      lc /target:  
targetPE /complist:filename [-outdir:path]  
/i:modules [/nologo] [/v]  
```  
  
|Параметр|Описание|  
|------------|-----------------|  
|**/complist:** *filename*|Задает имя файла, который содержит список лицензируемых компонентов, включаемых в LICENSES-файл. Для каждого компонента указывается его полное имя, в одной строке содержится только один компонент.<br /><br /> Пользователи, работающие с программой из командной строки, могут указать отдельный файл для каждой формы, входящей в проект. Программа Lc.exe принимает несколько входных файлов и создает один LICENSES-файл.|  
|**/h**[**elp**]|Отображает синтаксис команд и параметров программы.|  
|**/i:** *module*|Задает модули, в которых содержатся компоненты, перечисленные в файле **/complist**. Чтобы указать несколько модулей, используйте несколько флагов **/i**.|  
|**/nologo**|Отключает отображение эмблемы Майкрософт при запуске.|  
|**/outdir:** *path*|Задает каталог, в котором следует разместить полученные LICENSES-файлы.|  
|**/target:** *targetPE*|Задает исполняемый файл, для которого создается LICENSES-файл.|  
|**/v**|Задает режим компиляции с выводом сведений о ходе процесса.|  
|**@** *file*|Задает файл ответов (RSP).|  
|**/?**|Отображает синтаксис команд и параметров программы.|  
  
## <a name="example"></a>Пример  
  
1. Если применяется лицензированный элемент управления `MyCompany.Samples.LicControl1`, который содержится в библиотеке `Samples.DLL` приложения `HostApp.exe` *, `HostAppLic.txt` можно создать файл* , содержащий указанные ниже сведения.  
  
    ```text
    MyCompany.Samples.LicControl1, Samples.DLL  
    ```  
  
2. Создайте LICENSES-файл с именем `HostApp.exe.licenses`, используя следующую команду.  
  
    ```console  
    lc /target:HostApp.exe /complist:hostapplic.txt /i:Samples.DLL /outdir:c:\bindir  
    ```  
  
3. Создайте `HostApp.exe`, включив в него LICENSES-файл в качестве ресурса. Для создания приложения на языке C# используется следующая команда.  
  
    ```console
    csc /res:HostApp.exe.licenses /out:HostApp.exe *.cs  
    ```  
  
 Следующая команда компилирует `myApp.licenses` из списков лицензированных компонентов, указанных файлами `hostapplic.txt``hostapplic2.txt` и `hostapplic3.txt`. Аргумент `modulesList` задает модули, в которых содержатся лицензируемые компоненты.  
  
```console  
lc /target:myApp /complist:hostapplic.txt /complist:hostapplic2.txt /complist: hostapplic3.txt /i:modulesList  
```  
  
## <a name="response-file-example"></a>Пример файла ответов  

 Ниже приведен пример файла ответов `response.rsp`. Подробнее о файлах ответов см. в разделе [Файлы ответов](/visualstudio/msbuild/msbuild-response-files).  
  
```text  
/target:hostapp.exe  
/complist:hostapplic.txt
/i:WFCPrj.dll
/outdir:"C:\My Folder"  
```  
  
 В приведенной ниже команде используется файл `response.rsp`.  
  
```console  
lc @response.rsp  
```  
  
## <a name="see-also"></a>См. также

- [Инструменты](index.md)
- [Al.exe (компоновщик сборок)](al-exe-assembly-linker.md)
- [Оболочки командной строки для разработчиков](/visualstudio/ide/reference/command-prompt-powershell)
