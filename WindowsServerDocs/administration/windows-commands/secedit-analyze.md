---
title: secedit：分析
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 272a05b36ce998aaed9a3ee8bd8b9c273296c030
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636859"
---
# <a name="seceditanalyze"></a>secedit：分析



可讓您針對儲存在資料庫中的基準設定來分析目前的系統設定。

## <a name="syntax"></a>語法

```
Secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|db|必要。</br>指定資料庫的路徑和檔案名，其中包含將用來執行分析的預存設定。</br>如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg \<configuration file name>` 也必須指定命令列選項。|
|cfg|選擇性。</br>指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。</br>這個/cfg 選項只有在搭配參數使用時才有效 `/db \<database file name>` 。 如果未指定此項，則會針對已儲存在資料庫中的任何設定來執行分析。|
|overwrite|選擇性。</br>指定/cfg 參數中的安全性範本是否應該覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。</br>此命令列選項只有在 `/cfg \<configuration file name>` 同時使用參數時才有效。 如果未指定此參數，則會將/cfg 參數中的範本附加至儲存的範本。|
|log|選擇性。</br>指定要在進程中使用之記錄檔的路徑和檔案名。|
|quiet|選擇性。</br>抑制螢幕輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。|

## <a name="remarks"></a>備註

分析結果會儲存在資料庫的另一個區域中，並可在 [安全性設定及分析] 嵌入式管理單元中看到 MMC。

如果未提供記錄檔的路徑，則會使用預設的記錄檔 (*systemroot*\Documents 和 Settings \* UserAccount<em>\My Documents\Security\Logs \* DatabaseName</em>. log) 。

在 Windows Server 2008 中，已將 `Secedit /refreshpolicy` 取代為 `gpupdate` 。 如需有關如何重新整理安全性設定的詳細資訊，請參閱 [Gpupdate](gpupdate.md)。

## <a name="examples"></a>範例

在您使用 [安全性設定及分析] 嵌入式管理單元建立的安全性資料庫（SecDbContoso）上，執行安全性參數的分析。 使用提示將輸出導向檔案 SecAnalysisContosoFY11，以便您可以確認命令是否正確執行。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
假設分析顯示一些不足之處，以便修改安全性範本 SecContoso .inf。 再次執行命令以合併變更，將輸出導向至現有的檔案 SecAnalysisContosoFY11，而不提示。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit](secedit.md)
- [命令列語法關鍵](command-line-syntax-key.md)