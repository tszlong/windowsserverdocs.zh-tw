---
title: secedit：設定
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0bee2588911b32698fe2eff7a7f95389a447ce11
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636834"
---
# <a name="seceditconfigure"></a>secedit：設定



可讓您使用儲存在資料庫中的安全性設定來設定目前的系統設定。

## <a name="syntax"></a>語法

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定包含預存設定之資料庫的路徑和檔案名。</br>如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg \<configuration file name>` 也必須指定命令列選項。|
|cfg|選擇性。</br>指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。</br>這個/cfg 選項只有在搭配參數使用時才有效 `/db \<database file name>` 。 如果未指定此項，則會針對已儲存在資料庫中的任何設定來執行分析。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應該覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>此命令列選項只有在 `/cfg \<configuration file name>` 同時使用參數時才有效。 如果未指定此參數，則會將/cfg 參數中的範本附加至儲存的範本。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：</br>-SecurityPolicy</br>    系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等。</br>-Group_Mgmt</br>    安全性範本中指定之任何群組的限制群組設定。</br>-User_Rights</br>    使用者登入許可權和許可權授與。</br>- RegKeys</br>    本機登錄機碼上的安全性。</br>-時</br>    本機檔案儲存體上的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|log|選擇性。</br>指定進程之記錄檔的路徑和檔案名。|
|quiet|選擇性。</br>隱藏畫面和記錄檔輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。|

## <a name="remarks"></a>備註

如果未提供記錄檔的路徑，則會使用預設的記錄檔， (*systemroot*\Users \* UserAccount<em>\My Documents\Security\Logs \* DatabaseName</em>. log) 。

從 Windows Server 2008 開始，已 `Secedit /refreshpolicy` 取代為 `gpupdate` 。 如需有關如何重新整理安全性設定的詳細資訊，請參閱 [Gpupdate](gpupdate.md)。

## <a name="examples"></a>範例

在您使用 [安全性設定及分析] 嵌入式管理單元建立的安全性資料庫（SecDbContoso）上，執行安全性參數的分析。 使用提示將輸出導向檔案 SecAnalysisContosoFY11，以便您可以確認命令是否正確執行。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
假設分析顯示一些不足之處，以便修改安全性範本 SecContoso .inf。 再次執行命令以合併變更，將輸出導向至現有的檔案 SecAnalysisContosoFY11，而不提示。
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
- [命令列語法關鍵](command-line-syntax-key.md)