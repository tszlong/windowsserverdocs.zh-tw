---
title: 使用 Windows Admin Center 管理超融合式基礎結構
description: '使用 Windows Admin Center (Project Honolulu 管理超融合式基礎結構) '
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.openlocfilehash: 56d953c721fff2218b256fa99d83078485438c0f
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90765969"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>使用 Windows Admin Center 管理超融合式基礎結構

>適用於：Windows Admin Center、Windows Admin Center 預覽版

## <a name="what-is-hyper-converged-infrastructure"></a>什麼是超融合式基礎結構

超融合式基礎結構可將軟體定義的計算、儲存體和網路功能合併成一個叢集，以提供高效能、符合成本效益且易於調整的虛擬化。 這項功能是在 Windows Server 2016 中引進 [儲存空間直接存取](../../../storage/storage-spaces/storage-spaces-direct-overview.md)、 [軟體定義的網路](../../../networking/sdn/software-defined-networking.md) 功能和 [hyper-v](../../../virtualization/hyper-v/hyper-v-on-windows-server.md)。

> [!Tip]
> 想要取得超融合式基礎結構嗎？ Microsoft 建議合作夥伴 [提供這些 Windows Server 軟體定義](https://microsoft.com/wssd) 的解決方案。 它們是針對我們的參考架構來設計、組合和驗證，以確保相容性和可靠性，讓您可以快速啟動並執行。

> [!IMPORTANT]
> 本文所述的部分功能僅適用于 Windows Admin Center 預覽版。 [如何? 取得這個版本嗎？](../overview.md)

## <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center

[Windows Admin Center](../overview.md) 是新一代的 Windows Server 管理工具，也是傳統「現成」工具（例如伺服器管理員）的後續版本。 它是免費的，而且可以在沒有網際網路連線的情況下安裝和使用。 您可以使用 Windows Admin Center 來管理和監視執行 Windows Server 2016 或 Windows Server 2019 的超融合式基礎結構。

![超交集叢集儀表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>主要功能

超融合式基礎結構 Windows Admin Center 的重點包括：

- **適用于計算、儲存和即將推出網路的整合單一窗格。** 在單一用途、一致的互連體驗中，查看您的虛擬機器、主機伺服器、磁片區、磁片磁碟機等等。
- **建立及管理儲存空間和 Hyper-v 虛擬機器。** 完全簡單的工作流程，以建立、開啟、調整大小和刪除磁片區;以及建立、啟動、連接和移動虛擬機器;還有更多。
- **功能強大的全叢集監視。** 儀表板會在叢集中的每一部伺服器上，即時圖形記憶體和 CPU 使用量、儲存體容量、IOPS、輸送量和延遲，並在不正確的情況下使用清楚的警示。
- **軟體定義的網路功能 (SDN) 支援。** 管理和監視虛擬網路、子網、將虛擬機器連線至虛擬網路，以及監視 SDN 基礎結構。

Microsoft 正在積極開發超融合式基礎結構的 Windows Admin Center。 它會接收頻繁的更新，以改善現有的功能並新增新功能。

## <a name="before-you-start"></a>開始之前

若要在 Windows Admin Center 中將叢集管理為超融合式基礎結構，必須執行 Windows Server 2016 或 Windows Server 2019，並啟用 Hyper-v 和儲存空間直接存取。 此外，也可以透過 Windows Admin Center 啟用並管理軟體定義的網路功能。

> [!Tip]
> Windows Admin Center 也針對支援任何工作負載的叢集（適用于 Windows Server 2012 和更新版本）提供一般用途的管理體驗。 如果這聽起來更適合，當您將叢集新增至 Windows Admin Center 時，請選取 [**容錯移轉**](manage-failover-clusters.md) 叢集，而不是 **超**交集叢集。

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>準備 Windows Server 2016 叢集以進行 Windows Admin Center

超交集基礎結構的 Windows Admin Center 取決於發行 Windows Server 2016 之後新增的管理 Api。 使用 Windows Admin Center 管理 Windows Server 2016 叢集之前，您必須執行下列兩個步驟：

1. 確認叢集中的每一部伺服器都已安裝 [Windows server 2016 的2018-05 累積更新 (KB4103723) ](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) 或更新版本。 若要下載並安裝此更新，請移至 [**設定**  >  **更新 & 安全性**  >  **Windows Update** ]，然後選取 [**線上檢查來自 Microsoft Update 的更新**]。
2. 以系統管理員身分在叢集中執行下列 PowerShell Cmdlet：

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 在叢集中的任何伺服器上，您只需要執行一次 Cmdlet。 您可以在 Windows PowerShell 中于本機執行，或使用 (CredSSP) 的認證安全性服務提供者，從遠端執行。 視您的設定而定，您可能無法從 Windows Admin Center 中執行此 Cmdlet。

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>準備 Windows Server 2019 叢集以進行 Windows Admin Center

如果您的叢集執行 Windows Server 2019，則不需要上述步驟。 只要將叢集新增至 Windows Admin Center （如下一節所述），您就可以開始了！

### <a name="configure-software-defined-networking-optional"></a>設定軟體定義網路 (選用)  ###

您可以使用下列步驟，將執行 Windows Server 2016 或2019的超融合式基礎結構設定為使用軟體定義網路 (SDN) ：

1. 準備作業系統的 VHD，也就是您在超融合式基礎結構主機上安裝的作業系統。 此 VHD 將用於所有 NC/SLB/GW Vm。
2. 從下載 SDN Express 底下的所有資料夾和檔案 [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress) 。
3. 使用部署主控台準備不同的 VM。 此 VM 應能存取 SDN 主機。 此外，VM 應已安裝 RSAT Hyper-v 工具。
4. 將您為 SDN Express 下載的所有專案複製到部署主控台 VM。 並共用此 **SDNExpress** 資料夾。 請確定每個主機都可以存取 **SDNExpress** 共用資料夾，如設定檔行8所定義：
   ```
    \\$env:Computername\SDNExpress
   ```
5. 將作業系統的 VHD 複製到部署主控台 VM 上 [ **SDNExpress** ] 資料夾下的 [ **images** ] 資料夾。
6. 使用您的環境設定修改 SDN Express 設定。 根據您的環境資訊修改 SDN 快速設定之後，請完成下列兩個步驟。
7. 以系統管理員許可權執行 PowerShell 以部署 SDN：

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose
```

部署需要大約30–45分鐘。

## <a name="get-started"></a>開始使用

部署超交集基礎結構之後，您就可以使用 Windows Admin Center 來管理它。

### <a name="install-windows-admin-center"></a>安裝 Windows Admin Center

如果您尚未下載並安裝 Windows Admin Center，請先下載並安裝。 啟動並執行最快的方式是將它安裝在 Windows 10 電腦上，並從遠端系統管理您的伺服器。 這需要的時間不超過五分鐘。 [立即下載](../overview.md) ，或 [深入瞭解其他安裝選項](../deploy/install.md)。

### <a name="add-hyper-converged-cluster"></a>新增超交集叢集

若要將您的叢集新增至 Windows Admin Center：

1. 按一下 [所有連接] 底下的 [ **+ 新增** ]。
2. 加入宣告超交集叢集 **連接**。
3. 輸入叢集的名稱，並在出現提示時，輸入要使用的認證。
4. 按一下 [ **新增** ] 以完成。

叢集將會新增到您的連接清單。 按一下以啟動儀表板。

![新增超交集叢集連接](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>將已啟用 SDN 的超融合式叢集 (Windows Admin Center Preview) 

最新的 Windows Admin Center 預覽版針對超融合式基礎結構支援軟體定義的網路管理。 藉由將網路控制站 REST URI 新增至超交集叢集連接，您可以使用超交集叢集管理員來管理 SDN 資源和監視 SDN 基礎結構。

1. 按一下 [所有連接] 底下的 [ **+ 新增** ]。
2. 加入宣告超交集叢集 **連接**。
3. 輸入叢集的名稱，並在出現提示時，輸入要使用的認證。
4. 核取 **[設定網路控制** 站繼續]。
5. 輸入 **網路控制站 URI** ，然後按一下 [ **驗證**]。
6. 按一下 [ **新增** ] 以完成。

叢集將會新增到您的連接清單。 按一下以啟動儀表板。

![新增 SDN 啟用的超交集叢集連接](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 目前不支援具有 Northbound 通訊 Kerberos 驗證的 SDN 環境。

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>管理 Windows Server 2016 和 Windows Server 2019 有何不同？

可以。 超融合式基礎結構的 Windows Admin Center 接收頻繁的更新，以改善 Windows Server 2016 和 Windows Server 2019 的體驗。 不過，某些新功能只適用于 Windows Server 2019 –例如，切換重復資料刪除和壓縮。

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>我可以使用 Windows Admin Center 來管理其他使用案例的儲存空間直接存取 (非超交集) ，例如交集向外延展檔案伺服器 (SoFS) 或 Microsoft SQL Server？

超融合式基礎結構的 Windows Admin Center 不會特別針對儲存空間直接存取的其他使用案例提供管理或監視選項，例如，它無法建立檔案共用。 不過，儀表板和核心功能（例如建立磁片區或更換磁片磁碟機）適用于任何儲存空間直接存取叢集。

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>容錯移轉叢集和超交集叢集之間有何差異？

一般而言，「超交集」一詞指的是在相同的叢集伺服器上執行 Hyper-v 和儲存空間直接存取，以虛擬化計算和儲存體資源。 在 Windows Admin Center 的內容中，當您按一下 [連接] 清單中的 [ **+ 新增** ] 時，可以選擇新增 **容錯移轉叢集連接** 或超交集叢集 **連接**：

- **容錯移轉**叢集連線是容錯移轉叢集管理員傳統型應用程式的後續版本。 它可為任何支援任何工作負載的叢集（包括 Microsoft SQL Server）提供熟悉的一般用途管理體驗。 它適用于 Windows Server 2012 和更新版本。

- **超融合**式叢集連線是針對儲存空間直接存取和 hyper-v 量身打造的全新體驗。 它具有儀表板，並強調監控所用的圖表和警示。 它適用于 Windows Server 2016 和 Windows Server 2019。

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>為什麼我需要最新的 Windows Server 2016 累積更新？

超交集基礎結構的 Windows Admin Center 取決於自 Windows Server 2016 發行以來開發的管理 Api。 這些 Api 已新增至 [2018-05 的累積更新（適用于 Windows Server 2016 (KB4103723) ](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)，從2018到5月8日可用）。

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少費用？

Windows Admin Center 除了 Windows 本身以外，不需另付費用。

您可以在 Windows Server 或 Windows 10 的有效授權下免費使用 Windows Admin Center (可供個別下載)，這已依據 Windows 增補 EULA 取得授權。

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

不可以。

### <a name="does-it-require-an-internet-connection"></a>是否需要網際網路連線？

不會。

雖然 Windows Admin Center 可與 Microsoft Azure 雲端提供強大且方便的整合，但超融合式基礎結構的核心管理和監視體驗完全是在內部部署。 您可以在沒有網際網路連線的情況下安裝和使用它。

## <a name="things-to-try"></a>可以嘗試的方法

如果您剛開始使用，以下是一些快速教學課程，可協助您瞭解如何組織和運作超融合式基礎結構的 Windows Admin Center。 請在生產環境中執行良好的判斷來並小心。 這些影片記錄在 Windows Admin Center 1804 版和 Windows Server 2019 的 Insider preview 組建中。

### <a name="manage-storage-spaces-direct-volumes"></a>管理儲存空間直接存取磁片區

<ul>
               <li> (0:37) <a href="https://youtu.be/o66etKq70N8">如何建立三向鏡像磁碟區</a></li>
               <li> (1:17) <a href="https://youtu.be/R72QHudqWpE">如何建立鏡像加速同位磁片</a>區</li>
               <li> (1:02) <a href="https://youtu.be/j59z7ulohs4">如何開啟磁片區並新增</a>檔案</li>
               <li> (0:51) <a href="https://youtu.be/PRibTacyKko">如何開啟重復資料刪除和壓縮</a></li>
               <li> (0:47) <a href="https://youtu.be/hqyBzipBoTI">如何擴充磁片</a>區</li>
               <li> (0:26) <a href="https://youtu.be/DbjF8r2F6Jo">如何刪除磁片</a>區</li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>建立磁片區、三向鏡像</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>建立磁片區、鏡像加速同位</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>開啟磁片區並新增檔案</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>開啟重復資料刪除和壓縮</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>展開磁片區</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>刪除磁片區</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>建立新的虛擬機器

1. 從左側導覽窗格中，按一下 [ **虛擬機器** ] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [ **清查** ] 索引標籤，然後按一下 [ **新增** ] 以建立新的虛擬機器。
3. 輸入虛擬機器名稱，並在第1代和第2代虛擬機器之間選擇。
4. 您接著可以選擇要一開始建立虛擬機器的主機，或使用建議的主機。
5. 選擇虛擬機器檔案的路徑。 從下拉式清單中選擇磁片區，或按一下 **[流覽]** 以使用資料夾選擇器選擇資料夾。 虛擬機器組態檔和虛擬硬碟檔案會儲存在 `\Hyper-V\[virtual machine name]` 所選磁片區或路徑的路徑底下的單一資料夾中。
6. 選擇虛擬處理器數目、是否要啟用嵌套虛擬化、設定記憶體設定、網路介面卡、虛擬硬碟，以及選擇要從 .iso 映像檔案或從網路安裝作業系統。
7. 按一下 [建立]****，建立虛擬機器。
8. 一旦建立虛擬機器並顯示在虛擬機器清單中，您就可以啟動虛擬機器。
9. 啟動虛擬機器之後，您可以透過 VMConnect 連接至虛擬機器的主控台，以安裝作業系統。 從清單中選取虛擬機器，按一下 [**其他**連線  >  **]** 下載 .rdp 檔案。 在遠端桌面連線應用程式中開啟 .rdp 檔案。 因為這是連接到虛擬機器的主控台，所以您必須輸入 Hyper-v 主機的系統管理員認證。

[深入瞭解使用 Windows Admin Center 的虛擬機器管理](manage-virtual-machines.md)。

### <a name="pause-and-safely-restart-a-server"></a>暫停並安全地重新開機伺服器

1. 從 **儀表板**中，選取左側導覽中的 [ **伺服器** ]，或按一下儀表板右下角磚上的 [ **視圖伺服器] >**  連結。
2. 在頂端，從 [ **摘要** ] 切換至 [ **清查** ] 索引標籤。
3. 若要選取伺服器，請按一下其名稱以開啟 [ **伺服器** 詳細資料] 頁面。
4. 按一下 [ **暫停伺服器以進行維護**]。 如果可以安全地繼續進行，這會將虛擬機器移到叢集中的其他伺服器。 在這種情況下，伺服器會有狀態清空。 如果您想要的話，可以在 [ **虛擬機器 > 清查** ] 頁面上觀看虛擬機器的移動，其中的主機伺服器會清楚地顯示在方格中。 當所有虛擬機器都已移動後，伺服器狀態將會 **暫停**。
5. 按一下 [ **管理伺服器** ]，以存取 Windows Admin Center 中的所有每一伺服器管理工具。
6. 按一下 [ **重新開機**]，然後按一下 **[是]**。 您將會回到連接清單。
7. 回到 **儀表板**上，伺服器會在關閉時標示為紅色。
8. 備份之後，請再次流覽 **伺服器** 頁面，然後按一下 [ **從維護恢復伺服器** ]，將伺服器狀態設回 [完全啟動]。 在這段時間內，虛擬機器將會移回-使用者無須採取任何動作。

### <a name="replace-a-failed-drive"></a>更換故障的磁片磁碟機

1. 當磁片磁碟機失敗時，**儀表板**的左上方**警示**區域中會出現警示。
2. 您也可以從左側導覽中選取 **磁片磁碟機** ，或按一下右下角磚上的 [ **VIEW 磁片磁碟機] >** 連結，以流覽磁片磁碟機並查看其狀態。 在 [ **清查** ] 索引標籤中，方格支援排序、分組和關鍵字搜尋。
3. 從 **儀表板**中，按一下警示以查看詳細資料，例如磁片磁碟機的實體位置。
4. 若要深入瞭解，請按一下 [ **移** 至 **磁片磁碟機的詳細資料]** 頁面的快捷方式。
5. 如果您的硬體支援它，您可以按一下 [ **開啟亮/off** ] 來控制磁片磁碟機的指示器燈。
6. 儲存空間直接存取會自動淘汰並清除失敗的磁片磁碟機。 發生這種情況時，磁片磁碟機狀態會被淘汰，而且其儲存容量列是空的。
7. 移除失敗的磁片磁碟機，並插入其取代。
8. 在 **> 清查的磁片磁碟機**中，將會出現新的磁片磁碟機。 在這段時間內，警示將會清除，磁片區將會修復至 [確定] 狀態，而儲存體會重新平衡至新的磁片磁碟機–使用者無須動作。

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>使用 Windows Admin Center Preview) 管理虛擬網路 (SDN 啟用的 HCI 叢集

1. 從左側導覽中選取 [ **虛擬網路** ]。
2. 按一下 [ **新增** ] 以建立新的虛擬網路和子網，或選擇現有的虛擬網路，然後按一下 [設定] 來修改其 **設定** 。
3. 按一下現有的虛擬網路，以查看虛擬網路子網和套用至虛擬網路子網的存取控制清單的 VM 連線。

![管理虛擬網路](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>使用 Windows Admin Center Preview) 將虛擬機器連線至虛擬網路 (SDN 啟用的 HCI 叢集

1. 從左側導覽中選取 [ **虛擬機器** ]。
2. 選擇現有的虛擬機器 > 按一下 [**設定**] > 開啟 [**設定**] 中的 [**網路**] 索引標籤。
3. 設定 **虛擬網路** 和 **虛擬子網** 欄位，以將虛擬機器連線至虛擬網路。

您也可以在建立虛擬機器時設定虛擬網路。

![將虛擬機器連線至虛擬網路](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>使用 Windows Admin Center Preview) 監視軟體定義的網路基礎結構 (SDN 啟用的 HCI 叢集

1. 從左側導覽選取 [ **SDN 監視** ]。
2. 查看網路控制站、軟體 Load Balancer、虛擬閘道健全狀況的詳細資訊，並監視虛擬閘道集區、公用和私人 IP 集區使用方式和 SDN 主機狀態。

![監視 SDN 基礎結構](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="give-us-feedback"></a>提供意見反應

全都是您的意見反應！ 頻繁更新最重要的優點是聽取哪些工作和需要改進的功能。 以下是一些讓我們知道您所想要的方式：

- [在 UserVoice 上提交及投票功能要求](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Microsoft Tech 團體的 Windows Admin Center 論壇](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 推文至 `@servermgmt`

### <a name="additional-references"></a>其他參考資料

- [Windows Admin Center](../overview.md)
- [儲存空間直接存取](../../../storage/storage-spaces/storage-spaces-direct-overview.md)
- [Hyper-V](../../../virtualization/hyper-v/hyper-v-on-windows-server.md)
- [軟體定義網路](../../../networking/sdn/software-defined-networking.md)