---
title: taskkill
description: Taskkill 的 Windows 命令主題，它會結束一或多個工作或處理常式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e915f9cbeac0e310b65cb660b2edb93b5d002de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833421"
---
# <a name="taskkill"></a>taskkill

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束一個或多個工作或處理序。 處理序可以依處理序識別碼或映像名稱結束。 **taskkill**會取代**kill**工具。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

### <a name="parameters"></a>參數

|         參數         |                                                                                                                                        描述                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<電腦 >       |                                                                                    指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                                                     |
| /u \<網域 >\\\<使用者名稱 > | 使用*username*或*Domain*\\*username*指定之使用者的帳戶許可權來執行命令。 只有在指定 **/s**時，才可以指定 **/u** 。 預設為目前登入發出命令之電腦的使用者許可權。 |
|      /p \<密碼 >       |                                                                                                   指定 **/u**參數中指定之使用者帳戶的密碼。                                                                                                   |
|       /fi \<篩選 >       |          套用篩選以選取一組工作。 您可以使用一個以上的篩選準則，或使用萬用字元（ **\\** \*）來指定所有工作或映射名稱。 請參閱下[表以取得有效的篩選器名稱](#filter-names-operators-and-values)、運算子和值。           |
|     /pid \<ProcessID >     |                                                                                                                 指定要終止之進程的處理序識別碼。                                                                                                                 |
|     /im \<ImageName >      |                                                                                指定要終止之進程的映射名稱。 使用萬用字元（ **\\** \*）來指定所有映射名稱。                                                                                |
|            /f             |                                                                    指定強制終止進程。 遠端進程會忽略此參數;所有遠端進程都會強制終止。                                                                     |
|            /t             |                                                                                                          終止指定的進程和其啟動的任何子進程。                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>篩選名稱、運算子和值

| 篩選器名稱 |    有效的運算子     |                                                                有效的值                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   狀態    |         eq、ne         |                                                 執行&#124;未回應&#124;不明                                                 |
|  IMAGENAME  |         eq、ne         |                                                                  映射名稱                                                                  |
|     PID     | eq、ne、gt、lt、ge、le |                                                                  PID 值                                                                   |
|   本次   | eq、ne、gt、lt、ge、le |                                                                工作階段編號                                                                |
|   CPUtime   | eq、ne、gt、lt、ge、le | CPU 時間，格式為<em>HH</em> **：** <em>MM</em> **：** <em>SS</em>，其中*MM*和*SS*介於0到59之間，而*HH*則為任何不帶正負號的數位 |
|  MEMUSAGE   | eq、ne、gt、lt、ge、le |                                                              記憶體使用量（KB）                                                              |
|  使用者名稱   |         eq、ne         |                                               任何有效的使用者名稱（*使用者*或*網域*\\*使用者*）                                               |
|  伺服器   |         eq、ne         |                                                                 服務名稱                                                                 |
| SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE |         eq、ne         |                                                                 視窗標題                                                                 |
|   模組   |         eq、ne         |                                                                   DLL 名稱                                                                   |

## <a name="remarks"></a>備註
* 當指定遠端系統時，不支援 SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE 和 STATUS 篩選。
* 只有在套用篩選時，才<em>會接受 * */im</em>* 選項的萬用字元（ **\\** ）。
* 無論是否已指定 **/f**選項，一律會強制執行遠端進程終止。
* 將電腦名稱稱提供給主機名稱篩選器會導致關機，並停止所有進程。
* 您可以使用**tasklist**來判斷要終止之進程的處理序識別碼（PID）。

## <a name="examples"></a>範例

若要結束進程識別碼為1230、1241和1253的進程，請輸入：

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

若要強制結束系統啟動的進程，請輸入：

```
taskkill /f /fi USERNAME eq NT AUTHORITY\SYSTEM /im notepad.exe
```

若要結束遠端電腦 Srvmain 上的所有進程，並以映射名稱開頭為 note，使用使用者帳戶 Hiropln 的認證時，請輸入：

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi IMAGENAME eq note* /im *
```

若要結束處理序識別碼為2134的程式和它所啟動的任何子進程，但只有在系統管理員帳戶啟動這些進程時，請輸入：

```
taskkill /pid 2134 /t /fi username eq administrator
```

若要結束處理序識別碼大於或等於1000的所有進程（不論其映射名稱為何），請輸入：

```
taskkill /f /fi PID ge 1000 /im *
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
