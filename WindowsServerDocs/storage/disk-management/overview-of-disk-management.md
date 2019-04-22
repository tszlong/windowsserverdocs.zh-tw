---
title: 磁碟管理概觀
description: 系統公用程式中，可讓您執行進階儲存體的工作，例如初始化新的磁碟機、 擴充磁碟區、 壓縮的資料分割，以及變更磁碟機代號的 Windows 磁碟管理。
ms.date: 4/2/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2a467c64a3e0ff38b5165b9e001fc2deb2d92148
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819279"
---
# <a name="overview-of-disk-management"></a>磁碟管理概觀

> **適用於：** Windows 10，Windows 8.1、 Windows 7、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

系統中的公用程式可讓您執行的進階儲存體工作的 Windows 磁碟管理。 以下是一些磁碟管理 是適合的項目：

- 若要設定新的磁碟機，請參閱[初始化新的磁碟機](initialize-new-disks.md)。
- 若要延伸磁碟區不會已經屬於相同的磁碟機上的磁碟區的空間，請參閱[延伸基本磁碟區](extend-a-basic-volume.md)。
- 若要壓縮磁碟分割，通常是讓您可以延伸相鄰的資料分割，請參閱[壓縮基本磁碟區](shrink-a-basic-volume.md)。
- 若要變更磁碟機代號或指派新的磁碟機代號，請參閱[變更磁碟機代號](change-a-drive-letter.md)。

![顯示具有三個資料分割-499 MB 的系統磁碟分割、 較大的 C 磁碟機的 Windows 和另一個 499 MB 的磁碟分割，復原一般磁碟機的磁碟管理](media/disk-management.png)

> [!TIP]
>  如果您收到錯誤，或遵循這些程序時，項目無法運作，請參閱[疑難排解磁碟管理](troubleshooting-disk-management.md)主題。 如果那沒有幫助-不必驚慌 ！ 還有一大堆資訊儲存在[Microsoft 社群](https://answers.microsoft.com/en-us/windows)站台-請嘗試搜尋[檔案、 資料夾和儲存體](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639)區段，然後如果您仍然需要協助時，張貼的問題和 Microsoft 或其他成員社群將會嘗試幫助。 如果您有如何改善這些主題的意見反應，我們很希望聽聽您 ！ 只要回答*是此頁面有幫助？* 提示，然後留下任何註解，那里或公用的註解執行緒，本主題的底部。

以下是一些常見的工作，您可能想要執行，但在 Windows 中使用其他工具：

- 若要釋出磁碟空間，請參閱[釋出 Windows 10 中的磁碟機空間](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)。
- 若要重組磁碟機，請參閱[重組您的 Windows 10 電腦](https://support.microsoft.com/help/4026701/windows-defragment-your-windows-10-pc)。
- 需要多個硬碟，並將它們放在一起，類似於 RAID，請參閱[儲存空間](https://support.microsoft.com/help/12438/windows-10-storage-spaces)。

## <a name="about-those-extra-recovery-partitions"></a>有關這些額外的修復磁碟分割

如果您想知道 （我們已讀取您的意見 ！），Windows 通常會包含三個主要磁碟機 (通常是 C:\ 上的磁碟分割磁碟機）：

![磁碟 0 顯示三個資料分割-EFI 系統磁碟分割、 Windows 磁碟分割，以及修復磁碟分割](media/windows-partitions.png)

- **EFI 系統磁碟分割**-這使用新型電腦啟動 （開機） 您的電腦與您的作業系統。
- **Windows 作業系統磁碟機 （c:）** -這是 Windows 的安裝位置，通常您在其中放置您的應用程式及檔案的其餘部分。
- **修復磁碟分割**-這是特殊的工具可協助您復原 Windows，以防有無法啟動或執行其他嚴重問題的儲存位置。

雖然磁碟管理可能會顯示和免費的 100 %efi 系統磁碟分割和修復磁碟分割，它會把它。 這些資料分割通常是非常重要的檔案，您的電腦需要正常運作相當完整的。 最好的方式是讓它們來執行其工作，啟動您的電腦，並協助您解決的問題。

## <a name="see-also"></a>另請參閱

- [管理磁碟](manage-disks.md)
- [管理基本磁碟區](manage-basic-volumes.md)
- [疑難排解磁碟管理](troubleshooting-disk-management.md)
- [在 Windows 10 中的復原選項](https://support.microsoft.com/help/12415/windows-10-recovery-options)
- [尋找遺失的檔案到 Windows 10 更新之後](https://support.microsoft.com/help/12386/windows-10-find-lost-files-after-update)
- [備份和還原您的檔案](https://support.microsoft.com/help/17143/windows-10-back-up-your-files)
- [建立修復磁碟機](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)
- [建立系統還原點](https://support.microsoft.com/help/4027538/windows-create-a-system-restore-point)
- [尋找我的 BitLocker 修復金鑰](https://support.microsoft.com/help/4026181/windows-find-my-bitlocker-recovery-key)
