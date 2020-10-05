---
title: taskkill
description: Taskkill 命令的參考文章，此命令會結束一或多個工作或進程。
ms.topic: reference
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e022bc980e2acd7fb70bf13af52f8096fd6aaa1f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718075"
---
# <a name="taskkill"></a>taskkill

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束一個或多個工作或處理序。 處理序可以依處理序識別碼或映像名稱結束。 您可以使用 [ [tasklist 命令](tasklist.md) ] 命令來判斷處理序識別碼 (PID) ，以便結束進程。

> [!NOTE]
> 此命令會取代 **kill** tool。

## <a name="syntax"></a>語法

```
taskkill [/s <computer> [/u [<domain>\]<username> [/p [<password>]]]] {[/fi <filter>] [...] [/pid <processID> | /im <imagename>]} [/f] [/t]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
|  /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| u `<domain>\<username>` | 使用或所指定之使用者的帳戶許可權來執行此命令 `<username>` `<domain>\<username>` 。 只有在同時指定 **/s**時，才能指定 **/u**參數。 預設值是目前登入發出命令的電腦之使用者的許可權。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| filter `<filter>` | 套用篩選以選取一組工作。 您可以使用一個以上的篩選準則，或使用萬用字元 (`*`) 來指定所有的工作或映射名稱。 有效的篩選器會列在本文的 [ **篩選名稱]、[運算子] 和 [值** ] 區段中。 |
| /pid `<processID>` | 指定要終止之進程的處理序識別碼。 |
| /im `<imagename>` | 指定要終止之進程的映射名稱。 使用 (`*`) 萬用字元來指定所有的映射名稱。 |
| /f | 指定強制結束處理常式。 遠端進程會忽略此參數;所有遠端進程都會強制結束。 |
| /t | 結束指定的進程及其啟動的任何子進程。 |

#### <a name="filter-names-operators-and-values"></a>篩選名稱、運算子和值

| 篩選名稱 | 有效運算子 | 有效的值 (s)  |
|--|--|--|
| 狀態 | eq、ne | `RUNNING | NOT RESPONDING | UNKNOWN` |
| IMAGENAME | eq、ne | 映像名稱 |
| PID | eq, ne, gt, lt, ge, le | PID 值 |
| SESSION | eq, ne, gt, lt, ge, le | 工作階段編號 |
| CPUtime | eq, ne, gt, lt, ge, le | CPU 時間格式為 *HH： MM： SS*，其中 *MM* 和 *SS* 介於0和59之間，而 *HH* 是任何不帶正負號的數位 |
| MEMUSAGE | eq, ne, gt, lt, ge, le | 記憶體使用量（KB） |
| USERNAME | eq、ne | 任何有效的使用者名稱 (`<user>` 或 `<domain\user>`)  |
| 服務 | eq、ne | 服務名稱 |
| SYSTEM.WINDOWS.CONTROLS.PAGE.WINDOWTITLE | eq、ne | 視窗標題 |
| 模組 | eq、ne | DLL 名稱 |

## <a name="remarks"></a>備註

- 當指定遠端系統時，不支援 **system.windows.controls.page.windowtitle** 和 **STATUS** 篩選。

- `*` `*/im` 只有套用篩選時，才會接受選項的萬用字元 () 。

- 不論是否已指定 **/f** 選項，一律會強制執行遠端進程。

- 將電腦名稱稱提供給主機名稱篩選器會導致關機，並停止所有的進程。

## <a name="examples"></a>範例

若要結束處理常式識別碼為 *1230*、 *1241*和 *1253*的進程，請輸入：

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

若要強制結束進程 *Notepad.exe* 如果系統已啟動此程式，請輸入：

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

若要使用以*附注*開頭的映射名稱來結束遠端電腦*Srvmain*上的所有處理常式，使用使用者帳戶*Hiropln*的認證時，請輸入：

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

若要結束處理常式識別碼為 *2134* 的進程及其啟動的任何子進程，但只有系統管理員帳戶啟動這些處理常式時，請輸入：

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

若要結束處理序識別碼 *大於或等於 1000*的所有進程（不論其映射名稱為何），請輸入：

```
taskkill /f /fi "PID ge 1000" /im *
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [tasklist 命令](tasklist.md)
