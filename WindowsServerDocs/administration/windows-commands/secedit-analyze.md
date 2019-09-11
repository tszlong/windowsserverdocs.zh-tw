---
title: secedit：分析
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83f9e977a059e1a1f1b882d5a968054dacf6b3be
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868874"
---
# <a name="seceditanalyze"></a>secedit：分析



可讓您針對儲存在資料庫中的基準設定來分析目前的系統設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將執行分析的儲存設定。</br>如果 [檔案名] 指定的資料庫沒有與其相關聯的安全性範本（如設定檔所表示），則`/cfg \<configuration file name>`也必須指定命令列選項。|
|cfg|選擇性。</br>指定將匯入至資料庫以進行分析之安全性範本的路徑和檔案名。</br>只有在搭配`/db \<database file name>`參數使用時，此/cfg 選項才有效。 如果未指定此項，則會針對已經儲存在資料庫中的任何設定來執行分析。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>只有在同時使用`/cfg \<configuration file name>`參數時，此命令列選項才有效。 如果未指定此項，會將/cfg 參數中的範本附加至儲存的範本。|
|日誌|選擇性。</br>指定要在進程中使用之記錄檔的路徑和檔案名。|
|無訊息|選擇性。</br>隱藏螢幕輸出。 您仍然可以使用 Microsoft Management Console （MMC）的 [安全性設定及分析] 嵌入式管理單元來查看分析結果。|

## <a name="remarks"></a>備註

分析結果會儲存在資料庫的不同區域中，而且可以在 MMC 的 [安全性設定及分析] 嵌入式管理單元中查看。

如果未提供記錄檔的路徑，則會使用預設記錄檔（*systemroot*\Documents 和 Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>.log）。

在 Windows Server 2008 中`Secedit /refreshpolicy` ，已取代為`gpupdate`。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>典型

使用 [安全性設定及分析] 嵌入式管理單元，在您建立的安全性資料庫（SecDbContoso）上執行安全性參數的分析。 使用提示將輸出導向至檔案 SecAnalysisContosoFY11，讓您可以確認命令是否正確執行。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
假設分析顯示一些 inadequacies，因此已修改安全性範本 SecContoso。 再次執行命令以併入變更，將輸出導向至現有的檔案 SecAnalysisContosoFY11，而不提示。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
-   [命令列語法關鍵](command-line-syntax-key.md)