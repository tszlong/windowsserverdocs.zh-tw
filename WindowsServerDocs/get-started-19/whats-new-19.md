---
title: Windows Server 2019 的新功能
description: Windows Server 2019 新功能的完整概觀，包括桌面體驗、存放裝置移轉服務、系統深入解析、Azure 網路介面卡、儲存空間直接存取增強功能，以及其他變更。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 06/04/2019
ms.openlocfilehash: 7110fe78982fec616174a93514d86fb2e1cf9fa5
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810772"
---
# <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 的新功能

> 適用於：Windows Server 2019

本主題說明 Windows Server 2019 中的一些新功能。 Windows Server 2019 建置在 Windows Server 2016 的強大基礎，且四個索引鍵的佈景主題方面帶來眾多革新：混合式雲端、 安全性、 應用程式平台和超交集基礎結構 (HCI)。

若要了解 Windows Server 半年通道發行的新功能，請參閱[What's New in Windows Server](../get-started/whats-new-in-windows-server.md)。

## <a name="general"></a>一般

### <a name="windows-admin-center"></a>Windows Admin Center

Windows Admin Center 是以瀏覽器為基礎在本機部署的應用程式，用於管理伺服器、叢集、超融合式基礎結構以及 Windows 10 電腦。 除了 Windows 本身以外，不需另付費用，而且可以立即使用於生產環境中。

您可以安裝在 Windows Server 2019，以及 Windows 10 和舊版的 Windows 和 Windows Server 上的 Windows Admin Center，並使用它來管理伺服器和執行 Windows Server 2008 R2 的叢集和更新版本。

如需詳細資訊，請參閱 < [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

### <a name="desktop-experience"></a>桌面體驗

因為 Windows Server 2019 是長期維護通道 (LTSC) 發行，所以包含<b>桌面體驗</b>。 (半年通道\(SAC\)版本不包含所設計的桌面體驗; 它們絕對是 Server Core 和 Nano Server 容器映像版本。)如同 Windows Server 2016 中，作業系統安裝期間您可以選擇 Server Core 安裝或 Server 之間使用桌面體驗。

### <a name="system-insights"></a>系統深入解析

系統深入解析是 Windows Server 2019 中的新功能，讓 Windows Server 擁有原生的本機預測分析功能。 這些預測功能都由機器學習模型支援，可在本機分析 Windows Server 系統資料 (例如效能計數器和事件)，提供伺服器運作的深入解析，協助您減少 Windows Server 部署中被動式管理問題的相關營運費用。

## <a name="hybrid-cloud"></a>混合式雲端

### <a name="server-core-app-compatibility-feature-on-demand"></a>Server Core 應用程式相容性功能隨選安裝

[Server Core 應用程式相容性功能隨選安裝 (FOD)](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) 無須加入 Windows Server 桌面體驗圖形環境，即可納入包含桌面體驗的 Windows Server 二進位檔和元件子集，藉以大幅改進 Windows Server Core 安裝選項的應用程式相容性。  這樣做是為了增加 Server Core 的功能與相容性，同時盡可能保持精簡。  

這個選用的功能隨選安裝可用於不同的 ISO，僅能使用 DISM 新增到 Windows Server Core 安裝和映像。 

## <a name="security"></a>安全性

### <a name="windows-defender-advanced-threat-protection-atp"></a>Windows Defender 進階威脅防護 (ATP)

ATP 的深度平台感應器和回應動作會公開記憶體和核心層級攻擊，並透過隱藏惡意檔案和終止惡意處理程序來因應。

-   如需有關 Windows Defender ATP 的詳細資訊，請參閱 [Windows Defender ATP 功能概觀](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview) (英文)。

-   如需上架伺服器的詳細資訊，請參閱[將伺服器上架到 Windows Defender ATP 服務](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection)。

**Windows Defender ATP 惡意探索防護**是一組新的主機入侵預防功能。 Windows Defender 惡意探索防護的四個元件專門設計來鎖定裝置以抵禦各種攻擊方式，並封鎖惡意程式碼攻擊中常用的行為，同時讓您平衡安全性風險和生產力需求。

-   [降低攻擊面 (ASR)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) 是一組控制項，可透過封鎖可疑的惡意檔案 (例如 Office 檔案)、指令碼、橫向移動、勒索軟體行為以及電子郵件型威脅，讓企業防止惡意軟體進入機器。

-   [網路保護](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/network-protection-exploit-guard?ocid=cx-blog-mmpc)透過 Windows Defender SmartScreen 阻止裝置上任何傳向不受信任主機/IP 位址的輸出程序，保護端點免受到 Web 型威脅。

-   [受控資料夾存取權](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc)會阻止不受信任的處理程序存取受保護的資料夾，進而保護敏感性資料免受勒索軟體侵害。

-   [惡意探索保護](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard)是一組弱點攻擊防護功能 (取代 EMET)，可以輕鬆設定來保護您的應用程式和系統。

[Windows Defender 應用程式控制](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (也稱為程式碼完整性 (CI) 原則) 在 Windows Server 2016 推出。
客戶反應這是個很好的概念，但部署困難。
為了解決這個問題，我們建置了預設 CI 原則，這會允許所有 Windows 隨附檔案和 Microsoft 應用程式 (例如 SQL Server)，並封鎖可略過 CI 的已知執行檔。 

### <a name="security-with-software-defined-networking-sdn"></a>軟體定義網路 (SDN) 的安全性

[SDN 的安全性](https://docs.microsoft.com/windows-server/networking/sdn/security/sdn-security-top)提供許多功能以提高執行中工作負載的客戶信賴度，無論是內部部署或是雲端中服務提供者的形式。 

這些安全性增強功能已整合至 Windows Server 2016 中推出的完整 SDN 平台。

如需 SDN 中新功能的完整清單，請參閱 [Windows Server 2019 之 SDN 的新功能](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new)。

### <a name="shielded-virtual-machines-improvements"></a>受防護虛擬機器改進功能

- **分支辦公室的改善**

    您現在可以運用全新的[遞補 HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) 和[離線模式](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode)功能，在間歇連線到主機守護者服務的電腦上執行受防護的虛擬機器。 遞補 HGS 可讓您為 Hyper-V 設定第二組 URL，在無法連線到主要 HGS 伺服器時可嘗試使用。

    離線模式可在即使無法連線到 HGS 時仍繼續啟動受防護 VM，只要 VM 已成功啟動一次且主機的安全性設定未變更。

- **疑難排解增強功能**

    我們也透過啟用 VMConnect 加強的工作階段模式和 PowerShell Direct 支援，簡化[疑難排解受防護的虛擬機器](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)。 如果您遺失 VM 的網路連線，並且需要更新其設定來還原存取，這些工具會非常有用。 

    這些功能不需要進行設定，當受防護 VM 放置於執行 Windows Server 版本 1803 或更新版本的 Hyper-V 主機時，就會自動變為可用。

- **Linux 支援**

    如果您執行混合的作業系統環境，Windows Server 2019 現在支援在受防護的虛擬機器中執行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

### <a name="http2-for-a-faster-and-safer-web"></a>適用於更快速、更安全之網站的 HTTP/2

- 改善連線聯合，以提供不中斷及正確加密的瀏覽體驗。

- 升級 HTTP/2 的伺服器端加密套件交涉，可自動緩解連線失敗且容易部署。

- 將我們的預設 TCP 擁塞提供者變更為 Cubic，為您提供更多輸送量！

## <a name="storage"></a>儲存體

以下是我們對 Windows Server 2019 儲存空間所做的一些變更。 如需詳細資訊，請參閱[儲存空間的新功能](../storage/whats-new-in-storage.md)。

### <a name="storage-migration-service"></a>存放裝置移轉服務

儲存空間移轉服務是新的技術，可讓您更輕鬆地將伺服器移轉至較新版本的 Windows Server。 它提供圖形化工具，可用於清查伺服器上的資料、將資料與設定傳輸到較新的伺服器，然後選擇性地將舊伺服器的身分識別移動至新伺服器，因此應用程式和使用者不需要變更任何項目。 如需詳細資訊，請參閱[存放裝置移轉服務](../storage/storage-migration-service/overview.md)。

### <a name="storage-spaces-direct"></a>儲存空間直接存取

以下是儲存空間直接存取的新功能清單。 如需詳細資訊，請參閱[儲存空間直接存取的新功能](../storage/whats-new-in-storage.md#storage-spaces-direct)。 另請參閱[Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview)上取得的資訊會驗證儲存空間直接存取系統。

- **重複資料刪除和壓縮 ReFS 磁碟區**
- **持續性記憶體的原生支援**
- **在邊緣的雙節點超交集基礎結構的巢狀恢復功能**
- **兩部伺服器叢集使用 USB 快閃磁碟機為見證**
- **Windows Admin Center 支援**
- **效能歷程記錄**
- **調整最多 4 PB，每個叢集**
- **鏡像加速同位檢查的速度會 2 X**
- **磁碟機延遲極端值偵測**
- **以手動方式分隔的磁碟區來提高容錯的配置**

### <a name="storage-replica"></a>儲存體複本

以下是儲存體複本中的新功能。 如需詳細資訊，請參閱[儲存體複本的新功能](../storage/whats-new-in-storage.md#storage-replica)。

- 儲存體複本現已在 Windows Server 2019 Standard Edition 中提供。
- 測試容錯移轉是新功能，可允許掛接目的地存放裝置來驗證複寫或備份資料。 如需詳細資訊，請參閱[儲存體複本的常見問題集](../storage/storage-replica/storage-replica-frequently-asked-questions.md)。
- 儲存體複本記錄檔效能改進
- Windows Admin Center 支援

## <a name="failover-clustering"></a>容錯移轉叢集

以下是容錯移轉叢集的新功能清單。 如需詳細資訊，請參閱[容錯移轉叢集的新功能](../failover-clustering/whats-new-in-failover-clustering.md)。

- **叢集集合**
- **Azure 可感知叢集**
- **跨網域的叢集移轉**
- **USB 見證**
- **叢集基礎結構改進**
- **叢集感知更新的支援儲存空間直接存取**
- **檔案共用見證的增強功能**
- **叢集強化**
- **容錯移轉叢集不會再使用 NTLM 驗證**

## <a name="application-platform"></a>應用程式平台

### <a name="linux-containers-on-windows"></a>Windows 上的 Linux 容器

現在可以使用相同的 Docker 精靈，在相同容器主機上執行 Windows 和 Linux 容器。 這可讓您擁有異質性容器主機環境，同時提供彈性給應用程式開發人員。

### <a name="built-in-support-for-kubernetes"></a>針對 Kubernetes 的內建支援

Windows Server 2019 會透過支援 Windows 上的 Kubernetes 所需的半年通道發行版本，持續改進計算、網路功能和儲存空間。 即將推出的 Kubernetes 版本中會提供更多詳細資料。

- Windows Server 2019 中的[容器網路功能](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview)透過強化平台網路復原能力和支援容器網路功能外掛程式，大幅改善 Windows 上的 Kubernetes 的可用性。

- Kubernetes 上的部署工作負載將能使用網路安全性，使用內嵌工具來保護 Linux 和 Windows 服務。

### <a name="container-improvements"></a>容器改進功能
    
- **改良的整合式身分識別**

    我們將容器中的整合式 Windows 驗證調整的更簡單也更可靠，以解決舊版 Windows Server 中的數個限制。

- **更好的應用程式相容性**

    容器化以 Windows 為基礎的應用程式時，就得更容易：針對現有的應用程式相容性*windowsservercore*映像是否已增加。 對於需相依於其他 API 的應用程式，目前有第三個基本映像：*windows*。

- **大小和較高的效能降低**

    基本容器映像下載大小、磁碟大小和開機時間都已改進。 如此可加快容器工作流程

- **使用 Windows Admin Center 的管理體驗\(預覽\)**

    我們已透過 Windows Admin Center 的新擴充功能，大幅簡化查看哪些容器正在您的電腦上執行以及管理個別容器。 請至 [Windows Admin Center 公用摘要](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/using-extensions)尋找 "Containers" 擴充功能。

### <a name="encrypted-networks"></a>加密的網路

[加密的網路](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new) - 虛擬網路加密可讓虛擬網路流量在彼此於標示為 **「加密已啟用」** 的子網路內通訊的虛擬機器之間進行加密。 這項功能也利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。 DTLS 提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。

### <a name="network-performance-improvements-for-virtual-workloads"></a>虛擬工作負載的網路效能改進功能

[虛擬工作負載的網路效能改進功能](https://docs.microsoft.com/windows-server/networking/technologies/hpn/hpn-insider-preview)無需您不斷調整或過度佈建主機，即可將輸送到虛擬機器的網路輸送量發揮至極致。 如此可降低作業和維護成本，同時提高主機的可用密度。 這些新功能包括︰

* 在 vSwitch 中接收區段聯合

* 動態虛擬機器多佇列 (d.VMMQ)

### <a name="low-extra-delay-background-transport"></a>低額外延遲背景傳輸

低額外延遲背景傳輸 (LEDBAT) 是最佳化延遲的網路壅塞控制提供者，專門設定來自動產生頻寬給使用者和應用程式，並在網路未使用時消耗整個可用的頻寬。   
這項技術適用於在 IT 環境間部署大型的重大更新，而不至於影響客戶面向服務和相關頻寬。

### <a name="windows-time-service"></a>Windows Time 服務

[Windows Time 服務](https://docs.microsoft.com/windows-server/networking/windows-time-service/insider-preview)包含真實 UTC 相容閏秒支援、稱為「精確時間時間通訊協定」的新時間通訊協定，以及端對端可追蹤性。


### <a name="high-performance-sdn-gateways"></a>高效能 SDN 閘道

Windows Server 2019 中的[高效能 SDN 閘道](https://docs.microsoft.com/windows-server/networking/sdn/gateway-performance)大幅改善 IPsec 和 GRE 連線的效能，使用更少 CPU 即可提供超高效能輸送量。
<br/>

### <a name="new-deployment-ui-and-windows-admin-center-extension-for-sdn"></a>適用於 SDN 的新部署 UI 以及 Windows Admin Center 擴充功能

現在，使用 Windows Server 2019，透過新的部署 UI 和 Windows Admin Center 擴充功能可讓任何人控制 SDN 的能力，輕鬆進行部署與管理。 

### <a name="persistent-memory-support-for-hyper-v-vms"></a>Hyper-V VM 的持續性記憶體支援

若要運用虛擬機器中持續性記憶體 (又稱為 存放裝置類別記憶體) 的高輸送量和低延遲，現在可以直接投影到 VM 中。 這可協助大幅降低資料庫交易延遲或降低故障時低延遲記憶體中資料庫的修復時間。

