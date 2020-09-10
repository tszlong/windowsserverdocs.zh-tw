---
title: mqbkup
description: Mqbkup 命令的參考文章，此命令會將 MSMQ 訊息檔案和登錄設定備份至存放裝置，並還原先前儲存的訊息和設定。
ms.topic: reference
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 293138a35400613faacb3988add652ec17c97783
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640597"
---
# <a name="mqbkup"></a>mqbkup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 MSMQ 訊息檔案和登錄設定備份至存放裝置，並還原先前儲存的訊息和設定。

備份和還原作業都會停止本機 MSMQ 服務。 如果事先啟動 MSMQ 服務，公用程式將會嘗試在備份或還原作業結束時重新開機 MSMQ 服務。 如果服務在執行公用程式之前已停止，則不會嘗試重新開機服務。

使用 MSMQ 訊息備份/還原公用程式之前，您必須關閉所有使用 MSMQ 的本機應用程式。

## <a name="syntax"></a>語法

```
mqbkup {/b | /r} <folder path_to_storage_device>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /b | 指定備份操作。 |
| /r | 指定還原作業。 |
| `<folder path_to_storage_device>` | 指定儲存 MSMQ 訊息檔案和登錄設定的路徑。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果執行備份或還原作業時，指定的資料夾不存在，則公用程式會自動建立資料夾。

- 如果您選擇指定現有的資料夾，它必須是空的。 如果您指定非空白資料夾，公用程式會刪除其中包含的每個檔案和子資料夾。 在此情況下，系統會提示您授與刪除現有檔案和子資料夾的許可權。 您可以使用 **/y** 參數，表示您事先同意刪除指定資料夾中所有現有的檔案和子資料夾。

- 用來存放 MSMQ 訊息檔案的資料夾位置會儲存在登錄中。 因此，公用程式會將 MSMQ 訊息檔還原到登錄中指定的資料夾，而不是還原作業之前使用的儲存體資料夾。

### <a name="examples"></a>範例

若要備份所有 MSMQ 訊息檔案和登錄設定，並將它們儲存在 C：磁片磁碟機的 *msmqbkup* 資料夾中，請輸入：

```
mqbkup /b c:\msmqbkup
```

若要刪除 C：磁片磁碟機上 *oldbkup* 資料夾中所有現有的檔案和子資料夾，然後將 MSMQ 訊息檔案和登錄設定儲存在資料夾中，請輸入：

```
mqbkup /b /y c:\oldbkup
```

若要還原 MSMQ 訊息和登錄設定，請輸入：

```
mqbkup /r c:\msmqbkup
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [MSMQ Powershell 參考](/powershell/module/msmq/?view=win10-ps)
