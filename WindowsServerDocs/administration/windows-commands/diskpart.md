---
title: diskpart
description: Diskpart 命令直譯器的參考文章，可協助您管理電腦的磁片磁碟機。
ms.topic: reference
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3bdb980754fedefebddfb33b998b37b621c3505a
ms.sourcegitcommit: 4165d4a9198228d4ec809ccd7d791f8de2aeb159
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97091282"
---
# <a name="diskpart"></a>diskpart

> 適用于： Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 以及 Windows Server 2008 R2、Windows Server 2008

Diskpart 命令直譯器可協助您管理電腦的磁片磁碟機 (磁片、磁碟分割、磁片區或虛擬硬碟) 。

在您可以使用 **diskpart** 命令之前，您必須先列出，然後選取物件以提供焦點。 當物件具有焦點之後，您輸入的任何 diskpart 命令將會在該物件上作用。

## <a name="list-available-objects"></a>列出可用的物件

您可以使用下列方法列出可用的物件，並決定物件的數目或磁碟機號：

- `list disk` -顯示電腦上的所有磁片。

- `list volume` -顯示電腦上的所有磁片區。

- `list partition` -顯示在電腦上具有焦點的磁碟分割。

- `list vdisk` -顯示電腦上的所有虛擬磁片。

執行 **清單** 命令之後，會在具有焦點的物件旁出現星號 ( * ) 。

## <a name="determine-focus"></a>判斷焦點

當您選取物件時，焦點會一直處於該物件上，直到您選取其他物件為止。 例如，如果焦點設定在磁片0上，而您選取磁片磁碟機2上的磁片區8，則焦點會從磁片0移至磁片2，第8卷。

有些命令會自動變更焦點。 例如，當您建立新的資料分割時，焦點會自動切換至新的磁碟分割。

您只能將焦點提供給所選磁片上的磁碟分割。 在分割區具有焦點之後，如果有任何) 也有焦點，則相關的磁片區 (。 當磁片區具有焦點之後，如果磁片區對應到單一特定磁碟分割，則相關的磁片和磁碟分割也會有焦點。 如果不是這種情況，則會將焦點放在磁片上，而磁碟分割會遺失。

## <a name="syntax"></a>Syntax

若要啟動 diskpart 命令直譯器，請在命令提示字元中輸入：

```
diskpart <parameter>
```

> [!IMPORTANT]
> 您必須是本機系統 **管理員** 群組或具有類似許可權的群組，才能執行 diskpart。

### <a name="parameters"></a>參數

您可以從 Diskpart 命令直譯器執行下列命令：

| 命令 | 描述 |
| ------- | ----------- |
| [active](active.md) | 將具有焦點的磁碟分割標示為作用中。 |
| [add](add.md) | 將具有焦點的簡單磁碟區鏡像到指定的磁碟。 |
| [assign](assign.md) | 將磁碟機代號或掛接點指派給帶有焦點的磁碟區。 |
| [附加 vdisk](attach-vdisk.md) | 將 (（有時稱為掛接或表面）) 虛擬硬碟 (VHD) ，使其以本機硬碟的形式出現在主機電腦上。 |
| [attributes](attributes.md) | 顯示、設定或清除磁片或磁片區的屬性。 |
| [automount](automount.md) | 啟用或停用自動掛接功能。 |
| [break](break.md) | 將具有焦點的鏡像磁碟區分割成兩個簡單磁碟區。 |
| [clean](clean.md) | 從具有焦點的磁碟移除部分或全部磁碟分割或磁碟區格式化。 |
| [compact vdisk](compact-vdisk.md) | 減少動態擴充的虛擬硬碟 (VHD) 檔的實體大小。 |
| [convert](convert.md) | 將檔案配置表 (FAT) 和 FAT32 磁片區轉換為 NTFS 檔案系統，讓現有的檔案和目錄保持不變。 |
| [create](create.md) | 在磁片上、一或多個磁片上的磁片區，或虛擬硬碟 (VHD) 上建立磁碟分割。 |
| [delete](delete.md) | 刪除磁碟分割或磁片區。 |
| [detach vdisk](detach-vdisk.md) | 停止選取的虛擬硬碟 (VHD) ，使其不會顯示為主電腦上的本機硬碟機。 |
| [detail](detail.md) | 顯示所選磁片、磁碟分割、磁片區或虛擬硬碟 (VHD) 的相關資訊。 |
| [exit](exit.md) | 離開 diskpart 命令直譯器。 |
| [expand vdisk](expand-vdisk.md) | 將虛擬硬碟 (VHD) 擴充為您指定的大小。 |
| [extend](extend.md) | 將具有焦點的磁片區或磁碟分割（以及其檔案系統）擴充到磁片上的可用 (未配置) 空間。 |
| [filesystems](filesystems.md) | 顯示具有焦點之磁片區的目前檔案系統的相關資訊，並列出格式化磁片區所支援的檔案系統。 |
| [format](format.md) | 將磁片格式化以接受 Windows 檔案。 |
| [gpt](gpt.md) | 將) 的 gpt (屬性指派給分割區，並將焦點放在基本 GUID 磁碟分割表格 (gpt) 磁片上。 |
| [說明](help.md) | 顯示指定命令的可用命令或詳細說明資訊的清單。 |
| [import](import_1.md) | 將外部磁片群組匯入本機電腦的磁片群組。 |
| [inactive](inactive.md) | 在基本主機開機記錄上，將具有焦點的系統磁碟分割或開機磁碟分割標示為非使用中 (MBR) 磁片。 |
| [list](list.md) | 顯示磁片的磁碟分割清單、磁片中的磁片區，或虛擬硬碟 (Vhd) 。 |
| [merge vdisk](merge-vdisk.md) | 將差異虛擬硬碟 (VHD) 與其對應的父 VHD 合併。 |
| [offline](offline.md) | 使線上磁片或磁片區進入離線狀態。 |
| [online](online.md) | 會將離線磁片或磁片區設為線上狀態。 |
| [recover](recover.md) | 重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步具有過時資料的鏡像磁片區和 RAID-5 磁片區。 |
| [rem](rem.md) | 提供向指令檔新增註解的方法。 |
| [remove](remove.md) | 從磁片區移除磁碟機號或掛接點。 |
| [repair](repair.md) | 藉由將失敗的磁片區域取代為指定的動態磁碟，修復具有焦點的 RAID-5 磁片區。 |
| [rescan](rescan.md) | 尋找可能已新增到電腦的新磁片。 |
| [retain](retain.md) | 準備要作為開機或系統磁碟區的現有動態簡單磁片區。 |
| [san](san.md) | 為作業系統顯示或設定存放區域網路 (san) 原則。 |
| [select](select.md) | 將焦點移到磁片、磁碟分割、磁片區或虛擬硬碟 (VHD) 。 |
| [set id](set-id.md) | 變更具有焦點之資料分割的資料分割類型欄位。 |
| [shrink](shrink.md) | 依據您指定的數量，減少所選磁片區的大小。 |
| [uniqueid](uniqueid.md) | 針對具有焦點的磁片，顯示或設定 GUID 磁碟分割表格 (GPT) 識別碼或主開機記錄 (MBR) 簽章。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [磁碟管理概觀](../../storage/disk-management/overview-of-disk-management.md)

- [Windows PowerShell 中的儲存體 Cmdlet](/powershell/module/storage/)
