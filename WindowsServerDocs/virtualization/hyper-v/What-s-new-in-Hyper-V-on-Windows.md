---
title: Windows Server 2016 上的 Hyper-v 新功能
description: 提供 Hyper-v 新功能的摘要
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
ms.author: benarm
author: BenjaminArmstrong
ms.date: 09/21/2017
ms.openlocfilehash: 2a6107468e63f819e1957db0736c1a07c5dc24a0
ms.sourcegitcommit: faa5db4cdba4ad2b3a65533b6b49d960080923c9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91752878"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 新功能

>適用于： Windows Server 2019、Microsoft Hyper-V Server 2016、Windows Server 2016

本文說明 Windows Server 2019、Windows Server 2016 和 Microsoft Hyper-V Server 2016 上的 Hyper-v 的新功能和已變更的功能。 若要在以 Windows Server 2012 R2 建立的虛擬機器上使用新功能，並將其移動或匯入到在 Windows Server 2019 或 Windows Server 2016 上執行 Hyper-v 的伺服器，您必須手動升級虛擬機器設定版本。 如需相關指示，請參閱 [升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。

以下是本文所包含的功能，以及功能是新的或更新的。

## <a name="windows-server-version-1903"></a>Windows Server 版本 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>將 Hyper-v 管理員新增至 Server Core 安裝 (更新) 

您可能已經知道，在生產環境中使用 Windows Server 半年通道時，我們會建議使用核心安裝選項。 不過，Server Core 預設會省略一些有用的管理工具。 您可以藉由安裝應用程式相容性功能，來新增許多最常用的工具，但仍會遺漏某些工具。

因此，根據客戶的意見反應，我們已在此版本中為應用程式相容性功能新增了另一項工具： Hyper-v 管理員 (virtmgmt) 。

如需詳細資訊，請參閱 [Server Core 應用程式相容性功能](../../get-started-19/install-fod-19.md)。

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>安全性： (新) 的受防護虛擬機器改進

- **分公司改進功能**

    您現在可以運用全新的[遞補 HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#fallback-configuration) 和[離線模式](../../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#offline-mode)功能，在間歇連線到主機守護者服務的電腦上執行受防護的虛擬機器。 遞補 HGS 可讓您為 Hyper-V 設定第二組 URL，在無法連線到主要 HGS 伺服器時可嘗試使用。

    離線模式可在即使無法連線到 HGS 時仍繼續啟動受防護 VM，只要 VM 已成功啟動一次且主機的安全性設定未變更。

- **疑難排解改進功能**

    我們也透過啟用 VMConnect 加強的工作階段模式和 PowerShell Direct 支援，簡化[疑難排解受防護的虛擬機器](../../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms.md)。 如果您遺失 VM 的網路連線，並且需要更新其設定來還原存取，這些工具會非常有用。

    這些功能不需要進行設定，當受防護 VM 放置於執行 Windows Server 版本 1803 或更新版本的 Hyper-V 主機時，就會自動變為可用。

- **Linux 支援**

    如果您執行混合的作業系統環境，Windows Server 2019 現在支援在受防護的虛擬機器中執行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>與連線待命新的相容 \(\)

當 Hyper-v 角色安裝在使用 Always On/Always Connected (AOAC) 電源模型的電腦上時， **已連線的待命** 電源狀態現在已可供使用。

### <a name="discrete-device-assignment-new"></a>新的不同裝置指派 \(\)

這項功能可讓您將虛擬機器直接存取和獨佔存取權提供給某些 PCIe 硬體裝置。 以這種方式使用裝置會略過 Hyper-v 虛擬化堆疊，進而加快存取的速度。 如需支援硬體的詳細資訊，請參閱 [Windows Server 2016 上的 Hyper-v 系統需求中的](System-requirements-for-Hyper-V-on-Windows.md)「離散裝置指派」。 如需詳細資訊，包括如何使用這項功能和考慮，請參閱虛擬化 blog 中的「[離散裝置指派-描述和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)」文章。

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>第1代虛擬機器新) 中作業系統磁片的加密支援 \(

您現在可以使用第1代虛擬機器中的 BitLocker 磁片磁碟機加密來保護作業系統磁片。 金鑰儲存體是一項新功能，可建立小型的專用磁片磁碟機來儲存系統磁片磁碟機的 BitLocker 金鑰。 這是為了完成，而不是使用虛擬的可信賴平臺模組 (TPM) ，其僅適用于第2代虛擬機器。 若要解密磁片並啟動虛擬機器，Hyper-v 主機必須是授權的受保護網狀架構的一部分，或有其中一部虛擬機器保護的私密金鑰。 金鑰儲存體需要第8版的虛擬機器。 如需虛擬機器版本的詳細資訊，請參閱 [Windows 10 或 Windows Server 2016 上的 Hyper-v 升級虛擬機器版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="host-resource-protection-new"></a>主機資源保護 \( 新增\)

這項功能可透過尋找過度的活動層級，來防止虛擬機器使用超過其系統資源的共用。 這有助於防止虛擬機器的過度活動降低主機或其他虛擬機器的效能。 當監視偵測到具有過多活動的虛擬機器時，會提供較少的資源給虛擬機器。 此監視和強制預設為關閉。 使用 Windows PowerShell 開啟或關閉。 若要開啟它，請執行下列命令：

```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

如需此 Cmdlet 的詳細資訊，請參閱 [get-vmprocessor](/powershell/module/hyper-v/set-vmprocessor)。

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>網路介面卡 \( 和記憶體的熱新增和移除\)

您現在可以在虛擬機器正在執行時新增或移除網路介面卡，而不會產生停機時間。 這適用于執行 Windows 或 Linux 作業系統的第2代虛擬機器。

您也可以在虛擬機器執行時調整指派給虛擬機器的記憶體數量，即使您尚未啟用動態記憶體也一樣。 這適用于執行 Windows Server 2016 或 Windows 10 的第1代和第2代虛擬機器。

### <a name="hyper-v-manager-improvements-updated"></a>已更新 hyper-v 管理員改進 \(\)

-   **替代認證支援** -您現在可以在連接到另一個 Windows Server 2016 或 Windows 10 遠端主機時，在 hyper-v 管理員中使用一組不同的認證。 您也可以儲存這些認證，讓您更輕鬆地登入。

-   **管理舊版** -透過 windows server 2019、windows server 2016 和 Windows 10 中的 hyper-v 管理員，您可以管理在 windows server 2012、Windows 8、windows Server 2012 R2 和 Windows 8.1 上執行 hyper-v 的電腦。

-   **更新的管理通訊協定** -hyper-v 管理員現在會使用 ws-management 通訊協定與遠端 hyper-v 主機通訊，此通訊協定允許 CredSSP、KERBEROS 或 NTLM 驗證。 當您使用 CredSSP 連線到遠端 Hyper-v 主機時，您可以進行即時移轉，而不需要在 Active Directory 中啟用限制委派。 以 WS-MANAGEMENT 為基礎的基礎結構也可讓您更輕鬆地讓主機進行遠端系統管理。 WS-MAN 透過連接埠 80 連線，預設會開啟此連接埠。

### <a name="integration-services-delivered-through-windows-update-updated"></a>透過 Windows Update 更新傳遞的 Integration services \(\)

適用于 Windows 來賓的 integration services 更新會透過 Windows Update 散發。 針對服務提供者和私用雲端主控者，這可讓您控制將更新套用到擁有虛擬機器的租使用者。 租使用者現在可以使用單一方法，以所有更新（包括整合服務）更新其 Windows 虛擬機器。 如需 Linux 來賓整合服務的詳細資訊，請參閱 [hyper-v 上的 linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。

> [!IMPORTANT]
> 不再需要 vmguest.iso .iso 影像檔案，因此它不會包含在 Windows Server 2016 上的 Hyper-v 中。

### <a name="linux-secure-boot-new"></a>Linux 安全開機 \( 新\)

在第2代虛擬機器上執行的 Linux 作業系統現在可以在啟用安全開機選項的情況下開機。 Ubuntu 14.04 和更新版本、SUSE Linux Enterprise Server 12 和更新版本、Red Hat Enterprise Linux 7.0 和更新版本，以及 CentOS 7.0 和更新版本，可在執行 Windows Server 2016 的主機上啟用安全開機。 第一次啟動虛擬機器之前，您必須先將虛擬機器設定為使用 Microsoft UEFI 憑證授權單位單位。 您可以從 Hyper-v 管理員、Virtual Machine Manager 或提升許可權的 Windows Powershell 會話進行此作業。 針對 Windows PowerShell，請執行下列命令：

```
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority
```

如需 Hyper-v 上 Linux 虛擬機器的詳細資訊，請參閱 [hyper-v 上的 linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。 如需有關 Cmdlet 的詳細資訊，請參閱 [get-vmfirmware](/powershell/module/hyper-v/set-vmfirmware)。

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>第2代虛擬機器和 Hyper-v 主機更新的記憶體和處理器 \(\)

從第8版開始，第2代虛擬機器可以使用明顯更多的記憶體和虛擬處理器。 您也可以使用比先前支援更多的記憶體和虛擬處理器來設定主機。 這些變更支援新的案例，例如執行電子商務大型記憶體內部資料庫以進行線上交易處理 (OLTP) 和資料倉儲 (DW) 。 Windows Server blog 最近發行了虛擬機器的效能結果，此虛擬機器具有 5.5 tb 的記憶體和128個虛擬處理器，執行 4 TB 的記憶體內部資料庫。 效能高於實體伺服器效能的95%。 如需詳細資訊，請參閱 [Windows Server 2016 hyper-v 大型 VM 效能以進行記憶體中的交易處理](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 如需虛擬機器版本的詳細資訊，請參閱 [Windows 10 或 Windows Server 2016 上的 Hyper-v 升級虛擬機器版本](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 如需支援的最大設定的完整清單，請參閱 [規劃 Windows Server 2016 中的 hyper-v 擴充性](./plan/plan-hyper-v-scalability-in-windows-server.md)。

### <a name="nested-virtualization-new"></a>嵌套虛擬化 \( 新\)

這項功能可讓您使用虛擬機器作為 Hyper-v 主機，並在該虛擬化主機內建立虛擬機器。 這在開發和測試環境中特別有用。 若要使用嵌套虛擬化，您需要：

-   若要在實體 Hyper-v 主機和虛擬化主機上至少執行 Windows Server 2019、Windows Server 2016 或 Windows 10。

-   具有 Intel VT x (嵌套虛擬化的處理器目前僅適用于 Intel 處理器) 。

如需詳細資訊和指示，請參閱 [在具有嵌套虛擬化的虛擬機器中執行 hyper-v](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)。

### <a name="networking-features-new"></a>網路功能 \( 新功能\)

新的網路功能包括：

-   **遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (設定) **。 您可以在系結至 Hyper-v 虛擬交換器的網路介面卡上設定 RDMA，不論是否也使用 SET。 SET 提供的虛擬交換器具有與 NIC 小組相同的功能。 如需詳細資訊，請參閱 [ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

-   **虛擬機器多佇列 (VMMQ) **。 藉由為每個虛擬機器配置多個硬體佇列，改善 VMQ 輸送量。  預設佇列會變成一組虛擬機器的佇列，並在佇列之間散佈流量。

-   **軟體定義的網路的服務品質 (QoS) **。 透過預設類別頻寬內的虛擬交換器來管理流量的預設類別。

如需新網路功能的詳細資訊，請參閱 [網路功能的新](../../networking/What-s-New-in-Networking.md)功能。

### <a name="production-checkpoints-new"></a>新的生產檢查點 \(\)

生產檢查點是虛擬機器的「時間點」映射。 當虛擬機器執行生產工作負載時，這些方法可讓您套用符合支援原則的檢查點。 生產檢查點是以來賓內的備份技術為基礎，而不是儲存的狀態。 若為 Windows 虛擬機器，則會使用 (VSS) 的磁片區快照服務。 針對 Linux 虛擬機器，系統會清除檔案系統緩衝區，以建立與檔案系統一致的檢查點。 如果您想要使用以儲存狀態為基礎的檢查點，請改為選擇標準檢查點。 如需詳細資訊，請參閱 [在 hyper-v 中選擇標準或生產檢查點](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。

> [!IMPORTANT]
> 新的虛擬機器使用生產檢查點作為預設值。

### <a name="rolling-hyper-v-cluster-upgrade-new"></a>復原 Hyper-v 叢集升級 \( 新增\)

您現在可以將執行 Windows Server 2019 或 Windows Server 2016 的節點新增至具有執行 Windows Server 2012 R2 之節點的 Hyper-v 叢集。 這可讓您升級叢集，而不需要停機。 叢集會在 Windows Server 2012 R2 功能等級執行，直到您升級叢集中的所有節點，並使用 Windows PowerShell Cmdlet [ClusterFunctionalLevel](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)更新叢集功能等級。

> [!IMPORTANT]
> 在您更新叢集功能等級之後，您就無法將它返回 Windows Server 2012 R2。

針對功能等級為 Windows Server 2012 R2 的 Hyper-v 叢集，以及執行 Windows Server 2012 R2、Windows Server 2019 和 Windows Server 2016 的節點，請注意下列事項：

-   從執行 Windows Server 2016 或 Windows 10 的節點管理叢集、Hyper-v 和虛擬機器。

-   您可以在 Hyper-v 叢集中的所有節點之間移動虛擬機器。

-   若要使用新的 Hyper-v 功能，所有節點都必須執行 Windows Server 2016 或，而且必須更新叢集功能等級。

-   未升級現有虛擬機器的虛擬機器設定版本。 只有在升級叢集功能等級之後，才能升級設定版本。

-   您建立的虛擬機器與 Windows Server 2012 R2 （虛擬機器設定層級5）相容。

在您更新叢集功能等級之後：

-   您可以啟用新的 Hyper-v 功能。

-   若要讓新的虛擬機器功能可供使用，請使用 `Update-vmVersion` Cmdlet 手動更新虛擬機器設定層級。 如需相關指示，請參閱 [升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。
-   您無法將節點新增至執行 Windows Server 2012 R2 的 Hyper-v 叢集中。

> [!NOTE]
> Windows 10 上的 hyper-v 不支援容錯移轉叢集。

如需詳細資訊和指示，請參閱叢集 [作業系統輪流升級](https://technet.microsoft.com/library/dn850430.aspx)。

### <a name="shared-virtual-hard-disks-updated"></a>共用的虛擬硬碟 \( 已更新\)
您現在可以調整共用虛擬硬碟 ( .vhdx 檔案的大小，) 用於來賓叢集，而不需要停機。 當虛擬機器在線上時，共用的虛擬硬碟可以成長或壓縮。 來賓叢集現在也可以使用 Hyper-v 複本進行嚴重損壞修復，以保護共用的虛擬硬碟。

啟用集合的複寫。 在集合上啟用複寫 **只會透過 WMI 介面公開**。 如需詳細資訊，請參閱 [Msvm_CollectionReplicationService 類別](/previous-versions/windows/desktop/clushyperv/msvm-collectionreplicationservice) 的檔。 **您無法透過 PowerShell Cmdlet 或 UI 來管理集合的複寫。** Vm 應位於屬於 Hyper-v 叢集一部分的主機上，以存取特定于集合的功能。 這包括在獨立主機上共用的 VHD 共用 Vhd，但 Hyper-v 複本並不支援。

遵循 [虛擬硬碟共用](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn281956(v=ws.11))中共用 vhd 的指導方針，並確定您的共用 vhd 是來賓叢集的一部分。

具有共用 VHD 但沒有相關聯的來賓叢集的集合，無法為集合 (建立參考點，而不論共用的 VHD 是否包含在參考點建立中或不) 。

### <a name="virtual-machine-backupnew"></a>新的虛擬機器備份 \(\)

如果您要備份單一虛擬機器 (無論主機是否叢集化或未) ，您都不應該使用 VM 群組。  您也不應該使用快照集集合。 VM 群組和快照集集合僅供用來備份使用共用 vhdx 的來賓叢集。 相反地，您應該使用 [HYPER-V WMI v2 提供者](/windows/win32/hyperv_v2/windows-virtualization-portal)來製作快照集。 同樣地，請不要使用 [容錯移轉叢集 WMI 提供者](/previous-versions/windows/desktop/clushyperv/failover-clustering-hyper-v-wmi-provider-portal)。

### <a name="shielded-virtual-machines-new"></a>新的受防護虛擬機器 \(\)

受防護的虛擬機器使用幾項功能，讓主機上的 Hyper-v 系統管理員和惡意程式碼更難檢查、篡改或竊取受防護虛擬機器的狀態資料。 資料和狀態已加密，Hyper-v 系統管理員看不到影片輸出和磁片，而且虛擬機器只能在已知的狀況良好主機上執行，如主機守護者伺服器所決定。 如需詳細資訊，請參閱 [受防護網狀架構與受防護的 vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)。

> [!NOTE]
> 受防護的虛擬機器與 Hyper-v 複本相容。 若要複寫受防護的虛擬機器，您要複寫的主機必須獲得授權，才能執行該受防護的虛擬機器。

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>叢集虛擬機器的啟動順序優先順序 \( 新\)

這項功能可讓您更充分掌控要先啟動或重新開機的叢集虛擬機器。 這可讓您更輕鬆地啟動在使用這些服務的虛擬機器之前提供服務的虛擬機器。 定義集合、將虛擬機器放置於集合中，以及指定相依性。 使用 Windows PowerShell Cmdlet 來管理集合，例如 [ClusterGroupSet](/powershell/module/failoverclusters/new-clustergroupset)、 [ClusterGroupSet](/powershell/module/failoverclusters/get-clustergroupset)和 [ClusterGroupSetDependency](/powershell/module/failoverclusters/add-clustergroupsetdependency)。
.
### <a name="storage-quality-of-service-qos-updated"></a>儲存體服務品質 (QoS) \( 更新\)

您現在可在向外延展檔案伺服器上建立存放裝置 QoS 原則，並將它們指派給 Hyper-V 虛擬機器上的一或多個虛擬磁碟。 儲存體效能會隨儲存體負載的變動自動進行調整以符合原則。 如需詳細資訊，請參閱 [儲存體服務品質](../../storage/storage-qos/storage-qos-overview.md)。

### <a name="virtual-machine-configuration-file-format-updated"></a>已更新虛擬機器組態檔案格式 \(\)

虛擬機器組態檔使用新的格式，可讓您更有效率地讀取和寫入設定資料。 如果發生儲存體失敗，此格式也會讓資料損毀不太可能。 虛擬機器設定資料檔案會使用 >.vmcx 副檔名，而執行時間狀態資料檔案則使用 .vmrs 副檔名。

> [!IMPORTANT]
> >.vmcx 副檔名表示二進位檔案。 不支援編輯 >.vmcx 或. .vmrs 檔案。

### <a name="virtual-machine-configuration-version-updated"></a>已更新虛擬機器設定版本 \(\)

版本代表虛擬機器設定的相容性、儲存的狀態，以及具有 Hyper-v 版本的快照集檔案。 版本5的虛擬機器與 Windows Server 2012 R2 相容，而且可以在 Windows Server 2012 R2 和 Windows Server 2016 上執行。 具有 Windows Server 2016 和 Windows Server 2019 中所引進之版本的虛擬機器，將不會在 Windows Server 2012 R2 上的 Hyper-v 中執行。

如果您將虛擬機器從 Windows Server 2012 R2 移至或匯入到在 Windows Server 2016 或 Windows Server 2019 上執行 Hyper-v 的伺服器，則不會自動更新虛擬機器的設定。 這表示您可以將虛擬機器移回執行 Windows Server 2012 R2 的伺服器。 但是，這也表示您無法使用新的虛擬機器功能，直到您手動更新虛擬機器設定的版本。

如需檢查及升級版本的指示，請參閱 [升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 本文也會列出引進了某些功能的版本。

> [!IMPORTANT]
> -   更新版本之後，您就無法將虛擬機器移至執行 Windows Server 2012 R2 的伺服器。
> -   您無法將設定降級為先前的版本。
> -   當叢集功能等級為 Windows Server 2012 R2 時，Hyper-v 叢集上的 [VMVersion 指令程式](/powershell/module/hyper-v/update-vmversion) 會遭到封鎖。

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>第2代虛擬機器的虛擬化安全性 \( 新) 

以虛擬化為基礎的安全性支援裝置防護和認證防護等功能，提供更多的作業系統保護，以防範惡意程式碼的攻擊。 以虛擬化為基礎的安全性適用于第2代來賓虛擬機器，從第8版開始。 如需虛擬機器版本的詳細資訊，請參閱 [Windows 10 或 Windows Server 2016 上的 Hyper-v 升級虛擬機器版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="windows-containers-new"></a>Windows 容器 \( 新增\)

Windows 容器可讓許多隔離的應用程式在一個電腦系統上執行。 它們的建立速度很快，而且可高度擴充和攜。 有兩種類型的容器執行時間可供使用，各有不同程度的應用程式隔離。 Windows Server 容器使用命名空間和進程隔離。 Hyper-v 容器會為每個容器使用輕量的虛擬機器。

主要功能包括：

-   支援使用 HTTPS 的網站和應用程式

-   Nano server 可以裝載 Windows Server 和 Hyper-v 容器

-   能夠透過容器共用資料夾來管理資料

-   限制容器資源的能力

如需詳細資料，包括快速入門手冊，請參閱 [Windows 容器檔案](/virtualization/windowscontainers/index)。

### <a name="windows-powershell-direct-new"></a>Windows PowerShell 直接 \( 新增\)

這可讓您從主機在虛擬機器中執行 Windows PowerShell 命令。 Windows PowerShell 主機和虛擬機器之間的直接執行。 這表示它不需要網路或防火牆需求，而且不論您的遠端系統管理設定為何，都可以運作。

Windows PowerShell Direct 是 Hyper-v 系統管理員用來連線到 Hyper-v 主機上虛擬機器的現有工具的替代方案：

-   遠端管理工具，例如 PowerShell 或遠端桌面

-   Hyper-v 虛擬機器連線 (VMConnect) 

這些工具運作正常，但有取捨： VMConnect 是可靠的，但可能很難自動化。 遠端 PowerShell 的功能強大，但可能難以設定和維護。 當您的 Hyper-v 部署成長時，這些取捨可能會變得更重要。 Windows PowerShell 直接解決這種情況的方法，就像使用 VMConnect 一樣，提供強大的腳本和自動化體驗。

如需相關需求和指示，請參閱 [使用 PowerShell Direct 管理 Windows 虛擬機器](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)。
