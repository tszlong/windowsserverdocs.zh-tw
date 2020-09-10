---
title: Tasklist
description: 瞭解如何顯示在本機或遠端電腦上執行的處理常式清單。
ms.topic: reference
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a57fc47473be3d8d5eb3fabab6f613da283fa231
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640125"
---
# <a name="tasklist"></a>Tasklist

顯示目前在本機電腦或遠端電腦上正在執行的處理序清單。 **Tasklist** 取代了 **tlist.exe** 工具。



## <a name="syntax"></a>語法

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

### <a name="parameters"></a>參數

|          參數           |                                                                                                                                            描述                                                                                                                                             |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        /s \<Computer>        |                                                                                         指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。                                                                                         |
| u-sql\<Domain>\\\]\<UserName> | 使用使用者*名稱*或*網域*使用者名稱所指定之使用者的帳戶許可權來執行此命令 \* <em>。 \* \*</em> \* 只有在指定 **/s**時，才能指定/u。 預設值是目前登入發出命令的電腦之使用者的許可權。 |
|        /p \<Password>        |                                                                                                       指定 **/u** 參數中指定之使用者帳戶的密碼。                                                                                                        |
|         一樣 \<Module>         |                                                               列出載入的 DLL 模組符合指定模式名稱的所有工作。 如果未指定模組名稱，此選項會顯示每個工作載入的所有模組。                                                                |
|             /svc             |                                                                                    列出每個進程的所有服務資訊，而不進行截斷。 當 **/fo** 參數設定為 **table**時有效。                                                                                    |
|              /v              |                                                                                 在輸出中顯示詳細資訊工作資訊。 如需完整的詳細資訊輸出而不截斷，請同時使用 **/v** 和 **/svc** 。                                                                                 |
|  /fo {資料表 \| 清單 \| csv}  |                                                                             指定要用於輸出的格式。 有效的值為 **table**、 **list**和 **csv**。 輸出的預設格式為 **table**。                                                                             |
|             /nh              |                                                                                             隱藏輸出中的資料行標頭。 當 **/fo** 參數設定為 **資料表** 或 **csv**時有效。                                                                                              |
|        filter \<Filter>         |                                                                          指定要在查詢中包含或排除的進程類型。 請參閱下表以取得有效的篩選名稱、運算子和值。                                                                          |
|              /?              |                                                                                                                                在命令提示字元顯示說明。                                                                                                                                |

### <a name="filter-names-operators-and-values"></a>篩選名稱、運算子和值

| 篩選名稱 |    有效運算子     |                                                                 有效的值                                                                 |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   狀態    |         eq、ne         |                                                                   RUNNING                                                                    |
|  IMAGENAME  |         eq、ne         |                                                                  映像名稱                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  PID 值                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                工作階段編號                                                                |
| 會話 |         eq、ne         |                                                                 [工作階段名稱]                                                                 |
|   CPUTIME   | eq, ne, gt, lt, ge, le | CPU 時間格式為 <em>HH</em>**：**<em>MM</em>**：**<em>SS</em>，其中 *MM* 和 *SS* 介於0和59之間，而 *HH* 是任何不帶正負號的數位 |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              記憶體使用量（KB）                                                              |
|  USERNAME   |         eq、ne         |                                                             任何有效的使用者名稱                                                              |
|  服務   |         eq、ne         |                                                                 服務名稱                                                                 |
| SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE |         eq、ne         |                                                                 視窗標題                                                                 |
|   模組   |         eq、ne         |                                                                   DLL 名稱                                                                   |

## <a name="remarks"></a>備註

當指定遠端系統時，不支援 SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE 和 STATUS 篩選。

## <a name="examples"></a><a name="BKMK_examples"></a>範例

若要列出處理序識別碼大於1000的所有工作，並以 CSV 格式顯示它們，請輸入：
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
若要列出遠端電腦 "Srvmain" 上的進程的所有服務資訊，其 DLL 名稱開頭為 "ntdll.dll"，請輸入：
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
若要使用您目前登入的使用者帳戶的認證來列出遠端電腦 "Srvmain" 的處理常式，請輸入：
```
tasklist /s srvmain
```
若要列出遠端電腦 "Srvmain" 的處理常式，請使用使用者帳戶 Hiropln 的認證，輸入：
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)