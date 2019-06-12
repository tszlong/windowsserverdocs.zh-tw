---
title: 部署儲存空間直接存取
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: 將軟體定義儲存體與儲存空間直接存取 Windows Server 中部署為超交集基礎結構或聚合式 （也稱為分類式） 的基礎結構的逐步指示。
ms.localizationpriority: medium
ms.openlocfilehash: a4159c85be23025ef57084b47dcc77d4f749888f
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812361"
---
# <a name="deploy-storage-spaces-direct"></a>部署儲存空間直接存取

> 適用於：Windows Server 2019，Windows Server 2016

本主題提供逐步指示來部署[儲存空間直接存取](storage-spaces-direct-overview.md)。

> [!Tip]
> 要取得 Hyper-Converged 基礎結構嗎？ Microsoft 建議您購買的已驗證的硬體/軟體解決方案，從我們的合作夥伴，包括部署工具和程序。 這些解決方案是設計、 組合和針對我們參考架構，以確保相容性和可靠性，讓您快速並執行驗證。 如需 Windows Server 2019 解決方案，請瀏覽[Azure Stack HCI solutions 網站](https://azure.microsoft.com/overview/azure-stack/hci)。 如需 Windows Server 2016 解決方案，進一步了解[Windows Server 軟體定義](https://microsoft.com/wssd)。

> [!Tip]
> 您可以使用 HYPER-V 虛擬機器，包括 Microsoft Azure 中為[評估儲存空間直接存取，而不需要硬體](storage-spaces-direct-in-vm.md)。 您也可以檢閱便利[Windows Server 快速實驗室部署指令碼](https://aka.ms/wslab)，我們用來培訓等用途。

## <a name="before-you-start"></a>開始之前

檢閱[儲存空間直接存取硬體需求](Storage-Spaces-Direct-Hardware-Requirements.md)和先快速瀏覽這份文件以熟悉整體的方法和一些步驟相關聯的重要注意事項。

收集下列資訊：

- **部署選項。** 儲存空間直接存取支援[兩種部署選項： 超融合以及聚合](storage-spaces-direct-overview.md#deployment-options)，也稱為分類。 熟悉每個以決定何者最適合您的優點。 步驟 1-3 下列適用於這兩個部署選項。 交集的部署中的步驟 4 才需要。

- **伺服器名稱。** 熟悉您組織的電腦、 檔案、 路徑和其他資源的命名原則。 您必須佈建多部伺服器，每個都有唯一的名稱。

- **網域名稱。** 熟悉網域命名和加入網域的組織的原則。  您將能夠加入伺服器至您的網域，您必須指定網域名稱。 

- **RDMA 網路。** 有兩種類型的 RDMA 通訊協定： iWarp 和 RoCE。 請注意哪一個網路介面卡使用，而且如果 RoCE，也請注意 （v1 或 v2） 的版本。 為 RoCE，也請注意您機架頂端的交換器的模型。

- **VLAN ID。** 如果有的話，請注意，用於管理作業系統，伺服器上的網路介面卡的 VLAN ID。 您應該能夠從您的網路管理員取得。

## <a name="step-1-deploy-windows-server"></a>步驟 1：部署 Windows Server

### <a name="step-11-install-the-operating-system"></a>步驟 1.1:安裝作業系統

第一個步驟是將位於叢集中的每部伺服器上安裝 Windows Server。 儲存空間直接存取需要 Windows Server 2016 Datacenter Edition。 您可以使用 Server Core 安裝選項或伺服器含桌面體驗。

當您安裝 Windows Server 使用安裝精靈時，您可以選擇*Windows Server* （指的 Server Core） 和*Windows Server （伺服器含桌面體驗）* ，這是對等項目*完整*Windows Server 2012 R2 中可用的安裝選項。 如果您未選擇，您會得到的 Server Core 安裝選項。 如需詳細資訊，請參閱 <<c0> [ 安裝 Windows Server 2016 選項](../../get-started/Windows-Server-2016.md)。

### <a name="step-12-connect-to-the-servers"></a>步驟 1.2:連線到伺服器

本指南著重的 Server Core 安裝選項和個別的管理系統，此作業必須從遠端部署/管理：

- Windows Server 2016 的伺服器相同的更新管理
- 若要管理之伺服器的網路連線
- 加入相同網域或完全受信任的網域
- 適用於 Hyper-V 和容錯移轉叢集的遠端伺服器管理工具 (RSAT) 和 PowerShell 模組。 RSAT 工具和 PowerShell 模組是在 Windows Server 上，可以安裝而不需要安裝其他功能。 您也可以安裝[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)管理的 Windows 10 電腦上。

在管理系統上，安裝容錯移轉叢集與 Hyper-V 管理工具。 這可以透過伺服器管理員使用「新增角色及功能」  精靈完成。 在 [功能]  頁面上，選取 [遠端伺服器管理工具]  ，然後選取要安裝的工具。

進入 PS 工作階段，並使用伺服器名稱，或是您要連線之節點的 IP 位址。 您可在執行此命令之後，將會提示輸入密碼，輸入您在設定 Windows 時指定的系統管理員密碼。

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   以下是範例的進行方式是比較好用的指令碼，以防您需要執行這項操作一次以上相同的動作：

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> 如果您要從遠端部署從管理系統，您可能會收到類似的錯誤*WinRM 無法處理要求。* 若要修正此問題，使用 Windows PowerShell 將每個伺服器新增到您管理電腦上的受信任主機清單：  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> 注意： 信任的主機清單支援使用萬用字元，例如`Server*`。
>
> 若要檢視您的受信任主機清單，請輸入`Get-Item WSMAN:\Localhost\Client\TrustedHosts`。  
>   
> 若要清空的清單，請輸入`Clear-Item WSMAN:\Localhost\Client\TrustedHost`。  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>步驟 1.3:加入網域，並新增網域帳戶

到目前為止您已設定個別的伺服器與本機系統管理員帳戶， `<ComputerName>\Administrator`。

若要管理儲存空間直接存取，您必須將伺服器加入網域，並使用每一部伺服器上的 Administrators 群組中的 Active Directory 網域服務網域帳戶。

從管理系統，請以系統管理員權限開啟 PowerShell 主控台。 使用`Enter-PSSession`連接到每一部伺服器，並執行下列 cmdlet，以取代您自己的電腦名稱、 網域名稱和網域認證：

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

如果您的存放裝置系統管理員帳戶不是 Domain Admins 群組的成員，將您的存放裝置系統管理員帳戶新增至-的每個節點上本機 Administrators 群組或更棒的是，新增您用於存放裝置系統管理員群組。 您可以使用下列命令 (或撰寫可這樣做，請參閱 Windows PowerShell 函式[使用 PowerShell 將網域使用者新增至本機群組](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx)如需詳細資訊):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>步驟 1.4:安裝角色和功能

下一個步驟是在每一部伺服器上安裝伺服器角色。 您可以使用[Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md)， [伺服器管理員](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md))，或 PowerShell。 以下是要安裝的角色：

- 容錯移轉叢集
- Hyper-V
- 檔案伺服器 （如果您想要裝載的任何檔案共用，例如針對交集的部署）
- 資料中心橋接 (如果您正在使用 RoCEv2 而非 iWARP 網路介面卡)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

若要透過 PowerShell 安裝，請使用[Install-windowsfeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet。 您可以使用它在這類的單一伺服器上：

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

若要在同一時間的叢集中的所有伺服器上執行命令，使用這一小段指令碼，修改以符合您環境的指令碼開頭的變數的清單。

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>步驟 2：設定網路

如果您要部署儲存空間直接存取虛擬機器內，請略過本節。

儲存空間直接存取需要高頻寬、 低延遲網路在叢集中的伺服器之間。 需要至少 10 GbE 網路，建議使用遠端直接記憶體存取 (RDMA)。 只要有 Windows Server 2016 標誌，但是 iWARP 通常是容易設定，您可以使用 iWARP 或 RoCE。

> [!Important]
> 根據您的網路設備，以及特別是搭配 RoCE v2，可能需要一些機架頂端的交換器設定。 務必確保可靠性和效能的儲存空間直接存取正確的交換器設定。

Windows Server 2016 引進交換器內嵌小組 (SET) 中的 HYPER-V 虛擬交換器。 這可讓相同實體 NIC 連接埠時使用 RDMA，減少所需的實體 NIC 連接埠的所有網路流量使用。 交換器內嵌小組建議針對儲存空間直接存取。

若要設定的儲存空間直接存取網路的指示，請參閱[Windows Server 2016 交集的 NIC 和客體 RDMA 部署指南](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx)。

## <a name="step-3-configure-storage-spaces-direct"></a>步驟 3：設定儲存空間直接存取

下列步驟要在管理系統 (與要設定的伺服器相同版本) 上完成。 不必執行使用遠端 PowerShell 工作階段，但改為上管理系統，以系統管理權限的本機 PowerShell 工作階段中執行下列步驟。

### <a name="step-31-clean-drives"></a>步驟 3.1:清理磁碟機

您啟用儲存空間直接存取之前，請確定您的磁碟機是空的： 舊的資料分割或其他資料。 執行下列程式碼，以取代您的電腦名稱，若要移除所有任何舊的資料分割或其他資料。

> [!Warning]
> 此指令碼將會永久移除作業系統開機磁碟機以外的任何磁碟機上的任何資料 ！

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"

Invoke-Command ($ServerList) {
    Update-StorageProviderCache
    Get-StoragePool | ? IsPrimordial -eq $false | Set-StoragePool -IsReadOnly:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Remove-StoragePool -Confirm:$false -ErrorAction SilentlyContinue
    Get-PhysicalDisk | Reset-PhysicalDisk -ErrorAction SilentlyContinue
    Get-Disk | ? Number -ne $null | ? IsBoot -ne $true | ? IsSystem -ne $true | ? PartitionStyle -ne RAW | % {
        $_ | Set-Disk -isoffline:$false
        $_ | Set-Disk -isreadonly:$false
        $_ | Clear-Disk -RemoveData -RemoveOEM -Confirm:$false
        $_ | Set-Disk -isreadonly:$true
        $_ | Set-Disk -isoffline:$true
    }
    Get-Disk | Where Number -Ne $Null | Where IsBoot -Ne $True | Where IsSystem -Ne $True | Where PartitionStyle -Eq RAW | Group -NoElement -Property FriendlyName
} | Sort -Property PsComputerName, Count
```

輸出看起來像這樣，其中**計數**是每個模型中每一部伺服器的磁碟機數目：

```
Count Name                          PSComputerName
----- ----                          --------------
4     ATA SSDSC2BA800G4n            Server01
10    ATA ST4000NM0033              Server01
4     ATA SSDSC2BA800G4n            Server02
10    ATA ST4000NM0033              Server02
4     ATA SSDSC2BA800G4n            Server03
10    ATA ST4000NM0033              Server03
4     ATA SSDSC2BA800G4n            Server04
10    ATA ST4000NM0033              Server04
```

### <a name="step-32-validate-the-cluster"></a>步驟 3.2:驗證叢集

在此步驟中，您將執行叢集驗證工具，以確保伺服器節點都正確設定，以建立使用儲存空間直接存取叢集。 當叢集驗證 (`Test-Cluster`) 是執行建立叢集之前，請執行測試來驗證組態是否適合做為容錯移轉叢集順利運作。 面的範例使用`-Include`指定參數，然後按一下 特定的測試類別。 這可確保驗證中包含與儲存空間直接存取相關的測試。

您可以使用下列 PowerShell 命令驗證一組用來做為儲存空間直接存取叢集的伺服器。

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>步驟 3.3:建立叢集

在此步驟中，您將建立叢集，您已驗證過可用來使用下列 PowerShell cmdlet 將上述步驟中建立的叢集節點。

建立叢集時，您會收到一則警告指出-「 發生問題而建立的叢集的角色可能會導致它無法啟動。 如需詳細資訊，請檢視下面的報表檔案。」 您可以放心忽略這個警告。 這是因為沒有磁碟可供叢集仲裁使用。 建議您在建立叢集之後設定檔案共用見證或雲端見證。

> [!Note]
> 如果伺服器使用靜態 IP 位址，請修改下列命令，新增下列參數並指定 IP 位址：-StaticAddress &lt;X.X.X.X&gt; 以反映靜態 IP 位址。
> 在下列命令中，ClusterName 預留位置應取代為唯一的 netbios 名稱 (15 個以下的字元)。
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

叢集建立之後，叢集名稱的 DNS 項目需要一些時間才會被複寫。 時間則取決於環境和 DNS 複寫組態。 如果解析叢集不成功，在大多數情況下，使用節點 (屬於叢集的作用中成員) 的電腦名稱而不是叢集名稱可能會成功。

### <a name="step-34-configure-a-cluster-witness"></a>步驟 3.4:設定叢集見證

我們建議您設定叢集見證，讓具有三個或多部伺服器的叢集可以承受兩部伺服器失敗或離線。 雙重伺服器部署中需要一個叢集見證，否則可能是離線的伺服器會導致其他也變成無法使用。 您可以使用檔案共用作為見證，或使用雲端見證來搭配這些系統。 

如需詳細資訊，請參閱下列主題：

- [設定和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [部署容錯移轉叢集的雲端見證](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>步驟 3.5:啟用儲存空間直接存取

建立叢集之後, 使用`Enable-ClusterStorageSpacesDirect`PowerShell cmdlet，它會將儲存體系統放入儲存空間直接存取模式，並自動執行下列作業：

-   **建立集區：** 建立有類似"S2D on Cluster1"名稱的單一大型集區。

-   **設定儲存空間直接存取快取：** 如果有多個媒體 （磁碟機） 型別可供儲存空間直接存取使用，它可讓以最快速的快取裝置 （讀取和寫入在大部分情況下）

-   **層：** 建立兩個層級做為預設階層。 一個稱為「容量」，另一個稱為「效能」。 此 Cmdlet 會分析裝置，並使用混合的裝置類型和復原功能來設定每一層。

從管理系統以系統管理員權限開啟的 PowerShell 命令視窗中，起始下列命令。 叢集名稱是您在先前步驟中建立的叢集名稱。 如果此命令在其中一個本機節點上執行，則不需要 -CimSession 參數。

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

若要使用上面的命令啟用儲存空間直接存取，您也可以使用節點名稱而不是叢集名稱。 使用節點名稱可能更可靠，因為新建立的叢集名稱可能會發生 DNS 複寫延遲。

此命令完成可能需要幾分鐘的時間，完成之後，系統就可以建立磁碟區。

### <a name="step-36-create-volumes"></a>步驟 3.6:建立磁碟區

我們建議使用`New-Volume`cmdlet，因為它提供的最快且最直接的體驗。 這個單一 cmdlet 會自動建立虛擬磁碟、磁碟分割以及格式化，以相符名稱建立磁碟區，並將其加入至叢集共用磁碟區 – 全在一個簡易步驟中。

如需詳細資訊，請查看[建立儲存空間直接存取中的磁碟區](create-volumes.md)。

### <a name="step-37-optionally-enable-the-csv-cache"></a>步驟 3.7:選擇性地啟用 CSV 快取

您可以選擇啟用叢集共用磁碟區 (CSV) 快取至系統記憶體 (RAM) 做為寫入透過區塊層級的快取 Windows 快取管理員不已快取的讀取作業。 這可以改善應用程式，例如 HYPER-V 的效能。 CSV 快取可大幅提升效能的讀取要求，同時也是適用於向外延展檔案伺服器案例。

啟用 CSV 快取可減少在超交集叢集上，執行的 Vm，因此您必須有記憶體可用 vhd 的儲存體效能之間取得平衡，您可以使用的記憶體數量。

若要設定 CSV 快取的大小，開啟管理系統上的 PowerShell 工作階段與儲存體叢集具有系統管理員權限的帳戶，然後使用此指令碼，變更`$ClusterName`和`$CSVCacheSize`適當 （此變數範例會將 2 GB 的 CSV 快取，每一部伺服器）：

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

如需詳細資訊，請參閱 <<c0> [ 使用 CSV 在記憶體中讀取快取](csv-cache.md)。

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>步驟 3.8:部署超交集部署的虛擬機器

如果您要部署超交集叢集，最後一個步驟就是佈建儲存空間直接存取叢集上的虛擬機器。

虛擬機器的檔案應該儲存在系統的 CSV 命名空間 (範例： c:\\ClusterStorage\\Volume1) 就像叢集容錯移轉叢集上的 Vm。

您可以使用現成的工具或其他工具來管理儲存體和虛擬機器，例如 System Center Virtual Machine Manager。

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>步驟 4：聚合式解決方案部署向外延展檔案伺服器

如果您要部署的聚合式的解決方案下, 一個步驟是建立向外延展檔案伺服器執行個體，並設定某些檔案共用。 如果您要部署超交集叢集-您已完成，不需要這一節。

### <a name="step-41-create-the-scale-out-file-server-role"></a>步驟 4.1:建立向外延展檔案伺服器角色

設定檔案伺服器叢集服務的下一個步驟建立叢集的檔案伺服器角色，也就是當您建立持續可用的檔案共用裝載在其的向外延展檔案伺服器執行個體。

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>若要使用伺服器管理員中建立向外延展檔案伺服器角色

1. 在 容錯移轉叢集管理員 中，選取叢集，請前往**角色**，然後按一下 **設定角色...** .<br>高可用性精靈 隨即出現。
2. 在 **選取的角色**頁面上，按一下**檔案伺服器**。
3. 在 **檔案伺服器類型**頁面上，按一下**應用程式資料的向外延展檔案伺服器**。
4. 在 **用戶端存取點**頁面上，輸入在向外延展檔案伺服器的名稱。
5. 確認角色已成功設定移至**角色**並確認**狀態**資料行會顯示**執行**旁您所建立的叢集的檔案伺服器角色圖 1 所示。

   ![螢幕擷取畫面的容錯移轉叢集管理員將向外延展檔案伺服器顯示](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "顯示向外延展檔案伺服器容錯移轉叢集管理員")

    **圖 1**顯示向外延展檔案伺服器與執行中狀態的容錯移轉叢集管理員

> [!NOTE]
>  在建立叢集的角色之後, 可能會有一些網路會讓您無法在其中建立檔案共用，幾分鐘的時間，或可能較長的傳播延遲。  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>若要使用 Windows PowerShell 中建立向外延展檔案伺服器角色

 連接到檔案伺服器叢集的 Windows PowerShell 工作階段中輸入下列命令來建立向外延展檔案伺服器角色，變更*FSCLUSTER*符合您的叢集名稱和*SOFS*以符合您想要提供向外延展檔案伺服器角色的名稱：

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  在建立叢集的角色之後, 可能會有一些網路會讓您無法在其中建立檔案共用，幾分鐘的時間，或可能較長的傳播延遲。 如果 SOFS 角色就會立即失敗，並不會啟動，它可能是因為叢集的電腦物件沒有建立 SOFS 角色的電腦帳戶的權限。 為協助進行，請參閱此部落格文章：[向外延展檔案伺服器角色無法啟動事件識別碼 1205年、 1069 和 1194年](http://www.aidanfinn.com/?p=14142)。

### <a name="step-42-create-file-shares"></a>步驟 4.2:建立檔案共用

您已建立您的虛擬磁碟，並將資料新增到 Csv 之後，就可以開始建立檔案共用上的每個 CSV 的一個檔案共用，每個虛擬磁碟。 System Center Virtual Machine Manager (VMM) 可能是 handiest 方式，若要這樣做，因為它會處理權限，但如果您沒有它在您的環境中，您可以使用 Windows PowerShell 來部分自動化部署。

使用包含在指令碼[HYPER-V 工作負載的 SMB 共用設定](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)指令碼，這部分會自動建立群組和共用的程序。 它會寫入針對 HYPER-V 工作負載，因此如果您要部署的其他工作負載，您可能要修改的設定，或建立共用之後，執行額外的步驟。 比方說，如果您使用 Microsoft SQL Server，SQL Server 服務帳戶必須被授與完整控制權共用及檔案系統。

> [!NOTE]
>  您必須更新的群組成員資格，當您新增叢集節點，除非您使用 System Center Virtual Machine Manager 來建立您的共用。

若要使用 PowerShell 指令碼，以建立檔案共用，請執行下列作業：

1. 下載中包含的指令碼[HYPER-V 工作負載的 SMB 共用設定](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)到其中一個檔案伺服器叢集的節點。
2. 以網域系統管理員認證，在管理系統上，開啟 Windows PowerShell 工作階段，然後使用下列指令碼建立 HYPER-V 電腦物件中，Active Directory 群組變更為適當的變數的值您環境：

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. 以系統管理員認證，在其中一個儲存體節點中，開啟 Windows PowerShell 工作階段，然後使用下列指令碼建立的每個 CSV 的共用和共用的系統管理權限授與 Domain Admins 群組及計算叢集。

    ```PowerShell
    # Replace the values of these variables
    $StorageClusterName = "StorageSpacesDirect1"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $SOFSName = "SOFS"
    $SharePrefix = "Share"
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of the script itself
    CD $ScriptFolder
    Get-ClusterSharedVolume -Cluster $StorageClusterName | ForEach-Object
    {
        $ShareName = $SharePrefix + $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume")
        Write-host "Creating share $ShareName on "$_.name "on Volume: " $_.SharedVolumeInfo.friendlyvolumename
        .\FileShareSetup.ps1 -HyperVClusterName $StorageClusterName -CSVVolumeNumber $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume") -ScaleOutFSName $SOFSName -ShareName $ShareName -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName
    }
    ```

### <a name="step-43-enable-kerberos-constrained-delegation"></a>步驟 4.3 啟用 Kerberos 限制委派

若要設定 Kerberos 限制委派，針對遠端案例管理和更高的即時移轉安全性，從一個儲存體的叢集節點，會使用 KCDSetup.ps1 指令碼中包含[適用於 HYPER-V 工作負載SMB共用設定](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). 以下是一些指令碼的包裝函式：

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>後續步驟

部署後您的叢集的檔案伺服器，建議您測試您的方案使用綜合工作負載之前，先註冊任何實際工作負載的效能。 這可讓您確認方案正在正常執行，以及任何延遲的問題解決之前新增的工作負載的複雜性。 如需詳細資訊，請參閱 <<c0> [ 測試儲存空間效能使用綜合工作負載](https://technet.microsoft.com/library/dn894707.aspx)。

## <a name="see-also"></a>另請參閱

-   [Windows Server 2016 中的儲存空間直接存取](storage-spaces-direct-overview.md)
-   [了解的快取中儲存空間直接存取](understand-the-cache.md)
-   [規劃中儲存空間直接存取磁碟區](plan-volumes.md)
-   [儲存空間容錯](storage-spaces-fault-tolerance.md)
-   [儲存空間直接存取硬體需求](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [要用 RDMA，還是不用 RDMA 呢，這才是問題所在](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (TechNet 部落格)
