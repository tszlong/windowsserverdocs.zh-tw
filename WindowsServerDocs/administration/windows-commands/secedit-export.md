---
title: secedit：匯出
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aed2774bcba1ac3d5ba828901586acbbe24d255
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384254"
---
# <a name="seceditexport"></a>secedit：匯出



匯出儲存在使用安全性範本設定之資料庫中的安全性設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將執行分析的儲存設定。</br>如果 [檔案名] 指定的資料庫沒有與其相關聯的安全性範本（如設定檔所表示），則`/cfg \<configuration file name>`也必須指定命令列選項。|
|mergedpolicy|選擇性。</br>合併和匯出網域和本機原則安全性設定。|
|cfg|必要。</br>指定將匯入至資料庫以進行分析之安全性範本的路徑和檔案名。</br>只有在搭配`/db \<database file name>`參數使用時，此/cfg 選項才有效。 如果未指定此項，則會針對已經儲存在資料庫中的任何設定來執行分析。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，則會將資料庫中定義的所有安全性設定套用至系統。 若要設定多個區域，請以空格分隔每個區域。 以下是支援的安全性區域：</br>-SecurityPolicy</br>    系統的本機原則和網域原則，包括帳戶原則、稽核原則、安全性選項等等。</br>- Group_Mgmt</br>    安全性範本中指定之任何群組的限制群組設定。</br>- User_Rights</br>    使用者登入許可權和許可權授與。</br>- RegKeys</br>    本機登錄機碼上的安全性。</br>-</br>    本機檔案儲存的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|log|選擇性。</br>指定進程之記錄檔的路徑和檔案名。|
|無訊息|選擇性。</br>隱藏螢幕和記錄輸出。 您仍然可以使用 Microsoft Management Console （MMC）的 [安全性設定及分析] 嵌入式管理單元來查看分析結果。|

## <a name="remarks"></a>備註

除了將設定匯入另一部電腦以外，您還可以使用此命令在本機電腦上備份您的安全性原則。

如果未提供記錄檔的路徑，則會使用預設記錄檔（*systemroot*\Documents 和 Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>.log）。

在 Windows Server 2008 中`Secedit /refreshpolicy` ，已取代為`gpupdate`。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>典型

將安全性資料庫和網域安全性原則匯出至 inf 檔案，然後將該檔案匯入到不同的資料庫，以便在另一部電腦上複寫安全性原則設定。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
將該檔案匯入另一部電腦上的不同資料庫。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [命令列語法關鍵](command-line-syntax-key.md)