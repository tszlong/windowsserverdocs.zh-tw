---
title: DiskPart
description: 適用于**DiskPart**的參考主題，可協助您管理電腦的磁片磁碟機。
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: f127ff4ef1c2d143c956069d1ab3788382e70cc3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719452"
---
# <a name="diskpart"></a>DiskPart

> 適用于： Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2、Windows Server 2008

DiskPart 命令可協助您管理電腦的磁片磁碟機（磁片、磁碟分割、磁片區或虛擬硬碟）。

在您可以使用 DiskPart 命令之前，您必須先列出，然後選取要提供焦點的物件。 當物件有焦點時，您輸入的任何 DiskPart 命令都會在該物件上作用。

## <a name="list-available-objects"></a>列出可用的物件

您可以使用下列方法，列出可用的物件，並決定物件的編號或磁碟機號：

- `list disk`-顯示電腦上的所有磁片。

- `list volume`-顯示電腦上的所有磁片區。

- `list partition`-顯示在電腦上具有焦點的磁碟分割。

- `list vdisk`-顯示電腦上的所有虛擬磁片。

當您使用**清單**命令時，會在具有焦點的物件旁邊出現星號（*）。

## <a name="determine-focus"></a>判斷焦點

當您選取物件時，焦點會一直處於該物件上，直到您選取其他物件為止。 例如，如果焦點是在磁片0上設定，而您在磁片2上選取了第8卷，則焦點會從磁片0轉移到磁片2，第8卷。

有些命令會自動變更焦點。 例如，當您建立新的分割區時，焦點會自動切換到新的資料分割。

您只能將焦點提供給所選磁片上的資料分割。 如果磁碟分割具有焦點，則相關磁碟區 (如果有相關磁碟區) 亦具有焦點。 當磁碟區具有焦點時，如果該磁碟區對應到單一指定磁碟分割，則相關磁碟及磁碟分割亦具有焦點。 如果不是這種情況，將焦點放在磁片上並遺失分割區。

## <a name="diskpart-commands"></a>DiskPart 命令

若要啟動 DiskPart 命令直譯器，請在命令提示字元中輸入：

```
diskpart
```

> [!IMPORTANT]
> 您必須是本機**Administrators**群組或具有類似許可權的群組，才能執行 DiskPart。

您可以從 Diskpart 命令直譯器執行下列命令：

| Command | 描述 |
| ------- | ----------- |
| [使用中](active.md) | 將具有焦點的磁碟分割標示為作用中。 |
| [加入](add.md) | 將具有焦點的簡單磁碟區鏡像到指定的磁碟。 |
| [指派](assign.md) | 將磁碟機代號或掛接點指派給帶有焦點的磁碟區。 |
| [附加 vdisk](attach-vdisk.md) | 連接（有時稱為裝載或介面）虛擬硬碟（VHD），使其在主機電腦上顯示為本機硬碟。 |
| [屬性](attributes.md) | 顯示、設定或清除磁片或磁片區的屬性。 |
| [Automount (自動掛接)](automount.md) | 啟用或停用自動掛接功能。 | 
| [崩潰](break.md) | 將具有焦點的鏡像磁碟區分割成兩個簡單磁碟區。 |
| [病毒](clean.md) | 從具有焦點的磁碟移除部分或全部磁碟分割或磁碟區格式化。 |
| [Compact vdisk](compact-vdisk.md) | 減少動態擴充虛擬硬碟（VHD）檔案的實體大小。 |
| [將](convert.md) | 將檔案分配表（FAT）和 FAT32 磁片區轉換為 NTFS 檔案系統，讓現有的檔案和目錄保持不變。 |
| [建立](create.md) | 在磁片上、一或多個磁片上的磁片區，或虛擬硬碟（VHD）上建立分割區。 |
| [刪除](delete.md) | 刪除分割區或磁片區。 |
| [卸離 vdisk](detach-vdisk.md) | 停止選取的虛擬硬碟（VHD），使其不會顯示為主電腦上的本機硬碟。 |
| [資訊](detail.md) | 顯示所選磁片、磁碟分割、磁片區或虛擬硬碟（VHD）的相關資訊。 |
| [結束](exit.md) | 結束 DiskPart 指令直譯器。 |
| [展開 vdisk](expand-vdisk.md) | 將虛擬硬碟（VHD）擴充為您指定的大小。 |
| [Extend](extend.md) | 將具有焦點的磁片區或磁碟分割（連同其檔案系統）延伸到磁片上的可用（未配置）空間。 |
| [檔案系統](filesystems.md) | 顯示具有焦點之磁片區目前檔案系統的相關資訊，並列出格式化磁片區所支援的檔案系統。 |
| [[格式]](format.md) | 格式化磁片以接受 Windows 檔案。 |
| [GPT](gpt.md) | 指派 gpt 屬性給分割區，並將焦點放在基本的 GUID 磁碟分割表格（gpt）磁片上。 |
| [說明](help.md) | 顯示指定命令的可用命令或詳細說明資訊的清單。 |
| [匯入][](import.md) | 將外部磁片群組匯入本機電腦的磁片群組。 |
| [非作用中](inactive.md) | 在基本主開機記錄（MBR）磁片上，將具有焦點的系統磁碟分割或開機磁碟分割標記為非作用中。 |
| [清單](list.md) | 顯示磁片中的磁碟分割、磁片中的磁片區或虛擬硬碟（Vhd）的磁片清單。 |
| [合併 vdisk](merge-vdisk.md) | 合併差異虛擬硬碟（VHD）與其對應的父 VHD。 |
| [離線](offline.md) | 使線上磁片或磁片區處於離線狀態。 |
| [線上](online.md) | 使離線磁片或磁片區處於線上狀態。 |
| [復原](recover.md) | 重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步擁有過時資料的鏡像磁碟區和 RAID-5 磁片區。 |
| [剩餘](rem.md) | 提供向指令檔新增註解的方法。 |
| [移除](remove.md) | 從磁片區移除磁碟機號或掛接點。 |
| [糾正](repair.md) | 以指定的動態磁碟取代失敗的磁片區域，以修復具有焦點的 RAID-5 磁片區。 |
| [重新掃描](rescan.md) | 尋找可能已新增至電腦的新磁片。 |
| [保留](retain.md) | 準備要當做開機或系統磁碟區使用的現有動態簡單磁片區。 |
| [聖約瑟](san.md) | 顯示或設定作業系統的存放區域網路（san）原則。 |
| [選取](select.md) | 將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟（VHD）。 |
| [設定識別碼](set-id.md) | 變更具有焦點之資料分割的資料分割類型欄位。 |
| [排](shrink.md) | 依據您指定的數量減少所選磁片區的大小。 |
| [Uniqueid](uniqueid.md) | 顯示或設定具有焦點之磁片的 GUID 磁碟分割表格（GPT）識別碼或主要開機記錄（MBR）簽章。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [磁碟管理概觀](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Windows PowerShell 中的儲存體 Cmdlet](https://docs.microsoft.com/powershell/module/storage/)
