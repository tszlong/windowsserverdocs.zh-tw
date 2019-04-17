---
title: "廣告樹系修復-執行完整伺服器復原"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adfs
ms.openlocfilehash: 5de71cc005e9ec4c1425f9fa805b3c043ab4ea36
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>廣告樹系修復-執行完整伺服器復原 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
使用下列程序來執行完整伺服器修復的 Windows Server 2016 R2、 2012年或 2012年。 

## <a name="active-directory-full-server-recovery"></a>Active Directory 完整伺服器復原
完整伺服器復原時，需要您要還原至不同的硬體或其他作業系統的執行個體。 請牢記下列動作：

- 目標伺服器必須為等於備份的數字的數字磁碟機，或更高需要相同。
- 目標伺服器必須自以存取作業系統 DVD**修復您的電腦**選項。 
- 如果的目標俠 HYPER-V 備份執行 VM 中儲存在網路位置，您必須安裝舊版的網路介面卡。  
- 您在執行完整伺服器修復之後，您需要另行購買執行授權 SYSVOL 中, 所述[AD 樹系修復-執行 DFSR 複寫 SYSVOL 授權同步處理](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。


根據您案例中，使用下列程序來執行完整還原。  
  
### <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>執行完整伺服器本機的最新的映像備份與還原
  
1.  [開始] 的 Windows 安裝程式，指定的語言、 時間及貨幣格式與鍵盤選項，然後按一下**下一步**。  
2.  按一下**修復您的電腦**。</br>
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3.  按一下**疑難排解**。</br>
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4.  按一下**系統映像修復**。</br>
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5.  按一下**Windows Server 2016**。  
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6.  如果您的最新的本機備份還原，按一下 [**使用最新可用系統映像 （建議選項）** ，按一下 [**下**。
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7.  您現在將會提供的選項：
    -  格式並重新分割磁碟
    -  安裝驅動程式
    -  取消選取**進階**功能自動重新，並檢查是否為磁碟的錯誤。  這是預設支援。
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. 按一下**下一步**。
9. 按一下**完成**。  您將會詢問您是否確定您想要繼續提示。  按一下**[是]**。  
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. 一旦完成後執行授權 SYSVOL 中, 所述[AD 樹系修復-執行 DFSR 複寫 SYSVOL 授權同步處理](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。
 

### <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>執行任何影像本機或遠端伺服器完整還原
1.  [開始] 的 Windows 安裝程式，指定的語言、 時間及貨幣格式與鍵盤選項，然後按一下**下一步**。  
2.  按一下**修復您的電腦**。</br>
3.  按一下**疑難排解**，按一下 [**系統映像修復**，並按**Windows Server 2016**。  
4.  如果您的最新的本機備份還原，按一下 [**選擇系統映像**，按一下 [**下**。

5.  現在您可以選擇您想要還原備份的位置。  如果影像本機您可以從清單中選取它。  
6.  如果網路共用上影像，請選取**進階**。  您也可以選取**進階**如果您要安裝的驅動程式。
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7.  如果您從網路還原之後按**進階**選取**搜尋網路上的系統映像的**。  系統可能會提示您還原網路連接。  選取 [確定]。 </br>
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. 輸入底色備份分享位置 (例如，\\\server1\backups)，然後按一下**[確定]**。  您也可以輸入目標伺服器，例如 \\\192.168.1.3\backups 的 IP 位址。  
![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. 輸入認證需要存取分享，然後按一下 [確定]。  
11. 現在**選取要還原的日期和時間系統映像的**，按一下 [**下**。
12. 您現在將會提供的選項：
    1.   格式並重新分割磁碟
    2.   安裝驅動程式
    3.   取消選取**進階**功能自動重新，並檢查是否為磁碟的錯誤。  這是預設支援。
13. 按一下**下一步**。
14. 按一下**完成**。  您將會詢問您是否確定您想要繼續提示。  按一下**[是]**。   
15. 一旦完成後執行授權 SYSVOL 中, 所述[AD 樹系修復-執行 DFSR 複寫 SYSVOL 授權同步處理](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)。


### <a name="enabling-the-network-adapter-for-a-network-backup"></a>讓網路備份的網路介面卡
如果您需要可以從命令提示字元中從網路共用還原的網路介面卡，請使用下列步驟。

1.  [開始] 的 Windows 安裝程式，指定的語言、 時間及貨幣格式與鍵盤選項，然後按一下**下一步**。  
2.  按一下**修復您的電腦**。 我
3.  按一下**疑難排解**，按一下 [**命令提示字元**。  
4.  輸入下列命令並按一下 enter:  
  
    ```  
    wpeinit  
    ```   
5.  若要確認網路介面卡的名稱，請輸入：  
  
    ```  
    show interfaces  
    ```  
  
     輸入下列命令，並在每個命令之後按下 ENTER:  
  
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
  
     例如：  
  
    ```  
    set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
    ```  
  
     輸入`quit`以返回 [命令提示字元。 輸入`ipconfig /all`確認網路介面卡有 IP 位址，請嘗試 ping 裝載確認連接備份分享的伺服器的 IP 位址。 當您完成時，請關閉命令提示字元。  
  
6.  現在的網路介面卡正在運作，請選取 [上述步驟以完成還原。

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
