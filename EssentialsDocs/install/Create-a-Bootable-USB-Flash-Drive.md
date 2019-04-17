---
title: "建立可開機的 USB 快閃磁碟機"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fe8e35c-69f9-40b3-a270-22e2402510d8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9d587329e1141040b2511e1574649f1844dcec90
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-bootable-usb-flash-drive"></a>建立可開機的 USB 快閃磁碟機

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以建立可開機 USB 快閃磁碟機用來部署 Windows Server Essentials。 使用 DiskPart，這一個命令列工具準備 USB 快閃磁碟機是第一個步驟。 如 DiskPart 相關資訊，請查看[DiskPart 命令列選項](https://go.microsoft.com/fwlink/?LinkId=207073)。  
  
 針對其他案例，您可以建立，或使用可開機的 USB 快閃磁碟機，請查看下列主題：  
  
-   [從現有的 client 電腦備份還原完整的系統](https://technet.microsoft.com/library/jj713539.aspx#BKMK_CreateBootable)  
  
-   [還原或修復您執行的 Windows Server Essentials 的伺服器](https://technet.microsoft.com/library/jj593197.aspx#BKMK_Restore_2)  
  
### <a name="to-create-a-bootable-usb-flash-drive"></a>若要建立可開機的 USB 快閃磁碟機  
  
1.  將 USB 快閃磁碟機插入電腦正在執行。  
  
2.  以系統管理員身分開放命令提示字元視窗。  
  
3.  輸入`diskpart`。  
  
4.  在新命令列開啟視窗中，若要判斷 USB flash 磁碟機數字或磁碟機代號，在命令提示字元中，輸入`list disk`，然後按 ENTER。 `list disk`命令顯示電腦上所有的磁碟。 請注意的磁碟機號碼或 USB 快閃磁碟機的磁碟機代號。  
  
5.  在命令提示字元中，輸入`select disk <X>`、位置 X 是磁碟機數字或的 USB 磁碟機代號快閃磁碟機，然後按 ENTER。  
  
6.  輸入`clean`，並按一下 ENTER。 這個命令的 USB 快閃磁碟機刪除所有資料。  
  
7.  USB 快閃磁碟機上建立新的主要磁碟分割，輸入`create part pri`，然後按 ENTER。  
  
8.  若要選取您剛建立的磁碟分割，輸入`select part 1`，然後按 ENTER。  
  
9. 若要格式化的磁碟分割，輸入`format fs=ntfs quick`，然後按 ENTER。  
  
    > [!IMPORTANT]
    >  如果您 server 平台支援整合可延伸韌體介面 (UEFI)，您應該 FAT32 而非 NTFS 格式化 USB 快閃磁碟機。 若要將磁碟分割格式化為 FAT32，輸入`format fs=fat32 quick`，然後按 ENTER。  
  
10. 輸入`active`，然後按 ENTER。  
  
11. 輸入`exit`，然後按 ENTER。  
  
12. 當您完成準備自訂映像時，將它儲存到 USB 快閃磁碟機根。  
  
## <a name="see-also"></a>也了  

 [開始使用 Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)   

 [開始使用 Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)   

 [我們該如何協助您？](https://windows.microsoft.com/windows/support)
