---
title: "疑難排解電腦備份及還原 Windows Server Essentials 錯誤"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37e79661442ba9f66a564b6c6c8fb57db1978454
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>疑難排解電腦備份及還原 Windows Server Essentials 錯誤

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

使用下列程序進行疑難排解的 Windows Server Essentials，包括備份設定問題、完整或失敗備份、備份健康警示，以及問題的檔案、資料夾或全系統還原備份的電腦。  
  
> [!NOTE]
>  Windows Server Essentials 社群中最新的疑難排解資訊，請造訪[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums//winserveressentials/threads)。  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a>備份設定連接電腦的問題進行疑難排解  
 使用下列程序的備份設定的備份您 Windows Server Essentials 的伺服器的電腦問題的疑難排解。  
  
### <a name="errors"></a>錯誤  
  
-   備份設定未成功  
  
-   錯誤的電腦收集資訊  
  
-   從備份錯誤移除電腦  
  
### <a name="resolutions"></a>解析度  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>當您設定連接電腦的備份發生的錯誤的疑難排解  
  
1.  請確定您的電腦與網路透過網路的裝置。  
  
2.  確認電腦已連接到網路裝置也已連接到網路上，電源運作正常運作。  
  
3.  請確定執行 Windows Server Client 備份服務及 Windows Server Client 電腦備份提供者服務，伺服器上。  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>若要開始伺服器上的電腦備份服務  
  
    1.  在伺服器上，按一下 [ **[開始]**，按一下 [**系統管理工具]**，然後按一下 [**服務**。  
  
    2.  向下捲動，然後按一下**Windows Server Client 電腦備份提供者服務**。 如果不是服務的狀態**開始**，服務，以滑鼠右鍵按一下，然後按一下 [ **[開始]**。  
  
    3.  按一下**Windows Server Client 電腦備份服務**。 如果不是服務的狀態**開始**，服務，以滑鼠右鍵按一下，然後按一下 [ **[開始]**。  
  
    4.  關閉**服務**。  
  
4.  請確定 client 電腦上正在執行 Windows Server Client 電腦備份提供者服務。  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>若要開始 client 電腦上的電腦備份服務  
  
    1.  Client 在電腦上，按一下 [ **[開始]**，輸入**服務**中**搜尋程式及檔案**文字方塊，然後按下 Enter。  
  
    2.  向下捲動，然後按一下**Windows Server Client 電腦備份提供者服務**。 如果不是服務的狀態**開始**，服務，以滑鼠右鍵按一下，然後按一下 [ **[開始]**。  
  
    3.  關閉**服務**。  
  
5.  請檢查以判斷備份資料庫中是否有錯誤警示。 如果有錯誤，請依照下列修復備份資料庫警示中的指示。  
  
6.  解除安裝 Windows Server Essentials 連接器軟體的電腦，然後再重新安裝它。 如需詳細資訊，請查看主題[解除安裝的連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>疑難排解未正確完成備份  
 備份已失敗的狀態，備份成功的任何部分和任何資料時，可供您要還原。 不過時建立的備份有完整的狀態，, 並非所有的項目中指定備份設定備份，但某些資料可能可供您要還原。  
  
### <a name="errors"></a>錯誤  
  
-   備份不完整  
  
-   備份失敗  
  
### <a name="resolutions"></a>解析度  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>找出未備份成功的磁碟區  
  
1.  打開 Windows Server Essentials 儀表板，，然後按一下**備份的電腦和**。  
  
2.  按一下其中備份是否未成功完成，，然後按一下 [電腦名稱**檢視電腦屬性**在**工作**窗格。  
  
3.  按一下 [，是否未成功完成，然後按一下 [備份] **[檢視詳細資料**。  
  
4.  在**備份詳細資料**對話方塊中，遺漏的核取會顯示成功備份的每個磁碟區的狀態。  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>疑難排解磁碟區失敗備份  
  
1.  確認硬碟，已連接到電腦上，您電腦，運作正常運作。  
  
2.  執行**chkdsk /f /r**若要在硬碟上修正出現任何錯誤 (**/f**)，以及從任何磁復原可讀取的資訊 (**/r**)。 如需有關執行**chkdsk**，查看[CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562)。  
  
3.  請確定電腦已不關閉或中斷備份執行時的網路。  
  
4.  請確定還有備份執行每個磁碟區可用空間。 備份需要一些額外的磁碟空間，建立 VSS 快照 client 電腦上。 在任何不系統保留的磁碟區，必須免費至少 10%的電量的可用磁碟空間。 在系統保留的磁碟區，如果磁碟區大小小於 500 MB VSS 需要 32 MB 的可用空間來建立開發進程的快照。磁碟區是否 500 MB 或更高、VSS 需要 320 MB 的可用空間。  
  
     適用於磁碟區可用空間不足時，請嘗試這些解析度：  
  
    -   延伸磁碟區。 您可以擴充以外的系統保留的磁碟區任何基本或動態磁碟區。  
  
        ###### <a name="to-extend-a-volume"></a>延伸磁碟區  
  
        1.  在 [控制台] 中，按一下**系統及安全性**。  
  
        2.  在**系統管理工具]**，按一下 [**建立和格式化硬碟磁碟分割**。  
  
        3.  以滑鼠右鍵按一下您想要延伸磁碟區。 如果**延伸磁碟區**是支援，請選取該選項。 如果不選項，您無法延伸磁碟區。  
  
        4.  請依照下列延伸磁碟區延伸磁碟區精靈中的步驟操作。  
  
    -   Delete content 從以取得更多空間的磁碟區。  
  
        > [!NOTE]
        >  如果您需要釋放空間系統保留的磁碟區上，您可以將系統復原映像以不同的磁碟區。 適用於的指示，請查看[部署系統復原映像](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx)。  
  
    -   磁碟區排除 client 備份。 只有不是您可以維持一份備份磁碟區上的資料重要執行此動作。  
  
        > [!WARNING]
        >  如果您的系統保留的磁碟區排除 client 備份，client 系統將會備份，以及您將無法在電腦上執行完整的系統還原。  
  
5.  檢查有其他提醒，可能不還有磁碟空間不足，無法用來完成備份的伺服器上的伺服器上。 依照指示警示中的修正此問題。  
  
6.  執行**vssadmin**在 [命令提示字元疑難排解音量陰影複製服務 (VSS) 的問題。 如**vssadmin**，查看[VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332)。  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>備份健康警示問題的疑難排解  
  
### <a name="errors"></a>錯誤  
  
-   Windows Server 方案電腦備份提供者服務已停止運作  
  
-   Windows Server 方案 Client 電腦備份提供者服務停止運作  
  
### <a name="resolutions"></a>解析度  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>疑難排解備份健康警示  
  
1.  如果警示會告訴您備份資料庫有問題，請依照下列警示中的指示來修正這個問題。  
  
2.  如果警示會告訴您備份的服務不執行，請試著開始服務伺服器或 client 電腦您收到錯誤訊息。  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>若要開始伺服器上的備份服務  
  
    1.  在伺服器上，按一下 [ **[開始]**，，按一下 [**系統管理工具]**，然後按一下 [**服務**。  
  
        > [!NOTE]
        >  如果您正在從遠端管理的伺服器，您必須使用遠端桌面連接存取伺服器桌面。 使用遠端桌面連接相關資訊，請查看[連接到其他電腦使用遠端桌面連接](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection)。  
  
    2.  向下捲動，然後按一下**Windows Server Client 電腦備份提供者服務**。 如果不是服務的狀態**開始**，服務，以滑鼠右鍵按一下，然後按一下 [ **[開始]**。  
  
    3.  按一下**Windows Server Client 電腦備份服務**。 如果不是服務的狀態**開始**，服務，以滑鼠右鍵按一下，然後按一下 [ **[開始]**。  
  
    4.  關閉**服務**。  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>若要開始備份服務 client 的電腦上  
  
    1.  Client 在電腦上，按一下 [ **[開始]**，輸入**服務**中**搜尋程式及檔案**文字方塊，然後再按一下輸入。  
  
    2.  以滑鼠右鍵按一下**Windows Server Client 電腦備份提供者服務**，然後按一下 [ **[開始]**。  
  
    3.  關閉**服務**。  
  
3.  檢查事件登 client 電腦或 server 備份服務或驅動程式相關的許多問題。  
  
4.  重新開機，您收到錯誤訊息 client 電腦的伺服器。  
  
5.  檢查健康提醒，可能會造成影響 client 備份上的其他問題。  
  
##  <a name="BKMK_FileAndFolder"></a>疑難排解檔案或資料夾還原  
  
### <a name="errors"></a>錯誤  
  
-   檔案或資料夾還原未成功。  
  
### <a name="resolutions"></a>解析度  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>疑難排解失敗的檔案或資料夾還原  
  
1.  請確定您的電腦與網路透過網路的裝置。  
  
2.  確認電腦已連接到網路裝置也已連接到網路上，電源運作正常運作。  
  
3.  請檢查以判斷備份資料庫中是否有錯誤警示。 如果有錯誤，請依照下列修復備份資料庫警示中的指示。  
  
4.  請嘗試另一個備份還原的檔案或資料夾。  
  
5.  請務必**Windows Server 方案電腦還原驅動程式**安裝及正常運作。  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>若要查看 Windows 的狀態伺服器方案電腦還原驅動程式  
  
    1.  按一下**[開始]**，輸入**[裝置管理員]**中**搜尋程式及檔案**文字方塊，然後再按一下輸入。  
  
    2.  在 [裝置管理員] 中，按一下**系統裝置**，捲動到 [ **Windows Server 方案電腦還原驅動程式**。  
  
    3.  如果未出現 [驅動程式：  
  
        1.  以系統管理員權限，開放命令提示字元中，執行下列命令：  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe 嗎？  我**  
  
        2.  重新整理裝置管理員]。 應該會出現 [驅動程式。  
  
    4.  如果電腦螢幕會顯示的圖示，驅動程式是安裝及執行正常運作。 關閉裝置管理員]。  
  
    5.  如果顯示的圖示不是電腦螢幕  
  
        1.  以滑鼠右鍵按一下**Windows Server 方案電腦還原驅動程式**，然後按**屬性**。  
  
        2.  按一下**的驅動程式**索引標籤，然後按一下 [**更新驅動程式]**。  
  
        3.  按一下**[自動搜尋更新的驅動程式軟體]**，然後依照指示更新的驅動程式。  
  
    6.  關閉裝置管理員]。  
  
6.  解除安裝 Windows Server Essentials 連接器軟體的電腦，然後再重新安裝它。 如需詳細資訊，請查看主題[解除安裝的連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a>疑難排解完整的系統還原  
  
### <a name="errors"></a>錯誤  
  
-   無法登入 client 電腦之後完整的系統還原。  
  
### <a name="resolutions"></a>解析度  
 如果您可以變更電腦的名稱，以及您稍後需要將電腦名稱變更，復原之後，當您嘗試在您的網域帳號，登入之前儲存的備份還原您將會收到這個錯誤:「伺服器上的安全性資料庫不會有此工作站信任關係電腦負責」。 存取網路電腦再試一次，以移除連接器軟體、電腦從 Windows 網域，，再重新連接到伺服器。  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>若要取回網路存取，來還原電腦之後的電腦名稱變更  
  
1.  本機系統管理員身分登入電腦。  
  
2.  解除安裝的連接器軟體。 如需詳細資訊，請查看[解除安裝的連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。  
  
3.  移除網域的電腦。 如需詳細資訊，請查看[Windows 網域中移除的電腦](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。  
  
4.  再試一次將電腦連接到伺服器。 如需詳細資訊，請查看[電腦連接到伺服器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
## <a name="see-also"></a>也了  
  
-   [Windows Server Essentials 的支援](Support-Windows-Server-Essentials.md)