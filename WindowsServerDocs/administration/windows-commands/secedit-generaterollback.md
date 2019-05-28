---
title: Secedit:generaterollback
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3229e6ccb07c925a900b298a8332c5e48cefefe7
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564670"
---
# <a name="seceditgeneraterollback"></a>Secedit:generaterollback



可讓您產生指定的組態範本的復原範本。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定包含分析執行的預存的組態資料庫的路徑和檔案名稱。</br>如果檔案名稱指定的資料庫，尚未與其建立關聯的安全性範本 （如由組態檔），`/cfg \<configuration file name>`也必須指定命令列選項。|
|cfg|必要。</br>指定將匯入資料庫中進行分析的安全性範本的路徑和檔案名稱。</br>此 /cfg 選項只適用於搭配使用時`/db \<database file name>`參數。 如果未指定此項目，是分析會執行任何已儲存在資料庫中的設定。|
|rbk|必要。</br>指定要寫入復原資訊的安全性範本。 安全性範本會建立使用安全性範本嵌入式管理單元。 使用下列命令，可以建立復原檔案。|
|記錄檔|選擇性。</br>指定處理程序的記錄檔路徑和檔案名稱。|
|無訊息|選擇性。</br>隱藏畫面和記錄檔的輸出。 您仍然可以使用 安全性設定及分析嵌入式管理單元 Microsoft Management Console (MMC) 來檢視分析結果。|

## <a name="remarks"></a>備註

如果記錄檔的路徑未提供，預設的記錄檔 (*systemroot*\Users \*UserAccount *\My Documents\Security\Logs\*DatabaseName*.log) 會使用。

從 Windows Server 2008、windows`Secedit /refreshpolicy`已取代為`gpupdate`。 如需如何重新整理的安全性設定資訊，請參閱[Gpupdate](gpupdate.md)。

成功執行此命令將狀態 」 工作已順利完成。 」 與記錄檔僅所述的安全性範本與安全性原則設定之間不相符。 它會列出這些在安全性不符。

如果指定現有的復原範本，則此命令會覆寫它。 您可以使用下列命令來建立新的復原範本。 沒有額外的參數所需的其中一個條件。

## <a name="BKMK_Examples"></a>範例

建立使用 「 安全性設定及分析嵌入式管理單元，SecTmplContoso.inf，安全性範本之後建立復原組態檔來儲存原始設定。 寫出至 FY11 記錄檔的動作。
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
-   [命令列語法關鍵](command-line-syntax-key.md)