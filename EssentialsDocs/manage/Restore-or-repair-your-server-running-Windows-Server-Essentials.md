---
title: 還原或修復執行 Windows Server Essentials 的伺服器
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862349"
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>還原或修復執行 Windows Server Essentials 的伺服器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 本主題提供概觀和支援程序來還原或修復執行 Windows Server Essentials 的伺服器，並包含下列各節：  
  
-   [伺服器系統還原的概觀](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [還原或修復系統磁碟機](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [還原檔案和資料夾在伺服器上](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a> 伺服器系統還原的概觀  
 當您執行還原時伺服器所處的狀態會影響可用的還原方法，以及您可以執行多齊全的還原。  
  
 還原伺服器最常見的原因如下：  
  
-   伺服器上的病毒無法防護或刪除。  
  
-   伺服器組態設定值不正確，且無法啟動伺服器。  
  
-   您更換了系統磁碟機。  
  
-   您正在淘汰伺服器，而您想要還原至新的伺服器。  
  
 您可以從備份來還原伺服器，或者可以將伺服器還原為原廠預設設定。  
  
-   [從備份還原伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [將伺服器重設為原廠預設設定](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a> 從備份還原伺服器  
 本節提供要選擇何種備份類型的指引。  
  
 備份可供使用，如果您還原伺服器的最佳選擇是使用製造商的安裝媒體從外部備份還原。 還原過程將會從您所選擇的備份復原伺服器設定及資料夾。 您只需要設定設定值，並且還原備份之後建立的資料。  
  
 當您選擇從先前的備份還原來復原您的伺服器時，您必須選擇您想要還原的特定備份，而且您在直接連接到伺服器的外部硬碟上必須擁有有效的備份檔案：  
  
-   **如果您有伺服器的最新成功備份**，而且您知道備份中包含所有重要資料，您的選擇就相當簡單。 您只需要重新建立您上次良好備份之後所建立的資料，以及重新設定備份之後變更的設定。  
  
-   **如果您因病毒而還原伺服器**，則選取在收到病毒前所進行備份。 您可能需要返回幾天前選取乾淨的備份。  
  
-   **如果您因為組態設定不正確而還原伺服器**，則選取在造成伺服器發生問題的組態設定變更前所進行的備份。  
  
 當您從備份還原時，確切的程序與必要的後續工作會根據伺服器上的硬碟數目和是否更換系統磁碟機而不同：  
  
-   **如果伺服器只有一個硬碟，而且不更換磁碟機**，當您還原伺服器時，磁碟機的磁碟分割資訊會保留不變。 系統磁碟區會進行還原，並保留其他磁碟區上的資料。  
  
-   **如果伺服器只有一個硬碟，而且要更換磁碟機**，則系統磁碟區會進行還原，然後您必須手動將資料夾還原到資料磁碟區。 您必須建立任何非預設的共用資料夾，因為重建伺服器儲存體時不會建立這些資料夾。  
  
-   **如果伺服器有多個硬碟，而且不更換磁碟機 0 (包含系統磁碟區)**，當您還原伺服器時，磁碟機的磁碟分割資訊會保留不變。 系統磁碟區會進行還原，並保留所有其他磁碟區上的資料。  
  
-   **如果伺服器有多個硬碟，而且要更換磁碟機 0 (包含系統磁碟區)**，系統磁碟區會進行還原，然後您必須手動還原先前儲存在磁碟機 0 的任何共用資料夾。  
  
###  <a name="BKMK_FactoryReset"></a> 將伺服器重設為原廠預設設定  
 如果您沒有可供還原的備份，或者因為某些其他原因，您想要或需要執行完整系統還原而不還原先前的伺服器設定，您可以使用伺服器硬體製造商的安裝或復原媒體執行還原，將伺服器重設為原廠預設設定。  
  
 當您藉由重設為原廠預設設定來還原伺服器時，會刪除伺服器上所有現有的設定及安裝的應用程式，您也必須重新設定伺服器。 原廠重設之後，伺服器會重新啟動。  
  
 當您執行原廠重設時，可以選擇保留或刪除您的資料，但會發生下列結果：  
  
-   如果您選擇要保留您所有的資料，則系統磁碟區上的所有資料都會被刪除，但其他磁碟區上的資料則會被保留。  
  
    > [!CAUTION]
    >  如果磁碟設定不符合預設設定，磁碟上的所有資料都將會刪除。 如果您更換了系統磁碟，新的磁碟必須是大於原始磁碟的系統磁碟區。  
    >   
    >  如果系統磁碟機的磁碟分割資訊無法讀取，或如果您更換系統磁碟機，則系統磁碟機上的所有資料都會移除，即使您選擇要保留您所有的資料。  
  
-   如果您打算解除委任或重新安排伺服器，請選擇要刪除所有資料。 除了伺服器設定、其他設定和系統磁碟區上的資料，所有其他資料都會刪除，伺服器上的所有硬碟也都會重新格式化。  
  
> [!NOTE]
>  如果要在執行原廠重設之前先在伺服器上設定儲存空間，您要使用 [管理儲存空間] 主控台的 [進階] 區段，手動移除所有的儲存空間。  
  
 原廠重設之後，您必須執行下列工作：  
  
-   **重新設定伺服器。** 在伺服器上，使用「設定伺服器精靈」重新輸入組態設定。 設定遠端管理的 Windows Server Essentials 伺服器從用戶端電腦，請開啟網頁瀏覽器，然後鍵入 **http://***<YourServerName\>* 的網址列中。  
  
-   **將用戶端電腦重新連線到伺服器。** 如果電腦先前已連線到伺服器中，您必須解除安裝 Windows Server Essentials 連線程式軟體從電腦的電腦重新連線至伺服器。 如需詳細資訊，請參閱 [Uninstall the Connector software](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) 和 [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)。  
  
##  <a name="BKMK_Restore"></a> 還原或修復系統磁碟機  
 還原伺服器的第一個步驟是要還原或修復伺服器系統磁碟機。 還原系統磁碟機之後，您要盡可能執行任何方法以還原伺服器上的資料磁碟機，以及還原在還原時遺失的任何共用。  
  
 以下三種方法可供執行還原：  
  
-   [使用安裝媒體還原或修復您的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1)。 您可以使用伺服器製造商的安裝媒體從備份還原。  
  
-   **使用安裝媒體將伺服器還原為預設原廠設定**。 若要瞭解如何在您的伺服器上執行這項操作，請參閱伺服器製造商的說明文件。  
  
-   [使用復原 DVD 從用戶端電腦還原或重設您的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)。 如果您需要還原執行 Windows Server Essentials 的遠端管理的伺服器，您必須使用伺服器製造商的還原 DVD 從用戶端電腦執行還原。  
  
###  <a name="BKMK_Restore_1"></a> 還原或修復您的伺服器使用安裝媒體  
 下列程序描述如何使用 Windows Server Essentials 安裝媒體從備份還原您的伺服器系統磁碟機。 (若要瞭解如何使用安裝媒體來還原為原廠預設設定，請參閱伺服器製造商的說明文件)。  
  
> [!NOTE]
>  如果伺服器使用儲存空間，而且您要將資料還原至新的伺服器，您應該復原系統磁碟機的第一次，然後登入 Windows Server Essentials 儀表板、 儲存空間類似方式與在舊伺服器上，並設定然後復原 dat磁碟區。  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>使用安裝媒體從備份還原伺服器系統磁碟機  
  
1.  伺服器的 DVD 光碟機中插入 Windows Server Essentials 安裝 DVD，重新啟動伺服器，然後按任意鍵從 DVD 啟動。  
  
    > [!NOTE]
    >  如果還原程序沒有自動啟動，請檢查您伺服器的 BIOS 設定，確定 DVD 光碟機出現在開機功能表的第一個。  
  
     - 或者 -  
  
     如果製造商預先載入了伺服器上的安裝媒體，請在啟動時按 F8 鍵從安裝媒體啟動。  
  
2.  Windows Server 檔案載入之後，選擇您的語言和其他喜好設定，然後按 [下一步] 。  
  
3.  在精靈的下一頁，按一下 [修復您的電腦] 。  
  
    > [!CAUTION]
    >  請勿選擇 [立即安裝]  選項。 該選項會引導您完成完整的系統安裝，並刪除所有的組態設定和系統磁碟機上的所有資料。  
  
4.  在 [選擇選項] 頁面上，按一下 [疑難排解]。  
  
5.  在上**系統映像修復**頁面上，選取目前的系統？ 任一**Windows Server Essentials**或是**Windows Server Essentials**。  
  
     「重新製作電腦映像精靈」便會開啟。  
  
6.  在 [選取系統映像備份]  頁面上，您可以選擇使用最新的備份，或者您可以選取較早的備份。 系統將會還原到您選擇備份的時間當時的狀態，以還原或修復您的伺服器。 在備份儲存之後所新增的資料或進行的設定變更必須重新建立。  
  
     選取下列其中一個選項，然後按 [下一步]：  
  
    -   **使用最新可用的系統映像 (建議選項)**  
  
    -   **選取系統映像**  
  
    > [!NOTE]
    >  如果您有伺服器最新的成功備份，而且您知道備份包含所有重要資料，您的選擇就相當簡單。 您只需要重新建立您上次良好備份之後所建立的資料，以及重新設定備份之後變更的設定。  
    >   
    >  如果您因為病毒而需要還原伺服器，請選取在遭到病毒入侵之前所進行的備份。 您可能需要返回幾天前選取乾淨的備份。  
    >   
    >  如果您因為不正確的組態設定而需要還原伺服器，請選取在導致伺服器發生問題的組態設定變更之前的備份。  
  
7.  依照精靈的指示完成系統還原。  
  
8.  成功還原伺服器之後，請取出安裝 DVD (如果使用)，然後重新啟動伺服器。  
  
> [!NOTE]
>  若要還原並共用伺服器上的資料夾，您可能需要採取額外的步驟。 如需詳細資訊，請參閱 [Restore files and folders on the server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)。  
  
###  <a name="BKMK_Restore_2"></a> 還原或重設您的伺服器，從用戶端電腦使用復原 DVD  
 在 Windows Server Essentials 中，您可以啟動伺服器從可開機的 USB 快閃磁碟機，在建立時，，然後您從復原伺服器的用戶端電腦使用的復原 DVD，從伺服器製造商收到。 用戶端電腦必須與伺服器位於相同的網路。 無法在 Windows Server Essentials 中使用這個方法。  
  
 下列程序提供執行伺服器還原的一般步驟。 步驟也同樣適用於從備份還原，或還原為原廠預設設定。 如需更多特定指示，請參閱伺服器製造商的說明文件。  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>使用復原 DVD 從用戶端電腦還原或重設您的伺服器  
  
1.  插入您從伺服器製造商收到的用戶端電腦的 Windows Server Essentials 伺服器復原媒體。  
  
     「復原您的伺服器精靈」便會開啟。  
  
2.  依照精靈中的指示建立可開機的 USB 快閃磁碟機，以便用來啟動伺服器進入修復模式。  
  
3.  「復原您的伺服器精靈」準備好可開機的 USB 快閃磁碟機之後，在伺服器中插入快閃磁碟機，然後啟動伺服器進入修復模式。 若要瞭解如何啟動您的伺服器進入修復模式，請參閱伺服器硬體製造商的說明文件。  
  
     啟動伺服器進入修復模式之後，「復原您的伺服器精靈」會找出伺服器，然後建立連線。  
  
4.  依照精靈中的指示完成還原伺服器。  
  
> [!NOTE]
>  這個伺服器復原的方法在復原期間會忽略連接到伺服器的外部儲存裝置。 如果您想要清除外部儲存裝置上的資料，您必須以手動方式執行操作。  
  
> [!NOTE]
>  如果您在伺服器上建立了其他的共用資料夾，從備份還原資料之後，伺服器可能無法辨識其他的共用資料夾。 您必須再次共用這些資料夾。 如需詳細資訊，請參閱 [Restore files and folders on the server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)。  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a> 還原檔案和資料夾在伺服器上  
 根據您用來還原或修復您伺服器的方法，以及伺服器使用的儲存類型，您在還原系統磁碟機之後，可能需要復原資料磁碟區。 在某些情況下，您可能需要再次共用現有的資料夾，以便伺服器辨識它們。  
  
 以下是一些您可能需要還原檔案和資料夾的範例：  
  
-   [從伺服器備份還原檔案和資料夾](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup)。 如果您更換了系統磁碟，或者系統磁碟上的磁碟分割資訊無法讀取，您可以還原系統，但您無法從這個磁碟上的其他磁碟區還原資料。 若要從其他資料磁碟區還原檔案和資料夾，您必須使用「還原檔案或資料夾精靈」。  
  
-   [還原伺服器上的共用資料夾](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders)。 如果您在伺服器上建立了其他共用資料夾，從備份還原系統磁碟機之後，共用資料夾仍然會在資料磁碟分割上，或已還原到資料磁碟分割，但伺服器可能無法辨識。 您必須再次共用這些資料夾。  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a> 從伺服器備份還原檔案和資料夾  
 如果您的硬碟停止運作或不慎刪除您的檔案，「還原檔案或資料夾精靈」可協助您保護您的資料。 使用 Windows Server Essentials 的備份，您可以建立硬碟機上的所有資料的複本，並將資料儲存在外接式存放裝置。 如果您硬碟上的原始資料無意間被刪除、覆寫或因為故障而無法存取，您可以從備份還原資料。 「還原檔案或資料夾精靈」可協助您從現有的備份還原單一檔案或資料夾、多個檔案或資料夾，或還原整個硬碟。  
  
 系統還原之後，您可能需要使用「還原檔案或資料夾精靈」來還原在還原期間未保留的檔案與資料夾。 例如，如果您更換了系統磁碟，或者系統磁碟上的磁碟分割資訊無法讀取，您就無法從系統磁碟上的其他磁碟區還原資料。  
  
> [!NOTE]
>  您無法使用「還原檔案或資料夾精靈」來還原完整的系統磁碟機。 如需如何還原完整系統的相關資訊，請參閱 [Restore or repair your server using installation media](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) 或 [Restore or reset your server from a client computer using the recovery DVD](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)。  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>從伺服器備份還原檔案和資料夾  
  
1.  開啟 Windows Server Essentials 儀表板，然後再按一下**裝置** 索引標籤。  
  
2.  按一下伺服器的名稱，然後按一下 [工作] 窗格中的 [還原伺服器的檔案或資料夾]。  
  
     「還原檔案或資料夾精靈」便會開啟。  
  
3.  請依照精靈中的指示來還原檔案或資料夾。  
  
> [!WARNING]
>  如需有關備份和還原檔案和資料夾的詳細資訊，請參閱 < [Manage Backup and Restore](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_ConfigreSharedFolders"></a> 還原伺服器上的共用的資料夾  
 如果仍在使用的資料分割的共用的資料夾，或已還原到的資料分割，還原伺服器系統磁碟機之後，您可能需要設定讓伺服器辨識資料夾中的共用的資料夾。 下列程序說明如何新增之前已共用的共用資料夾。  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>將現有的資料夾新增至伺服器的共用資料夾  
  
1.  在檔案總管中，找出硬碟上的共用資料夾。  
  
2.  以滑鼠右鍵按一下共用資料夾，依序按一下 [內容] 、[共用]  索引標籤，然後記下資料夾權限。  
  
3.  登入 Windows Server Essentials 儀表板中，按一下**儲存體**索引標籤，然後再按一下**加入資料夾**中**伺服器資料夾工作**窗格。  
  
     「新增資料夾精靈」便會開啟。  
  
4.  在 [名稱] 方塊中輸入共用的名稱。  
  
5.  按一下 **瀏覽**，瀏覽至 *< 磁碟機\>\\< ServerName\>* \ServerFolders (例如*d:\Contoso\ServerFolders*)，選取您想要共用，然後按一下資料夾**確定**。  
  
6.  按一下 [下一步] 。  
  
7.  指定您在步驟 2 中記下的權限，然後按一下 [新增資料夾] 。  
  
    > [!IMPORTANT]
    >  這些權限將會取代現有的權限使用 Windows Server Essentials 儀表板未新增至資料夾。  
  
> [!IMPORTANT]
>  將資料夾新增至共用資料夾清單之後，請確定伺服器備份中包含這些資料夾。 如需將資料夾新增至伺服器備份的相關資訊，請參閱[設定或自訂伺服器備份](Set-up-or-customize-server-backup.md)。  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理備份和還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
