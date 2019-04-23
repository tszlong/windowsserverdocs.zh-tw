---
title: 使用儲存體移轉服務來移轉檔案伺服器
description: 搜尋引擎結果的主題的簡短描述
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 966f25eb0bd43513b3c544fb3dc97115ed668b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872749"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>若要移轉的伺服器使用儲存體移轉服務

本主題討論如何將伺服器，包括其檔案和設定，以使用另一部伺服器移轉[儲存體移轉服務](overview.md)和 Windows Admin Center。 一旦您已安裝服務，並開啟任何必要的防火牆連接埠移轉會採用三個步驟： 清查您的伺服器、 傳輸資料，以及移交到新的伺服器。

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>步驟 0:安裝儲存體移轉服務，並檢查防火牆連接埠

開始之前，請安裝存放裝置移轉服務，並確定必要的防火牆連接埠已開啟。

1. 請檢查[儲存體移轉服務需求](overview.md#requirements)並安裝[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)您的電腦上的管理伺服器，如果您還沒有這麼做。
2. 在 Windows Admin Center，連接到執行 Windows Server 2019 的 orchestrator 伺服器。 <br>這是您將在上安裝儲存體移轉服務，並使用來管理移轉的伺服器。 如果您要移轉只有一部伺服器，您可以使用目的地伺服器，只要它正在執行 Windows Server 2019。 我們建議您將個別的協調流程伺服器用於任何多伺服器的移轉。
1. 移至**伺服器管理員**（在 Windows Admin Center) >**儲存體移轉服務**，然後選取**安裝**安裝儲存體移轉服務和其必要的元件（圖 1 中顯示）。
    ![顯示 [安裝] 按鈕的 [儲存體移轉服務] 頁面的螢幕擷取畫面](media/migrate/install.png) **圖 1:安裝儲存體移轉服務**
1. 所有執行 Windows Server 2019 的目的地伺服器上安裝儲存體移轉服務 proxy。 這會增加時安裝在目的地伺服器上的傳輸速度。 <br>若要這樣做，請連接到 Windows Admin Center 以在目的地伺服器，然後移至**伺服器管理員**（在 Windows Admin Center) >**角色及功能**，選取**存放裝置移轉服務Proxy**，然後選取**安裝**。
1. 所有的來源伺服器和 Windows Admin Center，在執行 Windows Server 2012 R2 或 Windows Server 2016 中，任何目的地伺服器上連線到每一部伺服器，請移至**伺服器管理員**（在 Windows Admin Center) >**防火牆**  > **連入規則**，然後檢查 已啟用下列規則：
    - 檔案及印表機共用 (SMB-In)
    - Netlogon 服務 (Np-in)
    - Windows Management Instrumentation (Dcom-in)
    - Windows Management Instrumentation (WMI-In)

   > [!NOTE]
   > 如果您使用協力廠商防火牆，以開啟輸入連接埠範圍為 TCP/445 (SMB)、 TCP 135 （RPC/DCOM 端點對應程式，） 和 TCP 1025-65535 （RPC/DCOM 暫時連接埠）。

1. 如果您使用 orchestrator 伺服器來管理移轉，而且您想要下載事件或傳送的資料的記錄檔，請檢查該伺服器上，會啟用檔案及印表機共用 (Smb-in) 防火牆規則。

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>步驟 1：建立作業和清查您的伺服器以找出要移轉的項目

在此步驟中，您可以指定哪些伺服器移轉，並再掃描它們，以在其檔案和設定上收集資訊。

1. 選取 **新的工作**作業，命名，然後選取**確定**。
1. 在 **輸入認證**頁面上，輸入您想要移轉，在伺服器上運作，然後選取 系統管理員認證**下一步**。
1. 選取 **新增裝置**，輸入來源伺服器名稱，然後選取**確定**。 <br>重複此步驟的任何其他您想要清查的伺服器。
1. 選取 **開始掃描**。<br>當掃描完成後會顯示頁面更新。
    ![顯示伺服器準備好要掃描的螢幕擷取畫面](media/migrate/inventory.png) **圖 2:清查伺服器**
1. 選取要檢閱共用、 組態、 網路介面卡，以及清查到的磁碟區的每個伺服器。 <br><br>我們知道可能會干擾 Windows 作業，因此在此版本中，您會看到警告位於 Windows 系統資料夾中的任何共用的檔案或資料夾，將不會轉移儲存體移轉服務。 您必須在傳輸階段期間略過這些共用。 如需詳細資訊，請參閱 <<c0> [ 哪些檔案和資料夾排除傳輸](faq.md#excluded-files)。
1. 選取 [**下一步]** 移到傳輸資料。

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>步驟 2：將資料從舊伺服器轉移到目的地伺服器

在此步驟中，您可以傳輸資料之後指定要將它放在目的地伺服器上的位置。

 1. 上**將資料傳輸** > **輸入認證**頁面上，輸入系統管理員認證，在您想要移轉至目的地伺服器上運作，然後選取 **下一步**.
 1. 在上**新增目的地裝置和對應**列 頁面上，第一部來源伺服器。 輸入您想要移轉，然後選取 伺服器名稱**掃描裝置**。
 1. 將來源磁碟區對應到目的地磁碟區中，清除**Include**不要傳送 （包括任何系統管理共用位於 Windows 系統資料夾中），並接著選取的核取方塊的任何共用**下一步**.
    ![螢幕擷取畫面顯示的來源伺服器和磁碟區與共用，而且，它們會在目的地上轉移到](media/migrate/transfer.png) **圖 3:來源伺服器，並將其儲存體傳輸到**
 1. 新增目的地伺服器以及對應任何更多的來源伺服器，然後按**下一步**。
 1. 選擇性地調整 [傳送設定中，然後按**下一步]**。
 1. 選取 [ **Validate** ，然後選取**下一步]**。
 1. 選取 **開始傳輸**開始傳送資料。<br>第一次您傳輸時，我們將繼續任何現有的檔案在目的地中備份的資料夾。 後續的傳輸，依預設我們會更新目的地沒有備份第一個。 <br>此外，是聰明，足以應付重疊的共用存放裝置移轉服務 — 我們不會將同一個資料夾複製兩次相同的同一個作業。
 1. 傳輸完成後，查看目的地伺服器，以確定所有項目正確傳輸。 選取 **錯誤記錄檔僅**如果您想要下載記錄檔的未傳送任何檔案。

  > [!NOTE]
  > 如果您想要保留稽核記錄的傳輸，或打算執行一個以上的傳輸中的工作，請按一下**傳送記錄檔**来儲存 CSV 副本。 每個後續的傳輸會覆寫先前執行的資料庫資訊。 

此時，您會有三個選項：

- **移至下一個步驟**，以便在目的地伺服器採用的來源伺服器的身分識別，透過剪下。
- **請考慮在移轉完成**無須移轉來源伺服器的身分識別。
- **一次傳輸**，複製自最後一個傳輸已更新的檔案。

如果您的目標是要使用 Azure 檔案同步，您可以設定 Azure 檔案同步的目的地伺服器之後傳送檔案，或在移轉到目的地伺服器的領域 (請參閱[規劃 Azure 檔案同步部署](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning))。

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>步驟 3：選擇性地移交到新的伺服器

在此步驟中您可以移交從來源伺服器到目的地伺服器，移動到目的地伺服器的 IP 位址與電腦名稱。 完成此步驟之後，應用程式和使用者存取的名稱和您從移轉的伺服器位址透過新的伺服器。

 1. 如果您已離開移轉作業，在 Windows Admin Center，請移至**伺服器管理員** > **儲存體移轉服務**，然後選取您想要完成的作業。 
 1. 在 [**移交到新的伺服器** > **輸入認證**頁面上，選取**下一步]** 使用您先前輸入的認證。
 1. 在 **移交時設定**頁面上，指定的網路介面卡，才會於每個來源裝置的設定。 這會將移的 IP 位址從來源到目的地的轉換過程。
 1. 指定來源伺服器之後完全移轉移動其位址到目的地的使用哪些 IP 位址。 您可以使用 DHCP 或靜態位址。 如果使用靜態位址，新的子網路不得與舊的子網路相同或完全移轉將會失敗。
    ![螢幕擷取畫面顯示的來源伺服器和其 IP 位址和電腦名稱以及它們將會取代後完全移轉](media/migrate/cutover.png)
    **圖 4:來源伺服器和其網路組態將如何移動到目的地**
 1. 指定如何在目的地伺服器接管其名稱之後，請重新命名來源伺服器。 您可以使用隨機產生的名稱，或自行輸入。 然後選取**下一步**。
 1. 選取 **下一步**上**調整完全移轉設定**頁面。
 1. 選取 [ **Validate**上**驗證來源和目的地裝置**頁面，然後再選取**下一步]**。
 1. 當您準備好執行完全移轉時時，請選取**開始完全移轉**。 <br>使用者和應用程式時的位址和名稱會移動，並且在伺服器重新啟動數次，可能會中斷，但會受到移轉。 多久完全移轉的時間取決於如何快速地在伺服器重新啟動，以及 Active Directory 和 DNS 複寫時間。

## <a name="see-also"></a>另請參閱

- [儲存體移轉服務概觀](overview.md)
- [儲存體移轉服務常見問題集 (faq)](faq.md)
- [規劃 Azure 檔案同步部署](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)