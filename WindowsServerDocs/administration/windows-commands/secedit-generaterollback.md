---
title: secedit generaterollback
description: Secedit generaterollback 命令的參考文章，可讓您為指定的設定範本產生復原範本。
ms.topic: reference
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ae2e368ef387ea84095fcbcc51ad1e622225a2cc
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388814"
---
# <a name="secedit-generaterollback"></a>secedit/generaterollback

可讓您為指定的設定範本產生復原範本。 如果現有的復原範本存在，則再次執行此命令將會覆寫現有的資訊。

成功執行此命令會將安全性原則設定中指定之安全性範本的不符記錄檔記錄到 scesrv.dll 中。

## <a name="syntax"></a>語法

```
secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /db | 必要。 指定資料庫的路徑和檔案名，其中包含執行分析時所針對的儲存設定。 如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg <configuration file name>` 也必須指定選項。 |
| /cfg | 必要。 指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。 此選項只有在搭配參數使用時才有效 `/db <database file name>` 。 如果未指定此參數，則會針對已儲存在資料庫中的任何設定來執行分析。 |
| /rbk | 必要。 指定要在其中寫入復原資訊的安全性範本。 安全性範本是使用 [安全性範本] 嵌入式管理單元所建立。 您可以使用此命令來建立復原檔案。 |
| /log | 指定要在進程中使用之記錄檔的路徑和檔案名。 如果您未指定檔案位置，則會使用預設的記錄檔 `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` 。 |
| /quiet | 隱藏畫面和記錄檔輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。 |

## <a name="examples"></a>範例

若要建立復原設定檔案，請針對先前建立的 *SecTmplContoso .inf* 檔案儲存原始設定，然後將動作寫出至 *SecAnalysisContosoFY11* 記錄檔，然後輸入：

```
secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [secedit/analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [secedit/import](secedit-import.md)

- [secedit/validate](secedit-validate.md)