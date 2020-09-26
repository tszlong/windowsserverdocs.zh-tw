---
title: secedit analyze
description: Secedit 分析命令的參考文章，可讓您針對儲存在資料庫中的基準設定來分析目前的系統設定。
ms.topic: reference
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 783f9d1042b860adefc49f58f38f66e0571468e3
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388554"
---
# <a name="secedit-analyze"></a>secedit/analyze

可讓您針對儲存在資料庫中的基準設定來分析目前的系統設定。

## <a name="syntax"></a>語法

```
secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /db | 必要。 指定資料庫的路徑和檔案名，其中包含執行分析時所針對的儲存設定。 如果檔案名指定的資料庫沒有安全性範本 (如與其相關聯的設定檔) 所代表，則 `/cfg <configuration file name>` 也必須指定選項。 |
| /cfg | 指定將匯入資料庫以進行分析之安全性範本的路徑和檔案名。 此選項只有在搭配參數使用時才有效 `/db <database file name>` 。 如果未指定此參數，則會針對已儲存在資料庫中的任何設定來執行分析。 |
| /overwrite | 指定 **/cfg** 參數中的安全性範本是否應該覆寫儲存在資料庫中的任何範本或複合範本，而不是將結果附加至儲存的範本。 此選項只有在 `/cfg <configuration file name>` 同時使用參數時才有效。 如果未指定此參數，則會將 **/cfg** 參數中的範本附加至儲存的範本。 |
| /log | 指定要在進程中使用之記錄檔的路徑和檔案名。 如果您未指定檔案位置，則會使用預設的記錄檔 `<systemroot>\Documents and Settings\<UserAccount>\My Documents\Security\Logs\<databasename>.log` 。 |
| /quiet | 抑制螢幕輸出。 您仍然可以使用 [安全性設定及分析] 嵌入式管理單元來查看 Microsoft Management Console (MMC) 的分析結果。 |

## <a name="examples"></a>範例

若要在安全性資料庫（ *SecDbContoso*）上執行安全性參數的分析，然後將輸出導向檔案 *SecAnalysisContosoFY11*，包括確認命令是否正確執行的提示，請輸入：

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

若要在 *SecContoso .inf* 檔案中併入分析程式所需的變更，然後將輸出導向至現有的檔案， *SecAnalysisContosoFY11*，而不提示，請輸入：

```
secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/import](secedit-import.md)

- [secedit/validate](secedit-validate.md)