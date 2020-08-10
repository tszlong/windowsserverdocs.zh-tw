---
title: 變更磁碟機代號
description: 如何使用 [磁碟管理] 在 Windows 中變更或指派磁碟機代號。
ms.date: 06/08/2020
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52665f239bec56ad81d0300ed3312ec57b32cb1f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936069"
---
# <a name="change-a-drive-letter"></a>變更磁碟機代號

> **適用於：** Windows 10、Windows 8.1、Windows 7、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您不喜歡指派給磁碟機的磁碟機代號，或具備目前沒有磁碟機代號的磁碟機，您可以使用 [磁碟管理] 進行變更。 若要改為將磁碟機掛接到空的資料夾，以便顯示為另一個資料夾，請參閱[將磁碟機掛接到資料夾](assign-a-mount-point-folder-path-to-a-drive.md)。

> [!IMPORTANT]
> 如果您變更 Windows 或應用程式安裝所在磁碟機的磁碟機代號，應用程式可能無法執行或找不到該磁碟機。 因此，建議您不要變更 Windows 或應用程式安裝所在磁碟機的磁碟機代號。

變更磁碟機代號的方式如下：

1. 使用系統管理員權限開啟 [磁碟管理]。
    若要這麼做，請選取並按住 (或以滑鼠右鍵按一下) [開始] 按鈕，然後選取 [磁碟管理]。
1. 在 [磁碟管理] 中，選取並按住 (或以滑鼠右鍵按一下) 您想要變更或新增磁碟機代號的磁碟區，然後選取 [變更磁碟機代號及路徑]。

    ![顯示磁碟機的 [磁碟管理]](media/change-drive-letter.png)
    > [!TIP]
    > 如果您沒有看到 [變更磁碟機代號及路徑] 選項或該選項呈現灰色，可能是磁碟區尚未準備好接受磁碟機代號 (如果磁碟機尚未配置並需要[初始化](initialize-new-disks.md)，則可能會發生此情況)。 或者，它可能不適合進行存取，EFI 系統磁碟分割和修復磁碟分割即為此例。 如果您確認所擁有格式化磁碟區具有可存取的磁碟機代號但仍然無法變更它，很遺憾，本主題可能無法協助您；建議您[連絡 Microsoft](https://support.microsoft.com/contactus/) 或您電腦的製造商以取得進一步協助。

1. 若要變更磁碟機代號，請選取 [變更]。 若要新增磁碟機目前沒有的磁碟機代號，請選取 [新增]。

    ![[變更磁碟機代號及路徑] 對話方塊](media/change-drive-letter2.png)
1. 依序選取新的磁碟機代號和 [確定]，然後在出現提示指出依賴磁碟機代號的程式可能無法正確執行時，選取 [是]。

    ![顯示磁碟機代號即將變更的 [新增磁碟機代號或路徑] 對話方塊](media/change-drive-letter3.png)
