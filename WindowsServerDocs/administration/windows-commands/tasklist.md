---
title: Tasklist
description: Tasklist 命令的參考文章，此命令會顯示在本機或遠端電腦上執行的進程清單。
ms.topic: reference
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: af89112d98cb7799a2fa910d562ac8c2a35bba19
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718055"
---
# <a name="tasklist"></a>Tasklist

顯示目前在本機電腦或遠端電腦上正在執行的處理序清單。 **Tasklist** 取代了 **tlist.exe** 工具。

> [!NOTE]
> 此命令會取代 **tlist.exe** 工具。

## <a name="syntax"></a>語法

```
tasklist [/s <computer> [/u [<domain>\]<username> [/p <password>]]] [{/m <module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <filter> [/fi <filter> [ ... ]]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| u `<domain>\<username>` | 使用或所指定之使用者的帳戶許可權來執行此命令 `<username>` `<domain>\<username>` 。 只有在同時指定 **/s**時，才能指定 **/u**參數。 預設值是目前登入發出命令的電腦之使用者的許可權。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| 一樣 `<module>` | 列出載入的 DLL 模組符合指定模式名稱的所有工作。 如果未指定模組名稱，此選項會顯示每個工作載入的所有模組。 |
| Svc | 列出每個進程的所有服務資訊，而不進行截斷。 當 **/fo** 參數設定為 **table**時有效。 |
| /v | 在輸出中顯示詳細資訊工作資訊。 如需完整的詳細資訊輸出而不截斷，請同時使用 **/v** 和 **/svc** 。 |
| /fo `{table | list | csv}` | 指定要用於輸出的格式。 有效的值為 **table**、 **list**和 **csv**。 輸出的預設格式為 **table**。 |
| /nh | 隱藏輸出中的資料行標頭。 當 **/fo** 參數設定為 **資料表** 或 **csv**時有效。 |
| filter `<filter>` | 指定要在查詢中包含或排除的進程類型。 您可以使用一個以上的篩選準則，或使用萬用字元 (`\`) 來指定所有的工作或映射名稱。 有效的篩選器會列在本文的 [ **篩選名稱]、[運算子] 和 [值** ] 區段中。  |
| /? | 在命令提示字元顯示說明。 |

#### <a name="filter-names-operators-and-values"></a>篩選名稱、運算子和值

| 篩選名稱 | 有效運算子 | 有效的值 (s)  |
|--|--|--|
| 狀態 | eq、ne | `RUNNING | NOT RESPONDING | UNKNOWN`. 如果您指定遠端系統，則不支援此篩選器。 |
| IMAGENAME | eq、ne | 映像名稱 |
| PID | eq, ne, gt, lt, ge, le | PID 值 |
| SESSION | eq, ne, gt, lt, ge, le | 工作階段編號 |
| 會話 | eq、ne | [工作階段名稱] |
| CPUtime | eq, ne, gt, lt, ge, le | CPU 時間格式為 *HH： MM： SS*，其中 *MM* 和 *SS* 介於0和59之間，而 *HH* 是任何不帶正負號的數位 |
| MEMUSAGE | eq, ne, gt, lt, ge, le | 記憶體使用量（KB） |
| USERNAME | eq、ne | 任何有效的使用者名稱 (`<user>` 或 `<domain\user>`)  |
| 服務 | eq、ne | 服務名稱 |
| SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE | eq、ne | 視窗標題。 如果您指定遠端系統，則不支援此篩選器。 |
| 模組 | eq、ne | DLL 名稱 |

## <a name="examples"></a>範例

若要列出 *處理序識別碼大於 1000*的所有工作，並 *以 csv 格式顯示它們*，請輸入：

```
tasklist /v /fi "PID gt 1000" /fo csv
```

若要列出目前正在執行的系統處理常式，請輸入：

```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```

若要列出目前正在執行之所有處理常式的詳細資訊，請輸入：

```
tasklist /v /fi "STATUS eq running"
```

若要列出遠端電腦 *srvmain*上的進程的所有服務資訊，其 DLL 名稱 *以 ntdll.dll 開頭*，請輸入：

```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```

若要使用您目前登入的使用者帳戶的認證來列出遠端電腦 *srvmain*上的處理常式，請輸入：

```
tasklist /s srvmain
```

若要列出遠端電腦 *srvmain*上的處理常式，請使用 *使用者帳戶 Hiropln*的認證，輸入：

```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
