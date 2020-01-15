---
title: 使用 Windows 管理中心管理超融合式基礎結構
description: 使用 Windows 管理中心管理超融合式基礎結構（Project 檀香山）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6795464bfbadd12fc220e941ad2175eb83d0f050
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949948"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>使用 Windows 管理中心管理超融合式基礎結構

>適用於：Windows Admin Center、Windows Admin Center 預覽版

## <a name="what-is-hyper-converged-infrastructure"></a>什麼是超融合式基礎結構

超融合式基礎結構會將軟體定義的計算、儲存體和網路功能合併成一個叢集，以提供高效能、符合成本效益且可輕鬆調整的虛擬化。 這項功能是在 Windows Server 2016 中引進[儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)、[軟體定義的網路](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)功能和[hyper-v](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)。

> [!Tip]
> 想要取得超融合的基礎結構嗎？ Microsoft 建議合作夥伴[提供這些 Windows Server 軟體定義的](https://microsoft.com/wssd)解決方案。 它們會針對我們的參考架構進行設計、組裝和驗證，以確保相容性和可靠性，讓您快速啟動並執行。

> [!IMPORTANT]
> 本文所述的部分功能僅適用于 Windows 管理中心預覽。 [如何? 取得此版本嗎？](https://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center

[Windows 管理中心](../overview.md)是新一代的 windows Server 管理工具，是傳統「現成」工具的後續功能，例如伺服器管理員。 這是免費的，而且可以在沒有網際網路連線的情況下安裝及使用。 您可以使用 Windows 系統管理中心來管理和監視執行 Windows Server 2016 或 Windows Server 2019 的超融合式基礎結構。

![超交集叢集儀表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>重要功能

適用于超融合式基礎結構的 Windows 系統管理中心重點包括：

- **適用于計算、儲存和很快網路的統一單一窗格。** 您的虛擬機器、主機伺服器、磁片區、磁片磁碟機等等，都是在一個專門建立、一致、互連的體驗內。
- **建立和管理儲存空間和 Hyper-v 虛擬機器。** 建立、開啟、調整大小和刪除磁片區的徹底簡單工作流程;建立、啟動、連接及移動虛擬機器;還有更多。
- **功能強大的全叢集監視。** 儀表板會在叢集中的每一部伺服器上即時繪製記憶體和 CPU 使用量、儲存容量、IOPS、輸送量和延遲，並在某些情況下清除警示。
- **軟體定義網路（SDN）支援。** 管理和監視虛擬網路、子網、將虛擬機器連線至虛擬網路，以及監視 SDN 基礎結構。

適用于超融合基礎結構的 Windows 系統管理中心正由 Microsoft 積極開發。 它會接收經常更新，以改善現有的功能並新增功能。

## <a name="before-you-start"></a>開始之前

若要在 Windows 管理中心將您的叢集管理為超融合式基礎結構，它必須執行 Windows Server 2016 或 Windows Server 2019，並啟用 Hyper-v 和儲存空間直接存取。 此外，它也可以透過 Windows 管理中心啟用並管理軟體定義的網路功能。

> [!Tip]
> Windows 管理中心也提供一般用途的管理體驗，適用于任何支援任何工作負載的叢集，適用于 Windows Server 2012 和更新版本。 如果這聽起來更適合，當您將叢集新增至 Windows 系統管理中心時，請選取 [[**容錯移轉**](manage-failover-clusters.md)叢集]，而不是 [**超融合式**叢集]。

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>為 Windows 系統管理中心準備您的 Windows Server 2016 叢集

超融合式基礎結構的 Windows 系統管理中心取決於 Windows Server 2016 發行後新增的管理 Api。 您必須先執行下列兩個步驟，才能使用 Windows 管理中心管理 Windows Server 2016 叢集：

1. 請確認叢集中的每部伺服器都已安裝[Windows server 2016 （KB4103723）](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)或更新版本的2018-05 累積更新。 若要下載並安裝此更新，請移至 [**設定**] > **更新 & 安全性** > **Windows Update** ，然後選取 [**從線上檢查來自 Microsoft Update 的更新**]。
2. 以系統管理員身分在叢集上執行下列 PowerShell Cmdlet：

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 您只需要在叢集中的任何伺服器上執行 Cmdlet 一次。 您可以在 Windows PowerShell 中本機執行，或使用認證安全性服務提供者（CredSSP）從遠端執行。 根據您的設定而定，您可能無法從 Windows 系統管理中心執行此 Cmdlet。

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>為 Windows 系統管理中心準備您的 Windows Server 2019 叢集

如果您的叢集執行 Windows Server 2019，則不需要上述步驟。 只要將叢集新增到 Windows 管理中心，如下一節中所述，您就可以開始了！

### <a name="configure-software-defined-networking-optional"></a>設定軟體定義網路功能（選擇性） ###

您可以使用下列步驟，將執行 Windows Server 2016 或2019的超融合式基礎結構設定為使用軟體定義網路功能（SDN）：

1. 準備 OS 的 VHD，這是您在超融合式基礎結構主機上安裝的相同作業系統。 此 VHD 將用於所有 NC/SLB/GW Vm。
2. 從[https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress)下載 SDN Express 下的所有資料夾和檔案。
3. 使用部署主控台準備不同的 VM。 此 VM 應該能夠存取 SDN 主機。 此外，VM 應已安裝 RSAT Hyper-v 工具。
4. 將您為 SDN Express 下載的所有內容複寫到部署主控台 VM。 並共用此**SDNExpress**資料夾。 請確定每個主機都可以存取**SDNExpress**共用資料夾，如設定檔行8中所定義：
   ```
    \\$env:Computername\SDNExpress
   ```
5. 將 OS 的 VHD 複製到部署主控台 VM 上 [ **SDNExpress** ] 資料夾下的 [ **images** ] 資料夾。
6. 在您的環境設定中修改 SDN Express 設定。 根據您的環境資訊修改 SDN 快速設定之後，完成下列兩個步驟。
7. 以系統管理員許可權執行 PowerShell 以部署 SDN：

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

部署大約需要30–45分鐘的時間。

## <a name="get-started"></a>入門

一旦部署超融合式基礎結構之後，您就可以使用 Windows 系統管理中心來管理它。

### <a name="install-windows-admin-center"></a>安裝 Windows Admin Center

如果您還沒有這麼做，請下載並安裝 Windows 系統管理中心。 啟動並執行的最快方式是將它安裝在您的 Windows 10 電腦上，並從遠端系統管理您的伺服器。 這只需要不到五分鐘的時間。 [立即下載](https://aka.ms/windowsadmincenter)或[深入瞭解其他安裝選項](../deploy/install.md)。

### <a name="add-hyper-converged-cluster"></a>新增超融合式叢集

若要將您的叢集新增至 Windows 系統管理中心：

1. 按一下 [所有連接] 底下的 [ **+ 新增**]。
2. 選擇新增**超融合**叢集連線。
3. 輸入叢集的名稱，如果出現提示，則為要使用的認證。
4. 按一下 [**新增**] 完成。

叢集將會新增至您的連線清單。 按一下以啟動儀表板。

![新增超交集叢集連接](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>新增具有 SDN 功能的超交集叢集（Windows 管理中心預覽）

最新的 Windows 管理中心預覽支援對超融合式基礎結構進行軟體定義的網路管理。 藉由將網路控制站 REST URI 新增至您的超交集叢集連線，您可以使用超融合式叢集管理員來管理 SDN 資源，以及監視 SDN 基礎結構。

1. 按一下 [所有連接] 底下的 [ **+ 新增**]。
2. 選擇新增**超融合**叢集連線。
3. 輸入叢集的名稱，如果出現提示，則為要使用的認證。
4. 勾選 **[設定網路控制**站繼續]。
5. 輸入**網路控制站 URI** ，然後按一下 [**驗證**]。
6. 按一下 [**新增**] 完成。

叢集將會新增至您的連線清單。 按一下以啟動儀表板。

![新增 SDN 功能的超融合式叢集連接](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 目前不支援使用 Kerberos 驗證進行 Northbound 通訊的 SDN 環境。

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>管理 Windows Server 2016 和 Windows Server 2019 有何差異？

可以。 適用于超融合基礎結構的 Windows 系統管理中心會接收經常更新，以改善 Windows Server 2016 和 Windows Server 2019 的體驗。 不過，某些新功能僅適用于 Windows Server 2019 –例如，用於重復資料刪除和壓縮的切換參數。

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>我可以使用 Windows 系統管理中心來管理其他使用案例（非超融合式）的儲存空間直接存取，例如交集向外延展檔案伺服器（SoFS）或 Microsoft SQL Server？

超融合式基礎結構的 Windows 系統管理中心不會特別針對儲存空間直接存取的其他使用案例提供管理或監視選項–例如，它無法建立檔案共用。 不過，儀表板和核心功能（例如建立磁片區或更換磁片磁碟機）適用于任何儲存空間直接存取叢集。

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>容錯移轉叢集與超融合叢集之間的差異為何？

一般來說，「超交集」一詞指的是在相同的叢集伺服器上執行 Hyper-v 和儲存空間直接存取，以虛擬化計算和儲存體資源。 在 Windows 系統管理中心的內容中，當您按一下連線清單中的 [ **+ 新增**] 時，您可以選擇新增**容錯移轉**叢集連線或超交集叢集**連接**：

- **容錯移轉**叢集連線是容錯移轉叢集管理員桌面應用程式的後繼。 它為任何支援任何工作負載的叢集（包括 Microsoft SQL Server）提供了熟悉、一般用途的管理體驗。 適用于 Windows Server 2012 和更新版本。

- **超融合**式叢集連線是針對儲存空間直接存取和 hyper-v 量身打造的全新體驗。 它具有儀表板，並強調監控所用的圖表和警示。 適用于 Windows Server 2016 和 Windows Server 2019。

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>為什麼我需要最新的 Windows Server 2016 累積更新？

超融合式基礎結構的 Windows 系統管理中心取決於 Windows Server 2016 發行後所開發的管理 Api。 這些 Api 會新增至[Windows Server 2016 （KB4103723）的2018-05 累積更新（](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)從5月8日2018）。

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少費用？

Windows Admin Center 除了 Windows 本身以外，不需另付費用。

您可以在 Windows Server 或 Windows 10 的有效授權下免費使用 Windows Admin Center (可供個別下載)，這已依據 Windows 增補 EULA 取得授權。

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

不。

### <a name="does-it-require-an-internet-connection"></a>是否需要網際網路連線？

不。

雖然 Windows 系統管理中心提供與 Microsoft Azure 雲端的強大且便利的整合，但超融合基礎結構的核心管理和監視體驗完全在內部部署。 可以在沒有網際網路連線的情況下安裝及使用。

## <a name="things-to-try"></a>可以嘗試的方法

如果您剛開始使用，以下是一些快速教學課程，可協助您瞭解適用于超融合式基礎結構的 Windows 系統管理中心如何組織和運作。 請執行良好的 judgement，並小心使用生產環境。 這些影片已記錄在 Windows 管理中心1804版和 Windows Server 2019 的 Insider preview 組建中。

### <a name="manage-storage-spaces-direct-volumes"></a>管理儲存空間直接存取磁片區

<ul>
               <li>（0:37）<a href="https://youtu.be/o66etKq70N8">如何建立三向鏡像磁片</a>區</li>
               <li>（1:17）<a href="https://youtu.be/R72QHudqWpE">如何建立鏡像加速同位磁片</a>區</li>
               <li>（1:02）<a href="https://youtu.be/j59z7ulohs4">如何開啟磁片區並新增</a>檔案</li>
               <li>（0:51）<a href="https://youtu.be/PRibTacyKko">如何開啟重復資料刪除和壓縮</a></li>
               <li>（0:47）<a href="https://youtu.be/hqyBzipBoTI">如何擴充磁片</a>區</li>
               <li>（0:26）<a href="https://youtu.be/DbjF8r2F6Jo">如何刪除磁片</a>區</li>
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
            <strong>開啟磁片區並新增</strong>檔案
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>開啟重復資料刪除和壓縮</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>展開磁片</strong>區
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>刪除磁片</strong>區
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>建立新的虛擬機器

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**清查**] 索引標籤，然後按一下 [**新增**] 以建立新的虛擬機器。
3. 輸入虛擬機器名稱，然後選擇 [第1代] 和 [2 部] 虛擬機器。
4. 您可以接著選擇要一開始建立虛擬機器的主機，或使用建議的主機。
5. 選擇虛擬機器檔案的路徑。 從下拉式清單中選擇磁片區，或按一下 **[流覽]** 以選擇使用資料夾選擇器的資料夾。 虛擬機器組態檔和虛擬硬碟檔案會儲存在所選磁片區或路徑的 `\Hyper-V\[virtual machine name]` 路徑底下的單一資料夾中。
6. 選擇虛擬處理器數目，不論您是否要啟用嵌套虛擬化、設定記憶體設定、網路介面卡、虛擬硬碟，以及選擇要從 .iso 映像檔案或從網路安裝作業系統。
7. 按一下 [建立]，建立虛擬機器。
8. 一旦建立虛擬機器並出現在虛擬機器清單中，您就可以啟動虛擬機器。
9. 虛擬機器啟動之後，您可以透過 VMConnect 連線至虛擬機器的主控台，以安裝作業系統。 從清單中選取虛擬機器，按一下 [**更多** > **連接]** 以下載 .rdp 檔案。 在遠端桌面連線應用程式中開啟 .rdp 檔案。 因為這是連接到虛擬機器的主控台，所以您必須輸入 Hyper-v 主機的系統管理員認證。

[深入瞭解 Windows 管理中心的虛擬機器管理](manage-virtual-machines.md)。

### <a name="pause-and-safely-restart-a-server"></a>暫停並安全地重新開機伺服器

1. 從 [**儀表板**] 中，從左側導覽中選取 [**伺服器**]，或按一下儀表板右下角磚上的 [**查看伺服器 >** ] 連結。
2. 在頂端，從 [**摘要**] 切換至 [**清查**] 索引標籤。
3. 按一下伺服器的名稱以開啟 [**伺服器**詳細資料] 頁面，即可加以選取。
4. 按一下 [**暫停伺服器以進行維護**]。 如果可以安全地繼續進行，這會將虛擬機器移至叢集中的其他伺服器。 當此情況發生時，伺服器將會清空狀態。 如有需要，您可以觀賞虛擬機器在 [ **> 清查的虛擬機器**] 頁面上移動，其中的主機伺服器會清楚地顯示在方格中。 當所有虛擬機器都移動之後，伺服器狀態將會是 [已**暫停**]。
5. 按一下 [**管理伺服器**] 以存取 Windows 系統管理中心內的所有伺服器管理工具。
6. 依序按一下 [**重新開機** **] 和 [是]** 。 您將會回到 [連接] 清單。
7. 回到**儀表板**上，伺服器在關閉時，會以紅色標示。
8. 備份之後，再次流覽**伺服器**頁面，然後按一下 [**從維護繼續伺服器**]，將伺服器狀態設定回簡單的。 虛擬機器會在時間內返回，使用者不必採取任何動作。

### <a name="replace-a-failed-drive"></a>更換失敗的磁片磁碟機

1. 當磁片磁碟機失敗時，**儀表板**的左上方**警示**區域中會出現警示。
2. 您也可以從左側導覽中選取 [**磁片磁碟機**]，或按一下右下角磚上的 [ **VIEW 磁片磁碟機 >** ] 連結，以流覽磁片磁碟機並自行查看其狀態。 在 [**清查**] 索引標籤中，方格支援排序、群組和關鍵字搜尋。
3. 從 [**儀表板**] 中，按一下警示以查看詳細資料，例如磁片磁碟機的實體位置。
4. 若要深入瞭解，請按一下**磁片磁碟機**詳細資料頁面上的 [**移至磁片磁碟機**] 快捷方式。
5. 如果您的硬體支援它，您可以按一下 [**開啟/關閉燈**] 以控制磁片磁碟機的指示器光線。
6. 儲存空間直接存取會自動淘汰並 evacuates 失敗的磁片磁碟機。 當此情況發生時，磁片磁碟機狀態會是 [已淘汰]，而其 [儲存容量] 列是空的。
7. 移除失敗的磁片磁碟機，並插入其取代。
8. 在**磁片磁碟機 > 清查**中，將會出現新的磁片磁碟機。 警示會在時間清除，磁片區會回復為 [確定] 狀態，而存放裝置會重新平衡到新的磁片磁碟機–使用者不必採取任何動作。

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>管理虛擬網路（使用 Windows 管理中心預覽的已啟用 SDN HCI 叢集）

1. 從左側導覽中選取 [**虛擬網路**]。
2. 按一下 [**新增**] 以建立新的虛擬網路和子網，或選擇現有的虛擬網路，然後按一下 [設定] 以修改其**設定**。
3. 按一下現有的虛擬網路，以針對虛擬網路子網和套用至虛擬網路子網的存取控制清單，來查看 VM 連線。

![管理虛擬網路](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>將虛擬機器連線至虛擬網路（使用 Windows 管理中心預覽的已啟用 SDN HCI 叢集）

1. 從左側導覽中選取 [**虛擬機器**]。
2. 選擇現有的虛擬機器 > 按一下 [**設定**] > 開啟 [**設定**] 中的 [**網路**] 索引標籤。
3. 設定 [**虛擬網路**] 和 [**虛擬子網**] 欄位，將虛擬機器連接到虛擬網路。

您也可以在建立虛擬機器時設定虛擬網路。

![將虛擬機器連線至虛擬網路](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>監視軟體定義的網路基礎結構（使用 Windows 管理中心預覽的已啟用 SDN HCI 叢集）

1. 從左側導覽中選取 [ **SDN 監視**]。
2. 查看網路控制站、軟體 Load Balancer、虛擬閘道健康情況的詳細資訊，並監視您的虛擬閘道集區、公用和私人 IP 集區使用方式，以及 SDN 主機狀態。

![監視 SDN 基礎結構](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>意見

這就是您的意見反應！ 經常更新最重要的好處，就是知道有哪些功能，以及需要改進的功能。 以下是讓我們知道您所想要的一些方法：

- [在 UserVoice 上提交和投票功能要求](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Microsoft Tech 社區的 Windows 管理中心論壇](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 推文至 `@servermgmt`

### <a name="see-also"></a>請參閱

- [Windows Admin Center](../overview.md)
- [儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [軟體定義網路](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
