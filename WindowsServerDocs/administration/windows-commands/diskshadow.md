---
title: diskshadow
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c5648235a1c856c6aef09621e2381e74d08d70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869179"
---
# <a name="diskshadow"></a>diskshadow

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

diskshadow.exe 是一種工具，公開由磁碟區陰影複製服務所提供的功能\(VSS\)。 根據預設，diskshadow 使用與 diskraid 或 DiskPart 相似的互動式命令直譯器。 diskshadow 也包含可編寫指令碼的模式。  
  
> [!NOTE]  
> 執行 diskshadow 所需的最小的成員資格的本機 Administrators 群組或同等權限。  
  
如需如何使用 diskshadow 命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。  
  
## <a name="syntax"></a>語法  
互動模式中，輸入下列命令在命令提示字元中，啟動 diskshadow 命令直譯器：  
  
```  
diskshadow  
```  
  
指令碼模式中，輸入下列命令，其中*script.txt*是包含 diskshadow 命令的指令碼檔案：  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>diskshadow 命令  
在 diskshadow 命令直譯器，或透過指令碼檔案，您可以執行下列命令：  
  
|參數|描述|  
|-------|--------|  
|[set_2](set_2.md)|設定內容、 選項、 詳細資訊模式，以及用來建立陰影複製的中繼資料檔案。|  
|[模擬還原](simulate-restore.md)|測試電腦上的還原工作階段中的寫入器的參與，但不會發出**PreRestore**或是**PostRestore**寫入器的事件。|  
|[載入中繼資料](load-metadata.md)|載入中繼資料.cab 檔匯入傳送陰影複製之前，或載入在還原時的寫入器中繼資料。|  
|[writer](writer.md)|確認寫入器或元件就會包含或排除在備份或還原程序中的寫入器或元件。|  
|[add_1](add_1.md)|將磁碟區新增至包含要進行陰影複製，磁碟區組，或將別名新增至 「 別名 」 環境。|  
|[create_1](create_1.md)|啟動陰影複製建立程序，使用目前的內容和選項設定。|  
|[exec](exec.md)|執行本機電腦上的檔案。|  
|[開始備份](begin-backup.md)|啟動完整備份的工作階段。|  
|[結束備份](end-backup.md)|結束的完整備份的工作階段和問題**Backupcomplete**與適當的寫入器狀態，如有需要的事件。|  
|[開始還原](begin-restore.md)|啟動還原工作階段和問題**PreRestore**所涉及的寫入器的事件。|  
|[結束還原](end-restore.md)|結束還原工作階段和問題**PostRestore**所涉及的寫入器的事件。|  
|[reset](reset.md)|diskshadow 重設為預設狀態。|  
|[list](list.md)|清單寫入器、 陰影複製或在系統上目前已註冊的陰影複製提供者。|  
|[刪除陰影](delete-shadows.md)|刪除陰影複製。|  
|[import](import.md)|從載入的中繼資料檔案中會傳送陰影複製匯入到系統。|  
|[mask](mask.md)|移除已匯入所使用的硬體陰影複製**匯入**命令。|  
|[expose](expose.md)|會公開為磁碟機代號、 共用或掛接點的持續性的陰影複製。|  
|[unexpose](unexpose.md)|Unexposes 的陰影複製，使用已公開**公開**命令。|  
|[break_2](break_2.md)|取消關聯沒有來自 VSS 陰影複製磁碟區|  
|[revert](revert.md)|還原至指定的陰影複製磁碟區。|  
|[exit_1](exit_1.md)|diskshadow 就會結束。|  
  
## <a name="remarks"></a>備註  
  
-   最少，只有**新增**並**建立**建立陰影複製所需。 不過，這會違反的內容和選項的設定，將會複製備份，而且沒有備份的執行指令碼才會建立陰影複製。  
  
## <a name="BKMK_examples"></a>範例  
這是範例一連串命令，以建立備份陰影複製。 儲存至檔案作為 script.dsh，並使用 diskshadow 執行\/s script.dsh  
  
假設：  
  
-   您有現有的目錄稱為 c:\\diskshadowdata。  
  
-   您的系統磁碟區為 c： 與您的資料量是 d:。  
  
-   您有在 c: backupscript.cmd 檔案\\diskshadowdata。  
  
-   Backupscript.cmd 檔案將會執行陰影資料 p： 及問： 您備份的磁碟機的複本。  
  
您可以手動輸入下列命令，或編寫其指令碼：  
  
```  
#diskshadow script file  
set context persistent nowriters  
set metadata c:\diskshadowdata\example.cab  
set verbose on  
begin backup  
add volume c: alias Systemvolumeshadow  
add volume d: alias Datavolumeshadow  
  
create  
  
expose %Systemvolumeshadow% p:  
expose %Datavolumeshadow% q:  
exec c:\diskshadowdata\backupscript.cmd  
end backup  
#End of script  
```  
  
#### <a name="additional-references"></a>其他參考資料  
[命令列語法關鍵](command-line-syntax-key.md)  
  

