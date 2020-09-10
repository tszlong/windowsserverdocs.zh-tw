---
title: secedit： generaterollback
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 28b8fedf952bfa5466bc0a893a46f2e7f69165f6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636793"
---
# <a name="seceditgeneraterollback"></a>secedit： generaterollback



可讓您為指定的設定範本產生復原範本。

## <a name="syntax"></a>語法

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將用來執行分析的預存設定。</br>如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg \<configuration file name>` 也必須指定命令列選項。|
|cfg|必要。</br>指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。</br>這個/cfg 選項只有在搭配參數使用時才有效 `/db \<database file name>` 。 如果未指定此項，則會針對已儲存在資料庫中的任何設定來執行分析。|
|rbk|必要。</br>指定要在其中寫入復原資訊的安全性範本。 安全性範本是使用 [安全性範本] 嵌入式管理單元所建立。 您可以使用此命令來建立復原檔案。|
|log|選擇性。</br>指定進程之記錄檔的路徑和檔案名。|
|quiet|選擇性。</br>隱藏畫面和記錄檔輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。|

## <a name="remarks"></a>備註

如果未提供記錄檔的路徑，則會使用預設的記錄檔， (*systemroot*\Users \* UserAccount<em>\My Documents\Security\Logs \* DatabaseName</em>. log) 。

從 Windows Server 2008 開始，已 `Secedit /refreshpolicy` 取代為 `gpupdate` 。 如需有關如何重新整理安全性設定的詳細資訊，請參閱 [Gpupdate](gpupdate.md)。

成功執行此命令將會陳述工作已順利完成。 並只記錄所述安全性範本和安全性原則設定之間的不符。 它會在 scesrv.dll 中列出這些不符的情況。

如果指定了現有的復原範本，此命令將會覆寫它。 您可以使用此命令來建立新的復原範本。 任一條件都不需要任何其他參數。

## <a name="examples"></a>範例

使用 [安全性設定及分析] 嵌入式管理單元建立安全性範本之後，請 SecTmplContoso，建立復原設定檔以儲存原始設定。 將動作寫出至 FY11 記錄檔。
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
- [命令列語法關鍵](command-line-syntax-key.md)