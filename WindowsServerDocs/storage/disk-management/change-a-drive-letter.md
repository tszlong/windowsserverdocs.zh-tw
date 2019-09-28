---
title: 變更磁碟機代號
description: 如何使用 [磁碟管理] 在 Windows 中變更或指派磁碟機代號。
ms.date: 10/24/2018
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 3e18092a71e12cadb86052204738fafc8a149ff4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386016"
---
# <a name="change-a-drive-letter"></a>變更磁碟機代號

> **適用於：** Windows 10、Windows 8.1、Windows 7、Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您不喜歡指派給磁碟機的磁碟機代號，或具備目前沒有磁碟機代號的磁碟機，您可以使用 [磁碟管理] 進行變更。

> [!IMPORTANT]
> 如果您變更 Windows 或應用程式安裝所在磁碟機的磁碟機代號，應用程式可能無法執行或找不到該磁碟機。 因此，建議您不要變更 Windows 或應用程式安裝所在磁碟機的磁碟機代號。

以下說明如何變更磁碟機代號 (若要改為將磁碟機掛接到空的資料夾，以便顯示為另一個資料夾，請參閱 [Assign a mount point folder path to a drive](assign-a-mount-point-folder-path-to-a-drive.md) (將掛接點資料夾路徑指派給磁碟機))。

1. 使用系統管理員權限開啟 [磁碟管理]。 
    若要執行這項操作，請在工作列的搜尋方塊中鍵入**磁碟管理**，選取並按住 (或以滑鼠右鍵按一下) [磁碟管理]  ，然後選取 [以系統管理員身分執行]   > [是]  。 如果無法以系統管理員身分將它開啟，請改鍵入**電腦管理**，然後移至 [儲存體]   > [磁碟管理]  。
1. 在 [磁碟管理] 中，以滑鼠右鍵按一下您想要變更或新增磁碟機代號的磁碟機，然後選取 [變更磁碟機代號及路徑]  。

    ![顯示磁碟機的 [磁碟管理]](media/change-drive-letter.png)
    > [!TIP]
    > 如果您沒有看到 [變更磁碟機代號及路徑]  選項或該選項呈現灰色，可能是磁碟區尚未準備好接受磁碟機代號 (如果磁碟機尚未配置並需要[初始化](initialize-new-disks.md)，則可能會發生此情況)。 或者，它可能不適合進行存取，EFI 系統磁碟分割和修復磁碟分割即為此例。 如果您確認所擁有格式化磁碟區具有可存取的磁碟機代號但仍然無法變更它，很遺憾，本主題可能無法協助您；建議您[連絡 Microsoft](https://support.microsoft.com/contactus/) 或您電腦的製造商以取得進一步協助。

1. 若要變更磁碟機代號，請選取 [變更]  。 若要新增磁碟機目前沒有的磁碟機代號，請選取 [新增]  。

    ![[變更磁碟機代號及路徑] 對話方塊](media/change-drive-letter2.png)
1. 依序選取新的磁碟機代號和 [確定]  ，然後在出現提示指出依賴磁碟機代號的程式可能無法正確執行時，選取 [是]  。

    ![顯示磁碟機代號即將變更的 [新增磁碟機代號或路徑] 對話方塊](media/change-drive-letter3.png)