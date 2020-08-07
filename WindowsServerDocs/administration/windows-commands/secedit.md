---
title: secedit
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 570590710ea10758fe35e1cc2160709885bb60eb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882904"
---
# <a name="secedit"></a>secedit



藉由比較目前的設定與指定的安全性範本來設定和分析系統安全性。

## <a name="syntax"></a>語法

```
secedit
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|可讓您針對儲存在資料庫中的基準設定來分析目前的系統設定。  分析結果會儲存在資料庫的不同區域中，而且可以在 [安全性設定及分析] 嵌入式管理單元中查看。|
|[Secedit:configure](secedit-configure.md)|可讓您使用儲存在資料庫中的安全性設定來設定系統。|
|[Secedit:export](secedit-export.md)|可讓您匯出儲存在資料庫中的安全性設定。|
|[Secedit:generaterollback](secedit-generaterollback.md)|可讓您產生關於設定範本的復原範本。|
|[Secedit:import](secedit-import.md)|可讓您將安全性範本匯入資料庫，讓範本中指定的設定可以套用至系統，或針對系統進行分析。|
|[Secedit:validate](secedit-validate.md)|可讓您驗證安全性範本的語法。|

## <a name="remarks"></a>備註

針對所有的檔案名，如果未指定路徑，則會使用目前的目錄。

使用 [安全性範本] 嵌入式管理單元建立安全性範本，並執行 [安全性設定及分析] 嵌入式管理單元時，會建立下列檔案：


|           檔案           |                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv.dll .log        |                                                                                                                             **位置**：%windir%\security\logs</br>**建立者**：作業系統</br>**檔案類型**：文字</br>重新整理**頻率**：在執行 secedit/analyze、/configure、/export 或/Import 時覆寫。</br>**Content**：包含依原則類型分組的分析結果。                                                                                                                             |
| *使用者選取的名稱*。 sdb |                                                                                    **位置**：% windir% \* 使用者帳戶 <em> \Documents\Security\Database</br></em>*建立者* <em> ：執行 [安全性設定及分析] 嵌入式管理單元</br></em>*檔案類型* <em> ：專屬</br></em>重新整理*頻率* <em> ：每當建立新的安全性範本時，就會更新。</br></em>*內容* \* ：本機安全性原則和使用者建立的安全性範本。                                                                                    |
| *使用者選取的名稱*.log | **位置**：使用者定義，但預設為% windir% \* 使用者帳戶 <em> \Documents\Security\Logs</br></em>*建立者* <em> ：執行/analyze 和/configure 子命令 (或使用 [安全性設定及分析] 嵌入式管理單元) </br></em>*檔案類型* <em> ：文字</br></em>重新整理*頻率* <em> ： (執行/analyze 和/configure 子命令，或使用 [安全性設定及分析] 嵌入式管理單元) ; 覆寫。</br></em>*內容* \* ：</br>1. 記錄檔名稱</br>2. 日期和時間</br>3. 分析或調查的結果。 |
| *使用者選取的名稱*.inf |                                                                                     **位置**：% windir% \* 使用者帳戶 <em> \Documents\Security\Templates</br></em>*建立者* <em> ：執行安全性範本嵌入式管理單元</br></em>*檔案類型* <em> ：文字</br></em>重新整理*頻率* <em> ：每次更新安全性範本時</br></em>*內容* \* ：包含使用嵌入式管理單元選取之每個原則的範本設定資訊。                                                                                     |

> [!NOTE]
> Microsoft Management Console (MMC) 和 [安全性設定及分析] 嵌入式管理單元無法在 Server Core 上使用。

## <a name="additional-references"></a>其他參考資料

如需如何使用此命令的範例，請參閱任何子命令檔案中的範例一節。
- [命令列語法關鍵](command-line-syntax-key.md)