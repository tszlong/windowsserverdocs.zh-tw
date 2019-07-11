---
title: 磁碟管理概觀
description: '[磁碟管理] 是 Windows 中的系統公用程式，可讓您執行進階儲存體工作，例如將新磁碟機初始化、延伸磁碟區、壓縮磁碟分割及變更磁碟機代號。'
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a3885ae6b09ad431fd1ea5e4c593e02c7bb274d9
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812544"
---
# <a name="overview-of-disk-management"></a>磁碟管理概觀

> **適用於：** Windows 10、Windows 8.1、Windows 7、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[磁碟管理] 是 Windows 中的系統公用程式，可讓您執行進階儲存體工作。 以下是可透過 [磁碟管理] 執行的一些工作：

- 若要設定新的磁碟機，請參閱[將新磁碟機初始化](initialize-new-disks.md)。
- 若要將磁碟區延伸到相同磁碟機上尚不屬於磁碟區一部分的空間，請參閱[延伸基本磁碟區](extend-a-basic-volume.md)。
- 若要壓縮磁碟分割 (通常是為了可以延伸鄰近磁碟分割)，請參閱[壓縮基本磁碟區](shrink-a-basic-volume.md)。
- 若要變更磁碟機代號或指派新的磁碟機代號，請參閱[變更磁碟機代號](change-a-drive-letter.md)。

![顯示一般磁碟機的 [磁碟管理]，該磁碟機具有三個磁碟分割 - 499 MB 系統磁碟分割、用於 Windows 的較大型 C 磁碟機，以及用於修復的另一個 499 MB 磁碟分割](media/disk-management.png)

> [!TIP]
>  如果您在遵循這些程序時收到錯誤或出現問題，請參閱[針對磁碟管理問題進行疑難排解](troubleshooting-disk-management.md)主題。 如果還是沒有用，別慌張！ [Microsoft 社群](https://answers.microsoft.com/en-us/windows)網站上有大量的資訊，因此請嘗試搜尋[檔案、資料夾與儲存體](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639)一節；如果您還需要協助，請於此處張貼問題，Microsoft 或其他社群成員將會嘗試提供協助。 如果您對改善這些主題有任何意見反應，歡迎與我們分享！ 只要回答「此頁面有所助益嗎？」  提示，並於此處或在本主題底部的公開意見討論區中留下任何意見。

以下是您可能想要執行但使用其他 Windows 工具的一些常見工作：

- 若要釋放磁碟空間，請參閱[在 Windows 10 中釋放磁碟機空間](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)。
- 若要為您的磁碟機進行磁碟重組，請參閱[為您的 Windows 10 電腦進行磁碟重組](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc)。
- 若要將多個硬碟彙集在一起 (類似於 RAID)，請參閱[儲存空間](https://support.microsoft.com/help/12438/windows-10-storage-spaces)。

## <a name="about-those-extra-recovery-partitions"></a>關於這些額外的修復磁碟分割

如果您想知道 (我們已讀取您的意見！)，Windows 通常在主要磁碟機 (通常是 C:\ 磁碟機) 上包含三個磁碟分割：

![磁碟 0，其中顯示三個磁碟分割：EFI 系統磁碟分割、Windows 磁碟分割和修復磁碟分割](media/windows-partitions.png)

- **EFI 系統磁碟分割** - 新式電腦使用此磁碟分割來啟動 (開機) 您的電腦和作業系統。
- **Windows 作業系統磁碟機 (C:)** - 這是 Windows 的安裝位置，且通常是您放置其餘應用程式和檔案的位置。
- **修復磁碟分割** - 這是特殊工具的儲存位置，這些工具可在您無法啟動或遇到其他嚴重問題時，協助您還原 Windows。

雖然 [磁碟管理] 可能顯示 EFI 系統磁碟分割和修復磁碟分割 100% 可用，但並非如此。 這些磁碟分割通常佔滿了電腦正常運作所需的重要檔案。 最好保持原狀，讓它們可以執行啟動電腦和協助從問題中復原等工作。

## <a name="see-also"></a>請參閱

- [管理磁碟](manage-disks.md)
- [管理基本磁碟區](manage-basic-volumes.md)
- [針對磁碟管理問題進行疑難排解](troubleshooting-disk-management.md)
- [Windows 10 中的復原選項](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [尋找升級到 Windows 10 之後遺失的檔案](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [備份與還原您的檔案](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [建立修復磁碟機](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [建立系統還原點](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [尋找我的 BitLocker 修復金鑰](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
