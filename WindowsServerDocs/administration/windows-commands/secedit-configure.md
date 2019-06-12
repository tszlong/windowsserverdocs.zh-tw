---
title: Secedit： 設定
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9420945dca9b72de1937258201e7072d2bb115b2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441521"
---
# <a name="seceditconfigure"></a>Secedit： 設定



可讓您設定目前的系統設定使用儲存在資料庫中的安全性設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定的路徑和檔案名稱的資料庫，其中包含儲存的組態。</br>如果檔案名稱指定的資料庫，尚未與其建立關聯的安全性範本 （如由組態檔），`/cfg \<configuration file name>`也必須指定命令列選項。|
|cfg|選擇性。</br>指定將匯入資料庫中進行分析的安全性範本的路徑和檔案名稱。</br>此 /cfg 選項只適用於搭配使用時`/db \<database file name>`參數。 如果未指定此項目，是分析會執行任何已儲存在資料庫中的設定。|
|overwrite|選擇性。</br>指定 /cfg 參數中的安全性範本是否應該覆寫任何範本或複合而不是將結果附加到已儲存的範本資料庫中儲存的範本。</br>此命令列選項時才有效`/cfg \<configuration file name>`也使用參數。 如果未指定此項目，是 /cfg 參數中的範本會附加至已儲存的範本。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，定義在資料庫中的所有安全性設定會都套用至系統。 若要設定多個區域，區隔每個區域中的空間。 支援下列安全性區域：</br>-   SecurityPolicy</br>    本機原則和系統，包括帳戶原則的網域原則會稽核原則、 安全性選項等。</br>-   Group_Mgmt</br>    受限制的安全性範本中指定任何群組的群組設定。</br>-User_Rights</br>    使用者登入權限，並授與的權限。</br>-登錄機碼</br>    本機登錄機碼的安全性。</br>-FileStore</br>    在 本機檔案儲存體上的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|記錄檔|選擇性。</br>指定處理程序的記錄檔路徑和檔案名稱。|
|無訊息|選擇性。</br>隱藏畫面和記錄檔的輸出。 您仍然可以使用 安全性設定及分析嵌入式管理單元 Microsoft Management Console (MMC) 來檢視分析結果。|

## <a name="remarks"></a>備註

如果記錄檔的路徑未提供，預設的記錄檔 (*systemroot*\Users \*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>.log) 會使用。

從 Windows Server 2008、windows`Secedit /refreshpolicy`已取代為`gpupdate`。 如需如何重新整理的安全性設定資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>範例

執行分析的安全性資料庫，SecDbContoso.sdb，您可以使用 安全性設定及分析嵌入式管理單元建立的安全性參數。 將輸出導向至 SecAnalysisContosoFY11 提示之後，您可以確認命令正確執行的檔案。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
例如，假設分析揭露某些能力，因此安全性範本 SecContoso.inf，已修改。 再次執行命令來加入的變更，將導向至現有檔案 SecAnalysisContosoFY11 沒有提示之後的輸出。
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   [命令列語法關鍵](command-line-syntax-key.md)