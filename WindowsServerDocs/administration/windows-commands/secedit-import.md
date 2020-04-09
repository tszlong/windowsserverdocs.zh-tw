---
title: secedit：匯入
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47cd3b74a52af149706c6e6d238a076a9bc61f98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834881"
---
# <a name="seceditimport"></a>secedit：匯入



匯入儲存在先前從使用安全性範本設定的資料庫所匯出之 inf 檔案中的安全性設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將會執行匯入的預存設定。</br>如果 [檔案名] 指定的資料庫沒有相關聯的安全性範本（如設定檔所表示），則也必須指定 `/cfg \<configuration file name>` 命令列選項。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>只有在同時使用 `/cfg \<configuration file name>` 參數時，此命令列選項才有效。 如果未指定此項，會將/cfg 參數中的範本附加至儲存的範本。|
|cfg|必要。</br>指定將匯入至資料庫以進行分析之安全性範本的路徑和檔案名。</br>只有搭配 `/db \<database file name>` 參數使用時，此/cfg 選項才有效。 如果未指定此項，則會針對已經儲存在資料庫中的任何設定來執行分析。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>只有在同時使用 `/cfg \<configuration file name>` 參數時，此命令列選項才有效。 如果未指定此項，會將/cfg 參數中的範本附加至儲存的範本。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：</br>-SecurityPolicy</br>    系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等等。</br>-Group_Mgmt</br>    安全性範本中指定之任何群組的限制群組設定。</br>-User_Rights</br>    使用者登入許可權和許可權授與。</br>- RegKeys</br>    本機登錄機碼上的安全性。</br>-</br>    本機檔案儲存的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|log|選擇性。</br>指定進程之記錄檔的路徑和檔案名。|
|無訊息|選擇性。</br>隱藏螢幕和記錄輸出。 您仍然可以使用 Microsoft Management Console （MMC）的 [安全性設定及分析] 嵌入式管理單元來查看分析結果。|

## <a name="remarks"></a>備註

將 .inf 檔案匯入另一部電腦之前，請在將執行匯入的資料庫上執行命令 secedit/generaterollback，並在匯入檔案上執行 secedit/validate，以確認其完整性。

如果未提供記錄檔的路徑，則會使用預設記錄檔（*systemroot*\Documents 和 Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>）。

在 Windows Server 2008 中，`Secedit /refreshpolicy` 已由 `gpupdate`取代。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

將安全性資料庫和網域安全性原則匯出至 inf 檔案，然後將該檔案匯入到不同的資料庫，以便在另一部電腦上複寫安全性原則設定。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
只將檔案的安全性原則部分匯入另一部電腦上的不同資料庫。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)