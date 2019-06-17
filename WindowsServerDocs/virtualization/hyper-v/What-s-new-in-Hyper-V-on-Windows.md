---
title: 在 Windows Server 2016 上的 HYPER-V 中最新消息
description: 在 HYPER-V 中提供的新功能的摘要
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 8b7d9233b105f710d620b5142205fb2eadd0248a
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/14/2019
ms.locfileid: "67141374"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>在 Windows Server 上的 HYPER-V 中最新消息

>適用於：Windows Server 2019，Microsoft HYPER-V Server 2016 中，Windows Server 2016
  
這篇文章說明在 Windows Server 2019、 Windows Server 2016 和 Microsoft HYPER-V Server 2016 的新功能和變更功能的 HYPER-V。 若要使用的新功能與 Windows Server 2012 R2 建立和移動或匯入到執行 Windows Server 2019 上 HYPER-V 的伺服器或 Windows Server 2016 的虛擬機器上，您必須手動升級虛擬機器設定版本。 如需相關指示，請參閱 <<c0> [ 升級的虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。  
  
以下是這篇文章中包含的內容，以及功能是新的或更新。  

## <a name="windows-server-version-1903"></a>Windows Server 版 1903，

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>將 HYPER-V 管理員新增至 Server Core 安裝 （更新）

您可能已經知道，我們建議使用 Windows Server，在生產環境中的半年通道時，使用 Server Core 安裝選項。 不過，預設的 Server Core 省略了一些有用的管理工具。 您可以新增許多的最常用的工具來安裝應用程式相容性功能，但仍有某些遺漏的工具。

因此，根據客戶意見反應，我們新增一個更多的工具在此版本的應用程式相容性功能：HYPER-V 管理員 (virtmgmt.msc)。

如需詳細資訊，請參閱 < [Server Core 應用程式相容性功能](../../get-started-19/install-fod-19.md)。

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>安全性：受防護的虛擬機器改良 （新）

- **分支辦公室的改善**

    您現在可以運用全新的[遞補 HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) 和[離線模式](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode)功能，在間歇連線到主機守護者服務的電腦上執行受防護的虛擬機器。 遞補 HGS 可讓您為 Hyper-V 設定第二組 URL，在無法連線到主要 HGS 伺服器時可嘗試使用。

    離線模式可在即使無法連線到 HGS 時仍繼續啟動受防護 VM，只要 VM 已成功啟動一次且主機的安全性設定未變更。

- **疑難排解增強功能**

    我們也透過啟用 VMConnect 加強的工作階段模式和 PowerShell Direct 支援，簡化[疑難排解受防護的虛擬機器](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)。 如果您遺失 VM 的網路連線，並且需要更新其設定來還原存取，這些工具會非常有用。

    這些功能不需要進行設定，當受防護 VM 放置於執行 Windows Server 版本 1803 或更新版本的 Hyper-V 主機時，就會自動變為可用。

- **Linux 支援**

    如果您執行混合的作業系統環境，Windows Server 2019 現在支援在受防護的虛擬機器中執行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>與連線待命相容\(新\)

使用 Always on/always Connected (AOAC) 電源模式的電腦上安裝 HYPER-V 角色時**連線待命**電源狀態現已推出。  
  
### <a name="discrete-device-assignment-new"></a>不連續的裝置指派\(新\)

這項功能可讓您為某些 PCIe 硬體裝置中的虛擬機器直接存取和獨佔存取權。 使用裝置，如此一來，就會略過 HYPER-V 虛擬化堆疊，會導致更快速的存取。 如需支援的硬體的詳細資訊，請參閱 「 不連續的裝置指派 」 中[適用於 Windows Server 2016 上的 HYPER-V 系統需求](System-requirements-for-Hyper-V-on-Windows.md)。 如需詳細資訊，包括如何使用這個功能和考量，請參閱文章 「[離散的裝置指派 — 描述和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)」 在虛擬化部落格。

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>適用於第 1 代虛擬機器的作業系統磁碟加密支援\(新)

您現在可以保護在第 1 代虛擬機器中使用 BitLocker 磁碟機加密作業系統磁碟。 新功能，主要的儲存體，會建立小型的專用的磁碟機，來儲存系統磁碟機的 BitLocker 金鑰。 這是完成而不是使用虛擬信賴平台模組 (TPM)，這僅適用於第 2 代虛擬機器。 若要解密磁碟，並啟動虛擬機器，HYPER-V 主機必須是已獲授權的受防護網狀架構的一部分，或其中一個虛擬機器的守護者具有私密金鑰。 金鑰的儲存體需要第 8 版的虛擬機器。 如需有關虛擬機器版本，請參閱[更新虛擬機器版本，在 Windows 10 或 Windows Server 2016 的 HYPER-V 中](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。  
  
### <a name="host-resource-protection-new"></a>主機資源保護\(新\)

這項功能可協助防止虛擬機器超過其共用系統資源的使用，藉由尋找過多的層級的活動。 這可協助降低主機或其他虛擬機器的效能時，防止虛擬機器的過多的活動。 當監視偵測到虛擬機器，具有過多的活動時，將虛擬機器有較少的資源。 此監視和強制執行預設為關閉。 使用 Windows PowerShell 來開啟或關閉。 若要將它開啟，執行此命令：  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

如需有關此指令程式的詳細資訊，請參閱 < [Set-vmprocessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor)。

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>熱新增和移除網路介面卡和記憶體\(新\)

您現在可以新增或移除網路介面卡，當虛擬機器執行時，不會產生停機時間。 這適用於執行 Windows 或 Linux 作業系統的第 2 代虛擬機器。  
  
您也可以調整指派給虛擬機器執行時，即使您沒有啟用動態記憶體的記憶體數量。 這適用於第 1 代和第 2 代虛擬機器，執行 Windows Server 2016 或 Windows 10。  

### <a name="hyper-v-manager-improvements-updated"></a>HYPER-V 管理員改善\(更新\) 
  
-   **備用認證支援**-您現在可以使用一組不同的認證在 HYPER-V 管理員中當您連接到另一部 Windows Server 2016 或 Windows 10 的遠端主機。 您也可以儲存這些認證可讓您更輕鬆地再次登入。  
  
-   **管理舊版**-使用 HYPER-V 管理員在 Windows Server 2019、 Windows Server 2016 和 Windows 10 中，您可以管理 Windows Server 2012 中，Windows 8、 Windows Server 2012 R2 和 Windows 8.1 上執行 HYPER-V 的電腦。  
  
-   **更新管理通訊協定**-HYPER-V 管理員進行通訊與遠端 HYPER-V 主機使用 WS-MAN 通訊協定允許 CredSSP、 Kerberos 或 NTLM 驗證。 當您使用 CredSSP 來連線到遠端 HYPER-V 主機時，您可以執行即時移轉而不啟用限制的委派在 Active Directory 中。 WS MAN 架構基礎結構也可讓您更輕鬆地啟用遠端管理的主機。 WS-MAN 透過連接埠 80 連線，預設會開啟此連接埠。  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>透過 Windows Update 的整合服務傳遞\(更新\) 

Windows 客體整合服務更新是透過 Windows Update 散發。 如需服務提供者和私人雲端主機服務提供者，這樣會讓租用戶擁有之虛擬機器的未授權者手中套用更新的控制項。 租用戶現在可以更新自己的 Windows 虛擬機器的所有更新，包括 integration services 中，使用單一方法。 如需 Linux 客體的整合服務的詳細資訊，請參閱[Linux 和 FreeBSD 虛擬機器在 HYPER-V 上](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。  
  
> [!IMPORTANT]  
> 不再需要 vmguest.iso 映像檔，因此它不包含 Windows Server 2016 上的 HYPER-V。  
  
### <a name="linux-secure-boot-new"></a>Linux 安全開機\(新\) 

第 2 代虛擬機器上執行的 Linux 作業系統現在可以啟用安全開機選項進行開機。 Ubuntu 14.04 和更新版本、 SUSE Linux Enterprise Server 12 和更新版本、 Red Hat Enterprise Linux 7.0 和更新版本和 CentOS 7.0 和更新版本會執行 Windows Server 2016 主機上啟用安全開機。 您第一次啟動虛擬機器之前，您必須設定虛擬機器，以使用 Microsoft UEFI 憑證授權單位。 您可以從 HYPER-V 管理員、 Virtual Machine Manager 或 提升權限的 Windows Powershell 工作階段來這樣做。 適用於 Windows PowerShell，執行下列命令：  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
如需 HYPER-V 的 Linux 虛擬機器的相關詳細資訊，請參閱[Linux 和 FreeBSD 虛擬機器在 HYPER-V 上](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。 如需有關此指令程式的詳細資訊，請參閱[Set-vmfirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware)。

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>更多的記憶體和第 2 代虛擬機器和 HYPER-V 主機的處理器\(更新\)

從第 8 版開始，第 2 代虛擬機器可以使用更多的記憶體和虛擬處理器。 主機也可以設定更多的記憶體和虛擬處理器與比先前支援。 這些變更支援新的案例，例如執行電子商務的線上交易處理 (OLTP) 和資料倉儲 (DW) 的大型記憶體內部資料庫。 Windows Server 部落格最近發佈的 5.5 tb 的記憶體和執行 4 TB 的記憶體內資料庫的 128 個虛擬處理器與虛擬機器的效能結果。 效能已超過 95%的實體伺服器的效能。 如需詳細資訊，請參閱 < [Windows Server 2016 HYPER-V 記憶體中的交易處理的大型 VM 效能](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 如需有關虛擬機器版本的詳細資訊，請參閱[更新虛擬機器版本，在 Windows 10 或 Windows Server 2016 的 HYPER-V 中](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 支援的最大組態的完整清單，請參閱[規劃 Windows Server 2016 中的 HYPER-V 延展性](./plan/plan-hyper-v-scalability-in-windows-server.md)。 

### <a name="nested-virtualization-new"></a>巢狀虛擬化\(新\)

這項功能可讓您使用做為 HYPER-V 主機的虛擬機器，並建立該虛擬化主機的虛擬機器。 這可以是特別適用於開發和測試環境。 若要使用巢狀虛擬化，您將需要：  
  
-   若要在至少執行 Windows Server 2019、 Windows Server 2016 或 Windows 10 上實體 HYPER-V 主機和虛擬化的主機。  
  
-   處理器，配備 Intel VT x （巢狀虛擬化是僅適用於 Intel 處理器這一次）。  
  
如需詳細資訊和指示，請參閱 <<c0> [ 執行 HYPER-V 的巢狀虛擬化的虛擬機器中](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)。  
  
### <a name="networking-features-new"></a>網路功能\(新\)

新的網路功能包括：  
  
-   **遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)** 。 您可以設定繫結至 HYPER-V 虛擬交換器，不論是否也會使用設定的網路介面卡的 RDMA。 集合會提供一些與 NIC 小組的相同功能的虛擬交換器。 如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。  
  
-   **虛擬機器的多個佇列 (VMMQ)** 。 藉由配置每個虛擬機器的多個硬體佇列改善 VMQ 輸送量。  預設的佇列會變成一組虛擬機器，佇列和佇列之間分散流量。  
  
-   **服務品質 (QoS 的軟體定義網路)** 。 管理透過虛擬交換器中的預設類別的頻寬流量的預設類別。  
  
如需有關新網路功能的詳細資訊，請參閱[的新網路功能](../../networking/What-s-New-in-Networking.md)。  
  
### <a name="production-checkpoints-new"></a>生產檢查點\(新\)

生產檢查點是虛擬機器的 「 時間中點 」 映像。 這些可讓您將虛擬機器執行生產工作負載時，支援原則符合的檢查點。 生產檢查點根據在客體內，而不是已儲存狀態的備份技術而定。 對於 Windows 虛擬機器，會使用磁碟區快照服務 (VSS)。 針對 Linux 虛擬機器，檔案系統緩衝區排清建立與檔案系統一致檢查點。 如果您想要使用儲存的狀態為基礎的檢查點，請改為選擇標準檢查點。 如需詳細資訊，請參閱 <<c0> [ 在 HYPER-V 中的標準或生產檢查點之間的選擇](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。  
  
> [!IMPORTANT]  
> 新的虛擬機器會使用實際執行檢查點為預設值。  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>復原 HYPER-V 叢集升級\(新\)

您現在可以新增執行 Windows Server 2019 或 Windows Server 2016 的 HYPER-V 叢集與執行 Windows Server 2012 R2 節點的節點。 這可讓您升級叢集，而不需要停機。 叢集會在 Windows Server 2012 R2 的功能層級執行，直到您升級叢集中所有節點，然後使用 Windows PowerShell cmdlet 來更新叢集功能等級[Update-clusterfunctionallevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)。  
  
> [!IMPORTANT]  
> 更新叢集功能等級之後，您無法將它返回到 Windows Server 2012 R2。  
  
針對 Windows Server 2012 R2 的功能等級，與執行 Windows Server 2012 R2、 Windows Server 2019 和 Windows Server 2016 的節點的 HYPER-V 叢集，請注意下列各項：  
  
-   管理叢集、 HYPER-V 和虛擬機器從執行 Windows Server 2016 或 Windows 10 的節點。  
  
-   您可以在所有 HYPER-V 叢集中的節點之間都移動虛擬機器。  
  
-   若要使用新的 HYPER-V 功能，所有節點都必須都執行 Windows Server 2016 或，叢集功能等級必須加以更新。  
  
-   不升級現有的虛擬機器的虛擬機器設定版本。 只在您升級叢集功能等級之後，您可以升級設定版本。  
  
-   您所建立的虛擬機器會與 Windows Server 2012 R2 虛擬機器組態層級 5 相容。  
  
之後，您要更新的叢集功能等級：  
  
-   您可以啟用 HYPER-V 的新功能。  
  
-   若要提供新的虛擬機器功能，使用更新 VmConfigurationVersion cmdlet 來手動更新虛擬機器組態層級。 如需相關指示，請參閱 <<c0> [ 升級的虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。    
-   您無法將節點新增至執行 Windows Server 2012 R2 HYPER-V 叢集。  
  
> [!NOTE]  
> 在 Windows 10 上的 HYPER-V 不支援容錯移轉叢集。  
  
如需詳細資訊和指示，請參閱 <<c0> [ 叢集作業系統輪流升級](https://technet.microsoft.com/library/dn850430.aspx)。  

### <a name="shared-virtual-hard-disks-updated"></a>共用虛擬硬碟\(更新\)
您現在可以調整共用虛擬硬碟 （.vhdx 檔案） 用於進行來賓叢集處理，而不需要停機。 共用虛擬硬碟可以成長或壓縮虛擬機器仍在線上運作。 客體叢集現在也可以保護共用虛擬硬碟進行災害復原使用 HYPER-V 複本。

集合上啟用複寫。 集合上啟用複寫時**只透過 WMI 介面公開**。 請參閱文件[Msvm_CollectionReplicationService 類別](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx)如需詳細資訊。 **您無法管理的集合，以透過 PowerShell cmdlet 或 UI 的複寫。** Vm 應該是來存取集合的特定功能，HYPER-V 叢集的一部分的主機上。 這包括共用 VHD-HYPER-V 複本不支援在獨立主機上的共用的 Vhd。

共用 vhd 中遵循的指導方針[Virtual Hard Disk Sharing Overview](https://technet.microsoft.com/library/dn281956.aspx)，並確定您共用的 Vhd 是客體叢集的一部分。 

具有共用的 VHD，但沒有相關聯的客體叢集的集合無法建立 （不論是否將共用的 VHD 包含參考點建立時，或無法） 集合的參考點。 

### <a name="virtual-machine-backupnew"></a>虛擬機器備份\(新\)

如果您要備份單一虛擬機器 （不論主機是否已叢集化），您不應該使用 VM 群組。  也不應該使用快照集的集合。 VM 群組和快照集的集合是適用於僅供備份使用共用的 vhdx 客體叢集。 相反地，您應該採取的快照集，使用[HYPER-V WMI v2 提供者](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx)。 同樣地，不要使用[容錯移轉叢集 WMI 提供者](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx)。

### <a name="shielded-virtual-machines-new"></a>受防護的虛擬機器\(新\)

受防護的虛擬機器使用的數個功能，讓它更難 HYPER-V 系統管理員和主機上的惡意程式碼檢查、 竄改或竊取資料從受防護的虛擬機器的狀態。 資料和狀態加密後，HYPER-V 系統管理員無法看到視訊輸出和磁碟，而且可以限制為只在已知的良好的主機，由主機守護者伺服器上執行虛擬機器。 如需詳細資訊，請參閱 <<c0> [ 受防護網狀架構與受防護的 Vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)。
  
> [!NOTE]  
> 受防護的虛擬機器會與 HYPER-V 複本相容。 將受防護的虛擬機器的複寫，您必須授權您想要複寫至主應用的程式，才能執行該受防護的虛擬機器。  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>啟動叢集虛擬機器的優先順序\(新\)

這項功能可讓您更充分的叢集的虛擬機器已啟動或重新啟動第一次。 這可讓您更輕鬆地開始提供服務之前使用這些服務的虛擬機器的虛擬機器。 定義集、 將虛擬機器放在集，並指定相依性。 使用 Windows PowerShell cmdlet 來管理設定，例如[新增 ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset)， [Get ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset)，並[新增 ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency)。
.  
### <a name="storage-quality-of-service-qos-updated"></a>儲存體服務品質 (QoS)\(更新\)

您現在可在向外延展檔案伺服器上建立存放裝置 QoS 原則，並將它們指派給 Hyper-V 虛擬機器上的一或多個虛擬磁碟。 儲存體效能會隨儲存體負載的變動自動進行調整以符合原則。 如需詳細資訊，請參閱 <<c0> [ 存放裝置服務品質](../../storage/storage-qos/storage-qos-overview.md)。  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>虛擬機器設定檔案格式\(更新\)

虛擬機器設定檔會使用新的格式，讓您讀取和寫入設定資料更有效率。 格式也可讓資料損毀比較不可能發生儲存體失敗時。 虛擬機器設定資料檔使用的副檔名為.vmcx 檔案和執行階段狀態資料檔案使用的副檔名為.vmrs 檔案。  
  
> [!IMPORTANT]  
> .Vmcx 副檔名表示二進位檔案。 不支援編輯.vmcx 或.vmrs 檔案。  
  
### <a name="virtual-machine-configuration-version-updated"></a>虛擬機器設定版本\(更新\)

版本代表虛擬機器的組態，以儲存狀態，以及快照集檔案的 HYPER-V 版本的相容性。 第 5 版的虛擬機器與 Windows Server 2012 R2 相容，並可在 Windows Server 2012 R2 和 Windows Server 2016 上執行。 虛擬機器與 Windows Server 2016 中引進的版本和 Windows Server 2019 不會在 HYPER-V 中執行 Windows Server 2012 R2 上。   
  
如果您移動或匯入執行 Windows Server 2016 上的 HYPER-V 的伺服器或 Windows Server 2019 的虛擬機器，從 Windows Server 2012 R2 時，不自動更新虛擬機器的設定。 這表示您可以將虛擬機器移回執行 Windows Server 2012 R2 的伺服器。 但是，這也表示您無法使用新的虛擬機器功能，直到您手動更新虛擬機器組態的版本。  
  
如需檢查和升級版本的指示，請參閱[升級的虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 這篇文章也會列出一些功能引進的版本。   
  
> [!IMPORTANT]  
> -   更新的版本之後，您無法移動虛擬機器來執行 Windows Server 2012 R2 的伺服器。  
> -   您無法降級至舊版的設定。  
> -   [更新 VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)叢集功能等級是 Windows Server 2012 R2 時，將會封鎖在 HYPER-V 叢集上的 cmdlet。  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>第 2 代虛擬機器的 Virtualization-based 安全性\(新)

虛擬化型安全性可提供功能，例如 Device Guard 和 Credential Guard，防範惡意程式碼中提供加強的保護，防止他人入侵的作業系統。 從第 8 版的層代 2 的客體虛擬機器中使用虛擬化型安全性。 如需有關虛擬機器版本，請參閱[更新虛擬機器版本，在 Windows 10 或 Windows Server 2016 的 HYPER-V 中](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="windows-containers-new"></a>Windows 容器\(新\)

Windows 容器可讓一個電腦系統上執行許多隔離的應用程式。 它們是快速的建置，而且是可高度擴充且可攜式。 兩種類型的容器執行階段可供使用，各有不同程度的應用程式隔離。 Windows Server 容器會使用命名空間和處理序隔離。 HYPER-V 容器會使用輕量級的虛擬機器，每個容器。  
  
主要功能包括：  
  
-   網站和應用程式使用 HTTPS 的支援  
  
-   Nano 伺服器可以裝載 Windows Server 和 HYPER-V 容器  
  
-   能夠管理透過容器的共用資料夾的資料  
  
-   限制容器資源的能力  
  
如需詳細資訊，包括快速入門指南，請參閱[Windows 容器文件](https://docs.microsoft.com/virtualization/windowscontainers/index)。  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell Direct\(新\)

這可讓您的虛擬機器中執行 Windows PowerShell 命令，從主應用程式。 Windows PowerShell Direct 在主機與虛擬機器之間執行。 這表示它不需要網路或防火牆需求，而且不論您的遠端管理設定運作。  
  
Windows PowerShell Direct 是現有的工具，HYPER-V 系統管理員用來連線到 HYPER-V 主機上的虛擬機器的替代方案：  
  
-   遠端管理工具，例如 PowerShell 或遠端桌面  
  
-   HYPER-V 虛擬機器連線 (VMConnect)  
  
這些工具正常運作，但有取捨：VMConnect 很可靠，但可能難以自動化。 遠端 PowerShell 很強大，但可能難以設定及維護。 這些取捨可能變得更加重要，因為 HYPER-V 部署規模擴大的。 Windows PowerShell Direct 應付這個狀況，提供功能強大的指令碼和自動化體驗，很簡單，只要使用 VMConnect。
  
如需需求和指示，請參閱[使用 PowerShell Direct 管理 Windows 虛擬機器](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)。  
