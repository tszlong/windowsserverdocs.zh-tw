---
title: 管理超融合式基礎結構，使用 Windows Admin Center
description: 管理超融合式基礎結構，使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9ce4381735746ace6358aa2cb30f8b341c576054
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262841"
---
# 管理超融合式基礎結構，使用 Windows Admin Center

>適用於：Windows Admin Center、Windows Admin Center 預覽版

## 什麼是超融合式基礎結構

超融合式基礎結構中的軟體定義的運算、 儲存和網路功能到一個叢集以提供高效能，具成本效益，並可輕鬆地調整虛擬化。 使用[[儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)、 軟體定義網路](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking)」 和[HYPER-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)的 Windows Server 2016 中引進了這項功能。

> [!Tip]
> 想要取得超融合式基礎結構嗎？ Microsoft 建議這些[Windows Server 軟體定義](https://microsoft.com/wssd)解決方案，由我們的合作夥伴。 它們是設計、 組合，且我們參考架構，以確保相容性和可靠性，因此您讓啟動和執行快速對照驗證。

> [!IMPORTANT]
> 這篇文章中所述的功能的一些所只在 Windows Admin Center 預覽版中提供。 [如何取得此版本？](http://aka.ms/windowsadmincenter)

## 什麼是 Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md)是適用於 Windows Server，傳統 「 內建 」 工具例如伺服器管理員後續新一代管理工具。 它是免費和可以安裝和使用沒有網際網路連線。 您可以使用 Windows Admin Center 來管理並監視執行 Windows Server 2016 或 Windows Server 2019 的超融合式基礎結構。

![超融合式叢集儀表板](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## 主要功能

Windows Admin Center 超融合式基礎結構的醒目提示包括：

- **統一的單一-窗格-的-玻璃的運算、 儲存和很快網路功能。** 檢視您的虛擬機器、 主機伺服器、 磁碟區、 磁碟機，以及更多內一個特殊用途、 一致、 互連體驗。
- **建立和管理儲存空間和 HYPER-V 虛擬機器。** 建立、 開啟、 調整大小，並刪除磁碟區; 極端簡單的工作流程和建立、 開始、 連線到，並移動虛擬機器;及其他更多。
- **強大的整個叢集的監視。** 記憶體和 CPU 使用量、 儲存空間容量、 IOPS、 輸送量和延遲，圖表顯示在儀表板即時在叢集上，使用清楚的警示時的項目不是正確的每一個伺服器上。
- **軟體定義網路 (SDN) 的支援。** 管理和監視虛擬網路，子網路、 虛擬機器連線到虛擬網路，以及監視 SDN 基礎結構。

Microsoft 正在主動開發 Windows Admin Center 超融合式基礎結構。 它會收到頻繁的更新，改善現有功能，並新增新的功能。

## 開始之前

若要為 Windows Admin Center 中的超融合式基礎結構管理您的叢集，它需要執行 Windows Server 2016 或 Windows Server 2019，且已 HYPER-V 和儲存空間直接存取啟用。 或者，它也可以讓軟體定義網路啟用，且透過 Windows Admin Center 管理。

> [!Tip]
> Windows Admin Center 也會提供適用於 Windows Server 2012 及更新版本支援任何工作負載，可以使用任何叢集的一般用途的管理體驗。 如果這聽起來像是更符合，當您新增至 Windows Admin Center 的叢集，選取 [[**容錯移轉叢集**](manage-failover-clusters.md)，而不是**超融合式叢集**。

### Windows Admin center 準備您的 Windows Server 2016 叢集

Windows Admin Center 超融合式基礎結構，取決於管理 Windows Server 2016 已發行之後新增 Api。 您可以管理您的 Windows Server 2016 叢集使用 Windows Admin Center 之前，您將需要執行這兩個步驟：

1. 確認叢集中的每一個伺服器有安裝[2018年 05 累積更新，適用於 Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)或更新版本。 若要下載並安裝此更新，請移至 [**設定** > **更新 & 安全性** > **Windows Update**與選取**線上檢查是否有來自 Microsoft Update 的更新**。
2. 在叢集上以系統管理員身分執行下列 PowerShell cmdlet:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> 您只需要一次，在叢集中的任何伺服器上執行 cmdlet。 您可以在本機 Windows PowerShell 中執行，或使用認證的安全性服務提供者 (CredSSP) 從遠端執行它。 根據您的設定，您可能會無法執行此 cmdlet，從 Windows Admin Center 中。

### Windows Admin center 準備您的 Windows Server 2019 叢集

如果您的叢集是執行 Windows Server 2019，並不需要上述步驟執行。 下一節中所述，只要將叢集新增至 Windows Admin Center 與您準備好移 ！

### 設定軟體定義網路 （選擇性） ###

您可以設定您的超融合式基礎結構，執行 Windows Server 2016 或 2019年軟體定義網路 (SDN) 使用下列步驟：

1. 準備作業系統也就是您在超融合式基礎結構主機已安裝的相同 OS 的 VHD。 這個 VHD 將會用於所有 NC/SLB/GW Vm。
2. 下載所有的資料夾和下 SDN Express 從檔案[https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress)。
3. 準備使用部署主控台不同 VM。 此 VM 應該無法存取 SDN 主機。 此外，VM 也應該有安裝 RSAT HYPER-V 工具。
4. 複製您下載適用於 SDN 的快速部署主控台 VM 的所有項目。 和共用此**SDNExpress**資料夾。 請確定每一部主機可以存取**SDNExpress**共用的資料夾，設定檔案行 8 中定義：
```
    \\$env:Computername\SDNExpress
```
5. 將作業系統的 VHD 複製到部署主機 VM 上**SDNExpress**資料夾下的**映像**資料夾。
6. 修改 SDN 快速設定與您的環境設定。 之後您修改 SDN Express 組態根據您環境的資訊，請完成下列兩個步驟。
7. 執行 PowerShell 部署 SDN 的系統管理員權限：

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

部署需要約 30-45 分鐘。

## 開始

一旦您的超融合式基礎結構部署時，您可以使用 Windows Admin Center 進行管理。

### 安裝 Windows Admin Center

如果您尚未，下載並安裝 Windows Admin Center。 最快的方式，可讓啟動並執行是它安裝到您 Windows 10 的電腦上，並從遠端管理您的伺服器。 這會帶少於五分鐘的時間。 [立即下載](https://aka.ms/windowsadmincenter)或[安裝的其他選項更了解](../deploy/install.md)。

### 新增超融合式叢集

將您的叢集新增到 Windows Admin Center:

1. 按一下 [ **+ 新增**下所有的連線。
2. 選擇新增**超融合式叢集連線**。
3. 輸入的叢集名稱，如果出現提示，使用的認證。
4. 按一下 [**新增**] 以完成。

叢集將會新增到您的連線清單。 按一下以啟動在儀表板。

![新增超融合式叢集連線](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### 新增啟用 SDN 的超融合式叢集 （Windows Admin Center 預覽版）

最新的 Windows Admin Center 預覽版支援軟體定義網路管理超融合式基礎結構。 藉由新增到您的超融合式叢集連線的網路控制站的其餘部分 URI，您可以使用超融合式叢集管理員來管理您的 SDN 資源並監視 SDN 基礎結構。

1. 按一下 [ **+ 新增**下所有的連線。
2. 選擇新增**超融合式叢集連線**。
3. 輸入的叢集名稱，如果出現提示，使用的認證。
4. 檢查以繼續**設定網路控制卡**。
5. 輸入**網路控制站 URI** ，然後按一下 [**驗證**。
6. 按一下 [**新增**] 以完成。

叢集將會新增到您的連線清單。 按一下以啟動在儀表板。

![新增啟用 SDN 的超融合式叢集連線](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> 目前不支援透過 Northbound 通訊的 Kerberos 驗證的 SDN 環境。

## 常見問題集

### 是否有管理 Windows Server 2016 和 Windows Server 2019 有何差異？

是。 Windows Admin Center 超融合式基礎結構會收到頻繁的更新，以改善 Windows Server 2016 和 Windows Server 2019 體驗。 不過，某些新的功能都只適用於 Windows Server 2019 – 例如，適用於重複資料刪除和壓縮的切換開關。

### 可以使用 Windows Admin Center 來管理儲存空間直接存取的其他使用案例 （不超融合式），例如交集向外延展檔案伺服器 (SoFS) 或 Microsoft SQL Server？

超融合式基礎結構的 Windows Admin Center 不會提供管理或監視的選項，專為儲存空間直接存取的其他使用案例 – 例如，但無法建立檔案共用。 不過，儀表板和核心功能，例如建立磁碟區或取代的磁碟機，可以搭配任何儲存空間直接存取叢集。

### 什麼是容錯移轉叢集和超融合式叢集之間的差異？

一般情況下，一詞指的是 「 超融合式 」 上的相同執行 HYPER-V 和儲存空間直接存取叢集伺服器，來將虛擬化運算和儲存資源。 在 Windows Admin Center 的內容，當您按一下 [ **+ 新增**從連線清單中，您可以選擇加入**容錯移轉叢集連線**或**超融合式叢集連線**：

- 容錯移轉叢集管理員的傳統型應用程式後續**容錯移轉叢集連線**。 它提供任何支援任何工作負載，包括 Microsoft SQL Server 的叢集熟悉、 一般用途的管理體驗。 它是適用於 Windows Server 2012 和更新版本。

- **超融合式叢集連線**是針對儲存空間直接存取及 HYPER-V 量身打造的全新體驗。 它具有儀表板，並強調監控所用的圖表和警示。 它只適用於 Windows Server 2016 和 Windows Server 2019。

### 為什麼需要 Windows Server 2016 的最新的累積更新？

Windows Admin Center 超融合式基礎結構，取決於 Api 開發，因為 Windows Server 2016 已發行的管理。 這些 Api 會在[2018年 05 適用於 Windows Server 2016 (KB4103723) 的累積更新](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723)，截至 2018 年 8，可用來新增。

### 使用 Windows Admin Center 需要多少費用？

Windows Admin Center 除了 Windows 本身以外，不需另付費用。

您可以使用免費的有效授權的 Windows Server 或 Windows 10 中使用 Windows Admin Center （可供個別下載）-這已依據 Windows 增補 eula 取得授權底下。

### Windows Admin Center 是否需要 System Center？

否。

### 它需要網際網路連線嗎？

否。

雖然 Windows Admin Center 提供強大和完全是與 Microsoft Azure 雲端、 核心管理及監視體驗超融合式基礎結構的便利整合內部部署。 它可以安裝及使用沒有網際網路連線。

## 嘗試事項

如果您剛開始使用，以下是一些可協助您了解如何超融合式基礎結構的 Windows Admin Center 組織和運作方式的快速教學課程。 請練習良好 judgement，並請小心使用生產環境。 這些影片是使用 Windows Admin Center 版本 1804年與 Windows Server 2019 Insider Preview 組建記錄。

### 管理儲存空間直接存取磁碟區

<ul>
               <li>(0:37)<a href="https://youtu.be/o66etKq70N8">如何建立三向鏡像磁碟區</a></li>
               <li>(1:17)<a href="https://youtu.be/R72QHudqWpE">如何建立鏡像加速同位磁碟區</a></li>
               <li>(1:02)<a href="https://youtu.be/j59z7ulohs4">如何開啟 [磁碟區，然後新增檔案</a></li>
               <li>(0:51)<a href="https://youtu.be/PRibTacyKko">如何開啟重複資料刪除和壓縮</a></li>
               <li>(0:47)<a href="https://youtu.be/hqyBzipBoTI">如何擴展磁碟區</a></li>
               <li>(0:26)<a href="https://youtu.be/DbjF8r2F6Jo">如何刪除磁碟區</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>建立磁碟區，三向鏡像</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>建立磁碟區，鏡像加速同位</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>開啟 [磁碟區，然後新增檔案</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>開啟重複資料刪除和壓縮</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>展開 [磁碟區</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>刪除磁碟區</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### 建立新的虛擬機器

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，選擇 [**詳細目錄**] 索引標籤，然後按一下 \ [**新增]** 來建立新的虛擬機器。
3. 輸入的虛擬機器名稱，然後選擇 \ [1 和 2 代虛擬機器。
4. 然後，您可以選擇一開始上建立虛擬機器，或使用建議的主機的主機。
5. 選擇的虛擬機器檔案的路徑。 從下拉式清單中選擇磁碟區，或按一下 [**瀏覽**選擇資料夾，使用資料夾選擇器。 將下的單一資料夾中儲存的虛擬機器組態檔和虛擬硬碟檔案`\Hyper-V\[virtual machine name]`選取的磁碟區或路徑的路徑。
6. 是否啟用巢狀虛擬化，設定記憶體設定、 網路介面卡、 虛擬硬碟，然後選擇是否要從.iso 映像檔案，或從網路安裝作業系統，請選擇虛擬處理器，數目。
7. 按一下 [**建立**來建立虛擬機器。
8. 一旦建立虛擬機器，並出現在虛擬機器清單，您就可以開始在虛擬機器。
9. 一旦啟動虛擬機器時，您可以連線到透過 VMConnect 安裝作業系統的虛擬機器的主控台。 從清單中選取虛擬機器，請按一下 [**更多** > 以下載.rdp 檔案的**連線**。 遠端桌面連線應用程式中開啟.rdp 檔案。 因為這連線到虛擬機器的主控台，您將需要輸入 HYPER-V 主機的系統管理員認證。

[深入了解有關使用 Windows Admin Center 的虛擬機器管理](manage-virtual-machines.md)。

### 暫停並安全地重新啟動伺服器

1. 從**儀表板**中，選取**伺服器**上的左側，或按一下右下角的儀表板中的磚上的**檢視伺服器 >** 連結瀏覽。
2. 在頂端，從**摘要**切換到 [**詳細目錄**] 索引標籤。
3. 按一下其名稱來開啟**伺服器**詳細資料頁面上選取的伺服器。
4. 按一下 [**暫停伺服器以進行維護**。 如果它是可繼續執行，這會移動至叢集中的其他伺服器的虛擬機器。 伺服器將會有狀態清空發生這種情況時。 如果您想要您可以觀賞移動在**虛擬機器 > 詳細目錄**頁面上，其主機伺服器清楚地顯示在方格中的虛擬機器。 當所有虛擬機器已都移動時，伺服器狀態將會**暫停**。
5. 按一下 [**管理伺服器**存取 Windows Admin Center 中的所有每個伺服器管理工具。
6. 按一下 [**重新啟動**，然後**是**。 您將會開始回復到連線清單。
7. 重新開啟**儀表板**，伺服器被紅色向下時。
8. 一旦備份好時，一次瀏覽**伺服器**頁面，然後按一下 [**繼續維護的伺服器**對伺服器狀態重設為只需保持最新。 在時間中，虛擬機器將會移回 – 不不需要任何使用者動作。

### 取代失敗的磁碟機

1. 當磁碟機失敗時，警示會出現在**儀表板**的左上角左**警示**區域。
2. 您也可以選取**的磁碟機**上的左側瀏覽，或按一下 **> 檢視磁碟機**上的連結來瀏覽的磁碟機，並查看他們的狀態為自己右下角中的磚。 在 [**詳細目錄**] 索引標籤，方格支援排序、 群組，以及搜尋關鍵字。
3. 從**儀表板**中，按一下警示以查看詳細資料，例如磁碟機的實體位置。
4. 若要深入了解，按一下 [**移到磁碟機****的磁碟機**的詳細資料頁面上捷徑。
5. 如果您的硬體支援它，您可以按一下來控制的磁碟機指示器光線的 [**開啟/關閉的光線**。
6. 儲存空間直接存取，自動撤下並 evacuates 故障的磁碟機。 當發生這時，磁碟機狀態會停用，，而且其儲存空間容量列是空的。
7. 移除失敗的磁碟機，然後插入及其替代。
8. 在**磁碟機 > 詳細目錄**，會顯示新的磁碟機。 在時間警示將會清除、 磁碟區會修復回到 OK 狀態，並儲存空間將會重新平衡至新的磁碟機 – 不不需要任何使用者動作。

### 管理虛擬網路 （SDN 啟用 HCI 叢集使用 Windows Admin Center 預覽版）

1. 從左邊的瀏覽中，選取**虛擬網路**。
2. 按一下 [**新增]** 來建立新的虛擬網路和子網路，或選擇現有的虛擬網路，並按一下 [來修改其設定的**設定**。
3. 按一下現有的虛擬網路檢視 VM 連線到虛擬網路的子網路，以及存取控制清單套用到虛擬網路的子網路。

![管理虛擬網路](../media/manage-hyper-converged/manage-virtual-networks.png)

### 將虛擬機器連接至虛擬網路 （SDN 啟用 HCI 叢集使用 Windows Admin Center 預覽版）

1. 選取**虛擬機器**上的左側瀏覽。
2. 選擇現有的虛擬機器 > 按一下**設定**> 在**設定**中開啟 [**網路**] 索引標籤。
3. 設定連線至虛擬網路的虛擬機器的**虛擬網路**和**虛擬子網路**欄位。

建立虛擬機器時，您也可以設定虛擬網路。

![將虛擬機器連接至虛擬網路](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### 監視軟體定義網路基礎結構 （使用 Windows Admin Center 預覽版 SDN 啟用 HCI 叢集）

1. **SDN 監視**從選取左側的瀏覽。
2. 檢視網路控制卡、 軟體負載平衡器虛擬閘道的健康情況的相關詳細的資訊，並監視您的虛擬閘道集區、 公用和私人 IP 集區使用量和 SDN 主機狀態。

![監視器 SDN 基礎結構](../media/manage-hyper-converged/sdn-monitoring.png)

## 意見反應

它是所有關於您的意見反應 ！ 經常更新的最重要的好處是可聽到功能的運作，而且所需的改進。 以下是一些讓我們知道您的想法的方式：

- [提交並功能要求在 UserVoice 上投票](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [加入 Microsoft 技術社群上的 Windows Admin Center 論壇](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- 若要推文 `@servermgmt`

### 也請參閱

- [Windows Admin Center](../understand/windows-admin-center.md)
- [儲存空間直接存取](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [軟體定義的網路功能](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
