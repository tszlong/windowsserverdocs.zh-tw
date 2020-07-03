---
title: mqbkup
description: Mqbkup 命令的參考文章，它會將 MSMQ 訊息檔案和登錄設定備份到存放裝置，並還原先前儲存的訊息和設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 131d80f32a3c3324dad08b876dd4f4f8610b93e2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936298"
---
# <a name="mqbkup"></a>mqbkup

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 MSMQ 訊息檔案和登錄設定備份到存放裝置，並還原先前儲存的訊息和設定。

備份和還原作業都會停止本機 MSMQ 服務。 如果已事先啟動 MSMQ 服務，公用程式會嘗試在備份結束或還原作業後重新開機 MSMQ 服務。 如果在執行公用程式之前已停止服務，則不會嘗試重新開機服務。

使用 MSMQ 訊息備份/還原公用程式之前，您必須先關閉所有使用 MSMQ 的本機應用程式。

## <a name="syntax"></a>語法

```
mqbkup {/b | /r} <folder path_to_storage_device>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ------- | -------- |
| /b | 指定備份作業。 |
| /r | 指定還原作業。 |
| `<folder path_to_storage_device>` | 指定儲存 MSMQ 訊息檔案和登錄設定的路徑。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果在執行備份或還原作業時，指定的資料夾不存在，公用程式就會自動建立資料夾。

- 如果您選擇指定現有的資料夾，它必須是空的。 如果您指定非空白的資料夾，公用程式會刪除其中包含的每個檔案和子資料夾。 在此情況下，系統會提示您提供刪除現有檔案和子資料夾的許可權。 您可以使用 **/y**參數來表示您已事先同意刪除指定資料夾中的所有現有檔案和子資料夾。

- 用來儲存 MSMQ 訊息檔案的資料夾位置會儲存在登錄中。 因此，公用程式會將 MSMQ 訊息檔案還原至登錄中指定的資料夾，而不是還原作業之前使用的儲存體資料夾。

### <a name="examples"></a>範例

若要備份所有 MSMQ 訊息檔案和登錄設定，並將它們儲存在 C：磁片磁碟機的*msmqbkup*資料夾中，請輸入：

```
mqbkup /b c:\msmqbkup
```

若要刪除 C：磁片磁碟機上*oldbkup*資料夾中所有現有的檔案和子資料夾，然後將 MSMQ 訊息檔案和登錄設定儲存在資料夾中，請輸入：

```
mqbkup /b /y c:\oldbkup
```

若要還原 MSMQ 訊息和登錄設定，請輸入：

```
mqbkup /r c:\msmqbkup
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [MSMQ Powershell 參考](https://docs.microsoft.com/powershell/module/msmq/?view=win10-ps)
