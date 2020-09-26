---
title: secedit import
description: Secedit 匯入命令的參考文章，此命令會將安全性設定 ( .inf 檔案中，匯入先前從使用安全性範本設定的資料庫中匯出的) 。
ms.topic: reference
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 35705012d32a196934c0834b3de0c67f7210270b
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388231"
---
# <a name="secedit-import"></a>secedit/import

將安全性設定 ( .inf 檔案匯入) ，先前從使用安全性範本設定的資料庫匯出。

> [!IMPORTANT]
> 將 .inf 檔案匯入到另一部電腦之前，您必須在要執行匯入的資料庫上執行此 `secedit /generaterollback` 命令。
>
> 您也必須在匯 `secedit /validate` 入檔案上執行命令，以驗證其完整性。

## <a name="syntax"></a>語法

```
secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /db | 必要。 指定資料庫的路徑和檔案名，其中包含執行匯入的預存設定。 如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg <configuration file name>` 也必須指定選項。 |
| /overwrite | 指定 **/cfg** 參數中的安全性範本是否應該覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。 此選項只有在 `/cfg <configuration file name>` 同時使用參數時才有效。 如果未指定此參數，則會將 **/cfg** 參數中的範本附加至儲存的範本。 |
| /cfg | 必要。 指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。 此選項只有在搭配參數使用時才有效 `/db <database file name>` 。 如果未指定此參數，則會針對已儲存在資料庫中的任何設定來執行分析。 |
| /areas | 指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：<ul><li>**securitypolicy：** 系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等。</li><li>  **group_mgmt：** 安全性範本中指定之任何群組的限制群組設定。</li><li>**user_rights：** 使用者登入許可權和許可權授與。</li><li>**regkeys：** 本機登錄機碼上的安全性。</li><li>中的中 **：** 本機檔案儲存體上的安全性。</li><li>**服務：** 所有已定義服務的安全性。</li></ul> |
| /log | 指定要在進程中使用之記錄檔的路徑和檔案名。 如果您未指定檔案位置，則會使用預設的記錄檔 `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` 。 |
| /quiet | 隱藏畫面和記錄檔輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。 |

## <a name="examples"></a>範例

若要將安全性資料庫和網域安全性原則匯出至 .inf 檔案，然後將該檔案匯入至不同的資料庫以複寫另一部電腦上的原則設定，請輸入：

```
secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```

若只將檔案的安全性原則部分匯入至另一部電腦上的不同資料庫，請輸入：

```
secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [secedit/analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/validate](secedit-validate.md)