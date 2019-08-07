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
description: 使用 Windows Server 中的儲存空間直接存取, 將軟體定義的存放裝置部署為超融合基礎結構或聚合式 (也稱為分類式) 基礎結構的逐步指示。
ms.localizationpriority: medium
ms.openlocfilehash: 69cd27cba09bd9d23a461978416217a20b2979ec
ms.sourcegitcommit: b68ff64ecd87959cd2acde4a47506a01035b542a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68830901"
---
# <a name="deploy-storage-spaces-direct"></a>部署儲存空間直接存取

> 適用於：Windows Server 2019、Windows Server 2016

本主題提供部署[儲存空間直接存取](storage-spaces-direct-overview.md)的逐步指示。

> [!Tip]
> 想要取得超融合的基礎結構嗎？ Microsoft 建議向我們的合作夥伴購買經過驗證的硬體/軟體解決方案, 包括部署工具和程式。 這些解決方案是針對我們的參考架構而設計、組裝和驗證, 以確保相容性和可靠性, 讓您快速啟動並執行。 如需 Windows Server 2019 解決方案, 請造訪[AZURE STACK HCI 解決方案網站](https://azure.microsoft.com/overview/azure-stack/hci)。 如需 Windows Server 2016 解決方案, 請在[Windows Server 軟體定義](https://microsoft.com/wssd)中深入瞭解。

> [!Tip]
> 您可以使用 Hyper-v 虛擬機器 (包括在 Microsoft Azure 中) 來[評估沒有硬體的儲存空間直接存取](storage-spaces-direct-in-vm.md)。 您可能也會想要查看[Windows Server 快速實驗室部署腳本](https://aka.ms/wslab), 這是我們用來定型的課程。

## <a name="before-you-start"></a>開始之前

請參閱[儲存空間直接存取硬體需求](Storage-Spaces-Direct-Hardware-Requirements.md)並流覽此檔, 以熟悉與一些步驟相關的整體方法和重要注意事項。

收集下列資訊:

- **部署選項。** 儲存空間直接存取支援[兩種部署選項: 超交集和](storage-spaces-direct-overview.md#deployment-options)交集, 也稱為分類式。 讓自己熟悉各項優點, 以決定哪一種最適合您。 下列步驟1-3 適用于這兩種部署選項。 只有聚合式部署才需要步驟4。

- **伺服器名稱。** 熟悉您組織的電腦、檔案、路徑及其他資源的命名原則。 您必須布建數個伺服器, 每個都有唯一的名稱。

- **功能變數名稱。** 熟悉您組織的網域命名和加入網域的原則。  您會將伺服器加入您的網域, 而且您必須指定功能變數名稱。 

- **RDMA 網路功能。** RDMA 通訊協定有兩種類型: iWarp 和 RoCE。 請注意, 您的網路介面卡會使用哪一個, 如果 RoCE, 也請注意版本 (v1 或 v2)。 針對 RoCE, 也請記下機架頂端交換器的型號。

- **VLAN ID。** 請注意, 在伺服器上用於管理 OS 網路介面卡的 VLAN ID (如果有的話)。 您應該能夠從您的網路管理員取得。

## <a name="step-1-deploy-windows-server"></a>步驟 1：部署 Windows Server

### <a name="step-11-install-the-operating-system"></a>步驟 1.1:安裝作業系統

第一個步驟是在每個將位於叢集中的伺服器上安裝 Windows Server。 儲存空間直接存取需要 Windows Server 2016 Datacenter Edition。 您可以使用 Server Core 安裝選項或具有桌面體驗的伺服器。

當您使用安裝程式來安裝 Windows Server 時, 可以選擇*Windows server* (指的是 server Core) 和*windows Server (具有桌面體驗的伺服器)* , 這相當於*完全*安裝選項。適用于 Windows Server 2012 R2。 如果您沒有選擇, 將會取得 Server Core 安裝選項。 如需詳細資訊, 請參閱[Windows Server 2016 的安裝選項](../../get-started/Windows-Server-2016.md)。

### <a name="step-12-connect-to-the-servers"></a>步驟 1.2:連接到伺服器

本指南著重于 Server Core 安裝選項, 以及從不同的管理系統遠端部署/管理, 其必須具備下列各項:

- Windows Server 2016 與其管理的伺服器具有相同的更新
- 網路連線到其管理的伺服器
- 已加入相同的網域或完全受信任的網域
- 適用於 Hyper-V 和容錯移轉叢集的遠端伺服器管理工具 (RSAT) 和 PowerShell 模組。 RSAT 工具和 PowerShell 模組可在 Windows Server 上使用, 而且可以在不安裝其他功能的情況下安裝。 您也可以在 Windows 10 管理電腦上安裝[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。

在管理系統上，安裝容錯移轉叢集與 Hyper-V 管理工具。 這可以透過伺服器管理員使用「新增角色及功能」精靈完成。 在 [功能] 頁面上，選取 [遠端伺服器管理工具]，然後選取要安裝的工具。

進入 PS 工作階段，並使用伺服器名稱，或是您要連線之節點的 IP 位址。 當您執行此命令時, 系統會提示您輸入密碼, 請輸入您在設定 Windows 時指定的系統管理員密碼。

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   以下是在腳本中執行相同動作的範例, 如果您需要執行這項作業的方式不止一次:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> 如果您是從管理系統遠端部署, 您可能會收到如*WinRM 無法處理要求的錯誤。* 若要修正此問題, 請使用 Windows PowerShell 將每部伺服器新增至管理電腦上的 [信任的主機] 清單:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> 注意: [信任的主機] 清單支援萬用字元`Server*`, 例如。
>
> 若要查看信任的主機清單, `Get-Item WSMAN:\Localhost\Client\TrustedHosts`請輸入。  
>   
> 若要清空清單, 請`Clear-Item WSMAN:\Localhost\Client\TrustedHost`輸入。  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>步驟 1.3:加入網域並新增網域帳戶

到目前為止, `<ComputerName>\Administrator`您已使用本機系統管理員帳戶設定個別伺服器。

若要管理儲存空間直接存取, 您必須將伺服器加入網域, 並使用每一部伺服器上的 Administrators 群組中的 Active Directory Domain Services 網域帳戶。

從管理系統中, 以系統管理員許可權開啟 PowerShell 主控台。 使用`Enter-PSSession`來連線到每部伺服器, 並執行下列 Cmdlet, 並以您自己的電腦名稱稱、功能變數名稱和網域認證來取代:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

如果您的存放裝置系統管理員帳戶不是 Domain Admins 群組的成員, 請將您的儲存體系統管理員帳戶新增至每個節點上的本機 Administrators 群組 (或更棒的是) 新增用於存放裝置管理者的群組。 您可以使用下列命令 (或撰寫 Windows PowerShell 函式來執行此動作-如需詳細資訊, 請參閱[使用 PowerShell 將網域使用者新增至本機群組](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx)):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>步驟 1.4:安裝角色和功能

下一步是在每一部伺服器上安裝伺服器角色。 您可以使用[Windows 系統管理中心](../../manage/windows-admin-center/use/manage-servers.md)、[伺服器管理員](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)) 或 PowerShell 來執行這項操作。 以下是要安裝的角色:

- 容錯移轉叢集
- Hyper-V
- 檔案伺服器 (如果您想要裝載任何檔案共用, 例如用於聚合式部署)
- 資料中心橋接 (如果您正在使用 RoCEv2 而非 iWARP 網路介面卡)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

若要透過 PowerShell 安裝, 請使用[install-add-windowsfeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) Cmdlet。 您可以在單一伺服器上使用它, 如下所示:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

若要在叢集中的所有伺服器上同時執行命令, 請使用這個小部分的腳本, 修改腳本開頭的變數清單, 以符合您的環境。

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

如果您要在虛擬機器內部署儲存空間直接存取, 請略過本節。

儲存空間直接存取需要在叢集中的伺服器之間具有高頻寬、低延遲的網路功能。 至少需要 10 GbE 網路, 並且建議使用遠端直接記憶體存取 (RDMA)。 您可以使用 iWARP 或 RoCE, 前提是它具有 Windows Server 2016 標誌, 但 iWARP 通常較容易設定。

> [!Important]
> 根據您的網路設備, 特別是使用 RoCE v2, 可能需要一些最上層交換器的設定。 正確的交換器設定非常重要, 可確保儲存空間直接存取的可靠性和效能。

Windows Server 2016 引進了 Hyper-v 虛擬交換器內的交換器內嵌小組 (SET)。 這可讓您在使用 RDMA 時, 將相同的實體 NIC 埠用於所有網路流量, 以減少所需的實體 NIC 埠數目。 建議儲存空間直接存取使用交換器內嵌小組。

切換或 switchless 的節點互連
- 換必須正確設定網路交換器, 才能處理頻寬和網路類型。 如果使用執行 RoCE 通訊協定的 RDMA, 網路裝置和交換器設定更為重要。
- Switchless:您可以使用直接連線來連接節點, 避免使用交換器。 每個節點都必須與叢集的每個其他節點具有直接連線。

如需設定網路功能以進行儲存空間直接存取的指示, 請參閱[Windows Server 2016 交集 NIC 和來賓 RDMA 部署指南](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx)。

## <a name="step-3-configure-storage-spaces-direct"></a>步驟 3：設定儲存空間直接存取

下列步驟要在管理系統 (與要設定的伺服器相同版本) 上完成。 下列步驟不應使用 PowerShell 會話從遠端執行, 而是在管理系統上的本機 PowerShell 會話中, 以系統管理許可權執行。

### <a name="step-31-clean-drives"></a>步驟 3.1:清理磁片磁碟機

啟用儲存空間直接存取之前, 請確定您的磁片磁碟機是空的: 沒有舊的磁碟分割或其他資料。 執行下列腳本, 並以您的電腦名稱稱取代, 以移除所有舊的磁碟分割或其他資料。

> [!Warning]
> 此腳本會永久移除作業系統開機磁碟機以外任何磁片磁碟機上的任何資料!

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

輸出看起來會像這樣, 其中**Count**是每部伺服器中每個型號的磁片磁碟機數目:

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

在此步驟中, 您將執行叢集驗證工具, 以確保伺服器節點已正確設定, 以使用儲存空間直接存取來建立叢集。 叢集驗證 (`Test-Cluster`) 在叢集建立之前執行時, 會執行測試, 確認設定是否顯示為可成功做為容錯移轉叢集。 下面的範例會使用`-Include`參數, 然後指定特定的測試類別。 這可確保驗證中包含與儲存空間直接存取相關的測試。

您可以使用下列 PowerShell 命令驗證一組用來做為儲存空間直接存取叢集的伺服器。

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>步驟 3.3:建立叢集

在此步驟中, 您將使用下列 PowerShell Cmdlet 建立一個叢集, 其中包含您在上一個步驟中已驗證用來建立叢集的節點。

建立叢集時, 您會收到一則警告, 指出: 「建立叢集角色時發生問題, 可能會使它無法啟動。 如需詳細資訊，請檢視下面的報表檔案。」 您可以放心忽略這個警告。 這是因為沒有磁碟可供叢集仲裁使用。 建議您在建立叢集之後設定檔案共用見證或雲端見證。

> [!Note]
> 如果伺服器使用靜態 IP 位址，請修改下列命令，新增下列參數並指定 IP 位址：-StaticAddress &lt;X.X.X.X&gt; 以反映靜態 IP 位址。
> 在下列命令中，ClusterName 預留位置應取代為唯一的 netbios 名稱 (15 個以下的字元)。
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

叢集建立之後，叢集名稱的 DNS 項目需要一些時間才會被複寫。 時間則取決於環境和 DNS 複寫組態。 如果解析叢集不成功，在大多數情況下，使用節點 (屬於叢集的作用中成員) 的電腦名稱而不是叢集名稱可能會成功。

### <a name="step-34-configure-a-cluster-witness"></a>步驟 3.4:設定叢集見證

我們建議您為叢集設定見證, 讓具有三部或以上伺服器的叢集可以承受兩部伺服器故障或離線。 兩部伺服器的部署需要叢集見證, 否則伺服器離線也會導致另一個無法使用。 您可以使用檔案共用作為見證，或使用雲端見證來搭配這些系統。 

如需詳細資訊，請參閱下列主題：

- [設定和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [為容錯移轉叢集部署雲端見證](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>步驟 3.5:啟用儲存空間直接存取

建立叢集之後, 請使用`Enable-ClusterStorageSpacesDirect` PowerShell Cmdlet, 這會讓存放裝置系統進入儲存空間直接存取模式, 並自動執行下列動作:

-   **建立集區:** 建立具有類似 "S2D on Cluster1" 名稱的單一大型集區。

-   **設定儲存空間直接存取快取:** 如果有多個媒體 (磁片磁碟機) 類型可供儲存空間直接存取使用, 它會啟用最快速的快取裝置 (在大多數情況下為讀取和寫入)

-   **各**建立兩層做為預設層。 一個稱為「容量」，另一個稱為「效能」。 此 Cmdlet 會分析裝置，並使用混合的裝置類型和復原功能來設定每一層。

從管理系統以系統管理員權限開啟的 PowerShell 命令視窗中，起始下列命令。 叢集名稱是您在先前步驟中建立的叢集名稱。 如果此命令在其中一個本機節點上執行，則不需要 -CimSession 參數。

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

若要使用上面的命令啟用儲存空間直接存取，您也可以使用節點名稱而不是叢集名稱。 使用節點名稱可能更可靠，因為新建立的叢集名稱可能會發生 DNS 複寫延遲。

此命令完成可能需要幾分鐘的時間，完成之後，系統就可以建立磁碟區。

### <a name="step-36-create-volumes"></a>步驟 3.6:建立磁碟區

我們建議使用`New-Volume` Cmdlet, 因為它提供最快速且最直接的體驗。 這個單一 cmdlet 會自動建立虛擬磁碟、磁碟分割以及格式化，以相符名稱建立磁碟區，並將其加入至叢集共用磁碟區 – 全在一個簡易步驟中。

如需詳細資訊，請查看[建立儲存空間直接存取中的磁碟區](create-volumes.md)。

### <a name="step-37-optionally-enable-the-csv-cache"></a>步驟 3.7:選擇性啟用 CSV 快取

您可以選擇性地啟用叢集共用磁片區 (CSV) 快取, 以使用系統記憶體 (RAM) 做為讀取作業的寫入式區塊層級快取, 但 Windows 快取管理員尚未快取該快取。 這可以改善 Hyper-v 之類應用程式的效能。 CSV 快取可以提高讀取要求的效能, 也適用于向外延展檔案伺服器案例。

啟用 CSV 快取會減少可在超融合式叢集上執行 Vm 的記憶體數量, 因此您必須在儲存體效能與 Vhd 可用的記憶體之間取得平衡。

若要設定 CSV 快取的大小, 請使用在存放裝置叢集上具有系統管理員許可權的帳戶開啟管理系統上的 PowerShell 會話, 然後使用此腳本, 適當地`$ClusterName`變更`$CSVCacheSize`和變數 (這是範例會針對每部伺服器設定 2 GB CSV 快取):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

如需詳細資訊, 請參閱[使用 CSV 記憶體內部讀取](csv-cache.md)快取。

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>步驟 3.8:部署超交集部署的虛擬機器

如果您要部署超融合式叢集, 最後一個步驟是在儲存空間直接存取叢集上布建虛擬機器。

虛擬機器的檔案應該儲存在 systems CSV 命名空間 (例如: c:\\ClusterStorage\\Volume1) 上, 就像容錯移轉叢集上的叢集 vm 一樣。

您可以使用 [內建工具] 或其他工具來管理儲存體和虛擬機器, 例如 System Center Virtual Machine Manager。

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>步驟 4：部署融合式解決方案的向外延展檔案伺服器

如果您要部署交集解決方案, 下一個步驟是建立擴充檔案伺服器實例, 並設定一些檔案共用。 如果您要部署超融合式叢集, 則您已完成, 不需要此區段。

### <a name="step-41-create-the-scale-out-file-server-role"></a>步驟 4.1:建立向外延展檔案伺服器角色

為檔案伺服器設定叢集服務的下一步是建立叢集檔案伺服器角色, 這是在您建立持續可用的檔案共用所在的擴充檔案伺服器實例時。

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>若要使用伺服器管理員建立擴充檔案伺服器角色

1. 在 [容錯移轉叢集管理員] 中, 選取叢集, 移至 [**角色**], 然後按一下 [**設定角色 ...** ]。<br>[高可用性] 嚮導隨即出現。
2. 在 [**選取角色**] 頁面上, 按一下 [**檔案伺服器**]。
3. 在 [**檔案伺服器類型**] 頁面上, 按一下 [**應用程式資料的向外延展檔案伺服器**]。
4. 在 [**用戶端存取點**] 頁面上, 輸入向外延展檔案伺服器的名稱。
5. 前往 [**角色**] 確認已成功設定角色, 並確認 [**狀態**] 欄顯示您所建立之叢集檔案伺服器角色旁的 [執行中], 如 [圖 1] 所示。

   ![顯示向外延展檔案伺服器之容錯移轉叢集管理員的螢幕擷取畫面](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "顯示向外延展檔案伺服器容錯移轉叢集管理員")

    **圖 1**顯示具有執行狀態的向外延展檔案伺服器容錯移轉叢集管理員

> [!NOTE]
>  建立叢集角色之後, 可能會有一些網路傳播延遲, 而可能會讓您無法在幾分鐘內建立檔案共用, 或可能需要更久的時間。  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>使用 Windows PowerShell 建立擴充檔案伺服器角色

 在連線到檔案伺服器叢集的 Windows PowerShell 會話中, 輸入下列命令來建立向外延展檔案伺服器角色、變更*FSCLUSTER*以符合您的叢集名稱, 以及*SOFS*以符合您想要授與的名稱向外延展檔案伺服器角色:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  建立叢集角色之後, 可能會有一些網路傳播延遲, 而可能會讓您無法在幾分鐘內建立檔案共用, 或可能需要更久的時間。 如果 SOFS 角色立即失敗且無法啟動, 可能是因為叢集的電腦物件沒有建立 SOFS 角色電腦帳戶的許可權。 如需相關說明, 請參閱這篇文章:[向外延展檔案伺服器角色無法啟動, 事件識別碼為1205、1069和 1194](http://www.aidanfinn.com/?p=14142)。

### <a name="step-42-create-file-shares"></a>步驟 4.2:建立檔案共用

建立虛擬磁片並將其新增至 Csv 之後, 就可以在其上建立檔案共用-每個虛擬磁片每個 CSV 一個檔案共用。 System Center Virtual Machine Manager (VMM) 可能是執行這項作業的 handiest 方式, 因為它會為您處理許可權, 但如果您的環境中沒有它, 您可以使用 Windows PowerShell 來部分自動化部署。

使用[適用于 Hyper-v 工作負載的 SMB 共用](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)設定腳本中包含的腳本, 這會部分自動化建立群組和共用的流程。 它是針對 Hyper-v 工作負載所撰寫, 因此, 如果您要部署其他工作負載, 您可能必須修改設定, 或在建立共用之後執行其他步驟。 例如, 如果您使用 Microsoft SQL Server, SQL Server 服務帳戶就必須被授與共享和檔案系統的完整控制權。

> [!NOTE]
>  您必須在新增叢集節點時更新群組成員資格, 除非您使用 System Center Virtual Machine Manager 來建立共用。

若要使用 PowerShell 腳本建立檔案共用, 請執行下列動作:

1. 將[適用于 Hyper-v 工作負載的 SMB 共用](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)設定所包含的腳本下載到檔案伺服器叢集的其中一個節點。
2. 在管理系統上開啟具有網域系統管理員認證的 Windows PowerShell 會話, 然後使用下列腳本建立 Hyper-v 電腦物件的 Active Directory 群組, 將變數的值變更為適用于您的環境

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. 在其中一個存放裝置節點上使用系統管理員認證開啟 Windows PowerShell 會話, 然後使用下列腳本來建立每個 CSV 的共用, 並將共用的管理許可權授與 Domain Admins 群組和計算叢集。

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

### <a name="step-43-enable-kerberos-constrained-delegation"></a>步驟4.3 啟用 Kerberos 限制委派

若要針對遠端案例管理設定 Kerberos 限制委派, 並提高即時移轉安全性, 請在其中一個存放裝置叢集節點上, 使用[適用于 Hyper-v 工作負載的 SMB 共用](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)設定所包含的 KCDSetup 腳本。 以下是腳本的小型包裝函式:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>後續步驟

部署叢集檔案伺服器之後, 我們建議您先使用綜合工作負載測試解決方案的效能, 然後再啟動任何實際的工作負載。 這可讓您確認解決方案是否正常執行, 並在增加工作負載複雜度之前, 先解決任何延遲問題。 如需詳細資訊, 請參閱[使用綜合工作負載測試儲存空間效能](https://technet.microsoft.com/library/dn894707.aspx)。

## <a name="see-also"></a>另請參閱

-   [Windows Server 2016 中的儲存空間直接存取](storage-spaces-direct-overview.md)
-   [瞭解儲存空間直接存取中的快取](understand-the-cache.md)
-   [規劃儲存空間直接存取中的磁片區](plan-volumes.md)
-   [儲存空間容錯](storage-spaces-fault-tolerance.md)
-   [儲存空間直接存取硬體需求](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [要用 RDMA，還是不用 RDMA 呢，這才是問題所在](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (TechNet 部落格)
