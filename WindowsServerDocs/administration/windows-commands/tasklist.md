---
title: tasklist
description: 瞭解如何顯示在本機或遠端電腦上執行的處理常式清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b43f4c9a89fa60f2244253d48d3dca646fe8e02d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833431"
---
# <a name="tasklist"></a>tasklist

顯示目前在本機電腦或遠端電腦上正在執行的處理序清單。 **Tasklist**取代了**tlist.exe**工具。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

### <a name="parameters"></a>參數

|          參數           |                                                                                                                                            描述                                                                                                                                             |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        /s \<電腦 >        |                                                                                         指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                                                         |
| /u [\<網域 >\\\]\<使用者名稱 > | 使用*username*或*Domain*\*username 所指定使用者的帳戶許可權來執行命令。只有在指定 **/s**時，才可以指定<em>\*\*/u</em>\*。 預設為目前登入發出命令之電腦的使用者許可權。 |
|        /p \<密碼 >        |                                                                                                       指定 **/u**參數中指定之使用者帳戶的密碼。                                                                                                        |
|         /m \<模組 >         |                                                               列出載入符合指定模式名稱之 DLL 模組的所有工作。 如果未指定模組名稱，此選項會顯示每個工作載入的所有模組。                                                                |
|             /svc             |                                                                                    列出每個進程的所有服務資訊，而不進行截斷。 當 **/fo**參數設定為**table**時有效。                                                                                    |
|              /v              |                                                                                 在輸出中顯示詳細的工作資訊。 如需完整的詳細資訊輸出而不截斷，請同時使用 **/v**和 **/svc** 。                                                                                 |
|  /fo {table \| 清單 \| csv}  |                                                                             指定要用於輸出的格式。 有效值為**table**、 **list**和**csv**。 輸出的預設格式為**table**。                                                                             |
|             /nh              |                                                                                             隱藏輸出中的資料行標頭。 當 **/fo**參數設定為**table**或**csv**時有效。                                                                                              |
|        /fi \<篩選 >         |                                                                          指定要在查詢中包含或排除的處理常式類型。 請參閱下表以取得有效的篩選器名稱、運算子和值。                                                                          |
|              /?              |                                                                                                                                在命令提示字元顯示說明。                                                                                                                                |

### <a name="filter-names-operators-and-values"></a>篩選名稱、運算子和值

| 篩選器名稱 |    有效的運算子     |                                                                 有效的值                                                                 |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   狀態    |         eq、ne         |                                                                   運行                                                                    |
|  IMAGENAME  |         eq、ne         |                                                                  映射名稱                                                                  |
|     PID     | eq、ne、gt、lt、ge、le |                                                                  PID 值                                                                   |
|   本次   | eq、ne、gt、lt、ge、le |                                                                工作階段編號                                                                |
| 會話 |         eq、ne         |                                                                 會話名稱                                                                 |
|   CPUTIME   | eq、ne、gt、lt、ge、le | CPU 時間，格式為<em>HH</em> **：** <em>MM</em> **：** <em>SS</em>，其中*MM*和*SS*介於0到59之間，而*HH*則為任何不帶正負號的數位 |
|  MEMUSAGE   | eq、ne、gt、lt、ge、le |                                                              記憶體使用量（KB）                                                              |
|  使用者名稱   |         eq、ne         |                                                             任何有效的使用者名稱                                                              |
|  伺服器   |         eq、ne         |                                                                 服務名稱                                                                 |
| SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE |         eq、ne         |                                                                 視窗標題                                                                 |
|   模組   |         eq、ne         |                                                                   DLL 名稱                                                                   |

## <a name="remarks"></a>備註

當指定遠端系統時，不支援 SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE 和 STATUS 篩選。

## <a name="examples"></a><a name="BKMK_examples"></a>典型

若要列出處理序識別碼大於1000的所有工作，並將其顯示為 CSV 格式，請輸入：
```
tasklist /v /fi "PID gt 1000" /fo csv
```
若要列出目前正在執行的系統進程，請輸入：
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
若要列出目前正在執行之所有進程的詳細資訊，請輸入：
```
tasklist /v /fi "STATUS eq running"
```
若要列出遠端電腦 "Srvmain" 上之進程的所有服務資訊，其 DLL 名稱開頭為 "ntdll.dll"，請輸入：
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
若要使用您目前登入的使用者帳戶的認證來列出遠端電腦上的處理常式 "Srvmain"，請輸入：
```
tasklist /s srvmain 
```
若要使用使用者帳戶 Hiropln 的認證來列出遠端電腦上的處理常式 "Srvmain"，請輸入：
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)