---
title: 變更磁碟機代號
description: 如何變更或指派磁碟機代號在 Windows 中的使用 磁碟管理。
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 93da32a204c42b3f4fb503349ff732c9c050db31
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192635"
---
# <a name="change-a-drive-letter"></a>變更磁碟機代號

> **適用於：** Windows 10，Windows 8.1、 Windows 7、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

如果您不喜歡磁碟機代號指派給磁碟機，或如果您有尚無磁碟機代號的磁碟機，您可以使用磁碟管理來變更它。

> [!IMPORTANT]
> 如果您變更 Windows 或應用程式的安裝位置的磁碟機的磁碟機代號，應用程式可能無法執行，或尋找該磁碟機。 基於這個理由，我們建議您不要變更 Windows 或應用程式安裝所在的磁碟機的磁碟機代號。

以下是如何變更磁碟機代號 (若要改為掛接在空白磁碟機資料夾，讓它顯示為另一個資料夾，請參閱[指派磁碟機的掛接點資料夾路徑](assign-a-mount-point-folder-path-to-a-drive.md))。

1. 您可以開啟 磁碟管理系統管理員權限。 
    若要這樣做，請在工作列上的 [搜尋] 方塊中輸入**磁碟管理**、 選取並保存 （或以滑鼠右鍵按一下）**磁碟管理**，然後選取**系統管理員身分執行** > **是**。 如果您無法以系統管理員身分開啟它，輸入**電腦管理**相反的然後移至**儲存體** > **磁碟管理**。
1. 在 磁碟管理，以滑鼠右鍵按一下您要變更或新增磁碟機代號，然後選取磁碟的機**變更磁碟機代號及路徑**。

    ![顯示磁碟機的磁碟管理](media/change-drive-letter.png)
    > [!TIP]
    > 如果您沒有看到**變更磁碟機代號及路徑**選項，或它會呈現灰色，可能在磁碟區尚未準備好接收磁碟機代號，這可能是因為，如果是未配置的磁碟機，並需要[初始化](initialize-new-disks.md). 或者，也許不目的在於存取，這是 EFI 系統磁碟分割和修復磁碟分割的情況。 如果您已確認您有格式化的磁碟區，您可以存取的磁碟機代號，您仍然無法變更它，不幸的是本主題可能無法協助您，因此建議[連絡 Microsoft](https://support.microsoft.com/contactus/)或製造商，您如需詳細說明的電腦。

1. 若要變更的磁碟機代號，請選取**變更**。 若要新增的磁碟機代號，如果磁碟機還沒有，選取**新增**。

    ![[變更磁碟機代號及路徑] 對話方塊](media/change-drive-letter2.png)
1. 選取新的磁碟機代號，請選取 **[確定]** ，然後選取**是**當系統提示您關於如何依賴的磁碟機代號的程式可能無法正確執行。

    ![顯示 變更磁碟機代號的 變更磁碟機代號或路徑 對話方塊](media/change-drive-letter3.png)