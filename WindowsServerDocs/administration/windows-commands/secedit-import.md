---
title: secedit：匯入
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ad13d04ac894bc11aa26790239370147ce9d140
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037426"
---
# <a name="seceditimport"></a>secedit：匯入



匯入先前從使用安全性範本設定的資料庫所匯出的 inf 檔案中儲存的安全性設定。

## <a name="syntax"></a>語法

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將在其中執行匯入的預存設定。</br>如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg \<configuration file name>` 也必須指定命令列選項。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應該覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>此命令列選項只有在 `/cfg \<configuration file name>` 同時使用參數時才有效。 如果未指定此參數，則會將/cfg 參數中的範本附加至儲存的範本。|
|cfg|必要。</br>指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。</br>這個/cfg 選項只有在搭配參數使用時才有效 `/db \<database file name>` 。 如果未指定此項，則會針對已儲存在資料庫中的任何設定來執行分析。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應該覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>此命令列選項只有在 `/cfg \<configuration file name>` 同時使用參數時才有效。 如果未指定此參數，則會將/cfg 參數中的範本附加至儲存的範本。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：</br>-SecurityPolicy</br>    系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等。</br>-Group_Mgmt</br>    安全性範本中指定之任何群組的限制群組設定。</br>-User_Rights</br>    使用者登入許可權和許可權授與。</br>- RegKeys</br>    本機登錄機碼上的安全性。</br>-時</br>    本機檔案儲存體上的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|log|選擇性。</br>指定進程之記錄檔的路徑和檔案名。|
|quiet|選擇性。</br>隱藏畫面和記錄檔輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。|

## <a name="remarks"></a>備註

將 .inf 檔案匯入到另一部電腦之前，請在執行匯入的資料庫上執行命令 secedit/generaterollback，然後在匯入檔案上執行 secedit/validate，以驗證其完整性。

如果未提供記錄檔的路徑，則會使用預設的記錄檔 (*systemroot*\Documents 和 Settings \* UserAccount<em>\My Documents\Security\Logs \* DatabaseName</em>. log) 。

在 Windows Server 2008 中，已將 `Secedit /refreshpolicy` 取代為 `gpupdate` 。 如需有關如何重新整理安全性設定的詳細資訊，請參閱 [Gpupdate](gpupdate.md)。

## <a name="examples"></a>範例

將安全性資料庫和網域安全性原則匯出至 inf 檔案，然後將該檔案匯入至不同的資料庫，以便在另一部電腦上複寫安全性原則設定。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
只將檔案的安全性原則部分匯入至另一部電腦上的其他資料庫。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
- [命令列語法關鍵](command-line-syntax-key.md)