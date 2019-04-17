---
title: "廣告樹系修復-備份完整伺服器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adfs
ms.openlocfilehash: a86d61536f8b426e1a5258c661d4e53da63d4162
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>廣告樹系修復-備份資料  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
使用下列程序使用 Windows Server 備份或 wbadmin.exe DC 執行系統狀態備份。  
  
## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>執行系統狀態備份使用 Windows Server 備份  
1. 開放**伺服器管理員**，按一下 [**工具**，然後按一下 [ **Windows Server 備份**。
    - 在 Windows Server 2008 R2 和 Windows Server 2008，按一下**[開始]**，指向 [**系統管理工具**，然後按一下 [ **Windows Server 備份**。 
![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. 如果您的提示，請在**使用者 Account 控制項**對話方塊中，提供者備份認證，然後再按一下**[確定]**。
3. 按一下**本機備份**。
4. 在**動作**功能表上，按**備份一次**。
5. 備份一次精靈中，在**備份選項**頁面上，按一下 [**的其他不同選項**，然後按一下 [**下一步**。
![安裝備份](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. 在**選擇備份設定**頁面上，按一下 [**自訂)**，然後按一下 [**下一步**。
7. 在**的備份選取的項目**畫面中，按一下 [**新增項目**，然後選取**系統狀態**然後按一下**[確定]**。
    - Windows Server 2008 R2 和 Windows Server 2008、選取的備份包含磁碟區。 如果您選取 [**讓系統修復**核取方塊，選取所有重要的磁碟區。 
![安裝備份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  
8. 在**指定目的地類型**頁面上，按一下 [**本機磁碟機**或**遠端共用資料夾**，然後按一下 [**下一步**。  如果您的備份到遠端的共用資料夾，請執行下列動作：  
  
 1.  輸入的共用資料夾的路徑。  
 2.  在**存取控制**、**不要繼承**或**繼承**以判斷存取備份，然後按一下**下一步**。  
 3.  在**提供使用者的認證備份的**對話方塊中，提供位使用者的共用資料夾，具有寫入權限的使用者名稱和密碼，然後按一下 [ **[確定]**。
9. 適用於 Windows Server 2008 R2 和 Windows Server 2008、在**[進階選項指定**頁面上，選取**VSS 複製備份**，然後按一下 [**下一步**。
10. 在**選擇備份目的地**頁面上，選擇備份的位置。  如果您選取 [本機磁碟機選擇到本機磁碟機，或如果您選取 [遠端分享選擇網路共用。
11. 在確認畫面上，按一下 [**備份**。
12. 完成此按一下**關閉**。
13. 關閉 Windows Server 備份。

  
## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>若要備份系統狀態使用 Wbadmin.exe  
  
1.  打開提升權限的命令提示字元中，輸入下列命令並按下 ENTER:  
  
    ```  
    wbadmin start systemstatebackup -backuptarget:<targetDrive>:
    ```  
![安裝備份](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
