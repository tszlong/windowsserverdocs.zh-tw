---
title: 疑難排解 Windows Server Essentials 中的電腦備份與還原錯誤
description: 說明如何使用 Windows Server Essentials
ms.date: 06/25/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0207a143022c566530751e58a0ffeda03fce9385
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470023"
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>疑難排解 Windows Server Essentials 中的電腦備份與還原錯誤

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

使用下列程序可疑難排解在 Windows Server Essentials 中的電腦備份，包括備份設定問題、不完整或不成功的備份、備份的健康情況警示，以及檔案、資料夾或完整系統還原的問題。

> [!NOTE]
>  如需 Windows Server Essentials 社區的最新疑難排解資訊，請造訪[Windows Server Essentials 論壇](https://social.technet.microsoft.com/Forums//winserveressentials/threads)。

##  <a name="troubleshoot-backup-configuration-issues-for-a-connected-computer"></a><a name="BKMK_TroubleshootBackupConfigurationIssues"></a>針對連線電腦的備份設定問題進行疑難排解
 使用這些程序可疑難排解在 Windows Server Essentials 伺服器上備份之電腦的備份設定問題。

### <a name="errors"></a>Errors

-   備份設定未順利完成

-   收集電腦資訊時發生錯誤

-   從備份移除電腦時發生錯誤

### <a name="resolutions"></a>解決方式

##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>疑難排解設定連線電腦的備份時所發生的錯誤

1.  確定您的電腦已透過網路裝置連線到網路。

2.  確定電腦所連接的網路裝置也已連線到網路、電源已開啟，而且正常運作。

3.  確定 Windows Server Client Backup Service 與 Windows Server Client Computer Backup Provider Service 正在伺服器上執行。

    ###### <a name="to-start-computer-backup-services-on-the-server"></a>在伺服器上啟動電腦備份服務

    1.  在伺服器上，依序按一下 [開始]****、[系統管理工具]****，然後按一下 [服務]****。

    2.  向下捲動找到 [Windows Server Client Computer Backup Provider Service]**** 並且按一下。 如果服務的狀態不是 [已啟動]****，則以滑鼠右鍵按一下服務，然後按一下 [啟動]****。

    3.  按一下 [Windows Server Client Computer Backup Service]****。 如果服務的狀態不是 [已啟動]****，則以滑鼠右鍵按一下服務，然後按一下 [啟動]****。

    4.  關閉 [服務]****。

4.  確定 Windows Server Client Computer Backup Provider Service 正在用戶端電腦上執行。

    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>在用戶端電腦上啟動電腦備份服務

    1.  在用戶端電腦上，按一下 [開始]****，在 [搜尋程式及檔案]**** 文字方塊中輸入**服務**，然後按 Enter 鍵。

    2.  向下捲動找到 [Windows Server Client Computer Backup Provider Service]**** 並且按一下。 如果服務的狀態不是 [已啟動]****，則以滑鼠右鍵按一下服務，然後按一下 [啟動]****。

    3.  關閉 [服務]****。

5.  檢查警示以判斷備份資料庫中是否有錯誤。 如果有錯誤，請依照警示中的指示修復備份資料庫。

6.  從電腦解除安裝 Windows Server Essentials 連接器軟體，然後再重新安裝它。 如需詳細資訊，請參閱主題[解除安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。

##  <a name="troubleshoot-a-backup-that-did-not-complete-properly"></a><a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>針對未正確完成的備份進行疑難排解
 備份若處於不成功的狀態，則備份的所有部份都視為不成功，也沒有資料可供您還原。 不過，若備份處於不完整的狀態，則表示並非備份設定中指定的所有項目都已備份，但某些資料可能可供您還原。

### <a name="errors"></a>Errors

-   備份不完整

-   備份不成功

### <a name="resolutions"></a>解決方式

##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>找出未成功備份的磁碟區

1.  開啟 Windows Server Essentials 儀表板，然後按一下 [電腦和備份]****。

2.  按一下備份未成功完成的電腦名稱，然後按一下 [工作]**** 窗格中的 [檢視電腦內容]****。

3.  按一下未成功完成的備份，然後按一下 [檢視詳細資料]****。

4.  在 [備份詳細資料]**** 對話方塊中，每個成功備份的磁碟區，其狀態會顯示綠色核取記號。

##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>疑難排解失敗的磁碟區備份

1.  確定硬碟已連接到電腦、電源已開啟，而且正常運作。

2.  執行 **chkdsk /f /r** 修正硬碟上的任何錯誤 (**/f**)，以及從任何損壞的磁區復原可讀取的資訊 (**/r**)。 如需執行 **chkdsk**的詳細資訊，請參閱 [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562)。

3.  確定備份執行時電腦未關機或中斷網路連線。

4.  確定每個磁碟區有足夠的可用空間來執行備份。 在用戶端電腦上進行備份，需要一些額外的磁碟空間來建立 VSS 快照。 在不是系統保留的任何磁碟區上必須至少有 10% 的可用磁碟空間。 在系統保留的磁碟區上，如果磁碟區大小低於 500 MB，VSS 需要 32 MB 的可用空間才能建立快照；如果磁碟區是 500 MB 或更大，VSS 就需要 320 MB 的可用空間。

     如果磁碟區的可用空間不足，請嘗試下列其中一個解決方法：

    -   延伸磁碟區。 您可以延伸除了系統保留的磁碟區以外的任何基本或動態磁碟區。

        ###### <a name="to-extend-a-volume"></a>延伸磁碟區

        1.  在 [控制台] 中，按一下 [系統及安全性]****。

        2.  在 [系統管理工具]**** 下，按一下 [建立及格式化硬碟磁碟分割]****。

        3.  以滑鼠右鍵按一下您想要延伸的磁碟區。 如果 [延伸磁碟區]**** 已啟用，請選取該選項。 如果選項未啟用，您就無法延伸磁碟區。

        4.  依照「延伸磁碟區精靈」中的步驟延伸磁碟區。

    -   刪除磁碟區的內容以取得更多可用空間。

        > [!NOTE]
        >  如果您需要釋放系統保留的磁碟區的空間，您可以將系統復原映像移動到不同的磁碟區。 如需指示，請參閱 [部署系統復原映像](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx)。

    -   從用戶端備份排除磁碟區。 只有當您不太需要維護磁碟區資料的備份時，才執行這項操作。

        > [!WARNING]
        >  如果您從用戶端備份排除系統保留的磁碟區，就不會備份用戶端系統，您就無法在電腦上執行完整系統還原。

5.  檢查伺服器上指出伺服器沒有足夠磁碟空間的其他警示，以便讓備份順利完成。 依照警示中的指示修正問題。

6.  在命令提示字元中執行 **vssadmin** 可疑難排解磁碟區陰影複製服務 (VSS) 問題。 如需 **vssadmin**的相關資訊，請參閱 [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332)。

##  <a name="troubleshoot-backup-health-alert-issues"></a><a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>疑難排解備份健康情況警示問題

### <a name="errors"></a>Errors

-   Windows Server Solutions Computer Backup Provider Service 停止運作

-   Windows Server 解決方案用戶端電腦備份提供者服務 停止運作

### <a name="resolutions"></a>解決方式

##### <a name="to-troubleshoot-a-backup-health-alert"></a>疑難排解備份健康情況警示

1.  如果警示告訴您備份資料庫有問題，請依照警示中的指示修正問題。

2.  如果警示告訴您備份服務未執行，請嘗試在您收到錯誤訊息的伺服器或用戶端電腦上啟動服務。

    ###### <a name="to-start-backup-services-on-the-server"></a>在伺服器上啟動備份服務

    1.  在伺服器上，依序按一下 [開始]****、[系統管理工具]****，然後按一下 [服務]****。

        > [!NOTE]
        >  如果您從遠端管理伺服器，您必須使用「遠端桌面連線」存取伺服器桌面。 如需使用「遠端桌面連線」的相關資訊，請參閱 [使用「遠端桌面連線」連線到另一部電腦](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection)。

    2.  向下捲動找到 [Windows Server Client Computer Backup Provider Service]**** 並且按一下。 如果服務的狀態不是 [已啟動]****，則以滑鼠右鍵按一下服務，然後按一下 [啟動]****。

    3.  按一下 [Windows Server Client Computer Backup Service]****。 如果服務的狀態不是 [已啟動]****，則以滑鼠右鍵按一下服務，然後按一下 [啟動]****。

    4.  關閉 [服務]****。

    ###### <a name="to-start-backup-services-on-a-client-computer"></a>在用戶端電腦上啟動備份服務

    1.  在用戶端電腦上，按一下 [開始]****，在 [搜尋程式及檔案]**** 文字方塊中輸入**服務**，然後按 Enter 鍵。

    2.  以滑鼠右鍵按一下 [Windows Server Client Computer Backup Provider Service]****，然後按一下 [啟動]****。

    3.  關閉 [服務]****。

3.  查看用戶端電腦或伺服器上的事件記錄檔，了解與備份服務或驅動程式相關的問題。

4.  重新啟動您收到錯誤訊息的伺服器或用戶端電腦。

5.  檢查健康情況警示是否有可能會對用戶端備份產生影響的其他問題。

##  <a name="troubleshoot-a-file-or-folder-restore"></a><a name="BKMK_FileAndFolder"></a>針對檔案或資料夾還原進行疑難排解

### <a name="errors"></a>Errors

-   檔案或資料夾還原未順利完成。

### <a name="resolutions"></a>解決方式

##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>疑難排解失敗的檔案或資料夾還原

1.  確定您的電腦已透過網路裝置連線到網路。

2.  確定電腦所連接的網路裝置也已連線到網路、電源已開啟，而且正常運作。

3.  檢查警示以判斷備份資料庫中是否有錯誤。 如果有錯誤，請依照警示中的指示修復備份資料庫。

4.  嘗試從另一個備份還原檔案或資料夾。

5.  確定「Windows Server 解決方案電腦還原驅動程式」**** 已安裝且正常運作。

    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>檢查 Windows Server 解決方案電腦還原驅動程式的狀態

    1.  按一下 [開始]****，在 [搜尋程式及檔案]**** 文字方塊中輸入 **Device Manager**，然後按 Enter 鍵。

    2.  在裝置管理員中，按一下 [系統裝置]****，捲動到 [Windows Server 解決方案電腦還原驅動程式]****。

    3.  如果沒有顯示驅動程式：

        1.  以系統管理員權限開啟命令提示字元並執行下列命令：

             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe？-i**

        2.  重新整理裝置管理員。 驅動程式應該會出現。

    4.  如果顯示的圖示是電腦監視器，表示驅動程式已安裝並正確執行。 關閉裝置管理員。

    5.  如果顯示的圖示不是電腦監視器

        1.  以滑鼠右鍵按一下 [Windows Server 解決方案電腦還原驅動程式]****，然後按一下 [內容]****。

        2.  按一下 [驅動程式]**** 索引標籤，然後按一下 [更新驅動程式]****。

        3.  按一下 [自動搜尋更新的驅動程式軟體]****，然後依照指示更新驅動程式。

    6.  關閉裝置管理員。

6.  從電腦解除安裝 Windows Server Essentials 連接器軟體，然後再重新安裝它。 如需詳細資訊，請參閱主題[解除安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)。

##  <a name="troubleshoot-a-full-system-restore"></a><a name="BKMK_Troubleshootfullsystemrestoreissues"></a>針對完整系統還原進行疑難排解

### <a name="errors"></a>Errors

-   進行完整系統還原之後無法登入用戶端電腦。

### <a name="resolutions"></a>解決方式
 如果您變更電腦的名稱，而且之後需要還原至電腦名稱變更之前儲存的備份，在還原之後，當您嘗試以您的網域帳戶登入時，您會收到下列錯誤：「伺服器上的安全性資料庫沒有此工作站信任關係的電腦帳戶。」 若要再次使用網路存取電腦，請移除連接器軟體，從 Windows 網域移除電腦，然後再重新連線到伺服器。

##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>電腦名稱變更之後要再次使用網路存取還原的電腦

1.  以本機系統管理員身分登入電腦。

2.  解除安裝連接器軟體。 如需詳細資訊，請參閱[解除安裝連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)。

3.  從網域移除電腦。 如需詳細資訊，請參閱[從 Windows 網域移除電腦](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)。

4.  再次將電腦連線到伺服器。 如需詳細資訊，請參閱[將電腦連線到伺服器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。

## <a name="additional-references"></a>其他參考

-   [支援 Windows Server Essentials](Support-Windows-Server-Essentials.md)