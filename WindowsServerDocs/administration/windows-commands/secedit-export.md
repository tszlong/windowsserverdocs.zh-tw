---
title: secedit:export
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1f9d6268777d0791dbc0cdca2d4318399378698b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813489"
---
# <a name="seceditexport"></a>secedit:export



匯出儲存在資料庫中使用安全性範本設定的安全性設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]

```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定包含分析執行的預存的組態資料庫的路徑和檔案名稱。</br>如果檔案名稱指定的資料庫，尚未與其建立關聯的安全性範本 （如由組態檔），`/cfg \<configuration file name>`也必須指定命令列選項。|
|mergedpolicy|選擇性。</br>合併，並將匯出網域和本機原則安全性設定。|
|cfg|必要。</br>指定將匯入資料庫中進行分析的安全性範本的路徑和檔案名稱。</br>此 /cfg 選項只適用於搭配使用時`/db \<database file name>`參數。 如果未指定此項目，是分析會執行任何已儲存在資料庫中的設定。|
|區域|選擇性。</br>指定要套用至系統的安全性區域。 如果未指定此參數，定義在資料庫中的所有安全性設定會都套用至系統。 若要設定多個區域，區隔每個區域中的空間。 支援下列安全性區域：</br>-   SecurityPolicy</br>    本機原則和系統，包括帳戶原則的網域原則會稽核原則、 安全性選項等。</br>-   Group_Mgmt</br>    受限制的安全性範本中指定任何群組的群組設定。</br>-User_Rights</br>    使用者登入權限，並授與的權限。</br>-登錄機碼</br>    本機登錄機碼的安全性。</br>-FileStore</br>    在 本機檔案儲存體上的安全性。</br>-服務</br>    所有已定義服務的安全性。|
|記錄檔|選擇性。</br>指定處理程序的記錄檔路徑和檔案名稱。|
|無訊息|選擇性。</br>隱藏畫面和記錄檔的輸出。 您仍然可以使用 安全性設定及分析嵌入式管理單元 Microsoft Management Console (MMC) 來檢視分析結果。|

## <a name="remarks"></a>備註

您可以使用此命令來備份您的安全性原則，除了將設定匯入另一部電腦的本機電腦上。

如果記錄檔的路徑未提供，預設的記錄檔 (*systemroot*\Documents and 設定\*UserAccount*\My Documents\Security\Logs\*DatabaseName*。會使用記錄檔）。

在 Windows Server 2008、windows`Secedit /refreshpolicy`已取代為`gpupdate`。 如需如何重新整理的安全性設定資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>範例

將安全性資料庫和網域安全性原則匯出到 inf 檔案，然後該檔案匯入不同的資料庫才能複寫在另一部電腦上的安全性原則設定。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
該檔案匯入到不同的資料庫，另一部電腦上。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [命令列語法關鍵](command-line-syntax-key.md)