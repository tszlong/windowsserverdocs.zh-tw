---
title: secedit：設定
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2ab11a62d3c3b14be0d91345e6e18cc895d2c64
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926205"
---
# <a name="seceditconfigure"></a>secedit：設定



可讓您使用儲存在資料庫中的安全性設定來設定目前的系統設定。

## <a name="syntax"></a>語法

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|db|必要。</br>指定包含預存設定之資料庫的路徑和檔案名。</br>如果 [檔案名] 指定的資料庫沒有與其相關聯的安全性範本（如設定檔所表示），則 `/cfg \<configuration file name>` 也必須指定命令列選項。|
|cfg|選擇性。</br>指定將匯入至資料庫以進行分析之安全性範本的路徑和檔案名。</br>只有在搭配參數使用時，此/cfg 選項才有效 `/db \<database file name>` 。 如果未指定此項，則會針對已經儲存在資料庫中的任何設定來執行分析。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>只有在同時使用參數時，此命令列選項才有效 `/cfg \<configuration file name>` 。 如果未指定此項，會將/cfg 參數中的範本附加至儲存的範本。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：</br>-SecurityPolicy</br>    系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等等。</br>-Group_Mgmt</br>    安全性範本中指定之任何群組的限制群組設定。</br>-User_Rights</br>    使用者登入許可權和許可權授與。</br>- RegKeys</br>    本機登錄機碼上的安全性。</br>-</br>    本機檔案儲存的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|log|選擇性。</br>指定進程之記錄檔的路徑和檔案名。|
|quiet|選擇性。</br>隱藏螢幕和記錄輸出。 您仍然可以使用 Microsoft Management Console （MMC）的 [安全性設定及分析] 嵌入式管理單元來查看分析結果。|

## <a name="remarks"></a>備註

如果未提供記錄檔的路徑，則會使用預設記錄檔（*systemroot*\Users \* UserAccount<em>\My Documents\Security\Logs \* DatabaseName</em>.log）。

從 Windows Server 2008 開始，已 `Secedit /refreshpolicy` 取代為 `gpupdate` 。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="examples"></a>範例

使用 [安全性設定及分析] 嵌入式管理單元，在您建立的安全性資料庫（SecDbContoso）上執行安全性參數的分析。 使用提示將輸出導向至檔案 SecAnalysisContosoFY11，讓您可以確認命令是否正確執行。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
假設分析顯示一些 inadequacies，因此已修改安全性範本 SecContoso。 再次執行命令以併入變更，將輸出導向至現有的檔案 SecAnalysisContosoFY11，而不提示。
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
- [命令列語法關鍵](command-line-syntax-key.md)