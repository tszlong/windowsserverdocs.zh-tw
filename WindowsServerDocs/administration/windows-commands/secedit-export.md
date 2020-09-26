---
title: secedit export
description: Secedit 匯出的參考文章，可匯出儲存在以安全性範本設定的資料庫中的安全性設定。
ms.topic: reference
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dfd766674a4e4ac2e9552d36b4cb706117c173bd
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388241"
---
# <a name="secedit-export"></a>secedit/export

匯出儲存在以安全性範本設定的資料庫中的安全性設定。 您可以使用此命令，在本機電腦上備份您的安全性原則，以及將設定匯入至另一部電腦。

## <a name="syntax"></a>語法

```
secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /db | 必要。 指定資料庫的路徑和檔案名，其中包含執行匯出時所針對的儲存設定。 如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg <configuration file name>` 也必須指定選項。 |
| /mergedpolicy | 合併和匯出網域和本機原則安全性設定。 |
| /cfg | 必要。 指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。 此選項只有在搭配參數使用時才有效 `/db <database file name>` 。 如果未指定此參數，則會針對已儲存在資料庫中的任何設定來執行分析。 |
| /areas | 指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：<ul><li>**securitypolicy：** 系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等。</li><li>  **group_mgmt：** 安全性範本中指定之任何群組的限制群組設定。</li><li>**user_rights：** 使用者登入許可權和許可權授與。</li><li>**regkeys：** 本機登錄機碼上的安全性。</li><li>中的中 **：** 本機檔案儲存體上的安全性。</li><li>**服務：** 所有已定義服務的安全性。</li></ul> |
| /log | 指定要在進程中使用之記錄檔的路徑和檔案名。 如果您未指定檔案位置，則會使用預設的記錄檔 `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` 。 |
| /quiet | 隱藏畫面和記錄檔輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。 |

## <a name="examples"></a>範例

若要將安全性資料庫和網域安全性原則匯出至 inf 檔案，然後將該檔案匯入至不同的資料庫，以便在另一部電腦上複寫安全性原則設定，請輸入：

```
secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```

若要將範例檔案匯入到另一部電腦上的其他資料庫，請輸入：

```
secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [secedit/analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/import](secedit-import.md)

- [secedit/validate](secedit-validate.md)