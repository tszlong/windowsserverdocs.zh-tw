---
title: AD 樹系復原-執行完整伺服器復原
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: bf321ae769aa6f0da1cebce7700ea429161a0956
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824011"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>AD 樹系復原-執行完整伺服器復原 

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用下列程式執行 Windows Server 2016、2012 R2 或2012的完整伺服器復原。 

## <a name="active-directory-full-server-recovery"></a>Active Directory 完整伺服器復原

如果您要還原到不同的硬體或不同的作業系統實例，則需要完整伺服器復原。 請記住下列幾點：

- 目標伺服器上的磁片磁碟機數目必須等於備份中的數位，而且必須是相同的大小或更大。
- 必須從作業系統 DVD 開機目標伺服器，才能存取 [**修復您的電腦**] 選項。 
- 如果目標 DC 是在 Hyper-v 上的 VM 中執行，且備份存放在網路位置上，您就必須安裝傳統網路介面卡。 
- 執行完整伺服器復原之後，您必須個別執行 SYSVOL 的授權還原，如[AD 樹系復原-執行 DFSR 複寫的 sysvol 的授權同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)處理中所述。

視您的案例而定，使用下列其中一個程式來執行完整還原。 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>使用具有最新映射的本機備份執行完整伺服器還原
  
1. 啟動 Windows 安裝程式，指定語言、時間和貨幣格式，以及鍵盤選項，然後按 **[下一步]** 。 
2. 按一下 **\[修復您的電腦\]** 。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. 按一下 **\[疑難排解\]** 。</br>
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. 按一下 [**系統映射**復原]。</br>
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. 按一下 [ **Windows Server 2016**]。 
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. 如果您要還原最近的本機備份，請按一下 **[使用最新可用的系統映射（建議選項）** ]，然後按 **[下一步]** 。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. 現在您將可選擇：
   -  格式化和重新分割磁片
   -  安裝驅動程式
   -  取消選取 [自動重新開機] 和 [檢查磁片錯誤] 的 [ **Advanced** ] 功能。 預設會啟用這些功能。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. 按 [下一步]。
9. 按一下 **[完成]** 。 系統會提示您是否確定要繼續。 按一下 [是]。 
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. 一旦此動作完成，就會如[AD 樹系復原-執行 DFSR 複寫 SYSVOL 的授權同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)處理中所述，執行 SYSVOL 的授權還原。

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>以本機或遠端的任何映射執行完整伺服器還原

1. 啟動 Windows 安裝程式，指定語言、時間和貨幣格式，以及鍵盤選項，然後按 **[下一步]** 。 
2. 按一下 **\[修復您的電腦\]** 。</br>
3. 按一下 [**疑難排解**]，按一下 [**系統映射**復原]，然後按一下 [ **Windows Server 2016**]。 
4. 如果您要還原最近的本機備份，請按一下 [**選取系統映射**]，然後按 **[下一步]** 。
5. 現在您可以選取您想要還原的備份位置。 如果是本機映射，您可以從清單中選取它。 
6. 如果映射位於網路共用上，請選取 [ **Advanced**]。 如果您需要安裝驅動程式，您也可以選取 [ **Advanced** ]。
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. 如果您在按一下 [ **Advanced** ] 之後從網路還原，請**搜尋網路上的系統映射**。 系統可能會提示您還原網路連線。 選取 [確定]。 </br>
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. 輸入備份共用位置的 UNC 路徑（例如，\\\server1\backups），然後按一下 **[確定]** 。 您也可以輸入目標伺服器的 IP 位址，例如 \\\192.168.1.3\backups。 
   ![伺服器還原](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
9. 輸入存取共用所需的認證，然後按一下 [確定]。 
10. 現在，**選取要還原的系統映射的日期和時間**，然後按 **[下一步]** 。
11. 現在您將可選擇：
    - 格式化和重新分割磁片
    - 安裝驅動程式
    - 取消選取 [自動重新開機] 和 [檢查磁片錯誤] 的 [ **Advanced** ] 功能。 預設會啟用這些功能。
12. 按 [下一步]。
13. 按一下 **[完成]** 。 系統會提示您是否確定要繼續。 按一下 [是]。  
14. 一旦此動作完成，就會如[AD 樹系復原-執行 DFSR 複寫 SYSVOL 的授權同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)處理中所述，執行 SYSVOL 的授權還原。

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>啟用網路備份的網路介面卡

如果您需要從命令提示字元啟用網路介面卡，以從網路共用還原，請使用下列步驟。

1. 啟動 Windows 安裝程式，指定語言、時間和貨幣格式，以及鍵盤選項，然後按 **[下一步]** 。 
2. 按一下 **\[修復您的電腦\]** 。 I
3. 按一下 [**疑難排解**]，然後按一下 [**命令提示**字元]。 
4. 輸入下列命令，然後按下 ENTER：  

   ```  
   wpeinit  
   ```

5. 若要確認網路介面卡的名稱，請輸入：  

   ```  
   show interfaces  
   ```  

   輸入下列命令，然後在每個命令之後按 ENTER 鍵：  

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

   例如，  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   輸入 `quit` 以返回命令提示字元。 輸入 `ipconfig /all` 以確認網路介面卡具有 IP 位址，並嘗試 ping 裝載備份共用之伺服器的 IP 位址，以確認連線能力。 當您完成時，請關閉命令提示字元。 

6. 現在網路介面卡已正常運作，請選取上述步驟以完成還原。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
