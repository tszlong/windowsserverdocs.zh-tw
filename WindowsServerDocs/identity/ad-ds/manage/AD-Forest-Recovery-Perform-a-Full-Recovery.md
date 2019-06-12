---
title: AD 樹系復原-執行完整伺服器復原
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 9cf89c9f4875f602abea89e366cadfba8d0599c3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443008"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>AD 樹系復原-執行完整伺服器復原 

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用下列程序來執行適用於 Windows Server 2016、 2012 R2 或 2012年的完整伺服器復原。 

## <a name="active-directory-full-server-recovery"></a>Active Directory 完整伺服器復原

完整伺服器復原時需要您要還原至不同的硬體或不同的作業系統執行個體。 請記住下列幾點：

- 目標伺服器必須在備份中的數字相等的磁碟機數目和大小，或更新版本必須相同。
- 目標伺服器必須可從作業系統 DVD 啟動，才能存取**修復您的電腦**選項。 
- 如果 DC VM 中執行 HYPER-V 以及備份的目標儲存在網路位置，您必須安裝傳統網路介面卡。 
- 執行完整伺服器復原之後，您需要個別執行系統授權還原的 SYSVOL 中, 所述[AD 樹系復原-執行權威的同步處理的 DFSR 複寫的 SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

根據您的案例中，使用下列程序的其中一個來執行完整還原。 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>執行完整伺服器還原最新的映像的本機備份
  
1. 啟動 Windows 安裝程式中指定的語言、 時間和貨幣格式和鍵盤選項，按一下**下一步**。 
2. 按一下 **\[修復您的電腦\]** 。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. 按一下 **\[疑難排解\]** 。</br>
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. 按一下 **系統映像修復**。</br>
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. 按一下  **Windows Server 2016**。 
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. 如果您要還原的最新的本機備份，請按一下**使用最新可用的系統映像 （建議選項）** 然後按一下**下一步**。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. 您現在會提供選項：
   -  格式化並重新分割磁碟
   -  安裝驅動程式
   -  取消選取**進階**功能的自動重新啟動，並檢查磁碟錯誤。 根據預設，會啟用這些。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. 按一下 [下一步]  。
9. 按一下 **[完成]** 。 系統會詢問您是否確定要繼續提示。 按一下 [ **是**]。 
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. 完成之後執行系統授權還原的 SYSVOL，如中所述[AD 樹系復原-執行權威的同步處理的 DFSR 複寫的 SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>執行完整伺服器還原與任何本機或遠端的映像

1. 啟動 Windows 安裝程式中指定的語言、 時間和貨幣格式和鍵盤選項，按一下**下一步**。 
2. 按一下 **\[修復您的電腦\]** 。</br>
3. 按一下 **疑難排解**，按一下**系統映像修復**，然後按一下**Windows Server 2016**。 
4. 如果您要還原的最新的本機備份，請按一下**選取系統映像**然後按一下**下一步**。
5. 現在您可以選取您想要還原之備份的位置。 如果本機映像您可以從清單中選取它。 
6. 如果映像位於網路共用上，選取**進階**。 您也可以選取**進階**如果您要安裝驅動程式。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. 如果您按一下後還原從網路**進階**選取**搜尋網路上的系統映像**。 系統可能會提示您還原網路連線。 選取 [確定]。 </br>
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. 輸入備份的共用位置的 UNC 路徑 (例如\\\server1\backups)，按一下  **確定**。 您也可以如輸入目標伺服器的 IP 位址\\\192.168.1.3\backups。 
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
9. 輸入認證才能存取共用，並按一下 [確定]。 
10. 現在**選取要還原的日期和時間的系統映像**然後按一下**下一步**。
11. 您現在會提供選項：
    - 格式化並重新分割磁碟
    - 安裝驅動程式
    - 取消選取**進階**功能的自動重新啟動，並檢查磁碟錯誤。 根據預設，會啟用這些。
12. 按一下 [下一步]  。
13. 按一下 **[完成]** 。 系統會詢問您是否確定要繼續提示。 按一下 [ **是**]。  
14. 完成之後執行系統授權還原的 SYSVOL，如中所述[AD 樹系復原-執行權威的同步處理的 DFSR 複寫的 SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>啟用網路備份的網路介面卡

如果您要啟用從命令提示字元來還原從網路共用的網路介面卡，請使用下列步驟。

1. 啟動 Windows 安裝程式中指定的語言、 時間和貨幣格式和鍵盤選項，按一下**下一步**。 
2. 按一下 **\[修復您的電腦\]** 。 I
3. 按一下 **疑難排解**，按一下**命令提示字元**。 
4. 輸入下列命令，然後按 ENTER：  

   ```  
   wpeinit  
   ```

5. 若要確認網路介面卡的名稱，請輸入：  

   ```  
   show interfaces  
   ```  

   輸入下列命令，並在每個命令之後按 ENTER:  

   ```  
   netsh  
   ```  

   ```  
   interface  
   ```  
  
   ```  
   tcp  
   ```  

   ```  
   ipv4  
   ```  
  
   ```  
   set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
   ```  

   例如:  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   型別`quit`返回命令提示字元。 型別`ipconfig /all`來確認網路介面卡具有 IP 位址及嘗試 ping 裝載要確認連線能力的備份共用的伺服器的 IP 位址。 當您完成時，請關閉命令提示字元。 

6. 現在，正在使用的網路介面卡，請選取上述步驟，以完成還原。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
