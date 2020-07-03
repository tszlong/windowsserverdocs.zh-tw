---
title: net print
description: Net print 命令的參考文章。 此命令已被取代，在未來的 Windows 版本中不保證會受到支援。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af02ca14156c8a85ee54700983e2af6807752f91
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934826"
---
# <a name="net-print"></a>net print

> [!IMPORTANT]
> 此命令已被取代。 不過，您可以使用[prnjobs 命令](prnjobs.md)、 [Windows Management Instrumentation （WMI）](https://docs.microsoft.com/windows/win32/wmisdk/wmi-start-page)、 [Powershell 中的 PRINTMANAGEMENT.MSC](https://docs.microsoft.com/powershell/module/printmanagement)，或[適用于 IT 專業人員的腳本資源](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing)，來執行許多相同的工作。

顯示指定之印表機佇列或指定之列印工作的相關資訊，或控制指定的列印工作。

## <a name="syntax"></a>語法

```
net print {\\<computername>\<sharename> | \\<computername> <jobnumber> [/hold | /release | /delete]} [help]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ---------- | ----------- |
| `\\<computername>\<sharename>` | 指定您要顯示資訊的電腦和列印佇列（依名稱）。 |
| `\\<computername>` | 指定主控您想要控制之列印工作的電腦（依名稱）。 如果您未指定電腦，則會假設使用本機電腦。 需要 `<jobnumber>` 參數。 |
| `<jobnumber>` | 指定您想要控制的列印工作編號。 此號碼是由裝載列印工作所傳送之列印佇列的電腦所指派。 電腦將數位指派給列印工作之後，該號碼就不會指派給該電腦所裝載之任何佇列中的任何其他列印工作。 使用參數時為必要 `\\<computername>` 。 |
| `[/hold | /release | /delete]` | 指定要對列印工作採取的動作。 如果您指定了作業號碼，但未指定任何動作，則會顯示列印工作的相關資訊。<ul><li>**/hold** -延遲作業，讓其他列印工作略過它，直到釋放為止。</li><li>**/release** -釋放已延遲的列印工作。</li><li>**/delete** -從列印佇列中移除列印工作。</li></ul> |
| help | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- `net print\\<computername>`命令會顯示共用印表機佇列中列印工作的相關資訊。 以下是名為「*鐳射*」的共用印表機佇列中，所有列印工作的報表範例：

    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
    USER1          84        93844      printing
    USER2          85        12555      Waiting
    USER3          86        10222      Waiting
    ```

- 以下是列印工作的報表範例：

    ```
    Job #            35
    Status           Waiting
    Size             3096
    remark
    Submitting user  USER2
    Notify           USER2
    Job data type
    Job parameters
    additional info
    ```

### <a name="examples"></a>範例

若要列出* \\ 實際*執行電腦上*Dotmatrix*列印佇列的內容，請輸入：

```
net print \\Production\Dotmatrix
```

若要在* \\ 生產*電腦上顯示作業編號*35*的相關資訊，請輸入：

```
net print \\Production 35
```

若要在* \\ 生產*電腦上延遲作業編號*263* ，請輸入：

```
net print \\Production 263 /hold
```

若要在* \\ 生產*電腦上釋放作業編號*263* ，請輸入：

```
net print \\Production 263 /release
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考](print-command-reference.md)

- [prnjobs 命令](prnjobs.md)

- [Windows Management Instrumentation (WMI)](https://docs.microsoft.com/windows/win32/wmisdk/wmi-start-page)

- [Powershell 中的 Printmanagement.msc](https://docs.microsoft.com/powershell/module/printmanagement)

- [適用于 IT 專業人員的腳本資源](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing)
