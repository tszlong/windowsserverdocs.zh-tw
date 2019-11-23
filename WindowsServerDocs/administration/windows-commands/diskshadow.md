---
title: diskshadow
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8d9f34377473608d71ce7753972e5312d9eea0f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377775"
---
# <a name="diskshadow"></a>diskshadow

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

diskshadow 是一種工具，可公開磁片區陰影複製服務 \(VSS\)所提供的功能。 根據預設，diskshadow 會使用類似于 diskraid 或 DiskPart 的互動式命令直譯器。 diskshadow 也包含可編寫腳本的模式。  
  
> [!NOTE]  
> 若要執行 diskshadow，至少需要本機 Administrators 群組的成員資格或同等許可權。  
  
如需如何使用 diskshadow 命令的範例，請參閱[範例](#BKMK_examples)。  
  
## <a name="syntax"></a>語法  
針對互動模式，請在命令提示字元中輸入下列命令，以啟動 diskshadow 命令直譯器：  
  
```  
diskshadow  
```  
  
針對 [腳本模式]，輸入下列程式碼，其中*script .txt*是包含 diskshadow 命令的腳本檔案：  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>diskshadow 命令  
您可以在 diskshadow 命令直譯器中或透過腳本檔案執行下列命令：  
  
|參數|描述|  
|-------|--------|  
|[set_2](set_2.md)|設定用來建立陰影複製的內容、選項、詳細資訊模式和中繼資料檔案。|  
|[模擬還原](simulate-restore.md)|測試 writer 會參與電腦上的還原會話，而不會對寫入器發出**PreRestore**或**PostRestore**事件。|  
|[載入中繼資料](load-metadata.md)|在匯入可轉移的陰影複製之前載入中繼資料 .cab 檔案，或在還原時載入寫入器中繼資料。|  
|[編寫](writer.md)|確認已包含寫入器或元件，或從備份或還原程式中排除寫入器或元件。|  
|[add_1](add_1.md)|將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。|  
|[create_1](create_1.md)|使用目前的內容和選項設定，啟動陰影複製建立進程。|  
|[exec](exec.md)|在本機電腦上執行檔案。|  
|[開始備份](begin-backup.md)|啟動完整備份會話。|  
|[結束備份](end-backup.md)|結束完整的備份會話，並視需要發出具有適當寫入器狀態的**Backupcomplete**事件。|  
|[開始還原](begin-restore.md)|啟動還原會話，並將**PreRestore**事件發出至相關的寫入器。|  
|[結束還原](end-restore.md)|結束還原會話，並向相關的寫入器發出**PostRestore**事件。|  
|[啟動](reset.md)|將 diskshadow 重設為預設狀態。|  
|[list](list.md)|列出系統上的寫入器、陰影複製或目前已註冊的陰影複製提供者。|  
|[刪除陰影](delete-shadows.md)|刪除陰影複製。|  
|[導](import.md)|從載入的中繼資料檔案，將可轉移的陰影複製匯入到系統中。|  
|[遮罩](mask.md)|移除使用匯**入**命令匯入的硬體陰影複製。|  
|[向](expose.md)|將持續性陰影複製公開為磁碟機號、共用或掛接點。|  
|[diskshadow.exe 取消公開](unexpose.md)|Unexposes 使用**公開**命令公開的陰影複製。|  
|[break_2](break_2.md)|將陰影複製磁片區與 VSS 解除。|  
|[還原](revert.md)|將磁片區還原回指定的陰影複製。|  
|[exit_1](exit_1.md)|結束 diskshadow。|  
  
## <a name="remarks"></a>備註  
  
-   至少要有**add**和**create** ，才可建立陰影複製。 不過，這會要略過內容和選項設定，將會是複本備份，而且只會建立沒有備份執行腳本的陰影複製。  
  
## <a name="BKMK_examples"></a>典型  
這是命令的範例順序，會建立備份的陰影複製。 它可以儲存為 dsh 檔案，並以 diskshadow \/s 腳本執行。 dsh  
  
假設下列各項：  
  
-   您有一個名為 c：\\diskshadowdata 的現有目錄。  
  
-   您的系統磁碟區是 C：，而您的資料磁片區是 d：  
  
-   您在 c：\\diskshadowdata 中有一個 backupscript .cmd 檔案。  
  
-   您的 backupscript .cmd 檔案會將陰影資料 p：和 q：的複本執行到您的備份磁片磁碟機。  
  
您可以手動輸入這些命令或編寫它們的腳本：  
  
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
  
#### <a name="additional-references"></a>其他參考  
[命令列語法關鍵](command-line-syntax-key.md)  
  

