---
Title: DiskPart 命令
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: e04af7b6425e208013277d1aaa6f28af62871bcc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280085"
---
# <a name="diskpart-commands"></a>DiskPart 命令

適用於：Windows 10，Windows 8.1，Windows 8、 Windows 7、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 及 Windows Server 2008

DiskPart 命令可協助您管理您的電腦磁碟機 （磁碟、 磁碟分割、 磁碟區或虛擬硬碟）。 您可以使用 DiskPart 命令之前，您必須先列出，，然後選取 要給予焦點的物件。 當物件有焦點時，該物件會處理任何您所鍵入的 DiskPart 命令。

您可以列出可用的物件，並使用來判斷物件的編號或磁碟機代號**磁碟清單，清單中的磁碟區，清單資料分割**，並**列出 vdisk**命令。 **清單中的磁碟、 磁碟清單 vdisk**並**列出磁碟區**命令顯示電腦上的所有磁碟和磁碟區。 不過，**列出資料分割**命令只會顯示資料分割具有焦點的磁碟。 當您使用**清單**命令，使用星號 (\*) 具有焦點的物件旁邊會出現。

當您選取的物件時，該物件會保持焦點，直到您選取不同的物件。 例如，如果焦點設定在磁碟 0，且您選取 磁碟 2 上的磁碟區 8，焦點就轉移從磁碟 0 到磁碟 2 的第 8 卷。 有些命令會自動變更焦點。 比方說，當您建立新的資料分割時，焦點會自動切換到新的資料分割中。

您只可以將焦點給予所選磁碟上的磁碟分割中。 當資料分割具有焦點時，相關的磁碟區 （如果有的話） 亦具有焦點。 當磁碟區具有焦點，相關的磁碟和磁碟分割亦具有焦點，如果磁碟區對應至單一的特定分割區。 如果這不是，在磁碟上的焦點，且遺失的磁碟分割。

## <a name="diskpart-commands"></a>DiskPart 命令

若要啟動 DiskPart 命令直譯器，在命令提示字元中輸入：

`diskpart`

> [!IMPORTANT]
> 在本機的成員資格**系統管理員**群組或對等項目，是執行 DiskPart 所需的最小。 

在 Diskpart 命令直譯器，您可以執行下列命令：

  - [使用中](active.md)  
      
  - [新增](add.md)  
      
  - [指派](assign.md)  
      
  - [將連結 vdisk](attach-vdisk.md)  
      
  - [屬性](attributes.md)  
      
  - [自動掛接](automount.md)  
      
  - [中斷](break.md)  
      
  - [Clean](clean.md)  
      
  - [compact vdisk](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [建立](create.md)  
      
  - [刪除](delete.md)  
      
  - [中斷連結 vdisk](detach-vdisk.md)  
      
  - [詳細資料](detail.md)  
      
  - [結束](exit.md)  
      
  - [展開 vdisk](expand-vdisk.md)  
      
  - [擴充](extend.md)  
      
  - [檔案系統](filesystems.md)  
      
  - [格式](format.md)  
      
  - [GPT](gpt.md)  
      
  - [說明](help.md)  
      
  - [匯入](import.md)  
      
  - [Inactive](inactive.md)  
      
  - [清單](list.md)  
      
  - [合併 vdisk](merge-vdisk.md)  
      
  - [Offline](offline.md)  
      
  - [Online](online.md)  
      
  - [Recover](recover.md)  
      
  - [Rem](rem.md)  
      
  - [移除](remove.md)  
      
  - [Repair](repair.md)  
      
  - [重新掃描](rescan.md)  
      
  - [保留](retain.md)  
      
  - [San](san.md)  
      
  - [Select](select.md)  
      
  - [集合識別碼](set-id.md)  
      
  - [Shrink](shrink.md)  
      
  - [uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Windows PowerShell 中的儲存體 Cmdlet](https://docs.microsoft.com/powershell/module/storage/)
