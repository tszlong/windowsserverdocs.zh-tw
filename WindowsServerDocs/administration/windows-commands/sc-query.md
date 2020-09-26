---
title: Sc.exe 查詢
description: sc.exe query 命令的參考文章，可取得並顯示指定服務、驅動程式、服務類型或驅動程式類型的相關資訊。
ms.topic: reference
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d5994c54914165cb79ba0f505f1a44b2d7f019a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388211"
---
# <a name="scexe-query"></a>Sc.exe 查詢

取得並顯示指定服務、驅動程式、服務類型或驅動程式類型的相關資訊。

## <a name="syntax"></a>語法

```
sc.exe [<servername>] query [<servicename>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <Buffersize>] [ri= <Resumeindex>] [group= <groupname>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<servername>` | 指定服務所在的遠端伺服器名稱。 名稱必須使用通用命名慣例 (UNC) 格式 (例如， \\ myserver) 。 若要在本機執行 SC.exe，請不要使用此參數。 |
| `<servicename>` | 指定 **getkeyname** 作業所傳回的服務名稱。 此**查詢**參數不會搭配 (*servername*) 以外的其他**查詢**參數使用。 |
| `type= {driver | service | all}` | 指定要列舉的內容。 選項包括：<ul><li>**驅動程式** -指定只列舉驅動程式。</li><li>**服務** -只會指定列舉服務。 這是預設值。</li><li>**全部** -指定列舉驅動程式和服務。</li></ul> |
| `type= {own | share | interact | kernel | filesys | rec | adapt}` | 指定要列舉的服務類型或驅動程式類型。 選項包括：<ul><li>**自有** -指定在其本身的進程中執行的服務。 它不會與其他服務共用可執行檔。 這是預設值。</li><li>**共用** -指定以共用進程的形式執行的服務。 它會與其他服務共用可執行檔。</li><li>**核心** -指定驅動程式。</li><li>**filesys** -指定檔案系統驅動程式。</li><li>**rec** -指定可識別電腦上使用之檔案系統的檔案系統辨識驅動程式。</li><li>**互動** -指定可與桌面互動的服務，並接收來自使用者的輸入。 互動式服務必須在 LocalSystem 帳戶下執行。 此類型必須與**type = 自有**或**type = shared** (搭配使用，例如**type = 互動****類型 = 自有**) 。 使用 **type = 互動** 本身將會產生錯誤。</li></ul> |
| `state= {active | inactive | all}` | 指定要列舉之服務的啟動狀態。 選項包括：<ul><li>**active** -指定所有使用中的服務。 這是預設值。</li><li>**非** 使用中-指定所有暫停或停止的服務。</li><li>**全部** -指定所有服務。</li></ul> |
| `bufsize= <Buffersize>` | 指定列舉緩衝區)  (位元組大小。 預設的緩衝區大小為1024個位元組。 當查詢所產生的顯示超過1024個位元組時，您應該增加緩衝區的大小。 |
| `ri= <Resumeindex>` | 指定要開始或繼續列舉的索引編號。 預設值是 0 (零)。 如果傳回的資訊超過預設緩衝區可顯示的數目，請使用此參數搭配 `bufsize=` 參數。 |
| `group= <Groupname>` | 指定要列舉的服務群組。 預設會列舉所有群組。 根據預設，所有群組都會列舉 ( * * group = * * ) 。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 每個命令列選項 (參數) 都必須包含等號作為選項名稱的一部分。

- 選項和其值之間必須有一個空格 (例如， **type =** required。 如果省略空格，作業就會失敗。

- **查詢**作業會顯示服務的下列相關資訊： SERVICE_NAME (服務的登錄子機碼名稱) 、類型、狀態 (以及無法使用的狀態) 、WIN32_EXIT_B、SERVICE_EXIT_B、檢查點和 WAIT_HINT。

- 在某些情況下， **type =** 參數可以使用兩次。 **Type =** 參數的第一個外觀會指定是否要查詢服務、驅動程式或兩者 (**所有**) 。 **Type =** 參數的第二個外觀會指定**建立**作業的型別，以進一步縮小查詢範圍。

- 當 **查詢** 命令的顯示結果超過列舉緩衝區的大小時，會顯示類似如下的訊息：

  ```
  Enum: more data, need 1822 bytes start resume at index 79

  To display the remaining **query** information, rerun **query**, setting **bufsize=** to be the number of bytes and setting **ri=** to the specified index. For example, the remaining output would be displayed by typing the following at the command prompt:

  sc.exe query bufsize= 1822 ri= 79
  ```

## <a name="examples"></a>範例

若只要顯示作用中服務的資訊，請輸入下列其中一個命令：

```
sc.exe query
sc.exe query type= service
```

若要顯示作用中服務的資訊，以及指定緩衝區大小為2000個位元組，請輸入：

```
sc.exe query type= all bufsize= 2000
```

若要顯示 *wuauserv* 服務的資訊，請輸入：

```
sc.exe query wuauserv
```

若要顯示所有服務的資訊 (使用中和非使用中的) ，請輸入：

```
sc.exe query state= all
```

若要顯示所有服務的資訊 (使用中和非使用中) ，從行56開始，請輸入：

```
sc.exe query state= all ri= 56
```

若要顯示互動式服務的資訊，請輸入：

```
sc.exe query type= service type= interact
```

若只要顯示驅動程式的資訊，請輸入：

```
sc.exe query type= driver
```

若要顯示 *網路驅動程式介面規格 (NDIS) 群組*中驅動程式的資訊，請輸入：

```
sc.exe query type= driver group= NDIS
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
