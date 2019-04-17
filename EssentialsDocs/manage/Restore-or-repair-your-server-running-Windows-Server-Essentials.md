---
title: "還原或修復您執行的 Windows Server Essentials 的伺服器"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1ebc539928d13b0d34dfe5a0ee57ce6e98088e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>還原或修復您執行的 Windows Server Essentials 的伺服器

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
 
 本主題提供概觀和支援程序還原或修復執行 Windows Server Essentials 的伺服器，並包含下列區段：  
  
-   [伺服器系統還原的概觀](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [還原或修復系統磁碟機](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [還原的檔案和資料夾的伺服器上](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a>伺服器系統還原的概觀  
 當您執行還原伺服器的狀態影響的可還原方法，如何完整還原您可以執行。  
  
 還原伺服器的最常見原因有：  
  
-   在伺服器上的病毒無法 inoculated 復原或者。  
  
-   伺服器設定的錯誤，而且您無法伺服器。  
  
-   您取代系統磁碟機。  
  
-   您將會淘汰伺服器，並想要還原至新的伺服器。  
  
 您可以做伺服器從備份還原，或您可以將伺服器還原為原廠預設值。  
  
-   [從備份還原伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [重設為原廠預設值的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a>從備份還原伺服器  
 本章節提供何種選擇備份指導方針。  
  
 如果有可用的備份，最佳您選擇還原您的伺服器是從外部備份還原使用製造商的安裝媒體。 還原將會從您選擇備份復原伺服器設定與資料夾。 您只需要設定與還原備份後建立的資料。  
  
 當您選擇復原從先前的備份還原您的伺服器時，您必須選取您想要還原備份特定與您必須具有有效備份檔案伺服器直接連接外部硬碟機：  
  
-   **如果您有最新的伺服器成功備份**，並您的備份包含所有重要資料，您的選擇非常簡單。 您只需要將您一個好備份及重新設定設定變更後備份後建立的資料重新建立。  
  
-   **如果您因為病毒還原您的伺服器**，您知道的備份發生之前接收病毒選取。 您可能需要數天回復到選取全新的備份。  
  
-   **如果您要還原您的伺服器，因為不良的組態設定**，選取您認識的備份發生之前組態的設定會在伺服器造成問題的變更。  
  
 當您從備份還原時的確切程序和需要的後續而定的硬碟上伺服器是否會取代系統磁碟機：  
  
-   **如果伺服器單一的硬碟機並不會取代磁碟機**，磁碟機分割資訊保留當您將會還原伺服器。 還原系統磁碟區，以及剩餘磁碟區上的資料會保留。  
  
-   **如果伺服器有硬碟單一和磁碟機會取代**、還原系統磁碟區，然後再您必須手動還原資料夾資料磁碟區。 非預設的共用的資料夾需要建立，因為它們不會建立時重新建立伺服器儲存空間。  
  
-   **如果伺服器有多個困難的磁碟機，並不會取代磁碟機 0（包含系統磁碟區）**，磁碟機分割資訊保留當您將會還原伺服器。 還原系統磁碟區，並會保留所有剩餘磁碟區上的資料。  
  
-   **如果伺服器有多個困難的磁碟機，並會取代磁碟機 0（包含系統磁碟區）**，還原系統磁碟區，然後您必須手動還原共用之前已經 0 磁碟機上儲存的資料夾。  
  
###  <a name="BKMK_FactoryReset"></a>重設為原廠預設值的伺服器  
 如果您無法從或其他原因您想要或需要來執行完整的系統還原不需要還原先前的伺服器設定，您可以還原備份，您可以執行還原重設為原廠預設值的伺服器為利用安裝或復原媒體伺服器硬體製造商。  
  
 當您重設為原廠預設設定來還原您的伺服器時，刪除所有現有的設定和您的伺服器上安裝的應用程式，您必須再試一次設定您的伺服器。 原廠重設之後，則您的伺服器重新開機。  
  
 當您執行的原廠重設時，您可以選擇保留您的資料或 delete，請使用下列影響：  
  
-   如果您選擇讓您的資料，所有的資料刪除系統磁碟區，但在其他磁碟區上的資料會保留。  
  
    > [!CAUTION]
    >  如果不符合磁碟設定預設設定，將會刪除磁碟上的所有資料。 如果您取代系統磁碟，新的磁碟必須大於原始磁碟的系統磁碟區。  
    >   
    >  如果系統磁碟機的磁碟分割資訊讀取，或如果您取代系統磁碟機，請所有系統磁碟機上的資料將被都移除，即使您選擇要保留您的資料。  
  
-   如果您打算解除或重新安排伺服器，請選擇所有的資料。 除了伺服器設定、其他設定，並系統磁碟區上的資料，刪除所有其他資料，並且在伺服器上的所有硬碟會重新都設定。  
  
> [!NOTE]
>  如果您執行原廠重設之前儲存空間設定伺服器上，您應該使用**進階**區段**管理儲存空間**主機手動移除所有的儲存空間。  
  
 原廠重設之後，您將需要執行下列工作：  
  
-   **重新設定伺服器。** 在伺服器上，使用設定伺服器精靈重新輸入設定。 若要設定 client 電腦從遠端管理的 Windows Server Essentials 伺服器，開放的網頁瀏覽器，然後輸入**http://***< YourServerName\ >*在網址列。  
  
-   **重新連接到伺服器 client 電腦。** 如果電腦先前已連接到伺服器，您必須解除安裝 Windows Server Essentials 連接器軟體的電腦之前，您將電腦連接到伺服器再試一次。 如需詳細資訊，請查看[解除安裝的連接器軟體](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)和[電腦連接到伺服器](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="BKMK_Restore"></a>還原或修復系統磁碟機  
 還原伺服器的第一個步驟是來還原或修復伺服器系統磁碟機。 您的系統磁碟機來還原之後，您將會執行任何需要還原伺服器上的任何分享遺失了信件中還原還原資料磁碟機。  
  
 適用於執行還原三種方式︰  
  
-   [還原或修復您的伺服器使用安裝媒體](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1)。 使用安裝媒體伺服器製造商從備份還原。  
  
-   **使用安裝媒體來還原預設值為原廠設定伺服器**。 若要了解如何將您的伺服器上執行此動作，請查看製造商伺服器的文件。  
  
-   [還原或重設您的伺服器 client 使用從電腦修復 DVD](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)。 如果您需要還原執行的 Windows Server Essentials 的遠端管理的伺服器，您必須使用還原 DVD 伺服器製造商 client 電腦的執行還原。  
  
###  <a name="BKMK_Restore_1"></a>還原或修復您的伺服器使用安裝媒體  
 下列程序告訴您如何使用 Windows Server Essentials 的安裝媒體來從備份還原您的伺服器系統磁碟機。 （若要了解如何使用安裝媒體來還原為原廠預設設定，請查看製造商伺服器的文件）。  
  
> [!NOTE]
>  如果伺服器會使用儲存空間，並將資料還原到新的伺服器，您應該復原系統磁碟機第一次，再登入 Windows Server Essentials 儀表板、儲存空間類似方式與在舊的伺服器，並設定然後復原資料磁碟區。  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>從使用安裝媒體的備份還原伺服器系統磁碟機  
  
1.  Windows Server Essentials 安裝 DVD 插入伺服器 DVD 光碟機、重新開機伺服器，並按任意鍵從 DVD 開始。  
  
    > [!NOTE]
    >  如果還原程序會不會自動開始，檢查以確保 DVD 光碟機，會顯示在開機功能表中的第一個伺服器的 BIOS 設定。  
  
     -或者-  
  
     若製造商預先載入的伺服器上安裝媒體，請在開始從安裝媒體開機按 F8。  
  
2.  Windows Server 之後檔案載入，選擇您的語言及其他喜好設定]，然後按一下**下一步**。  
  
3.  在精靈中的下一個頁面上，按一下 [**修復您的電腦**。  
  
    > [!CAUTION]
    >  請勿選擇**立即安裝]**選項。 這個選項會引導您完成的完整的系統安裝刪除所有設定和所有系統磁碟機上的資料。  
  
4.  在**選擇一個選項**頁面上，按**的「疑難排解」**。  
  
5.  在**系統映像修復**頁面上，選取目前的系統？任一**Windows Server Essentials**或**Windows Server Essentials**。  
  
     重新影像您的電腦] 精靈會開啟。  
  
6.  在**選擇系統映像備份**頁面上，您可以選擇要使用的最新的備份，或者您可以選取較早的備份。 系統將會還原到您所選擇的備份還原或修復您的伺服器的時間時的狀態。 已新增的資料或對設定所做的已儲存的備份之後, 必須重新建立。  
  
     選取下列其中一個選項，其中一個，然後按一下**下一步**:  
  
    -   **使用最新可用系統映像（建議選項）**  
  
    -   **選取 [系統映像**  
  
    > [!NOTE]
    >  如果您有最新，成功備份，您知道的備份包含所有重要資料，您的選擇是伺服器的非常簡單。 您只需要將您一個好備份及重新設定設定變更後備份後建立的資料重新建立。  
    >   
    >  如果您因為病毒還原您的伺服器，選取 [建立的備份，您知道發生之前接收病毒。 您可能需要數天回復到選取全新的備份。  
    >   
    >  如果您因為不正確設定還原您的伺服器，選取 [建立的備份發生問題所造成的伺服器的組態設定變更之前您知道。  
  
7.  依照精靈中的指示，完成系統還原。  
  
8.  伺服器成功還原之後，如果您使用，移除 DVD 安裝並重新伺服器。  
  
> [!NOTE]
>  若要還原並分享伺服器上的資料夾，您可能需要進行額外的步驟。 如需詳細資訊，請查看[還原的檔案和資料夾的伺服器上](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)。  
  
###  <a name="BKMK_Restore_2"></a>還原或重設您的伺服器 client 使用從電腦修復 DVD  
 在 Windows Server Essentials，您可以開始伺服器開機的 USB 快閃磁碟機您所建立，並再您復原伺服器的 client 的電腦使用修復 DVD，您收到來自伺服器的製造商。 Client 的電腦必須是相同的網路伺服器上。 此方法不提供 Windows Server Essentials。  
  
 下列程序會提供一般步驟執行伺服器還原。 步驟會還原回從或還原為原廠預設設定也同樣適用。 更多特定的指示，會看到來自 server 製造商的文件。  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>還原或重設 client 使用從電腦修復 DVD 伺服器  
  
1.  插入您收到來自伺服器製造商 client 電腦中的 Windows Server Essentials 伺服器復原媒體。  
  
     [修復您的伺服器精靈開啟。  
  
2.  依照精靈中的指示來建立可開機 USB 快閃磁碟機，您要用來開始修復模式中的伺服器。  
  
3.  [修復您的伺服器] 精靈會準備開機的 USB 快閃磁碟機之後，快閃磁碟機插入伺服器，並再開始修復模式中的伺服器。 若要了解如何修復模式開始您的伺服器，請參考文件從硬體製造商的伺服器。  
  
     伺服器開始修復模式之後，[修復您的伺服器精靈尋找伺服器，並建立的連接，然後。  
  
4.  依照精靈中的指示，完成還原伺服器。  
  
> [!NOTE]
>  這種伺服器復原的方法到外部存放裝置連接到伺服器期間復原。 如果您想要清除儲存的外部裝置上的資料，您必須手動執行動作。  
  
> [!NOTE]
>  如果從備份還原資料之後，您可以建立伺服器上的額外的共用的資料夾，其他的共用的資料夾可能無法辨識伺服器。 您必須再次分享這些資料夾。 如需詳細資訊，請查看[還原的檔案和資料夾的伺服器上](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)。  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a>還原的檔案和資料夾的伺服器上  
 根據您用來還原或修復您的伺服器，並使用的儲存空間伺服器類型的方法，您可能需要後還原系統磁碟機復原資料磁碟區。 在某些案例中，您可能需要再次共用現有的資料夾，以便伺服器會它們。  
  
 您可能需要還原的檔案和資料夾，以下是一些事情：  
  
-   [從伺服器備份還原的檔案和資料夾](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup)。 如果您取代系統磁碟或磁碟分割上的資訊系統磁碟無法讀取，您可以還原系統，但您無法從這個磁碟上的其他磁碟區還原資料。 若要從其他資料磁碟區還原的檔案和資料夾，您必須使用 [還原的檔案和資料夾精靈。  
  
-   [還原在伺服器上的共用的資料夾](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders)。 如果從備份還原的系統磁碟機之後，您可以建立伺服器上的額外的共用的資料夾的共用的資料夾仍是資料磁碟分割或回復到資料的磁碟分割，但無法辨識的伺服器。 您必須再次分享這些資料夾。  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a>從伺服器備份還原的檔案和資料夾  
 還原的檔案和資料夾精靈可協助您保護您的資料，如果您的硬碟停止運作或不小心會清除您的檔案。 與 Windows Server Essentials 的備份，您可以在您的硬碟上建立的所有的資料複本，並外接的存放裝置上儲存資料。 如果您硬碟上的原始資料不小心清除資料，覆寫」，或因為發生問題而無法存取，您可以從備份還原資料。 還原的檔案或資料夾精靈可協助您從現有的備份還原單一檔案或資料夾、多個檔案或資料夾或整硬碟機。  
  
 系統復原之後，您可能需要使用 [還原的檔案和資料夾精靈還原的檔案和資料夾的期間還原不會保留。 例如，如果您取代系統磁碟或磁碟分割上的資訊系統磁碟無法讀取，您無法從系統磁碟上的其他磁碟區還原資料。  
  
> [!NOTE]
>  您無法使用 [還原的檔案和資料夾精靈還原完整的系統磁碟機。 如需有關如何還原完整的系統資訊，[還原或修復您使用安裝媒體的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1)或[還原或重設您的伺服器 client 使用從電腦修復 DVD](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)。  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>若要從伺服器備份還原的檔案和資料夾  
  
1.  打開 Windows Server Essentials 儀表板，，然後按一下**裝置**索引標籤。  
  
2.  按一下伺服器的名稱，然後按一下**還原的檔案或資料夾伺服器的**在**工作**窗格。  
  
     還原的檔案和資料夾精靈開啟。  
  
3.  依照精靈中的指示來還原的檔案或資料夾。  
  
> [!WARNING]
>  如需有關備份及還原的檔案和資料夾的詳細資訊，請查看[管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_ConfigreSharedFolders"></a>還原在伺服器上的共用的資料夾  
 還原伺服器的系統磁碟機，如果仍然資料磁碟分割上的共用的資料夾，或回復到資料的磁碟分割之後，您可能需要設定才能辨識資料夾伺服器的共用的資料夾。 下列程序告訴您如何新增共用的資料夾已經之前分享。  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>將現有的資料夾新增至伺服器的共用資料夾  
  
1.  在檔案總管] 中找到的硬碟上的共用的資料夾。  
  
2.  以滑鼠右鍵按一下 [共用] 資料夾，請按一下**屬性**，按一下 [**共用**索引標籤，然後再寫資料夾權限。  
  
3.  登入 Windows Server Essentials 儀表板，請按一下**存放裝置**索引標籤，然後按一下 [**資料夾新增**中**伺服器的工作資料夾**窗格。  
  
     [新增資料夾精靈開啟。  
  
4.  輸入名稱共用中的**名稱**方塊。  
  
5.  按一下**瀏覽**，瀏覽至*< drive\ > \\ < ServerName\ >*\ServerFolders (例如*d:\Contoso\ServerFolders*)，選取的資料夾，您要分享，然後按一下**[確定]**。  
  
6.  按一下**下一步**。  
  
7.  指定您要記下在執行「步驟 2 的權限，然後按一下**資料夾新增**。  
  
    > [!IMPORTANT]
    >  這些權限，將會取代現有的權限，不會加到資料夾，請使用 Windows Server Essentials 儀表板。  
  
> [!IMPORTANT]
>  將資料夾新增到清單的共用資料夾您完成後，請確認資料夾會包含在伺服器備份。 有關如何將資料夾新增至伺服器備份，請查看[設定或自訂伺服器備份](Set-up-or-customize-server-backup.md)。  
  
## <a name="see-also"></a>也了  
  
-   [管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
