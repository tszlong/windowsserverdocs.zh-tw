---
title: 建立可開機的 USB 快閃磁碟機
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 05/04/2018
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2716ffb7ce8f74d7c729565064de91e0598d0753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884679"
---
# <a name="create-a-bootable-usb-flash-drive"></a>建立可開機的 USB 快閃磁碟機

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以建立可開機 USB 快閃磁碟機用來部署 Windows Server Essentials。 第一個步驟是使用 DiskPart 這項命令列公用程式來準備 USB 快閃磁碟機。 如需 DiskPart 的相關資訊，請參閱 [DiskPart 命令列選項](https://go.microsoft.com/fwlink/?LinkId=207073)。  


> [!TIP]
> 若要建立可開機 USB 快閃磁碟機用於復原或重新安裝 Windows，而不是伺服器的電腦上，請參閱[建立復原磁碟機](https://support.microsoft.com/help/4026852/windows-create-a-recovery-drive)。
  
 如需建立或使用可開機的 USB 快閃磁碟機之其他案例，請參閱下列主題：  
  
-   [從現有的用戶端電腦備份還原完整的系統](../manage/restore-a-full-system-from-an-existing-client-computer-backup.md)  
  
-   [還原或修復執行 Windows Server Essentials 的伺服器](../manage/restore-or-repair-your-server-running-windows-server-essentials.md)  

  
### <a name="to-create-a-bootable-usb-flash-drive"></a>建立可開機的 USB 快閃磁碟機  
  
1.  將 USB 快閃磁碟機插入執行的電腦中。  
  
2.  開啟命令提示字元視窗，以系統管理員身分執行。  
  
3.  輸入 `diskpart`。  
  
4.  在開啟的新命令列視窗中，若要判斷 USB 快閃磁碟機編號或磁碟機代號，請在命令提示字元中輸入 `list disk`，然後按 ENTER。 `list disk` 命令會顯示電腦上的所有磁碟。 記下 USB 快閃磁碟機的磁碟機編號或磁碟機代號。  
  
5.  在命令提示字元中輸入 `select disk <X>`，其中 X 是 USB 快閃磁碟機的磁碟機編號或磁碟機代號，然後按 ENTER。  
  
6.  輸入 `clean`，然後按 ENTER。 此命令會刪除 USB 快閃磁碟機的所有資料。  
  
7.  若要在 USB 快閃磁碟機上建立新的主要磁碟分割，輸入 `create part pri`，然後按 ENTER。  
  
8.  若要選取您剛才建立的磁碟分割，輸入 `select part 1`，然後按 ENTER。  
  
9. 若要將磁碟分割格式化，輸入 `format fs=ntfs quick`，然後按 ENTER。  
  
    > [!IMPORTANT]
    >  如果您的伺服器平台支援「整合可延伸韌體介面 (UEFI)」，則您應將 USB 快閃磁碟機格式化成 FAT32，而非 NTFS。 若要將磁碟分割格式化成 FAT32，輸入 `format fs=fat32 quick`，然後按 ENTER。  
  
10. 輸入 `active`，然後按 ENTER。  
  
11. 輸入 `exit`，然後按 ENTER。  
  
12. 完成自訂映像的準備後，請將它儲存至 USB 快閃磁碟機的根目錄。  
  
## <a name="see-also"></a>另請參閱  

 [與 Windows Server Essentials ADK 快速入門](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)   

 [與 Windows Server Essentials ADK 快速入門](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)   

 [我們該如何協助您？](https://windows.microsoft.com/windows/support)
