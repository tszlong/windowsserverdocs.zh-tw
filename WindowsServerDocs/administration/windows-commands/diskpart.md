---
title: DiskPart 命令
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7155dbf34f9986b3ebdd8b549b6a861cf7fcfe3a
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560440"
---
# <a name="diskpart-commands"></a>DiskPart 命令

適用於：Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2、Windows Server 2008

DiskPart 命令可協助您管理電腦的磁片磁碟機 (磁片、磁碟分割、磁片區或虛擬硬碟)。 在您可以使用 DiskPart 命令之前, 您必須先列出, 然後選取要提供焦點的物件。 當物件有焦點時, 您輸入的任何 DiskPart 命令都會在該物件上作用。

您可以使用**list disk、list volume、list partition**和**list vdisk**命令, 列出可用的物件, 並決定物件的編號或磁碟機號。 **List disk、list vdisk**和**list volume**命令會顯示電腦上的所有磁片和磁片區。 不過, [**清單磁碟分割**] 命令只會顯示具有焦點之磁片上的資料分割。 當您使用**清單**命令時, 星號 (\*) 會出現在具有焦點的物件旁邊。

當您選取物件時, 焦點會保留在該物件上, 直到您選取不同的物件為止。 例如, 如果焦點是在磁片0上設定, 而您在磁片2上選取了第8卷, 則焦點會從磁片0轉移到磁片 2, 第8卷。 有些命令會自動變更焦點。 例如, 當您建立新的分割區時, 焦點會自動切換到新的資料分割。

您只能將焦點提供給所選磁片上的資料分割。 當分割區具有焦點時, 相關的磁片區 (如果有的話) 也會有焦點。 當磁片區具有焦點時, 如果磁片區對應至單一特定分割區, 則相關的磁片和分割區也會有焦點。 如果不是這種情況, 將焦點放在磁片上並遺失分割區。

## <a name="diskpart-commands"></a>DiskPart 命令

若要啟動 DiskPart 命令直譯器, 請在命令提示字元中輸入:

`diskpart`

> [!IMPORTANT]
> 若要執行 DiskPart, 至少需要本機**Administrators**群組的成員資格或同等許可權。 

您可以在 Diskpart 命令直譯器中執行下列命令:

  - [使用中](active.md)  
      
  - [新增](add.md)  
      
  - [值賦](assign.md)  
      
  - [附加 vdisk](attach-vdisk.md)  
      
  - [特性](attributes.md)  
      
  - [Automount (自動掛接)](automount.md)  
      
  - [崩潰](break.md)  
      
  - [病毒](clean.md)  
      
  - [Compact vdisk](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [建立](create.md)  
      
  - [刪除](delete.md)  
      
  - [卸離 vdisk](detach-vdisk.md)  
      
  - [資訊](detail.md)  
      
  - [結束](exit.md)  
      
  - [展開 vdisk](expand-vdisk.md)  
      
  - [擴充](extend.md)  
      
  - [檔](filesystems.md)  
      
  - [格式](format.md)  
      
  - [GPT](gpt.md)  
      
  - [說明](help.md)  
      
  - [匯入](import.md)  
      
  - [非使用](inactive.md)  
      
  - [名單](list.md)  
      
  - [合併 vdisk](merge-vdisk.md)  
      
  - [離線](offline.md)  
      
  - [線上](online.md)  
      
  - [Recover](recover.md)  
      
  - [剩餘](rem.md)  
      
  - [移除](remove.md)  
      
  - [糾正](repair.md)  
      
  - [L](rescan.md)  
      
  - [幻燈片](retain.md)  
      
  - [聖約瑟](san.md)  
      
  - [請](select.md)  
      
  - [設定識別碼](set-id.md)  
      
  - [排](shrink.md)  
      
  - [Uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Windows PowerShell 中的儲存體 Cmdlet](https://docs.microsoft.com/powershell/module/storage/)
