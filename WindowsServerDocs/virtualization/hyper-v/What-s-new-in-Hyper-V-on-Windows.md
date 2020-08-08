---
title: Windows Server 2016 上的 Hyper-v 新功能
description: 提供 Hyper-v 中新功能的摘要
manager: dongill
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: kbdazure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: d70dacd2f6ea407350641b33111d40c6059a4110
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954565"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 新功能

>適用于： Windows Server 2019、Microsoft Hyper-v Server 2016、Windows Server 2016

本文說明 Windows Server 2019、Windows Server 2016 和 Microsoft Hyper-v Server 2016 上的 Hyper-v 的新功能和已變更的功能。 若要在使用 Windows Server 2012 R2 建立的虛擬機器上使用新功能，並將其移動或匯入至在 Windows Server 2019 或 Windows Server 2016 上執行 Hyper-v 的伺服器，您必須手動升級虛擬機器設定版本。 如需指示，請參閱[升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。

以下是本文所包含的這是什麼，以及其功能是新增或更新的。

## <a name="windows-server-version-1903"></a>Windows Server 版本 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>將 Hyper-v 管理員新增至 (更新) 的 Server Core 安裝

您可能已經知道，在生產環境中使用 Windows Server 半年通道時，我們會建議使用核心安裝選項。 不過，Server Core 預設會省略一些有用的管理工具。 您可以藉由安裝應用程式相容性功能，來新增許多最常用的工具，但仍會遺漏某些工具。

因此，根據客戶的意見反應，我們已在此版本中為應用程式相容性功能新增了一項工具： Hyper-v 管理員 (virtmgmt) 。

如需詳細資訊，請參閱 [Server Core 應用程式相容性功能](../../get-started-19/install-fod-19.md)。

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>安全性：受防護的虛擬機器 (新) 的增強功能

- **分公司改進功能**

    您現在可以運用全新的[遞補 HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) 和[離線模式](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode)功能，在間歇連線到主機守護者服務的電腦上執行受防護的虛擬機器。 遞補 HGS 可讓您為 Hyper-V 設定第二組 URL，在無法連線到主要 HGS 伺服器時可嘗試使用。

    離線模式可在即使無法連線到 HGS 時仍繼續啟動受防護 VM，只要 VM 已成功啟動一次且主機的安全性設定未變更。

- **疑難排解改進功能**

    我們也透過啟用 VMConnect 加強的工作階段模式和 PowerShell Direct 支援，簡化[疑難排解受防護的虛擬機器](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)。 如果您遺失 VM 的網路連線，並且需要更新其設定來還原存取，這些工具會非常有用。

    這些功能不需要進行設定，當受防護 VM 放置於執行 Windows Server 版本 1803 或更新版本的 Hyper-V 主機時，就會自動變為可用。

- **Linux 支援**

    如果您執行混合的作業系統環境，Windows Server 2019 現在支援在受防護的虛擬機器中執行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>與連線待命新的相容 \(\)

當 Hyper-v 角色安裝在使用 [Always On/Always Connected (AOAC) 電源模式] 的電腦上時，現在可以使用 [**連線待命**] 電源狀態。

### <a name="discrete-device-assignment-new"></a>新的個別裝置指派 \(\)

這項功能可讓您對某些 PCIe 硬體裝置提供虛擬機器直接存取和獨佔存取權。 以這種方式使用裝置會略過 Hyper-v 虛擬化堆疊，這會導致更快速的存取。 如需所支援硬體的詳細資訊，請參閱[Windows Server 2016 上 hyper-v 的系統需求](System-requirements-for-Hyper-V-on-Windows.md)中的「離散裝置指派」。 如需詳細資訊，包括如何使用這項功能和考慮，請參閱虛擬化 blog 中的「[離散裝置指派-描述和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)」文章。

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>第1代虛擬機器中作業系統磁片的加密支援 \( 新) 

您現在可以使用第1代虛擬機器中的 BitLocker 磁片磁碟機加密來保護作業系統磁片。 新功能 [金鑰存放裝置] 會建立一個小型的專用磁片磁碟機來儲存系統磁片磁碟機的 BitLocker 金鑰。 這是用來完成的，而不是使用虛擬的可信賴平臺模組 (TPM) ，這僅適用于第2代虛擬機器。 若要解密磁片並啟動虛擬機器，Hyper-v 主機必須屬於授權的受防護網狀架構，或具有其中一個虛擬機器的防護者的私密金鑰。 金鑰儲存需要第8版的虛擬機器。 如需虛擬機器版本的詳細資訊，請參閱在[windows 10 或 Windows Server 2016 上的 hyper-v 中升級虛擬機器版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="host-resource-protection-new"></a>主機資源保護 \( 新增\)

這項功能藉由尋找過多層級的活動，協助防止虛擬機器使用超過其系統資源的共用。 這可協助防止虛擬機器的過多活動降低主機或其他虛擬機器的效能。 當監視偵測到具有過多活動的虛擬機器時，會提供較少的資源給虛擬機器。 此監視和強制執行預設為關閉。 使用 Windows PowerShell 開啟或關閉。 若要開啟它，請執行此命令：

```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

如需此 Cmdlet 的詳細資訊，請參閱[VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor)。

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>網路介面卡和記憶體新增的熱新增和移除 \(\)

您現在可以在虛擬機器執行時新增或移除網路介面卡，而不會產生停機時間。 這適用于執行 Windows 或 Linux 作業系統的第2代虛擬機器。

即使您尚未啟用動態記憶體，您也可以調整指派給虛擬機器執行時的記憶體數量。 這適用于第1代和第2代虛擬機器，執行 Windows Server 2016 或 Windows 10。

### <a name="hyper-v-manager-improvements-updated"></a>已更新 hyper-v 管理員的改良功能 \(\)

-   **替代認證支援**-當您連線到另一個 Windows Server 2016 或 Windows 10 遠端主機時，您現在可以在 [hyper-v 管理員] 中使用一組不同的認證。 您也可以儲存這些認證，讓您更輕鬆地登入。

-   **管理舊版**-在 windows server 2019、windows server 2016 和 windows 10 中使用 hyper-v 管理員，您可以管理在 windows server 2012、windows 8、windows Server 2012 R2 和 Windows 8.1 上執行 hyper-v 的電腦。

-   **更新的管理通訊協定**-hyper-v 管理員現在會使用 WS-MAN 通訊協定與遠端 hyper-v 主機通訊，這允許 CredSSP、KERBEROS 或 NTLM 驗證。 當您使用 CredSSP 連線到遠端 Hyper-v 主機時，您可以執行即時移轉，而不在 Active Directory 中啟用限制委派。 WS-MANAGEMENT 架構也可讓您更輕鬆地啟用主機以進行遠端系統管理。 WS-MAN 透過連接埠 80 連線，預設會開啟此連接埠。

### <a name="integration-services-delivered-through-windows-update-updated"></a>透過更新傳遞的 Integration services Windows Update \(\)

Windows 來賓的 integration services 更新會透過 Windows Update 散發。 對於服務提供者和私人雲端主控者，這會控制將更新套用到擁有虛擬機器的租使用者。 租使用者現在可以使用單一方法來更新其 Windows 虛擬機器與所有更新，包括 integration services。 如需 Linux 來賓整合服務的詳細資訊，請參閱[hyper-v 上的 linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。

> [!IMPORTANT]
> 不再需要 vmguest.iso .iso 影像檔案，因此不會包含在 Windows Server 2016 上的 Hyper-v 中。

### <a name="linux-secure-boot-new"></a>Linux 安全開機 \( 新增\)

在第2代虛擬機器上執行的 Linux 作業系統現在可以在啟用安全開機選項的情況下開機。 Ubuntu 14.04 和更新版本、SUSE Linux Enterprise Server 12 和更新版本、Red Hat Enterprise Linux 7.0 和更新版本，以及 CentOS 7.0 和更新版本，可在執行 Windows Server 2016 的主機上啟用安全開機。 第一次啟動虛擬機器之前，您必須先將虛擬機器設定為使用 Microsoft UEFI 憑證授權單位單位。 您可以從 Hyper-v 管理員、Virtual Machine Manager 或提高許可權的 Windows Powershell 會話執行此動作。 若為 Windows PowerShell，請執行下列命令：

```
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority
```

如需 Hyper-v 上 Linux 虛擬機器的詳細資訊，請參閱[hyper-v 上的 linux 和 FreeBSD 虛擬機器](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。 如需 Cmdlet 的詳細資訊，請參閱[set-vmfirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware)。

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>第2代虛擬機器和 Hyper-v 主機的記憶體和處理器 \( 已更新\)

從第8版開始，第2代虛擬機器可以使用更多的記憶體和虛擬處理器。 您也可以使用比先前支援更多的記憶體和虛擬處理器來設定主機。 這些變更支援新案例，例如執行電子商務大型記憶體內部資料庫，以進行線上交易處理 (OLTP) 和資料倉儲 (DW) 。 Windows Server blog 最近發佈了虛擬機器的效能結果，其中包含 5.5 tb 的記憶體和128虛擬處理器，並執行 4 TB 記憶體內部資料庫。 效能大於實體伺服器效能的95%。 如需詳細資訊，請參閱[記憶體中交易處理的 Windows Server 2016 hyper-v 大型 VM 效能](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 如需虛擬機器版本的詳細資訊，請參閱在[windows 10 或 Windows Server 2016 上的 hyper-v 中升級虛擬機器版本](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 如需支援的最大設定的完整清單，請參閱[在 Windows Server 2016 中規劃 hyper-v 擴充性](./plan/plan-hyper-v-scalability-in-windows-server.md)。

### <a name="nested-virtualization-new"></a>新增的嵌套虛擬化 \(\)

這項功能可讓您使用虛擬機器做為 Hyper-v 主機，並在該虛擬化主機內建立虛擬機器。 這在開發和測試環境中特別有用。 若要使用嵌套虛擬化，您需要：

-   若要在實體 Hyper-v 主機和虛擬化主機上至少執行 Windows Server 2019、Windows Server 2016 或 Windows 10。

-   具有 Intel VT x (嵌套虛擬化的處理器目前僅適用于 Intel 處理器，) 。

如需詳細資訊和指示，請參閱[在具有嵌套虛擬化的虛擬機器中執行 hyper-v](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)。

### <a name="networking-features-new"></a>新網路功能 \(\)

新的網路功能包括：

-   ** (RDMA) 和交換器內嵌小組 (設定) 的遠端直接記憶體存取**。 不論是否也使用 SET，您都可以在系結至 Hyper-v 虛擬交換器的網路介面卡上設定 RDMA。 SET 提供具有與 NIC 小組相同功能的虛擬交換器。 如需詳細資訊，請參閱[ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

-   ** (VMMQ) 的虛擬機器多佇列**。 藉由配置每個虛擬機器的多個硬體佇列，改善 VMQ 輸送量。  預設佇列會變成一組虛擬機器的佇列，而流量會分散在佇列之間。

-   **軟體定義網路的服務品質 (QoS) **。 透過預設類別頻寬內的虛擬交換器，管理流量的預設類別。

如需有關新網路功能的詳細資訊，請參閱[網路的新](../../networking/What-s-New-in-Networking.md)功能。

### <a name="production-checkpoints-new"></a>生產檢查點 \( 新增\)

生產檢查點是虛擬機器的「時間點」映射。 當虛擬機器執行生產工作負載時，這些方法可讓您套用符合支援原則的檢查點。 生產檢查點是以來賓內的備份技術為基礎，而不是儲存的狀態。 對於 Windows 虛擬機器，會使用磁片區快照服務 (VSS) 。 針對 Linux 虛擬機器，會清除檔案系統緩衝區，以建立與檔案系統一致的檢查點。 如果您想要根據儲存的狀態使用檢查點，請改為選擇標準檢查點。 如需詳細資訊，請參閱[在 hyper-v 中的標準或生產檢查點之間選擇](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。

> [!IMPORTANT]
> 新的虛擬機器使用生產檢查點做為預設值。

### <a name="rolling-hyper-v-cluster-upgrade-new"></a>輪替 Hyper-v 叢集升級 \( 新增\)

您現在可以將執行 Windows Server 2019 或 Windows Server 2016 的節點新增至具有執行 Windows Server 2012 R2 節點的 Hyper-v 叢集。 這可讓您在不需停機的情況下升級叢集。 叢集會在 Windows Server 2012 R2 功能層級執行，直到您升級叢集中的所有節點，並使用 Windows PowerShell Cmdlet 更新叢集功能等級（ [ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)）。

> [!IMPORTANT]
> 更新叢集功能等級之後，您就無法將它傳回 Windows Server 2012 R2。

對於功能等級為 Windows Server 2012 R2 且節點執行 Windows Server 2012 R2、Windows Server 2019 和 Windows Server 2016 的 Hyper-v 叢集，請注意下列事項：

-   從執行 Windows Server 2016 或 Windows 10 的節點管理叢集、Hyper-v 和虛擬機器。

-   您可以在 Hyper-v 叢集中的所有節點之間移動虛擬機器。

-   若要使用新的 Hyper-v 功能，所有節點都必須執行 Windows Server 2016 或，而且必須更新叢集功能等級。

-   尚未升級現有虛擬機器的虛擬機器設定版本。 只有在升級叢集功能等級之後，才可以升級設定版本。

-   您所建立的虛擬機器與 Windows Server 2012 R2、虛擬機器設定層級5相容。

更新叢集功能等級之後：

-   您可以啟用新的 Hyper-v 功能。

-   若要讓新的虛擬機器功能可用，請使用 VmConfigurationVersion Cmdlet 來手動更新虛擬機器設定層級。 如需指示，請參閱[升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。
-   您無法將節點新增至執行 Windows Server 2012 R2 的 Hyper-v 叢集。

> [!NOTE]
> Windows 10 上的 hyper-v 不支援容錯移轉叢集。

如需詳細資訊和指示，請參閱叢集[作業系統輪流升級](https://technet.microsoft.com/library/dn850430.aspx)。

### <a name="shared-virtual-hard-disks-updated"></a>已更新共用虛擬硬碟 \(\)
您現在可以調整共用虛擬硬碟 ( .vhdx 檔案) 用於來賓叢集，而不需要停機。 當虛擬機器在線上時，可以增加或壓縮共用的虛擬硬碟。 來賓叢集現在也可以使用 Hyper-v 複本來保護共用虛擬硬碟，以進行嚴重損壞修復。

在集合上啟用複寫。 在集合上啟用複寫**只會透過 WMI 介面公開**。 如需詳細資訊，請參閱[Msvm_CollectionReplicationService 類別](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx)的檔。 **您無法透過 PowerShell Cmdlet 或 UI 管理集合的複寫。** Vm 應該位於 Hyper-v 叢集一部分的主機上，才能存取集合特有的功能。 這包括在獨立主機上的共用 VHD 共用 Vhd 不受 Hyper-v 複本支援。

請遵循[虛擬硬碟共用總覽](https://technet.microsoft.com/library/dn281956.aspx)中共用 vhd 的指導方針，並確定您的共用 vhd 是來賓叢集的一部分。

具有共用 VHD 但沒有相關聯之來賓叢集的集合無法建立集合的參考點 (不論共用 VHD 是否包含在參考點建立中，或不) 。

### <a name="virtual-machine-backupnew"></a>虛擬機器備份 \( 新增\)

如果您要備份單一虛擬機器 (不論主機是否已叢集化，而不是) ，則不應使用 VM 群組。  您也不應該使用快照集集合。 VM 群組和快照集集合僅用於備份使用共用 vhdx 的來賓叢集。 相反地，您應該使用[HYPER-V WMI v2 提供者](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx)來製作快照集。 同樣地，請不要使用[容錯移轉叢集 WMI 提供者](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx)。

### <a name="shielded-virtual-machines-new"></a>受防護的虛擬機器 \( 新增\)

受防護的虛擬機器使用數種功能，讓主機上的 Hyper-v 系統管理員和惡意程式碼更難檢查、篡改或竊取受防護虛擬機器的狀態資料。 資料和狀態會經過加密，Hyper-v 系統管理員看不到影片輸出和磁片，而且可以限制虛擬機器只能在已知的狀況良好主機上執行，如主機守護者伺服器所決定。 如需詳細資訊，請參閱[受防護網狀架構與受防護的 vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)。

> [!NOTE]
> 受防護的虛擬機器與 Hyper-v 複本相容。 若要複寫受防護的虛擬機器，您要複寫到的主機必須獲得授權，才能執行受防護的虛擬機器。

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>叢集虛擬機器新增的開始順序優先順序 \(\)

這項功能可讓您更充分掌控要先啟動或重新開機哪些叢集虛擬機器。 這可讓您在使用這些服務的虛擬機器之前，更容易啟動提供服務的虛擬機器。 定義集合、將虛擬機器放置在集合中，以及指定相依性。 使用 Windows PowerShell Cmdlet 來管理集合，例如[ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset)、 [ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset)和[ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency)。
.
### <a name="storage-quality-of-service-qos-updated"></a>存放裝置服務品質 (QoS) \( 已更新\)

您現在可在向外延展檔案伺服器上建立存放裝置 QoS 原則，並將它們指派給 Hyper-V 虛擬機器上的一或多個虛擬磁碟。 儲存體效能會隨儲存體負載的變動自動進行調整以符合原則。 如需詳細資訊，請參閱[存放裝置服務品質](../../storage/storage-qos/storage-qos-overview.md)。

### <a name="virtual-machine-configuration-file-format-updated"></a>已更新虛擬機器組態檔案格式 \(\)

虛擬機器組態檔使用新格式，讓讀取和寫入設定資料更有效率。 如果發生儲存體失敗，格式也會讓資料損毀的可能性較低。 虛擬機器設定資料檔案會使用 .vmcx 副檔名，而執行時間狀態資料檔案會使用 .vmrs 副檔名。

> [!IMPORTANT]
> .Vmcx 副檔名表示二進位檔案。 不支援編輯. .vmcx 或. .vmrs 檔案。

### <a name="virtual-machine-configuration-version-updated"></a>已更新虛擬機器設定版本 \(\)

版本代表虛擬機器設定的相容性、儲存的狀態，以及具有 Hyper-v 版本的快照集檔案。 第5版的虛擬機器與 Windows Server 2012 R2 相容，而且可以在 Windows Server 2012 R2 和 Windows Server 2016 上執行。 Windows server 2016 和 Windows Server 2019 中引進版本的虛擬機器不會在 Windows Server 2012 R2 的 Hyper-v 中執行。

如果您將虛擬機器移動或匯入至在 Windows Server 2016 或 Windows Server 2019 上執行 Hyper-v 的伺服器（從 Windows Server 2012 R2），則不會自動更新虛擬機器的設定。 這表示您可以將虛擬機器移回執行 Windows Server 2012 R2 的伺服器。 但是，這也表示您無法使用新的虛擬機器功能，直到您手動更新虛擬機器設定的版本。

如需有關檢查和升級版本的指示，請參閱[升級虛擬機器版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 本文也會列出其中引進了一些功能的版本。

> [!IMPORTANT]
> -   更新版本之後，您就無法將虛擬機器移至執行 Windows Server 2012 R2 的伺服器。
> -   您無法將設定降級為先前的版本。
> -   當叢集功能等級為 Windows Server 2012 R2 時，Hyper-v 叢集上的[VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) Cmdlet 會遭到封鎖。

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>第2代虛擬機器的虛擬化安全性 \( 新) 

以虛擬化為基礎的安全性功能，例如 Device Guard 和 Credential Guard，針對惡意程式碼的攻擊提供更高的作業系統保護。 以虛擬化為基礎-從8版開始，第2代來賓虛擬機器中有提供安全性。 如需虛擬機器版本的詳細資訊，請參閱在[windows 10 或 Windows Server 2016 上的 hyper-v 中升級虛擬機器版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="windows-containers-new"></a>Windows 容器 \( 新增\)

Windows 容器可讓許多隔離的應用程式在一個電腦系統上執行。 它們很快就能建立，而且高度可擴充且可攜。 有兩種類型的容器執行時間可供使用，每個都有不同程度的應用程式隔離。 Windows Server 容器會使用命名空間和進程隔離。 Hyper-v 容器會針對每個容器使用輕量虛擬機器。

主要功能包括：

-   支援使用 HTTPS 的網站和應用程式

-   Nano server 可以裝載 Windows Server 和 Hyper-v 容器

-   能夠透過容器共用資料夾來管理資料

-   限制容器資源的能力

如需詳細資訊，包括快速入門手冊，請參閱[Windows 容器檔案](https://docs.microsoft.com/virtualization/windowscontainers/index)。

### <a name="windows-powershell-direct-new"></a>Windows PowerShell Direct \( 新增\)

這可讓您從主機在虛擬機器中執行 Windows PowerShell 命令。 Windows PowerShell Direct 會在主機和虛擬機器之間執行。 這表示它不需要網路或防火牆需求，而且不論您的遠端系統管理設定為何都有效。

Windows PowerShell Direct 是 Hyper-v 系統管理員用來連線到 Hyper-v 主機上虛擬機器之現有工具的替代方案：

-   遠端管理工具，例如 PowerShell 或遠端桌面

-   Hyper-v 虛擬機器連接 (VMConnect) 

這些工具的運作良好，但有取捨： VMConnect 是可靠的，但可能難以自動化。 遠端 PowerShell 的功能強大，但可能很難設定和維護。 當您的 Hyper-v 部署成長時，這些取捨可能會變得更重要。 Windows PowerShell Direct 藉由提供功能強大的腳本和自動化體驗來解決這種情況，就像使用 VMConnect 一樣簡單。

如需相關需求和指示，請參閱[使用 PowerShell Direct 管理 Windows 虛擬機器](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)。
