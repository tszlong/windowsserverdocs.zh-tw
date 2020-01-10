---
title: secedit： generaterollback
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 797f93f010a903d6ec21f568d8d665cb2e42e692
ms.sourcegitcommit: 5ed817fff1262e390403a4f4f6c38fdc12300c29
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2020
ms.locfileid: "75702739"
---
# <a name="seceditgeneraterollback"></a>secedit： generaterollback



可讓您為指定的設定範本產生復原範本。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parameters

|參數|說明|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將執行分析的儲存設定。</br>如果 [檔案名] 指定的資料庫沒有相關聯的安全性範本（如設定檔所表示），則也必須指定 `/cfg \<configuration file name>` 命令列選項。|
|cfg|必要。</br>指定將匯入至資料庫以進行分析之安全性範本的路徑和檔案名。</br>只有搭配 `/db \<database file name>` 參數使用時，此/cfg 選項才有效。 如果未指定此項，則會針對已經儲存在資料庫中的任何設定來執行分析。|
|rbk|必要。</br>指定要在其中寫入復原資訊的安全性範本。 安全性範本是使用 [安全性範本] 嵌入式管理單元所建立。 您可以使用此命令來建立回復檔案。|
|log|選用。</br>指定進程之記錄檔的路徑和檔案名。|
|quiet|選用。</br>隱藏螢幕和記錄輸出。 您仍然可以使用 Microsoft Management Console （MMC）的 [安全性設定及分析] 嵌入式管理單元來查看分析結果。|

## <a name="remarks"></a>備註

如果未提供記錄檔的路徑，則會使用預設記錄檔（*systemroot*\Users \*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>）。

從 Windows Server 2008 開始，`Secedit /refreshpolicy` 已由 `gpupdate`取代。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

成功執行此命令將會出現「工作已順利完成」。 和只會記錄所指定安全性範本與安全性原則設定之間的不符。 它會在 scesrv.dll 中列出這些不符的情況。

如果指定了現有的復原範本，此命令將會覆寫它。 您可以使用此命令來建立新的復原範本。 任一條件都不需要任何其他參數。

## <a name="BKMK_Examples"></a>範例

使用 [安全性設定及分析] 嵌入式管理單元建立安全性範本之後，SecTmplContoso 建立復原設定檔以儲存原始設定。 將動作寫出至 FY11 記錄檔。
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
-   [命令列語法關鍵](command-line-syntax-key.md)