---
title: 存放裝置服務品質
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage-qos
ms.topic: get-started-article
ms.assetid: 8dcb8cf9-0e08-4fdd-9d7e-ec577ce8d8a0
author: kumudd
ms.date: 10/10/2016
ms.openlocfilehash: 1a320a53ccda78ea19c8dc7b8e22c2bb2c1d236b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854111"
---
# <a name="storage-quality-of-service"></a>存放裝置服務品質

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

Windows Server 2016 中的「存放裝置服務品質」(QoS) 可提供方法，以使用 Hyper-V 與向外延展檔案伺服器角色來集中監視和管理虛擬機器的存放裝置效能。 此功能會使用相同的檔案伺服器叢集，自動改善多個虛擬機器之間存放裝置資源的公平性，並允許使用已標準化的 IOPS 來設定以原則為基礎的最小和最大效能目標。  

您可以使用 Windows Server 2016 中的存放裝置 QoS，來完成下列動作：  

-   **減少雜訊鄰近的問題。** 根據預設，存放裝置 QoS 可確保單一虛擬機器無法取用所有存放裝置資源及佔用其他虛擬機器的存放裝置頻寬。  

-   **監視端對端儲存體效能。** 在啟動儲存於向外延展檔案伺服器上的虛擬機器之後，就能立即監視它們的效能。 所有執行中虛擬機器的效能詳細資料，以及向外延展檔案伺服器叢集的設定，都可從單一位置進行檢視  

-   **管理每個工作負載商務需求的存放裝置 I/O。** 存放裝置 QoS 原則會定義虛擬機器的效能最小值和最大值，並確保它們會相符。 即使是在密集且過多佈建的環境中，這還是能為虛擬機器提供一致性效能。 如果無法符合原則，可使用警示來追蹤 VM 何時超出原則範圍或指派了無效的原則。  

本文件概述企業如何從新的存放裝置 QoS 功能中受益。 本文件假設您先前已具備使用 Windows Server、Windows Server 容錯移轉叢集、向外延展檔案伺服器、Hyper-V 和 Windows PowerShell 的知識。

## <a name="overview"></a><a name="BKMK_Overview"></a>簡要  
本節說明使用存放裝置 QoS 的需求、使用存放裝置 QoS 的軟體定義解決方案概觀，以及一份存放裝置 QoS 相關術語的清單。  

### <a name="storage-qos-requirements"></a><a name="BKMK_Requirements"></a>存放裝置 QoS 需求  
存放裝置 QoS 支援兩種部署案例：  

-   **使用向外延展檔案伺服器的 Hyper-V。** 此案例需要下列兩項：  

    -   做為向外延展檔案伺服器叢集的存放裝置叢集  

    -   至少具有一部伺服器且已啟用 Hyper-V 角色的計算叢集  

    針對存放裝置 QoS，存放裝置伺服器上需要有容錯移轉叢集，但容錯移轉叢集中不需要計算伺服器。 所有伺服器 (用於「存放裝置」和「計算」) 都必須執行 Windows Server 2016。  

    如果您尚未針對評估用途來部署向外延展檔案伺服器叢集，如需使用現有伺服器或虛擬機器來建置一個的逐步指示，請參閱 [Windows Server 2012 R2 存放裝置︰使用儲存空間、SMB 向外延展和共用 VHDX (實體) 的逐步指示](https://blogs.technet.com/b/josebda/archive/2013/07/31/windows-server-2012-r2-storage-step-by-step-with-storage-spaces-smb-scale-out-and-shared-vhdx-physical.aspx)。  

-   **使用叢集共用磁片區的 hyper-v。** 此案例需要下列兩項：  

    -   已啟用 Hyper-V 角色的計算叢集  

    -   使用叢集共用磁碟區 (CSV) 進行儲存的 Hyper-V  

需要容錯移轉叢集。 所有伺服器都必須執行相同版本的 Windows Server 2016。  

### <a name="using-storage-qos-in-a-software-defined-storage-solution"></a><a name="BKMK_SolutionOverview"></a>在軟體定義的存放裝置解決方案中使用存放裝置 QoS  
存放裝置服務品質內建於向外延展檔案伺服器和 Hyper-V 所提供的 Microsoft 軟體定義存放裝置解決方案中。 向外延展檔案伺服器會使用 SMB3 通訊協定，向 Hyper-V 伺服器公開檔案共用。 新的原則管理員已新增到檔案伺服器叢集中，可提供中央存放裝置的效能監視。  

![向外延展檔案伺服器和存放裝置 QoS](media/overview-Clustering_SOFSStorageQoS.png)  

**圖1：在向外延展檔案伺服器的軟體定義存放裝置解決方案中使用存放裝置 QoS**  

當 Hyper-V 伺服器啟動虛擬機器時，原則管理員即會監視它們。 原則管理員會將存放裝置 QoS 原則和任何限制或保留項目傳遞回 Hyper-V 伺服器，其可適當地控制虛擬機器的效能。  

變更存放裝置 QoS 原則或虛擬機器的效能需求時，原則管理員會通知 Hyper-V 伺服器來調整其行為。 這個意見反應迴圈可確保所有虛擬機器 VHD 都會根據所定義的存放裝置 QoS 原則，以一致的方式執行。  

### <a name="glossary"></a><a name="BKMK_Glossary"></a>詞彙  

|詞彙|描述|  
|--------|---------------|  
|標準化的 IOPS|所有的存放裝置使用量都是以「標準化的 IOPS」來測量。  這是存放裝置每秒輸入/輸出作業的計數。  任何等於或小於 8 KB 的 IO 都會被視為一個標準化的 IO。  任何大於 8 KB 的 IO 都會被視為多個標準化的 IO。 例如，256 KB 的要求會被視為 32 個標準化的 IOPS。<p>Windows Server 2016 讓您能夠指定要用來將 IO 標準化的大小。  在存放裝置叢集上，可以指定標準化的大小，並在整個標準化計算叢集中生效。  預設值會維持 8 KB。|  
|流程|每個由 Hyper-V 伺服器開啟到 VHD 或 VHDX 檔案的檔案控制代碼都會被視為一個「流程」。 如果一個虛擬機器連接了兩個虛擬硬碟，則每個檔案中都會有 1 個連至檔案伺服器叢集的流程。 如果 VHDX 會與多個虛擬機器共用，則每個虛擬機器中將會有 1 個流程。|  
|InitiatorName|針對每個流程要對向外延展檔案伺服器報告的虛擬機器名稱。|  
|InitiatorID|符合虛擬機器識別碼的識別碼。  這一律可用來唯一識別個別流程的虛擬機器，即使虛擬機器具有相同的 InitiatorName 也一樣。|  
|Policy(Windows Intune 說明：原則)|存放裝置 QoS 原則會儲存於叢集資料庫中，並具有下列屬性︰PolicyId、MinimumIOPS、MaximumIOPS、 ParentPolicy 和 PolicyType。|  
|PolicyId|原則的唯一識別碼。  根據預設所產生，但可視需要加以指定。|  
|MinimumIOPS|原則將提供的最小值標準化 IOPS。  也稱為「保留項目」。|  
|MaximumIOPS|原則將限制的最大值標準化 IOPS。  也稱為「限制」。|  
|彙總 |原則類型，指定的 MinimumIOPS 與 MaximumIOPS 和 Bandwidth 會在指派給原則的所有流程中加以共用。 在該存放系統上指派原則的所有 VHD，在它們到所有共用之間都會有單一配置的 I/O 頻寬。|  
|專用|原則類型，會針對個別 VHD/VHDx 管理指定的Minimum 與 MaximumIOPS 和 Bandwidth。|  

## <a name="how-to-set-up-storage-qos-and-monitor-basic-performance"></a><a name="BKMK_SetUpQoS"></a>如何設定存放裝置 QoS 和監視基本效能  
本節說明如何啟用新的存放裝置 QoS 功能，以及如何在不套用自訂原則的情況下監視存放裝置效能。  

### <a name="set-up-storage-qos-on-a-storage-cluster"></a><a name="BKMK_SetupStorageQoSonStorageCluster"></a>在存放裝置叢集上設定存放裝置 QoS  
本節將討論如何在執行 Windows Server 2016 的新或現有容錯移轉叢集和向外延展檔案伺服器上啟用存放裝置 QoS。  

#### <a name="set-up-storage-qos-on-a-new-installation"></a>在新的安裝上設定存放裝置 QoS  
如果您已在 Windows Server 2016 上設定了新的容錯移轉叢集和叢集共用磁碟區 (CSV)，則將會自動設定存放裝置 QoS 功能。  

#### <a name="verify-storage-qos-installation"></a>確認存放裝置 QoS 的安裝  
在建立容錯移轉叢集並設定 CSV 磁碟之後，**存放裝置 QoS 資源**即會顯示為叢集核心資源，並出現在容錯移轉叢集管理員與 Windows PowerShell 中。 這表示容錯移轉叢集系統將會管理此資源，而您應該不需對此資源執行任何動作。  我們會在容錯移轉叢集管理員和 PowerShell 中顯示此資源，使其可與其他容錯移轉叢集系統資源 (例如新的健全狀況服務) 保持一致。  

![[存放裝置 QoS 資源] 會出現在 [叢集核心資源] 中](media/overview-Clustering_StorageQoSFCM.png)  

**圖2：存放裝置 QoS 資源在容錯移轉叢集管理員中顯示為叢集核心資源**  

請使用下列 PowerShell Cmdlet 來檢視存放裝置 QoS 資源的狀態。  

```PowerShell  
PS C:\> Get-ClusterResource -Name "Storage Qos Resource"  

Name                   State      OwnerGroup        ResourceType                 
----                   -----      ----------        ------------                 
Storage Qos Resource   Online     Cluster Group     Storage QoS Policy Manager  
```  

### <a name="set-up-storage-qos-on-a-compute-cluster"></a><a name="BKMK_SetupStorageQoSonComputeCluster"></a>在計算叢集上設定存放裝置 QoS  
Windows Server 2016 中的 Hyper-V 角色具備存放裝置 QoS 的內建支援，且預設會加以發行。  

#### <a name="install-remote-administration-tools-to-manage-storage-qos-policies-from-remote-computers"></a>安裝遠端系統管理工具以從遠端電腦管理存放裝置 QoS 原則  
您可以管理存放裝置 QoS 原則，並使用遠端伺服器管理工具監視來自計算主機的流程。  這些都是可在所有 Windows Server 2016 安裝中取得的選用功能，可在 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=45520)網站上針對 Windows 10 個別下載。

**RSAT-Clustering** 選用功能包括適用於遠端管理容錯移轉叢集 (包括存放裝置 QoS) 的 Windows PowerShell 模組。  

-   Windows PowerShell：Add-WindowsFeature RSAT-Clustering  

**RSAT-Hyper-V-Tools** 選用功能包括適用於遠端管理 Hyper-V 的 Windows PowerShell 模組。  

-   Windows PowerShell：Add-WindowsFeature RSAT-Hyper-V-Tools  

#### <a name="deploy-virtual-machines-to-run-workloads-for-testing"></a>基於測試目的部署虛擬機器來執行工作負載  
您需要將一些具有相關工作負載的虛擬機器儲存於向外延展檔案伺服器上。  如需如何模擬負載並執行某些壓力測試的一些秘訣，請參閱下列頁面以取得建議的工具 (DiskSpd) 和一些使用方式範例︰[DiskSpd、PowerShell 和存放裝置效能︰測量本機磁碟和 SMB 檔案共用的 IOPS、輸送量和延遲](https://blogs.technet.com/b/josebda/archive/2014/10/13/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares.aspx)。  

本指南中所示的範例案例包含五個虛擬機器。 BuildVM1、BuildVM2、BuildVM3 和 BuildVM4 正以低到中等的儲存需求來執行桌面工作負載。 TestVm1 正以高等級的儲存需求來執行線上交易處理的效能評定。  

### <a name="view-current-storage-performance-metrics"></a>檢視目前的存放裝置效能計量  
本節涵蓋：  

-   如何使用 `Get-StorageQosFlow` Cmdlet 來查詢流程。  

-   如何使用 `Get-StorageQosVolume` Cmdlet 來檢視磁碟區的效能。  

#### <a name="query-flows-using-the-get-storageqosflow-cmdlet"></a>使用 Get-StorageQosFlow Cmdlet 查詢流程  

Get-StorageQosFlow Cmdlet 會顯示目前由 Hyper-V 伺服器所起始的所有流程。 向外延展檔案伺服器叢集會收集所有資料，因此只能在向外延展檔案伺服器中的任何節點上，或使用 -CimSession 參數針對遠端伺服器，來使用此 Cmdlet。  

**下列範例命令示範如何使用 Get-StorageQoSFlow，來檢視伺服器上 Hyper-V 開啟的所有檔案**。  

```PowerShell
PS C:\> Get-StorageQosFlow  

InitiatorName    InitiatorNodeNam StorageNodeName  FilePath        Status  
                 e  
-------------    ---------------- ---------------  --------        ------  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM1         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM2         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
```  

下列範例命令已格式化，可顯示虛擬機器名稱、Hyper-V 主機名稱、IOPS 及 VHD 檔案名稱 (依 IOPS 排序)。  

```PowerShell  
PS C:\> Get-StorageQosFlow | Sort-Object StorageNodeIOPs -Descending | ft InitiatorName, @{Expression={$_.InitiatorNodeName.Substring(0,$_.InitiatorNodeName.IndexOf('.'))};Label="InitiatorNodeName"}, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName InitiatorNodeName StorageNodeIOPs Status File  
------------- ----------------- --------------- ------ ----  
WinOltp1      plang-c1                     3482     Ok IOMETER.VHDX  
BuildVM2      plang-c2                      544     Ok BUILDVM2.VHDX  
BuildVM1      plang-c2                      497     Ok BUILDVM1.VHDX  
BuildVM4      plang-c2                      316     Ok BUILDVM4.VHDX  
BuildVM3      plang-c2                      296     Ok BUILDVM3.VHDX  
BuildVM4      plang-c2                      195     Ok WIN8RTM_ENTERPRISE_VL_BU...  
TR20-VMM      plang-z400                    156     Ok DATA1.VHDX  
BuildVM3      plang-c2                       81     Ok WIN8RTM_ENTERPRISE_VL_BU...  
WinOltp1      plang-c1                       65     Ok BOOT.VHDX  
                                             18     Ok DefaultFlow  
                                             12     Ok DefaultFlow  
WinOltp1      plang-c1                        4     Ok 9914.0.AMD64FRE.WINMAIN....  
TR20-VMM      plang-z400                      4     Ok DATA2.VHDX  
TR20-VMM      plang-z400                      3     Ok BOOT.VHDX  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
```  

下列範例命令示範如何根據 InitiatorName 篩選流程，以便輕易地找到存放裝置效能與特定虛擬機器的設定。  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName BuildVm1 | Format-List

FilePath           : C:\ClusterStorage\Volume2\SHARES\TWO\BUILDWORKLOAD\BUILDVM1.V  
                     HDX  
FlowId             : ebfecb54-e47a-5a2d-8ec0-0940994ff21c  
InitiatorId        : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
InitiatorIOPS      : 464  
InitiatorLatency   : 26.2684  
InitiatorName      : BuildVM1  
InitiatorNodeName  : plang-c2.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 500  
PolicyId           : b145e63a-3c9e-48a8-87f8-1dfc2abfe5f4  
Reservation        : 500  
Status             : Ok  
StorageNodeIOPS    : 475  
StorageNodeLatency : 7.9725  
StorageNodeName    : plang-fs1.plang.nttest.microsoft.com  
TimeStamp          : 2/12/2015 2:58:49 PM  
VolumeId           : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName     :  
MaximumIops        : 500  
MinimumIops        : 500  
```  

`Get-StorageQosFlow` Cmdlet 所傳回的資料包括：  

-   Hyper-V 主機名稱 (InitiatorNodeName)。  

-   虛擬機器的名稱及其識別碼 (InitiatorName 和 InitiatorId)  

-   Hyper-V 主機最近針對虛擬磁碟所觀察到的平均效能 (InitiatorIOPS、InitiatorLatency)  

-   存放裝置叢集最近針對虛擬磁碟所觀察到的平均效能 (StorageNodeIOPS、StorageNodeLatency)  

-   目前套用到檔案的原則 (如果有的話) 和所產生的組態 (PolicyId、Reservation、Limit)  

-   原則的狀態  

    -   **Ok** - 沒有任何問題存在  

    -   InsufficientThroughput - 原則已套用，但無法傳遞最小值 IOPS。  如果某一個 VM 或所有 VM 的最小值比存放磁碟區可傳遞的還多，則可能會發生此情況。  

    -   **UnknownPolicyId** - 已將原則指派給在 Hyper-V 主機上的虛擬機器，但從檔案伺服器中遺失。  此原則應該從虛擬機器設定中移除，或者應該在檔案伺服器叢集上建立相符的原則。  

#### <a name="view-performance-for-a-volume-using-get-storageqosvolume"></a>使用 Get-StorageQosVolume 檢視磁碟區的效能  
除了每個流程的效能計量之外，系統也會在每個存放磁碟區層級上收集存放裝置效能計量。  這可讓您輕鬆查看標準化 IOPS、延遲，以及已套用至磁碟區的彙總限制和保留項目中的平均總使用率。  

```PowerShell
PS C:\> Get-StorageQosVolume | Format-List  

Interval       : 300000  
IOPS           : 0  
Latency        : 0  
Limit          : 0  
Reservation    : 0  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 434f561f-88ae-46c0-a152-8c6641561504  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 0  

Interval       : 300000  
IOPS           : 1097  
Latency        : 3.1524  
Limit          : 0  
Reservation    : 1568  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 1568  

Interval       : 300000  
IOPS           : 5354  
Latency        : 6.5084  
Limit          : 0  
Reservation    : 781  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 781  
```  

## <a name="how-to-create-and-monitor-storage-qos-policies"></a><a name="BKMK_CreateQoSPolicies"></a>如何建立和監視存放裝置 QoS 原則  
本節說明如何建立存放裝置 QoS 原則、將這些原則套用到虛擬機器，並在套用原則之後，監視存放裝置叢集。  

### <a name="create-storage-qos-policies"></a>建立存放裝置 QoS 原則  
存放裝置 QoS 原則是在向外延展檔案伺服器叢集中進行定義和管理。  您可以視需要建立多個原則以進行彈性部署 (每個存放裝置叢集最多 10,000 個)。  

每個指派給虛擬機器的 VHD/VHDX 檔案可能是使用原則來設定。 不同的檔案和虛擬機器可以使用相同的原則，或者它們每一個都可使用個別原則來設定。  如果使用相同的原則來設定多個 VHD/VHDX 檔案或多個虛擬機器，即會將它們彙總在一起，且將公平地共用 MinimumIOPS 和 MaximumIOPS。 如果您針對多個 VHD/VHDX 檔案或虛擬機器使用不同的原則，則會個別追蹤每一個的最小值和最大值。  

如果您針對不同的虛擬機器建立多個類似原則，且虛擬機器具有同等的存放裝置需求，它們將會收到類似的 IOPS 共用。  如果有一個 VM 的需求較多而其他的較少，則 IOPS 會遵循該需求。  

### <a name="types-of-storage-qos-policies"></a>存放裝置 QoS 原則的類型  
有兩種原則類型︰「彙總」(先前稱為 SingleInstance) 和「專用」(先前稱為 MultiInstance)。 彙總原則會針對一組它們所套用的 VHD/VHDX 檔案和虛擬機器組合來套用最大值和最小值。 事實上，它們會共用一組指定的 IOPS 和頻寬。 專用原則會針對每個 VHD/VHDX 個別套用最小值和最大值。 這可讓您輕鬆地建立單一原則，將類似限制套用到多個 VHD/VHDX 檔案。  

例如，假設您建立一個彙總原則，其最小值為 300 個 IOPS 且最大值為 500 個 IOPS。 如果您將此原則套用到 5 個不同的 VHD/VHDX 檔案，即可確定這 5 個 VHD/VHDX 檔案組合將保證至少有 300 個 IOPS (如果有需求且存放系統可以提供該效能)，而且不會超過 500 個 IOPS。 如果 VHD/VHDX 檔案對於 IOPS 具有類似的高度需求且存放系統可以掌握，則每個 VHD/VHDX 檔案大約可取得 100 個 IOPS。  

不過，如果您使用類似限制來建立專用原則，並將它套用到 5 個不同虛擬機器上的 VHD/VHDX 檔案，每個虛擬機器至少將取得 300 個 IOPS 且不會超過 500 個 IOPS。 如果虛擬機器對於 IOPS 具有類似的高度需求且存放系統可以掌握，則每個虛擬機器大約可取得 500 個 IOPS。 。  如果其中一個虛擬機器具有多個已設定相同 MulitInstance 原則的 VHD/VHDX 檔案，它們將會共用限制，如此一來，來自具有該原則之檔案的 VM 總 IO 將不會超過限制。  

因此，如果您有一組 VHD/VHDX 檔案，而您想要讓它們展現相同的效能特性且想省去建立多個類似原則的麻煩，您可以使用單一的專用原則並套用到每個虛擬機器的檔案。

將指派給單一匯總原則的 VHD/VHDx 檔案數目保持為20或更少。  此原則類型的目的是要在叢集上使用幾個 Vm 進行匯總。

### <a name="create-and-apply-a-dedicated-policy"></a>建立和套用專用原則  
首先，使用 `New-StorageQosPolicy` Cmdlet，在向外延展檔案伺服器上建立原則，如下列範例所示：  

```PowerShell
$desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  
```  

接下來，將它套用到 Hyper-V 伺服器上適當的虛擬機器硬碟。  記下上一個步驟的 PolicyId，或將它儲存在指令碼的變數中。  

在向外延展檔案伺服器上，使用 PowerShell 來建立存放裝置 QoS 原則，並取得其原則識別碼，如下列範例所示：  

```PowerShell
PS C:\> $desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  

C:\> $desktopVmPolicy.PolicyId  

Guid  
----  
cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

在 Hyper-V 伺服器上，使用 PowerShell，利用原則識別碼來建立存放裝置 QoS 原則，如下列範例所示：  

```PowerShell
Get-VM -Name Build* | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

### <a name="confirm-that-the-policies-are-applied"></a>確認已套用原則  
使用 `Get-StorageQosFlow` PowerShell Cmdlet，來確認已將 MinimumIOPs 和 MaximumIOPs 套用到適當的流程，如下列範例所示。  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName |  
 ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
BuildVM1          Ok         100         200             250     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             251     Ok BUILDVM2.VHDX  
BuildVM3          Ok         100         200             252     Ok BUILDVM3.VHDX  
BuildVM4          Ok         100         200             233     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               1     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               5     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666               4     Ok BOOT.VHDX  
WinOltp1          Ok           0           0               0     Ok 9914.0.AMD6...  
WinOltp1          Ok           0           0            5166     Ok IOMETER.VHDX  
WinOltp1          Ok           0           0               0     Ok BOOT.VHDX  
```  

在 Hyper-V 伺服器上，您也可以使用提供的指令碼 **Get-VMHardDiskDrivePolicy.ps1**，來查看已在虛擬硬碟上套用哪些原則。  

```PowerShell
PS C:\> Get-VM -Name BuildVM1 | Get-VMHardDiskDrive | Format-List  

Path                          : \\plang-fs.plang.nttest.microsoft.com\two\BuildWorkload  
                                \BuildVM1.vhdx  
DiskNumber                    :  
MaximumIOPS                   : 0  
MinimumIOPS                   : 0  
QoSPolicyID                   : cd6e6b87-fb13-492b-9103-41c6f631f8e0  
SupportPersistentReservations : False  
ControllerLocation            : 0  
ControllerNumber              : 0  
ControllerType                : IDE  
PoolName                      : Primordial  
Name                          : Hard Drive  
Id                            : Microsoft:AE4E3DD0-3BDE-42EF-B035-9064309E6FEC\83F8638B  
                                -8DCA-4152-9EDA-2CA8B33039B4\0\0\D  
VMId                          : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
VMName                        : BuildVM1  
VMSnapshotId                  : 00000000-0000-0000-0000-000000000000  
VMSnapshotName                :  
ComputerName                  : PLANG-C2  
IsDeleted                     : False  
```  

### <a name="query-for-storage-qos-policies"></a>存放裝置 QoS 原則的查詢  
`Get-StorageQosPolicy` 會列出所有已設定的原則及其在向外延展檔案伺服器上的狀態。  

```PowerShell
PS C:\> Get-StorageQosPolicy  

Name                 MinimumIops          MaximumIops          Status  
----                 -----------          -----------          ------  
Default              0                    0                    Ok  
Limit500             0                    500                  Ok  
SilverVm             500                  500                  Ok  
Desktop              100                  200                  Ok  
Limit500             0                    0                    Ok  
VMM                  100                  2000                 Ok  
Vdi                  1                    100                  Ok  
```  

狀態會根據系統的執行方式，隨著時間而改變。  

-   **Ok** - 使用該原則的所有流程正在接收其要求的 MinimumIOPs。  

-   **InsufficientThroughput** - 一或多個使用此原則的流程不會接收最小值 IOPS  

您也可以使用管線傳送原則給 `Get-StorageQosPolicy`，以取得所有設定來使用原則的流程狀態，如下所示：  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name Desktop | Get-StorageQosFlow | ft InitiatorName, *IOPS, Status, FilePath -AutoSize  

InitiatorName MaximumIops MinimumIops InitiatorIOPS StorageNodeIOPS Status FilePat  
                                                                           h  
------------- ----------- ----------- ------------- --------------- ------ -------  
BuildVM4              100          50           187              17     Ok C:\C...  
BuildVM3              100          50           194              25     Ok C:\C...  
BuildVM1              200         100           195             196     Ok C:\C...  
BuildVM2              200         100           193             192     Ok C:\C...  
BuildVM4              200         100           187             169     Ok C:\C...  
BuildVM3              200         100           194             169     Ok C:\C...  
```  

### <a name="create-an-aggregated-policy"></a>建立彙總原則  
如果您想要讓多個虛擬硬碟共用單一集區的 IOPS 和頻寬，可以使用彙總原則。  例如，如果您從兩個虛擬機器將相同的彙總原則套用到硬碟，即會根據需求在它們之間將最小值分割開來。  系統將會保證這兩個磁碟合併的最小值，且它們一起將不會超過指定的最大值 IOPS 或頻寬。  

相同的方法也可用來針對虛擬機器的所有 VHD/VHDx 檔案提供單一配置，這類虛擬機器會包含一項服務或屬於多主機環境中的租用戶。  

建立指定的 PolicyType 以外的「專用」和「彙總」原則程序並無任何差異。  

下列範例示範如何建立彙總的存放裝置 QoS 原則，並在向外延展檔案伺服器上取得它的 policyID：  

```PowerShell
PS C:\> $highPerf = New-StorageQosPolicy -Name SqlWorkload -MinimumIops 1000 -MaximumIops 5000 -PolicyType Aggregated  
[plang-fs]: PS C:\Users\plang\Documents> $highPerf.PolicyId  

Guid  
----  
7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

下列範例示範如何使用在上述範例中取得的 policyID，在 Hyper-V 伺服器上套用存放裝置 QoS 原則：  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID 7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

下列範例示範如何從檔案伺服器檢視存放裝置 QoS 原則的效果：  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | format-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | for  
mat-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
```  

每個虛擬硬碟都將根據其負載調整 MinimumIOPs、MaximumIOPs 及 MaximumIobandwidth 值。  這可確保針對磁碟群組所使用的頻寬總量會保持在原則所定義的範圍內。  在上述範例中，前兩個磁碟會處於閒置狀態，而第三個磁碟最多可以使用最大值 IOPS。  如果前兩個磁碟再次開始發出 IO，則第三個磁碟的最大值 IOPS 將會自動降低。  

### <a name="modify-an-existing-policy"></a>修改現有的原則  
建立原則之後，即可變更 Name、MinimumIOPs、MaximumIOPs 及 MaximumIoBandwidth 的屬性。  不過，在建立原則之後，就無法變更原則類型 (彙總/專用)。  

下列 Windows PowerShell Cmdlet 示範如何變更現有原則的 MaximumIOPs 屬性：  

```PowerShell
[DBG]: PS C:\demoscripts>> Get-StorageQosPolicy -Name SqlWorkload | Set-StorageQosPolicy -MaximumIops 6000  
```  

下列 Cmdlet 會確認變更：  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
SqlWorkload             1000                   6000                   Ok    

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageQosPolicy -Name SqlWorkload | Get-Storag  
eQosFlow | Format-Table InitiatorName, PolicyId, MaximumIops, MinimumIops, StorageNodeIops -A  
utoSize  

InitiatorName PolicyId                             MaximumIops MinimumIops StorageNodeIops  
------------- --------                             ----------- ----------- ---------------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        6000        1000            4507  
```  

## <a name="how-to-identify-and-address-common-issues"></a><a name="BKMK_KnownIssues"></a>如何識別及解決常見問題  
本節說明如何尋找具有無效存放裝置 QoS 原則的虛擬機器、如何重新建立比對原則、如何從虛擬機器移除原則，以及如何識別不符合存放裝置 QoS 原則需求的虛擬機器。  

### <a name="identify-virtual-machines-with-invalid-policies"></a><a name="BKMK_FindingVMsWithInvalidPolicies"></a>識別具有無效原則的虛擬機器  

如果在從虛擬機器移除原則之前，先從檔案伺服器中刪除該原則，則虛擬機器將會繼續執行，就像未曾套用任何原則一樣。  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload | Remove-StorageQosPolicy  

Confirm  
Are you sure you want to perform this action?  
Performing the operation "DeletePolicy" on target "MSFT_StorageQoSPolicy (PolicyId =  
"7e2f3e73-1ae4-4710-8219-0769a4aba072")".  
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):  
```  

流程的狀態現在會顯示 "UnknownPolicyId"  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName          Status MinimumIops MaximumIops StorageNodeIOPs          Status File  
-------------          ------ ----------- ----------- ---------------          ------ ----  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0              10              Ok Def...  
                           Ok           0           0              13              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
BuildVM1                   Ok         100         200             193              Ok BUI...  
BuildVM2                   Ok         100         200             196              Ok BUI...  
BuildVM3                   Ok          50          64              17              Ok WIN...  
BuildVM3                   Ok          50         136             179              Ok BUI...  
BuildVM4                   Ok          50         100              23              Ok WIN...  
BuildVM4                   Ok         100         200             173              Ok BUI...  
TR20-VMM                   Ok          33         666               2              Ok DAT...  
TR20-VMM                   Ok          25         975               3              Ok DAT...  
TR20-VMM                   Ok          75        1025              12              Ok BOO...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId 991...  
WinOltp1      UnknownPolicyId           0           0            4926 UnknownPolicyId IOM...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId BOO...  
```  

#### <a name="recreate-a-matching-storage-qos-policy"></a><a name="BKMK_RecreateMatchingPolicy"></a>重新建立符合的存放裝置 QoS 原則  
如果已在無意中移除原則，您可以使用舊的 PolicyId 建立一個新的原則。  首先，取得所需的 PolicyId  

```PowerShell
PS C:\> Get-StorageQosFlow -Status UnknownPolicyId | ft InitiatorName, PolicyId -AutoSize  

InitiatorName PolicyId  
------------- --------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

接下來，使用該 PolicyId 建立新的原則  

```PowerShell
PS C:\> New-StorageQosPolicy -PolicyId 7e2f3e73-1ae4-4710-8219-0769a4aba072 -PolicyType Aggregated -Name RestoredPolicy -MinimumIops 100 -MaximumIops 2000  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
RestoredPolicy          100                    2000                   Ok  
```  

最後，確認已套用該原則。  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               8     Ok DefaultFlow  
                  Ok           0           0               9     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
BuildVM1          Ok         100         200             192     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             193     Ok BUILDVM2.VHDX  
BuildVM3          Ok          50         100              24     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM3          Ok         100         200             166     Ok BUILDVM3.VHDX  
BuildVM4          Ok          50         100              12     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM4          Ok         100         200             178     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666              10     Ok BOOT.VHDX  
WinOltp1          Ok          25         500               0     Ok 9914.0.AMD64FRE.WINMA...  
```  

#### <a name="remove-storage-qos-policies"></a><a name="BKMK_RemovePolicyFromVM"></a>移除存放裝置 QoS 原則  

如果已刻意移除原則，或者 VM 是使用您不再需要的原則所匯入，則可能會移除該原則。  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID $null  
```  

一旦從虛擬硬碟設定中移除 PolicyId 之後，狀態將會是 "Ok"，且將不會套用最小值或最大值。  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ----------- ----------- --------------- ------ ----  
                        0           0               0     Ok DefaultFlow  
                        0           0              16     Ok DefaultFlow  
                        0           0              12     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
BuildVM1              100         200             197     Ok BUILDVM1.VHDX  
BuildVM2              100         200             192     Ok BUILDVM2.VHDX  
BuildVM3                9           9              23     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM3               91         191             171     Ok BUILDVM3.VHDX  
BuildVM4                8           8              18     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM4               92         192             163     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               2     Ok DATA2.VHDX  
TR20-VMM               33         666               1     Ok DATA1.VHDX  
TR20-VMM               33         666               5     Ok BOOT.VHDX  
WinOltp1                0           0               0     Ok 9914.0.AMD64FRE.WINMAIN.1412...  
WinOltp1                0           0            1811     Ok IOMETER.VHDX  
WinOltp1                0           0               0     Ok BOOT.VHDX  
```  

### <a name="find-virtual-machines-that-are-not-meeting-storage-qos-policies"></a><a name="BKMK_VMsThatDoNotMeetStorageQoSPoilicies"></a>尋找不符合存放裝置 QoS 原則的虛擬機器  
**InsufficientThroughput** 狀態會指派給任何符合下列情況的流程：  

-   已由原則設定最小值定義的 IOPS；以及  

-   正在以符合或超過最小值的比率起始 IO；以及  

-   尚未達到最小值 IOP 率  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs                 Status File  
------------- ----------- ----------- ---------------                 ------ ----  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0              15                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
BuildVM3               50         100              20                     Ok WIN8RTM_ENTE...  
BuildVM3              100         200             174                     Ok BUILDVM3.VHDX  
BuildVM4               50         100              11                     Ok WIN8RTM_ENTE...  
BuildVM4              100         200             188                     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               3                     Ok DATA1.VHDX  
TR20-VMM               78        1032             180                     Ok BOOT.VHDX  
TR20-VMM               22         968               4                     Ok DATA2.VHDX  
WinOltp1             3750        5000               0                     Ok 9914.0.AMD64...  
WinOltp1            15000       20000           11679 InsufficientThroughput IOMETER.VHDX  
WinOltp1             3750        5000               0                     Ok BOOT.VHDX  
```  

您可以判斷流程的任何狀態，包括 **InsufficientThroughput**，如下列範例所示：  

```PowerShell
PS C:\> Get-StorageQosFlow -Status InsufficientThroughput | fl  

FilePath           : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
FlowId             : 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
InitiatorId        : 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
InitiatorIOPS      : 12168  
InitiatorLatency   : 22.983  
InitiatorName      : WinOltp1  
InitiatorNodeName  : plang-c1.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 20000  
PolicyId           : 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
Reservation        : 15000  
Status             : InsufficientThroughput  
StorageNodeIOPS    : 12181  
StorageNodeLatency : 22.0514  
StorageNodeName    : plang-fs2.plang.nttest.microsoft.com  
TimeStamp          : 2/13/2015 12:07:30 PM  
VolumeId           : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName     :  
MaximumIops        : 20000  
MinimumIops        : 15000  
```  

## <a name="monitor-health-using-storage-qos"></a><a name="BKMK_Health"></a>使用存放裝置 QoS 監視健全狀況  
新的健全狀況服務可以簡化存放裝置叢集的監視，提供單一位置來檢查任一個節點中是否有任何可採取動作的事件。 本節說明如何使用 `debug-storagesubsystem` Cmdlet，來監視存放裝置叢集的健康情況。  

### <a name="view-storage-status-with-debug-storagesubsystem"></a>使用 Debug-StorageSubSystem 檢視存放裝置狀態  
叢集儲存空間也會在單一位置中提供存放裝置叢集健康情況的相關資訊。 這可協助系統管理員快速識別目前在存放裝置部署中的問題，並在問題出現或解除時進行監視。  

#### <a name="vm-with-invalid-policy"></a>具有無效原則的 VM  
具有無效原則的 VM 也會透過存放子系統的健康情況監視進行報告。  以下範例的狀態與本文件的[尋找具有無效原則的 VM](#BKMK_FindingVMsWithInvalidPolicies) 一節中所述的狀態相同。  

```PowerShell
C:\> Get-StorageSubSystem -FriendlyName Clustered* | Debug-StorageSubSystem  

EventTime                 :  
FaultId                   : 0d16d034-9f15-4920-a305-f9852abf47c3  
FaultingObject            :  
FaultingObjectDescription : Storage QoS Policy 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
FaultingObjectLocation    :  
FaultType                 : Storage QoS policy used by consumer does not exist.  
PerceivedSeverity         : Minor  
Reason                    : One or more storage consumers (usually Virtual Machines) are  
                            using a non-existent policy with id  
                            5d1bf221-c8f0-4368-abcf-aa139e8a7c72. Consumer details:  

                            Flow ID: 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
                            Initiator ID: 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
                            Initiator Name: WinOltp1  
                            Initiator Node: plang-c1.plang.nttest.microsoft.com  
                            File Path:  
                            C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
RecommendedActions        : {Reconfigure the storage consumers (usually Virtual Machines)  
                            to use a valid policy., Recreate any missing Storage QoS  
                            policies.}  
PSComputerName            :  
```  

#### <a name="lost-redundancy-for-a-storage-spaces-virtual-disk"></a>遺失儲存空間虛擬磁碟的備援  

在此範例中，叢集儲存空間有一個已建立為三向鏡像的虛擬磁碟。  失敗的磁碟已從系統中移除，但未新增取代的磁碟。  存放子系統報告遺失了 HealthStatus 為 **Warning**，但 OperationalStatus 為 **OK** 的備援，因為磁碟區仍在線上。  

```PowerShell
PS C:\> Get-StorageSubSystem -FriendlyName Clustered*  

FriendlyName                   HealthStatus                   OperationalStatus  
------------                   ------------                   -----------------  
Clustered Windows Storage o... Warning                        OK  

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageSubSystem -FriendlyName Clustered* | Deb  
ug-StorageSubSystem  

EventTime                 :  
FaultId                   : dfb4b672-22a6-4229-b2ed-c29d7485bede  
FaultingObject            :  
FaultingObjectDescription : Virtual disk 'Two'  
FaultingObjectLocation    :  
FaultType                 : VirtualDiskDegradedFaultType  
PerceivedSeverity         : Minor  
Reason                    : Virtual disk 'Two' does not have enough redundancy remaining to  
                            successfully repair or regenerate its data.  
RecommendedActions        : {Rebalance the pool, replace failed physical disks, or add new  
                            physical disks to the storage pool, then repair the virtual  
                            disk.}  
PSComputerName            :  
```  

### <a name="sample-script-for-continuous-monitoring-of-storage-qos"></a>持續監視存放裝置 QoS 的範例指令碼  

本節包含的範例指令碼將示範如何使用 WMI 指令碼來監視常見的錯誤。  其設計目的是讓開發人員能夠開始即時擷取健康情況事件。  

**範例腳本：**  

```PowerShell
param($cimSession)  
# Register and display events  
Register-CimIndicationEvent -Namespace root\microsoft\windows\storage -ClassName msft_storagefaultevent -CimSession $cimSession  

while ($true)  
{  
     $e = (Wait-Event)  
     $e.SourceEventArgs.NewEvent  
     Remove-Event $e.SourceIdentifier  
}  
```  

## <a name="frequently-asked-questions"></a>常見問題集  

### <a name="how-do-i-retain-a-storage-qos-policy-being-enforced-for-my-virtual-machine-if-i-move-its-vhdvhdx-files-to-another-storage-cluster"></a>如果我將存放裝置 QoS 原則的 VHD/VHDx 檔案移到其他存放裝置叢集，該如何針對我的虛擬機器持續強制執行該原則？  

指定原則的 VHD/VHDX 檔案上的設定是原則識別碼的 GUID。  建立原則時，您可以使用 **PolicyID** 參數來指定 GUID。  如果未指定該參數，即會建立隨機的 GUID。  因此，您可以在 VM 目前儲存其 VHD/VHDX 檔案的存放裝置叢集上取得 PolicyID，並在目的地存放裝置叢集上建立相同原則，然後指定要使用相同的 GUID 來建立該原則。  將 VM 檔案移到新的存放裝置叢集時，具有相同 GUID 的原則將會生效。  

您可以使用 System Center Virtual Machine Manager 來將原則套用到多個存放裝置叢集，這能讓這個案例變得更容易。  
### <a name="if-i-change-the-storage-qos-policy-why-dont-i-see-it-take-effect-immediately-when-i-run-get-storageqosflow"></a>當我變更存放裝置 QoS 原則時，為何我不會在執行 Get-StorageQoSFlow 時看到它立即生效？  

如果您有一個流程正達到原則的最大值且您變更了該原則以使其變得更高或更低，而您接著立即使用 PowerShell Cmdlet 來判斷流程的延遲/IOPS/頻寬，則最多需要 5 分鐘，才能顯示在流程上變更原則的完整效果。  新的限制會在幾秒鐘內生效，但 **Get-StorgeQoSFlow** PowerShell Cmdlet 會利用一個 5 分鐘的滑動視窗來使用每個計數器的平均值。  否則，如果它目前正在顯示值，而您在某一列中多次執行 PowerShell Cmdlet，則您可能會看到截然不同的值，因為適用於 IOPS 和延遲的值在每秒之間都會呈現大幅波動。

### <a name="what-new-functionality-was-added-in-windows-server-2016"></a><a name="BKMK_Updates"></a>Windows Server 2016 中新增了哪些新功能

在 Windows Server 2016 中，已將存放裝置 QoS 原則類型名稱重新命名。  **Multi-instance** 原則類型已重新命名為**專用**，而 **Single-instance** 已重新命名為**彙總**。 同時也修改了專用原則的管理行為 - 相同虛擬機器中已將相同**專用**原則套用到其中的 VHD/VHDX 檔案將不會共用 I/O 配置。  

Windows Server 2016 中有兩個新的存放裝置 QoS 功能：  

-   **最大頻寬**  

    Windows Server 2016 中的存放裝置 QoS 讓您能夠指定已指派給原則之流程可能會耗用的最大頻寬。  在 **StorageQosPolicy** Cmdlet 中指定它時參數為 **MaximumIOBandwidth**，而輸出會以每秒位元組來表示。  
    如果已在原則中設定 **MaximimIops** 和 **MaximumIOBandwidth**，則它們都將生效，而流程第一個達到的項目將會限制流程的 I/O。  

-   **IOPS 正規化是可設定的**  

    存放裝置 QoS 會使用標準化的 IOPS。  預設值是使用 8 K 的標準化大小。  Windows Server 2016 中的存放裝置 QoS 讓您能夠為存放裝置叢集指定不同的標準化大小。  這個標準化大小會影響存放裝置叢集上的所有流程，並在變更之後立即生效 (在幾秒內)。  最小值是 1 KB，而最大值為 4 GB (建議不要設定超過 4 MB，因為很少會有超過 4 MB 的 IO)。  

    當您因為標準化計算中的變更而變更 IOPS 標準化時，要考慮的是相同 IO 模式/輸送量在存放裝置 QoS 輸出中顯示不同的 IOPS 數量。  如果您正在比較存放裝置叢集之間的 IOPS，您可能也想要確認每一個正在使用的標準化值，因為它將影響所報告的標準化 IOPS。    

#### <a name="example-1-creating-a-new-policy-and-viewing-the-maximum-bandwidth-on-the-storage-cluster"></a>範例 1︰建立新的原則，並檢視存放裝置叢集上的最大頻寬  
在 PowerShell 中，您可以指定一個用數字表示的單位。  下列範例使用 10 MB 做為最大頻寬值。  存放裝置 QoS 將轉換此值，並以每秒位元組為單位來儲存它。因此，會將 10 MB 轉換為每秒 10485760 個位元組。  

```PowerShell
PS C:\Windows\system32> New-StorageQosPolicy -Name HR_VMs -MaximumIops 1000 -MinimumIops 20 -MaximumIOBandwidth 10MB  

Name   MinimumIops MaximumIops MaximumIOBandwidth Status  
----   ----------- ----------- ------------------ ------  
HR_VMs 20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQosPolicy  

Name    MinimumIops MaximumIops MaximumIOBandwidth Status  
----    ----------- ----------- ------------------ ------  
Default 0           0           0                  Ok  
HR_VMs  20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQoSFlow | fL InitiatorName,FilePath,InitiatorIOPS,InitiatorLatency,InitiatorBandwidth  

InitiatorName      : testsQoS  
FilePath           : C:\ClusterStorage\Volume2\TESTSQOS\VIRTUAL HARD DISKS\TESTSQOS.VHDX  
InitiatorIOPS      : 5  
InitiatorLatency   : 1.5455  
InitiatorBandwidth : 37888  
```  

#### <a name="example-2-get-iops-normalization-settings-and-specify--a-new-value"></a>範例 2︰取得 IOPS 標準化設定，並指定新值  

下列範例示範如何取得存放裝置叢集的 IOPS 標準化設定 (預設值為 8 KB)，接著將它設定為 32 KB，然後再次顯示。  注意，在此範例中，請指定 "32 KB"，因為 PowerShell 可讓您指定單位，而不需轉換為位元組。   輸出會以每秒位元組為單位來顯示值。  

```PowerShell
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
8192  

PS C:\Windows\system32> Set-StorageQosPolicyStore -IOPSNormalizationSize 32KB  
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
32768  
```    

## <a name="see-also"></a>另請參閱  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Windows Server 2016 中的儲存體複本](../storage-replica/storage-replica-overview.md)  
- [Windows Server 2016 中的儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)  
