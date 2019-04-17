---
title: 部署儲存空間直接存取
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 8/16/2018
description: 部署軟體定義存放裝置，使用儲存空間直接存取 Windows Server 中做為超融合式基礎結構或 （也稱為分離） 的融合式基礎結構的逐步指示。
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 65e4f760be73104e67847f77e834e7b5e065211b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5429040"
---
# 部署儲存空間直接存取

> 適用於： Windows Server 2019、 Windows Server 2016

本主題提供部署[儲存空間直接存取](storage-spaces-direct-overview.md)的逐步指示。

> [!Tip]
> 想要取得超融合式基礎結構嗎？ Microsoft 建議這些[Windows Server 軟體定義](https://microsoft.com/wssd)解決方案，由我們的合作夥伴。 它們是設計、 組合，且核對我們參考架構，以確保相容性和可靠性，因此您讓啟動和執行快速。

> [!Tip]
> 您可以使用 HYPER-V 虛擬機器，包括 Microsoft Azure 中[儲存空間直接存取硬體不](storage-spaces-direct-in-vm.md)評估。 您也可以檢閱方便[Windows Server 快速實驗室部署指令碼](https://aka.ms/wslab)，我們會使用基於訓練。

## 開始之前

檢閱[儲存空間直接存取硬體需求](Storage-Spaces-Direct-Hardware-Requirements.md)，並瀏覽這份文件來自行熟悉的整體的做法及與一些步驟相關聯的重要資訊。

收集下列資訊：

- **部署選項。** 儲存空間直接存取的支援[兩個部署選項： 超融合式和融合](storage-spaces-direct-overview.md#deployment-options)，也稱為分離。 熟悉每決定最適合您的優點。 步驟 1-3 下列適用於這兩個部署選項。 步驟 4 僅用於交集的部署。

- **伺服器名稱。** 熟悉您組織的電腦、 檔案、 路徑及其他資源的命名原則。 您將需要佈建數部伺服器，每一個都有唯一的名稱。

- **網域名稱。** 熟悉您組織的網域命名原則和網域加入的原則。  您將會將伺服器加入您的網域，且您必須指定網域名稱。 

- **RDMA 網路。** There are two types of RDMA protocols: iWarp and RoCE. Note which one your network adapters use, and if RoCE, also note the version (v1 or v2). For RoCE, also note the model of your top-of-rack switch.

- **VLAN 識別碼。** 如果有的話，請注意 VLAN 識別碼，用於管理作業系統在伺服器上的網路介面卡。 您應該能夠從您的網路管理員取得。

## 步驟 1︰部署 Windows Server

### 步驟 1.1︰ 安裝作業系統

第一個步驟是將會在叢集中的每一個伺服器上安裝 Windows Server。 儲存空間直接存取需要 Windows Server 2016 Datacenter Edition。 您可以使用 [Server Core 安裝選項或伺服器含有桌面體驗。

當您安裝 Windows Server 使用安裝精靈時，您可以選擇*Windows Server* （Server Core 參考） 和*Windows Server （含有桌面體驗的伺服器）*，這是相當於 [*完整*] 安裝選項在 Windows Server 2012 R2 中使用。 如果您沒有選擇，您將取得 Server Core 安裝選項。 如需詳細資訊，請參閱[Windows Server 2016 的安裝選項](../../get-started/Windows-Server-2016.md)。

### 步驟 1.2： 連線到伺服器

本指南著重 Server Core 安裝選項和個別的管理系統，必須從遠端部署/管理：

- Windows Server 2016 與它所管理之伺服器相同的更新
- 若要管理之伺服器的網路連線
- 加入相同的網域或完全受信任的網域
- 適用於 Hyper-V 和容錯移轉叢集的遠端伺服器管理工具 (RSAT) 和 PowerShell 模組。 RSAT 工具和 PowerShell 模組用於 Windows Server 可以安裝而不安裝其他功能。 您也可以在 Windows 10 管理電腦上安裝[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。

在管理系統上，安裝容錯移轉叢集與 Hyper-V 管理工具。 這可以透過伺服器管理員使用 **「新增角色及功能」** 精靈完成。 在 **\[功能\]** 頁面上，選取 **\[遠端伺服器管理工具\]**，然後選取要安裝的工具。

進入 PS 工作階段，並使用伺服器名稱，或是您要連線之節點的 IP 位址。 您可以執行此命令之後，將會提示輸入密碼，請輸入您指定 Windows 時的系統管理員密碼。

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   以下是執行相同的動作的方式，是比較好用的指令碼，以防您需要執行此動作一次以上的範例：

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> 如果您要從管理系統從遠端進行部署，您可能會收到以下錯誤*WinRM 無法處理要求。* 若要修正這個問題，使用 Windows PowerShell 將每個伺服器新增至信任主機清單在您的管理電腦上：  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> 注意： 信任的主機清單支援萬用字元，例如`Server*`。
>
> 若要檢視您的信任主機清單，請輸入`Get-Item WSMAN:\Localhost\Client\TrustedHosts`。  
>   
> 若要空的清單，輸入`Clear-Item WSMAN:\Localhost\Client\TrustedHost`。  

### 步驟 1.3： 加入網域，以及新增網域帳戶

到目前為止您設定好的個別伺服器與本機系統管理員帳戶， `<ComputerName>\Administrator`。

若要管理儲存空間直接存取，您將需要加入網域的伺服器，並使用 Active Directory 網域服務的網域帳戶中的每一個伺服器上的 Administrators 群組。

從管理系統，請以系統管理員權限開啟 PowerShell 主控台。 使用`Enter-PSSession`連線至每個伺服器，並執行下列 cmdlet，以取代您自己的電腦名稱、 網域名稱及網域認證：

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

如果您的儲存體系統管理員帳戶不是網域系統管理員群組的成員，將您的儲存體系統管理員帳戶新增到本機 Administrators 群組內的每個節點上，或更好的方法，新增您用於存放裝置系統管理員群組。 您可以使用下列命令 （或撰寫的 Windows PowerShell 函式，若要這樣做，-如需詳細資訊，請參閱[使用 PowerShell 將網域使用者新增至本機群組](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx)）：

```
Net localgroup Administrators <Domain\Account> /add
```

### 步驟 1.4︰ 安裝角色和功能

下一個步驟是在每個伺服器上安裝伺服器角色。 您可以使用[Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md)，[伺服器管理員](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)），或 PowerShell。 以下是要安裝的角色：

- 容錯移轉叢集
- Hyper-V
- 檔案伺服器 （如果您想要裝載任何的檔案共用，例如交集的部署）
- 資料中心橋接 (如果您正在使用 RoCEv2 而非 iWARP 網路介面卡)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

若要透過 PowerShell 安裝，請使用[安裝 Add-windowsfeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet。 您可以使用它，就像這樣在單一伺服器上：

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

若要為相同的時間叢集中所有伺服器上執行命令，使用指令碼，修改的開頭的指令碼以符合您的環境變數清單這一點。

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## 步驟 2：設定網路

如果您要部署儲存空間直接存取虛擬機器內，請略過本節。

儲存空間直接存取需要高頻寬、 低延遲叢集中的伺服器之間網路功能。 至少 10 GbE 網路功能需要且建議遠端直接記憶體存取 (RDMA)。 You can use either iWARP or RoCE as long as it has the Windows Server 2016 logo, but iWARP is usually easier to set up.

> [!Important]
> Depending on your networking equipment, and especially with RoCE v2, some configuration of the top-of-rack switch may be required. 正確的參數設定是以確保可靠性和效能的儲存空間直接存取重要。

Windows Server 2016 導入了交換器內嵌小組 (SET) 內的 HYPER-V 虛擬交換器。 這可讓相同實體 NIC 連接埠，同時使用 RDMA，減少所需的實體 NIC 連接埠數目用於所有網路流量。 交換器內嵌小組建議的儲存空間直接存取。

若要設定的儲存空間直接存取的網路功能的指示，請參閱[Windows Server 2016 融合 NIC 和客體 RDMA 部署指南](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx)。

## 步驟 3：設定儲存空間直接存取

下列步驟要在管理系統 (與要設定的伺服器相同版本) 上完成。 下列步驟不應該會使用 PowerShell 工作階段，從遠端執行而改為使用系統管理權限，在管理系統上執行的本機 PowerShell 工作階段中。

### 步驟 3.1： 清除的磁碟機

啟用儲存空間直接存取之前，請確定您的磁碟機都是空的： 沒有舊的磁碟分割或其他資料。 執行下列指令碼，以取代您的電腦名稱，若要移除所有任何舊的磁碟分割或其他資料。

> [!Warning]
> 這個指令碼將會永久移除任何以外的作業系統開機磁碟機的磁碟機上的任何資料 ！

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

輸出看起來像這樣，**計數**所在的每個模型中每個伺服器的磁碟機數目：

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

### 步驟 3.2： 驗證叢集

在此步驟中，您將執行叢集驗證工具，確保伺服器節點都正確設定，以使用儲存空間直接存取建立叢集。 當叢集驗證 (`Test-Cluster`) 是在建立叢集之前執行，它會執行測試來驗證組態是否適合而能夠做為容錯移轉叢集順利運作。 直接下列範例使用`-Include`參數，然後特定的測試類別指定。 這可確保驗證中包含與儲存空間直接存取相關的測試。

您可以使用下列 PowerShell 命令驗證一組用來做為儲存空間直接存取叢集的伺服器。

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### 步驟 3.3： 建立叢集

在此步驟，您將建立與您已驗證過可用來使用下列 PowerShell cmdlet 前述步驟中建立的叢集節點的叢集。

在建立叢集時，您會收到一則警告指出-「 時發生問題，建立叢集的角色可能會導致它無法啟動。 如需詳細資訊，請檢視下面的報表檔案。」 您可以放心忽略這個警告。 這是因為沒有磁碟可供叢集仲裁使用。 建議您在建立叢集之後設定檔案共用見證或雲端見證。

> [!Note]
> 如果伺服器使用靜態 IP 位址，請修改下列命令，新增下列參數並指定 IP 位址：-StaticAddress &lt;X.X.X.X&gt; 以反映靜態 IP 位址。
> 在下列命令中，ClusterName 預留位置應取代為唯一的 netbios 名稱 (15 個以下的字元)。
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

叢集建立之後，叢集名稱的 DNS 項目需要一些時間才會被複寫。 時間則取決於環境和 DNS 複寫組態。 如果解析叢集不成功，在大多數情況下，使用節點 (屬於叢集的作用中成員) 的電腦名稱而不是叢集名稱可能會成功。

### 步驟 3.4︰ 設定叢集見證

我們建議您設定叢集見證，因此具有三個或多個伺服器的叢集可以承受兩部伺服器失敗或離線。 兩個伺服器部署需要一個叢集見證，否則任一種伺服器離線會導致其他變成也無法使用。 您可以使用檔案共用作為見證，或使用雲端見證來搭配這些系統。 

如需詳細資訊，請參閱下列主題：

- [設定和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)
- [部署容錯移轉叢集的雲端見證](../../failover-clustering/deploy-cloud-witness.md)

### 步驟 3.5：啟用儲存空間直接存取

建立叢集之後, 使用`Enable-ClusterStorageSpacesDirect`PowerShell cmdlet，這將會儲存系統進入的儲存空間直接存取模式，並自動執行下列作業：

-   **建立集區**︰建立有類似 "S2D on Cluster1" 名稱的單一大型集區。

-   **設定儲存空間直接存取快取**︰如果有多個媒體 (磁碟機) 類型可供儲存空間直接存取使用，則允許以最快速的快取裝置來執行 (在大部分情況下可讀取和寫入)

-   **區間：** 建立兩個層，做為預設階層。 一個稱為「容量」，另一個稱為「效能」。 此 Cmdlet 會分析裝置，並使用混合的裝置類型和復原功能來設定每一層。

從管理系統以系統管理員權限開啟的 PowerShell 命令視窗中，起始下列命令。 叢集名稱是您在先前步驟中建立的叢集名稱。 如果此命令在其中一個本機節點上執行，則不需要 -CimSession 參數。

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

若要使用上面的命令啟用儲存空間直接存取，您也可以使用節點名稱而不是叢集名稱。 使用節點名稱可能更可靠，因為新建立的叢集名稱可能會發生 DNS 複寫延遲。

此命令完成可能需要幾分鐘的時間，完成之後，系統就可以建立磁碟區。

### 步驟 3.6：建立磁碟區

我們建議使用`New-Volume`cmdlet，因為它提供最快速與最簡單的體驗。 這個單一 cmdlet 會自動建立虛擬磁碟、磁碟分割以及格式化，以相符名稱建立磁碟區，並將其加入至叢集共用磁碟區 – 全在一個簡易步驟中。

如需詳細資訊，請查看[建立儲存空間直接存取中的磁碟區](create-volumes.md)。

### 步驟 3.7： 選擇性地啟用 CSV 快取

您可以選擇啟用叢集共用磁碟區 (CSV) 快取寫入透過區塊層級的快取讀取已由 Windows 快取管理員不快取的作業為使用系統記憶體 (RAM)。 這可以提升效能的應用程式，例如 HYPER-V。 CSV 快取可以激發讀取要求的效能，並向外延展檔案伺服器案例對於也很有用。

啟用 CSV 快取降低記憶體可用來在超融合式叢集上，執行 Vm，所以您必須取得平衡存放裝置效能與 Vhd 可用的記憶體數量。

若要設定 CSV 快取的大小，開啟 PowerShell 工作階段在管理系統存放裝置叢集上, 擁有系統管理員權限的帳戶，然後使用此指令碼，變更`$ClusterName`和`$CSVCacheSize`做為適當的變數 (此範例會設定2 GB CSV 快取每部伺服器）：

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

如需詳細資訊，請參閱[使用 CSV 中記憶體內部讀取快取](csv-cache.md)。

### 步驟 3.8： 部署超交集部署的虛擬機器

如果您要部署的超融合式叢集，最後一個步驟是儲存空間直接存取叢集上的虛擬機器佈建。

虛擬機器的檔案應該儲存在系統的 CSV 命名空間 (範例︰c:\\ClusterStorage\\Volume1)，就像叢集容錯移轉叢集上的叢集 VM。

您可以使用內建工具或其他工具來管理儲存體和虛擬機器，例如 System Center 虛擬機器 Manager。

## 步驟 4： 融合式解決方案部署向外延展檔案伺服器

如果您要部署融合式的解決方案下, 一個步驟是建立向外延展檔案伺服器執行個體，並設定某些檔案共用。 如果您要部署超融合式叢集-您完成後，而且不需要這一節。

### 步驟 4.1： 建立向外延展檔案伺服器角色

設定您的檔案伺服器的叢集服務的下一個步驟就建立叢集的檔案伺服器角色，也就是當您建立裝載您持續可用的檔案共用的向外延展檔案伺服器執行個體。

#### 若要使用伺服器管理員中建立向外延展檔案伺服器角色

1.  在容錯移轉叢集管理員中，選取叢集，移至 [**角色**]，然後按一下 [ **...設定角色**。<br>高可用性精靈會出現。
2.  在**選取的角色**頁面上，按一下 [**檔案伺服器**。
3.  在**檔案伺服器的類型**頁面上，按一下**向外延展檔案伺服器應用程式資料**。
4.  在**用戶端存取點**頁面上，輸入向外延展檔案伺服器的名稱。
5.  請確認角色順利設定移至**角色**，並確認**狀態**欄顯示**執行**旁邊的叢集的檔案伺服器角色，您建立，如圖 1 所示。

    ![螢幕擷取畫面的容錯移轉叢集管理員顯示向外延展檔案伺服器](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Failover Cluster Manager showing the Scale-Out File Server")

     **圖 1**顯示與執行狀態向外延展檔案伺服器容錯移轉叢集管理員

> [!NOTE]
>  建立叢集的角色之後, 可能會有一些網路傳播延遲，可能會使您無法在其上建立的檔案共用，幾分鐘的時間，或可能較長的時間。  
  
#### 若要使用 Windows PowerShell 建立向外延展檔案伺服器角色

 在 Windows PowerShell 工作階段連接到檔案伺服器叢集，輸入下列命令以建立向外延展檔案伺服器角色，以符合您的叢集名稱*FSCLUSTER*及*SOFS*以符合的名稱，變更您想要授予向外延展檔案伺服器角色：

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  建立叢集的角色之後, 可能會有一些網路傳播延遲，可能會使您無法在其上建立的檔案共用，幾分鐘的時間，或可能較長的時間。 如果 SOFS 角色立即失敗，而不會啟動，它可能是因為叢集電腦物件沒有可建立 SOFS 角色的電腦帳戶的權限。 針對協助與該，請參閱此部落格文章：[向外延展檔案伺服器角色失敗到 [開始] 畫面與事件識別碼 1205，1069 和 1194年](http://www.aidanfinn.com/?p=14142)。

### 步驟 4.2： 建立檔案共用

您已經建立您的虛擬磁碟，並加入 Csv 之後，就可以建立檔案共用上他們的每個 CSV 每個虛擬磁碟的一個檔案共用。 System Center 虛擬機器 Manager (VMM) 可能是若要這樣做，因為它會處理權限，但如果您沒有它在您的環境中，您可以使用 Windows PowerShell 將部分自動化部署 handiest 的方式。

使用包含在[HYPER-V 工作負載的 SMB 共用設定](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)指令碼，部分會建立群組和共用的程序自動化的指令碼。 它撰寫對於 HYPER-V 工作負載，因此如果您要部署其他工作負載，您可能要修改設定，或在建立共用之後執行額外的步驟。 例如，如果您使用 Microsoft SQL Server，SQL Server 服務帳戶必須授與共用和檔案系統上的完整控制權。

> [!NOTE]
>  您必須更新群組成員資格，當您新增叢集節點，除非您使用 System Center 虛擬機器 Manager 來建立您的共用。

若要使用 PowerShell 指令碼建立的檔案共用，執行下列動作：

1. 下載包含[HYPER-V 工作負載的 SMB 共用設定](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)至其中一個檔案伺服器叢集的節點中的指令碼。
2. 使用網域系統管理員認證，在管理系統上開啟 Windows PowerShell 工作階段，然後使用下列指令碼建立 Active Directory 群組對於 HYPER-V 的電腦物件中，變更值的變數，視您環境：

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. 開啟 Windows PowerShell 工作階段上的其中一個存放裝置節點中，系統管理員認證，然後使用下列指令碼來為每個 CSV 建立共用，並針對共用的系統管理權限授與網域系統管理員群組和計算叢集。

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

### 步驟 4.3 啟用 Kerberos 限制委派

若要安裝遠端案例的管理以及增加的即時移轉安全性，其中包含從其中一個存放裝置叢集節點上，限制的 Kerberos 委派使用包含在[SMB 共用設定，對於 HYPER-V 工作負載](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a)KCDSetup.ps1 指令碼。 以下是一些包裝函式的指令碼：

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## 後續步驟

部署您的叢集的檔案伺服器之後，我們建議測試您的方案使用綜合工作負載在之前移任何實際的工作負載的效能。 這可讓您確認方案正常執行，然後新增複雜的工作負載之前運作所遇到的任何延遲問題。 如需詳細資訊，請參閱[測試儲存空間效能使用綜合工作負載](https://technet.microsoft.com/library/dn894707.aspx)。

## 請參閱

-   [Windows Server 2016 中的儲存空間直接存取](storage-spaces-direct-overview.md)
-   [了解儲存空間直接存取中的快取](understand-the-cache.md)
-   [規劃儲存空間直接存取中的磁碟區](plan-volumes.md)
-   [儲存空間容錯](storage-spaces-fault-tolerance.md)
-   [儲存空間直接存取的硬體需求](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [要用 RDMA，還是不用 RDMA 呢，這才是問題所在](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (TechNet 部落格)
