---
title: mqbkup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a809bfc788ba25afab280e80e5c6f0dc9c70dc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842899"
---
# <a name="mqbkup"></a>mqbkup

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

備份 MSMQ 訊息的檔案和登錄設定的儲存裝置，並還原先前儲存的訊息和設定。   
備份和還原作業將會停止本機 MSMQ 服務。 如果已事先啟動 MSMQ 服務，此公用程式會嘗試重新啟動 MSMQ 服務結尾的備份或還原作業。 如果服務已停止執行此公用程式之前，會不會嘗試重新啟動服務。  
使用 MSMQ 訊息備份/還原公用程式之前，您必須關閉所有使用 MSMQ 的本機應用程式。  
## <a name="syntax"></a>語法  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/b|指定備份作業|  
|/r|指定還原作業|  
|<folder path_to_storage\_device>|指定 MSMQ 訊息的檔案和登錄設定的儲存位置的路徑|  
|/?|在命令提示字元顯示說明。|  
## <a name="BKMK_Examples"></a>範例  
備份所有的 MSMQ 訊息的檔案和登錄設定，並將其在存放*Msmqbkup* c： 磁碟機上的資料夾。  
```  
mqbkup /b c:\msmqbkup  
```  
如果指定的資料夾不存在，此公用程式會自動建立一個。 如果您選擇指定現有的資料夾，此資料夾必須是空的。 如果您指定的空資料夾，此公用程式將會刪除每個檔案和其內所含的子資料夾。 在此情況下，系統會提示您提供要刪除現有的檔案和子資料夾的權限。 您可以使用 **/y**參數來指出您事先同意的所有現有的檔案和子資料夾中指定的資料夾刪除。  
若要刪除所有檔案和資料夾中的子*Oldbkup*資料夾 c： 磁碟機和存放區 MSMQ 訊息的檔案和登錄設定值，這個資料夾中的。  
```  
mqbkup /b /y c:\oldbkup  
```  
若要還原 MSMQ 訊息和登錄設定：  
```  
mqbkup /r c:\msmqbkup  
```  
用來儲存 MSMQ 訊息檔案資料夾的位置會儲存在登錄中。 因此，此公用程式會在登錄中指定的資料夾，不適用於還原作業之前所使用的儲存體資料夾還原 MSMQ 訊息檔案。 如果登錄中指定的資料夾不存在，還原作業會自動建立它們。 如果資料夾的目錄存在，且不是空白，此公用程式會提示您刪除這些資料夾的目前內容的權限。  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
