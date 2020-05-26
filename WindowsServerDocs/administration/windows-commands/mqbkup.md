---
title: mqbkup
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bcd31ba6fd2a85c00e7c684f4aeec12c4899c259
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820828"
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
#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|/b|指定備份作業|
|/r|指定還原作業|
|<資料夾 path_to_storage \_ 裝置>|指定儲存 MSMQ 訊息檔案和登錄設定的路徑|
|/?|在命令提示字元顯示說明。|
## <a name="examples"></a>範例
若要備份所有 MSMQ 訊息檔案和登錄設定，並將它們儲存在 C：磁片磁碟機的*Msmqbkup*資料夾中。
```
mqbkup /b c:\msmqbkup
```
如果指定的資料夾不存在，公用程式就會自動建立一個。 如果您選擇指定現有的資料夾，此資料夾必須是空的。 如果您指定非空白的資料夾，公用程式會刪除其中包含的每個檔案和子資料夾。 在此情況下，系統會提示您提供刪除現有檔案和子資料夾的許可權。 您可以使用 **/y**參數來表示您已事先同意刪除指定資料夾中的所有現有檔案和子資料夾。
若要刪除 C：磁片磁碟機上*Oldbkup*資料夾中的所有檔案和子資料夾，並將 MSMQ 訊息檔案和登錄設定儲存在此資料夾中。
```
mqbkup /b /y c:\oldbkup
```
若要還原 MSMQ 訊息和登錄設定：
```
mqbkup /r c:\msmqbkup
```
用來儲存 MSMQ 訊息檔案的資料夾位置會儲存在登錄中。 因此，公用程式會將 MSMQ 訊息檔案還原至登錄中指定的資料夾，而不是還原作業之前使用的儲存體資料夾。 如果登錄中指定的資料夾不存在，還原作業將會自動建立它們。 如果資料夾目錄存在且不是空的，公用程式會提示您提供刪除這些資料夾目前內容的許可權。
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
