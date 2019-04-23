---
title: 管理超融合部署基礎結構與 Windows Admin Center
description: 管理超融合部署基礎結構與 Windows Admin Center （專案檀香山）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 02/11/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4d849120d2daaa40cb797cc5e7d4c23c74da5bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874259"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>管理超融合部署基礎結構與 Windows Admin Center

>適用於：Windows Admin Center，Windows Admin Center 預覽

## <a name="what-is-hyper-converged-infrastructure"></a>什麼是 Hyper-Converged 基礎結構

軟體定義的計算、 儲存體和網路來連線到一個叢集以提供高效能、 符合成本效益，並可輕鬆擴充的虛擬化，則會合併超交集基礎結構。 這項功能在具有 Windows Server 2016 引進[儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)並[HYPER-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)。

> [!Tip]
> 要取得 Hyper-Converged 基礎結構嗎？ Microsoft 建議這些[Windows Server 軟體定義](https://microsoft.com/wssd)來自合作夥伴解決方案。 它們是設計、 組合和針對我們參考架構，以確保相容性和可靠性，讓您快速並執行驗證。

> [!IMPORTANT]
> 這篇文章中所述的功能有些只以 Windows Admin Center 預覽形式提供。 [如何取得這個版本？](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md)適用於 Windows Server，傳統的 「 現成 」 工具和伺服器管理員一樣的後續版本會為下一代的管理工具。 它是免費的可以安裝及使用沒有網際網路連線。 您可以使用 Windows Admin Center 來管理和監視執行 Windows Server 2016 或 Windows Server 2019 Insider Preview 組建的 Hyper-Converged 基礎結構。

![超交集叢集儀表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>重要功能

Windows Admin Center Hyper-Converged 基礎結構的重點包括：

- **統一的單一-窗格-的-玻璃計算、 儲存體和推出的網路。** 檢視您的虛擬機器、 主機伺服器、 磁碟區、 磁碟機，以及一個特殊用途、 一致、 互連的體驗中的多個。
- **建立和管理儲存空間 」 與 「 HYPER-V 虛擬機器。** 若要建立、 開啟、 調整大小，並刪除磁碟區; 徹底簡化工作流程和建立、 啟動、 連線，並移動虛擬機器;另外還有更多功能。
- **功能強大的全叢集監視。** 儀表板圖形記憶體和 CPU 使用量、 儲存體容量、 IOPS、 輸送量和延遲即時的方式，跨每一部伺服器在叢集中，清除警示時有問題。
- **軟體定義網路 (SDN) 支援。（新功能 Windows Admin Center 預覽）** 管理及監控虛擬網路，子網路、 虛擬機器連線至虛擬網路和監視 SDN 基礎結構。

Hyper-Converged 基礎結構的 Windows Admin Center 是由 Microsoft 主動開發。 它會接收頻繁的更新，改善現有功能，並加入新功能。

## <a name="before-you-start"></a>開始之前

若要以 Windows Admin Center Hyper-Converged 基礎結構中管理您的叢集，它必須執行 Windows Server 2016 或 Windows Server 2019，預覽組建，並為 HYPER-V 和儲存空間直接存取啟用。

> [!Tip]
> Windows Admin Center 也會提供一般用途的管理體驗 Windows Server 2012 和更新版本支援的任何工作負載，可以使用任何叢集。 如果這聽起來比較適合，像是 Windows Admin Center 中加入您的叢集時，選取[**容錯移轉叢集中**](manage-failover-clusters.md)而非**Hyper-Converged 叢集**。

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Windows Admin Center 準備您的 Windows Server 2016 叢集

Hyper-Converged 基礎結構的 Windows Admin Center 取決於 Windows Server 2016 發行之後，新增 Api 管理。 您可以管理 Windows Admin Center 與 Windows Server 2016 叢集之前，您將需要執行這兩個步驟：

1. 請確認已安裝在叢集中的每一部伺服器[2018年-05 適用於 Windows Server 2016 (KB4103723) 的累積更新](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)或更新版本。 若要下載並安裝此更新，請前往**設定** > **更新與安全性** > **Windows 更新**，然後選取**線上檢查來自 Microsoft Update 的更新**。
2. 在叢集上以系統管理員身分執行下列 PowerShell cmdlet:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 您只需要在叢集中的任何伺服器上一次，執行 cmdlet。 您可以在本機 Windows PowerShell 中執行，或使用認證安全性服務提供者 (CredSSP) 從遠端執行它。 根據您的設定，您可能無法執行此 cmdlet 從 Windows Admin Center 內。

> [!Important]
> 部署非英文地區設定中，有是防止儀表板載入 （僅限第一次） 的 Windows Admin Center 1804年版中的已知的問題。 因應措施是執行`Add-ClusterResource -Name 'SDDC Management' -Group 'Cluster Group' -ResourceType 'SDDC Management'`取代 *'叢集群組'* 使用的當地語系化名稱，例如 *'群組 du 叢集'* 以法文顯示。 將在下一次更新中解決這個問題。
>
> **更新：** 這現在已經修正 Windows Admin Center 預覽版 1806年。

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Windows Admin Center 準備您的 Windows Server 2019 叢集

如果您的叢集執行的 Windows Server 2019 Insider Preview 組建，上述步驟不是必要的。 只要將叢集新增至 Windows Admin Center，在下一節中所述，您都已就緒 ！ [下載最新的預覽組建的 Windows Server 2019](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)。

### <a name="configure-software-defined-networking-optional"></a>設定軟體定義網路 （選擇性） ###

您可以設定您的 Hyper-Converged 基礎結構，執行 Windows Server 2016 或 2019年軟體定義網路 (SDN) 使用下列步驟：

1. 準備的 VHD，也就是您在超交集基礎結構主機安裝相同作業系統的 os。 此 VHD 將用於 NC/SLB/GW 的所有 Vm。
2. 下載的所有資料夾和檔案在 SDN Express 從[ https://github.com/Microsoft/SDN/tree/master/SDNExpress ](https://github.com/Microsoft/SDN/tree/master/SDNExpress)。
3. 準備使用部署主控台的不同 VM。 此 VM 應該能夠存取 SDN 的主機。 此外，VM 也應該已安裝 RSAT HYPER-V 工具。
4. 複製您下載適用於 SDN Express 部署主控台 VM 的所有項目。 及分享此消息**SDNExpress**資料夾。 請確定每個主機可以存取**SDNExpress**共用資料夾中，所定義的組態檔的行 8 中：
```
    \\$env:Computername\SDNExpress
```
5. 複製的 os VHD**映像**下方的資料夾**SDNExpress**部署主控台 VM 上的資料夾。
6. 修改您的環境設定 SDN Express 組態。 修改您環境的資訊為基礎的 SDN Express 組態之後，請完成下列兩個步驟。
7. 執行 PowerShell 部署 SDN 的系統管理員權限：

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

部署將需要大約 30 – 45 分鐘的時間。

## <a name="get-started"></a>立即開始

Hyper-Converged 基礎結構部署之後，您可以將使用 Windows Admin Center 加以管理。

### <a name="install-windows-admin-center"></a>安裝 Windows Admin Center

如果您還沒有這麼做，下載並安裝 Windows Admin Center。 最快的方式來啟動並執行是將它安裝在您的 Windows 10 電腦，並從遠端管理您的伺服器。 這會帶少於五分鐘。 [立即下載](https://aka.ms/windowsadmincenter)或是[深入了解其他安裝選項](../deploy/install.md)。

### <a name="add-hyper-converged-cluster"></a>加入超交集叢集

若要新增您的叢集到 Windows Admin Center:

1. 按一下  **+ 新增**下所有連接。
2. 選擇新增**Hyper-Converged 叢集連線**。
3. 輸入叢集的名稱，如果出現提示，要使用的認證。
4. 按一下 **新增**才能完成。

叢集會新增至您的連線清單。 按一下以啟動儀表板。

![新增超交集叢集連線](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>新增 SDN 啟用超交集叢集 （Windows Admin Center 預覽）

最新的 Windows Admin Center 預覽版支援 Hyper-Converged 基礎結構軟體定義網路管理。 藉由新增至超交集叢集連線的網路控制站的 REST URI，您可以使用超交集叢集管理員來管理 SDN 資源和監視 SDN 基礎結構。

1. 按一下  **+ 新增**下所有連接。
2. 選擇新增**Hyper-Converged 叢集連線**。
3. 輸入叢集的名稱，如果出現提示，要使用的認證。
4. 請檢查**設定網路控制卡**以繼續。
5. 請輸入**網路控制站 URI**然後按一下**Validate**。
6. 按一下 **新增**才能完成。

叢集會新增至您的連線清單。 按一下以啟動儀表板。

![新增 SDN 啟用超交集叢集連線](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 使用 Northbound 通訊的 Kerberos 驗證的 SDN 環境目前不支援。

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019-insider-preview"></a>是否有任何差別管理 Windows Server 2016 和 Windows Server 2019 Insider Preview？

是的。 Hyper-Converged 基礎結構的 Windows Admin Center 接收頻繁的更新可改善 Windows Server 2016 和 Windows Server 2019 Insider Preview 的體驗。 不過，某些新功能僅適用於 Insider Preview – 比方說，重複資料刪除和壓縮的切換開關。

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>可以使用 Windows Admin Center 來管理儲存空間直接存取的其他使用案例 （不超融合式），例如交集的向外延展檔案伺服器 (SoFS) 或 Microsoft SQL Server 嗎？

Hyper-Converged 基礎結構的 Windows Admin Center 不提供管理或監視的選項，專為儲存空間直接存取的其他使用案例，例如，它無法建立檔案共用。 不過，儀表板和核心功能，例如建立磁碟區或更換磁碟機，適合於任何儲存空間直接存取叢集。

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>容錯移轉叢集和 Hyper-Converged 叢集之間的差異為何？

一般情況下，「 超聚合式 」 是指 HYPER-V 上執行和儲存空間直接存取相同的詞彙叢集化虛擬化運算和儲存體資源的伺服器。 中的 Windows Admin Center，當您按一下 內容 **+ 新增**從 連線 清單中，您可以選擇新增**容錯移轉叢集連線**或**Hyper-Converged 叢集連接**:

- **容錯移轉叢集連線**是容錯移轉叢集管理員的傳統型應用程式的後續版本。 它用於任何支援的任何工作負載，包括 Microsoft SQL Server 的叢集提供熟悉的一般用途的管理體驗。 它適用於 Windows Server 2012 和更新版本。

- **Hyper-Converged 叢集連線**全新的體驗量身打造的儲存空間直接存取和 HYPER-V。 它具有儀表板，並強調監控所用的圖表和警示。 適用於 Windows Server 2016 和 preview 組建的 Windows Server 2019。

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>為什麼需要 Windows Server 2016 的最新累計更新？

Hyper-Converged 基礎結構的 Windows Admin Center 取決於 Api 開發，因為 Windows Server 2016 發行的管理。 這些 Api 中新增[2018年-05 適用於 Windows Server 2016 (KB4103723) 的累積更新](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)，自 2018 5 月 8 日起，您可以使用。

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少費用？

Windows Admin Center 除了 Windows 本身以外，不需另付費用。

您可以使用不需要額外收費的有效授權的 Windows Server 或 Windows 10 中的 Windows Admin Center （提供個別下載）-Windows 增補使用者授權合約下授權。

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

資料分割

### <a name="does-it-require-an-internet-connection"></a>它需要網際網路連線嗎？

資料分割

雖然 Windows Admin Center 提供功能強大，因此方便整合 Microsoft Azure 雲端、 核心管理與監視體驗 Hyper-Converged 基礎結構完全在內部。 它可以安裝並使用沒有網際網路連線。

## <a name="things-to-try"></a>若要嘗試的事項

如果您剛開始使用，以下是一些快速教學課程可協助您了解 Hyper-Converged 基礎結構的 Windows Admin Center 組織的方式，並運作。 請執行良好的判斷，並謹慎使用生產環境。 這些影片所記錄的 Windows Admin Center 1804年版和 Windows Server 2019 Insider Preview 組建。

### <a name="manage-storage-spaces-direct-volumes"></a>管理儲存空間直接存取磁碟區

<ul>
               <li>(0:37)<a href="https://youtu.be/o66etKq70N8">如何建立三向鏡像磁碟區</a></li>
               <li>(1:17)<a href="https://youtu.be/R72QHudqWpE">如何建立鏡像加速同位檢查的磁碟區</a></li>
               <li>(1:02)<a href="https://youtu.be/j59z7ulohs4">如何開啟磁碟區，並將檔案新增</a></li>
               <li>(0:51)<a href="https://youtu.be/PRibTacyKko">如何開啟重複資料刪除和壓縮</a></li>
               <li>(0:47)<a href="https://youtu.be/hqyBzipBoTI">如何擴充磁碟區</a></li>
               <li>(0:26)<a href="https://youtu.be/DbjF8r2F6Jo">如何刪除磁碟區</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>建立磁碟區，三向鏡像</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>建立磁碟區，鏡像加速同位檢查</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>開啟磁碟區，並將檔案新增</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>開啟 重複資料刪除和壓縮</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>擴充磁碟區</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>刪除磁碟區</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>建立新的虛擬機器

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 在 [虛擬機器] 工具頂端，選擇**清查**索引標籤，然後按一下**新增**來建立新的虛擬機器。
3. 輸入虛擬機器名稱，然後選擇層代 1 和 2 的虛擬機器之間。
4. 您可以選擇哪一部主機一開始建立虛擬機器上，或使用的建議的主機。
5. 選擇虛擬機器檔案的路徑。 從下拉式清單中選擇的磁碟區，或按一下**瀏覽**選擇資料夾，使用資料夾選擇器。 虛擬機器設定檔和虛擬硬碟檔案會儲存在同一個資料夾下`\Hyper-V\[virtual machine name]`所選磁碟區或路徑的路徑。
6. 是否已啟用巢狀虛擬化，設定記憶體設定、 網路介面卡、 虛擬硬碟，然後選擇您要從.iso 映像檔，或從網路安裝作業系統，請選擇虛擬處理器的數目。
7. 按一下 [建立]，建立虛擬機器。
8. 一旦虛擬機器已建立，並會出現在虛擬機器清單，您可以啟動虛擬機器。
9. 虛擬機器啟動之後，您可以連線到透過 VMConnect 以安裝作業系統的虛擬機器的主控台。 從清單中選取虛擬機器，請按一下**更多** > **Connect**下載.rdp 檔案。 遠端桌面連線應用程式中開啟.rdp 檔案。 因為這連接到虛擬機器的主控台，您必須輸入 HYPER-V 主機的系統管理員認證。

[深入了解 Windows Admin Center 使用的虛擬機器管理](manage-virtual-machines.md)。

### <a name="pause-and-safely-restart-a-server"></a>暫停及安全地重新啟動伺服器

1. 從**儀表板**，選取**伺服器**從左手邊或按一下瀏覽**檢視伺服器 >** 連結中的儀表板右下角的圖格.
2. 在頂端，從切換**摘要**要**清查** 索引標籤。
3. 按一下以開啟其名稱來選取伺服器**Server**詳細資料頁面。
4. 按一下 **暫停伺服器以進行維護**。 如果它是安全地繼續進行，這會將虛擬機器到叢集中的其他伺服器。 伺服器將會有狀態耗盡而發生這種情況。 如果您想，您可以觀看移動的虛擬機器**虛擬機器 > 清查**與其主機伺服器清楚地顯示在方格中的頁面。 已移動的所有虛擬機器、 伺服器狀態將會**已暫停**。
5. 按一下 **管理伺服器**存取 Windows Admin Center 中的所有每個伺服器管理工具。
6. 按一下 **重新啟動**，然後**是**。 您將會開始重新連接清單。
7. 回到**儀表板**，伺服器會以紅色顯示，雖然它已關閉。
8. 一旦備份時，再次瀏覽**伺服器**頁面，然後按一下**繼續維護伺服器**設回只需最新的伺服器狀態。 在時間中，虛擬機器會移回 – 不不需要任何使用者動作。

### <a name="replace-a-failed-drive"></a>取代故障的磁碟機

1. 當磁碟機失敗時，警示會出現在左上方**警示**區域**儀表板**。
2. 您也可以選取**磁碟機**從左手的邊或按一下 瀏覽**檢視的磁碟機 >** 中瀏覽磁碟機，並瞭解其狀態的右下角的圖格的連結。 在 **清查**索引標籤上，方格支援排序、 分組和關鍵字搜尋。
3. 從**儀表板**，按一下警示，以查看詳細資料，例如磁碟機的實體位置。
4. 若要深入了解，請按一下**移至 磁碟機**捷徑**磁碟機**詳細資料頁面。
5. 如果您的硬體支援的話，您可以按一下**開啟/關閉燈光**來控制磁碟機的指示燈。
6. 儲存空間直接存取會自動淘汰及 evacuates 失敗的磁碟機。 已發生的時間磁碟機狀態已遭淘汰，且其儲存體容量列是空的。
7. 移除失敗的磁碟機並插入取代它。
8. 在 **磁碟機 > 清查**，會出現新的磁碟機。 在時間中，警示將會清除磁碟區將會回到 [確定] 狀態，修復和儲存體將會重新平衡到新的磁碟機，不不需要任何使用者動作。

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>管理虛擬網路 （SDN 啟用 HCI 叢集使用 「 Windows Admin Center 預覽）

1. 選取 **虛擬網路**從左側瀏覽。
2. 按一下 **的新**建立新的虛擬網路和子網路，或選擇現有的虛擬網路，然後按一下**設定**修改其組態。
3. 按一下 現有的虛擬網路，以檢視 VM 連線至虛擬網路子網路和存取控制清單套用至虛擬網路子網路。

![管理虛擬網路](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>虛擬機器連接至虛擬網路 （SDN 啟用 HCI 叢集使用 「 Windows Admin Center 預覽）

1. 選取 **虛擬機器**從左側瀏覽。
2. 選擇現有的虛擬機器 > 按一下 **設定**> 開啟**網路**索引標籤**設定**。
3. 設定**虛擬網路**並**虛擬子網路**欄位，以將虛擬機器連接至虛擬網路。

建立虛擬機器時，您也可以設定虛擬網路。

![連線到虛擬網路的虛擬機器](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>監視軟體定義網路基礎結構 （使用 Windows Admin Center 預覽版的 SDN 功能 HCI 叢集）

1. 選取  **SDN 監視**從左側瀏覽。
2. 檢視網路控制站、 軟體負載平衡器，虛擬閘道的健康情況的詳細的資訊，並監視您的虛擬閘道集區、 公用及私用 IP 集區的使用量和 SDN 主機狀態。

![監視 SDN 基礎結構](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>意見反應

其實完全在於您的意見反應 ！ 經常更新的最重要的優點是了解什麼行得通、 什麼需要改善。 以下是一些方法，讓我們知道讀者在想什麼：

- [提交並票選功能在 UserVoice 上的要求](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Microsoft 技術社群的 Windows Admin Center 論壇](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 要推文 `@servermgmt`

### <a name="see-also"></a>另請參閱

- [Windows Admin Center](../understand/windows-admin-center.md)
- [儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [軟體定義網路](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
