---
title: Windows Server 版本 1709 的新功能
description: 關於運算、身分識別、管理、自動化、網路功能、安全性、存放裝置的新功能。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: 32ce591a8b50c6e35c3fde4fedb177b6d76fccdd
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976732"
---
# <a name="whats-new-in-windows-server-version-1709"></a>Windows Server 版本 1709 的新功能

>適用於：Windows Server (半年度管道)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;若要深入了解 Windows 的最新功能，請參閱[What's New in Windows Server](whats-new-in-windows-server.md)。 本節內容說明 Windows Server 版本 1709 的新功能和變更。 此處所列的新功能和變更是您使用這個版本時最可能帶來最大影響的新功能和變更。 另請參閱 [Windows Server 版本 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/)。
   

## <a name="new-cadence-of-releases"></a>新的發行頻率

從這個版本開始，您有兩個接收 Windows Server 功能更新的選項：
- **長期維護通道 (LTSC)** :這是如往常般企業 5 年的主流支援及 5 年的延伸支援。 您可以選擇升級至下一輪每隔 2-3 年發行一次的 LTSC 版本，使用過去 20 年同樣的支援方式。
- **半年通道 (SAC)** :這是軟體保證權益，而且完全支援在生產環境中。 不同之處在於，提供 18 個月的支援，並且每 6 個月會有新的版本。

下表提供發行通道的摘要說明。

|   | 半年通道 | 長期維護通道 |
| ------------- | ------------- | ------------ |
| 發行頻率  | 一年兩次 (春季和秋季)  | 每隔 2-3 年一次 |
| 支援排程  | 18 個月主流生產環境支援  | 5 年主要支援 + 5 年延伸支援 |
| 可用性  | 軟體保證或 Azure (雲端託管)  | 所有通道 |
| 命名慣例  | Windows Server 版本 YYMM  | Windows Server YYYY |

如需詳細資訊，請參閱 <<c0> [ 比較的服務通道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview)。

## <a name="application-containers-and-micro-services"></a>應用程式容器和微服務

- Server Core 容器映像已進一步針對隨即轉移案例最佳化，您可以在進行最少變更的情況下將現有程式碼基底或應用程式移轉到容器中，而且大小還會縮小 60%。 
- Nano Server 容器映像將近縮小 80%。
    - 在 Windows Server 半年通道中，做為容器基底 OS 映像的 Nano Server 已從 390 MB 減少到 80 MB。
- 使用 Hyper-V 隔離的 Linux 容器 

如需詳細資訊，請參閱[Nano Server 在 Windows Server 下一個發行版本中的變更](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel)和[適用於開發人員的 Windows Server 版本 1709](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/)。

## <a name="modern-management"></a>現代化管理

請查看 [Project Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu)，以了解可協助 IT 系統管理員管理核心疑難排解、設定及維護案例的簡化整合式安全體驗。  Project Honolulu 包含提供簡化整合式安全可延伸介面的新一代工具。
Project Honolulu 包含直覺式全新管理體驗，適用於管理電腦、Windows 伺服器、容錯移轉叢集，以及以儲存空間直接存取為基礎的超融合式基礎結構，並且降低營運成本。

## <a name="compute"></a>計算

**Nano 容器和 Server Core 容器**:最先，此版本是有關驅動應用程式創新。 Nano Server 或 Nano as Host 已被取代並更換成 Nano 容器，這是以容器映像方式執行的 Nano。 

如需容器的詳細資訊，請參閱[容器網路功能概觀](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview)。

**以 Server Core 做為容器** (與基礎結構) 主機，可依據現代化程序為現有應用程式提供更好的彈性、密度和效能，並且為已經使用雲端模型開發的新應用程式建立品牌。

**VM 啟動順序**也透過作業系統及應用程式感知獲得改善，提供有關在何時先將 VM 視為已啟動再啟動下一個 VM 方面增強的觸發程序。

**對 VM 的存放裝置類別記憶體支援**可讓 NTFS 格式的直接存取磁碟區建立在非揮發性 DIMM 上並公開給 Hyper-V VM。 這樣就能讓 Hyper-V VMs 運用存放裝置類別記憶體裝置的低延遲效能優勢。

**虛擬化持續性記憶體 (vPMEM)** 的啟用方式為，在主機的直接存取磁碟區上建立 VHD 檔案 (.vhdpmem)、將 vPMEM 控制器新增至 VM，然後將建立的裝置 (.vhdpmem) 新增至 VM。 在主機上使用直接存取磁碟區的 vhdpmem 檔案來支援 vPMEM，可啟用配置彈性，並運用熟悉的管理模型，將磁碟新增至 VM。

**容器儲存空間 – 叢集共用磁碟區 (CSV) 上的持續性資料磁碟區**。 在 Windows Server 版本 1709，以及包含最新更新的 Windows Server 2016 中，我們已新增對容器的支援，以便存取位於 CSV (包括儲存空間直接存取上的 CSV) 的持續性資料磁碟區 如此一來，不論容器執行個體正在哪一個叢集節點上執行，都能讓應用程式容器持續存取磁碟區。 如需詳細資訊，請參閱[叢集共用磁碟區 (CSV)、儲存空間直接存取 (S2D)、SMB 全域對應的相關容器儲存空間支援](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/)。

**容器儲存空間 – SMB 全域對應的持續性資料磁碟區**。 在 Windows Server 版本 1709，我們已新增對應 SMB 檔案共用至容器內部磁碟機代號的支援，這就稱為 SMB 全域對應。 這個對應的磁碟機接著便可供本機伺服器上的所有使用者存取，使資料磁碟區上的容器 I/O 可以透過掛接磁碟機到達基礎檔案共用。 如需詳細資訊，請參閱[叢集共用磁碟區 (CSV)、儲存空間直接存取 (S2D)、SMB 全域對應的相關容器儲存空間支援](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/)。

**虛擬機器組態檔格式 (已更新)** 。 已針對設定版本為 8.2 及更高版本的虛擬機器新增額外檔案 (.vmgs)。 VMGS 表示 VM 來賓狀態，而且是全新內部檔案，包含先前屬於 VM 執行階段狀態檔案一部分的裝置狀態。

## <a name="security-and-assurance"></a>安全性和保證

**網路加密**可讓您快速加密軟體定義網路基礎結構上的網路區段，以符合安全性與合規性需求。

做為受防護 VM 的**主機守護者服務 (HGS)** 已啟用。 在此版本之前，建議的是部署 3 節點實體叢集。 雖然這樣可確保 HGS 環境不會遭到系統管理員盜用，但是成本高到令人卻步。

現在支援**做為受防護 VM 的 Linux**。

如需詳細資訊，請參閱[受防護網狀架構與受防護的 VM 概觀](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)。

**SMBLoris 弱點**：已解決可能導致阻斷服務所謂「SMBLoris」的問題。

## <a name="storage"></a>儲存體

**儲存體複本**:新增 Windows Server 2016 中的儲存體複本的災害復原保護現在已擴充成包括：
- **測試容錯移轉**：掛接目的地存放裝置的選項現在可以透過測試容錯移轉功能來使用。 您可以在目的地節點上暫時掛接已複寫存放裝置的快照集以作測試或備份之用。  如需詳細資訊，請參閱[儲存體複本的常見問題集](https://aka.ms/srfaq)。 
- **專案檀香山支援**:伺服器對伺服器複寫的圖形化管理現已支援專案檀香山中。 這樣就不再需要使用 PowerShell 來管理常見的嚴重損壞狀況保護工作負載。

**SMB**： 
- **SMB1 和來賓驗證移除**:Windows Server，版本 1709年不再預設會安裝在 SMB1 用戶端和伺服器。 此外，在 SMB2 和更新版本中以客體身分驗證的功能也預設為關閉。 如需詳細資訊，請檢閱 [在 Windows 10 版本 1709 及 Windows Server 版本 1709 中，預設不安裝 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。 

- **SMB2/SMB3 安全性與相容性**:已新增安全性及應用程式相容性的其他選項，包括停用 oplocks SMB2 + 中的舊版應用程式，以及需要簽章或加密從用戶端的每個連線為基礎的功能。 如需詳細資訊，請檢閱 SMBShare PowerShell 模組說明。

**重複資料刪除**： 
- **現在重複資料刪除支援 ReFS**:您不再必須選擇使用 ReFS 現代的檔案系統的優點，以及重複資料刪除： 現在，啟用重複資料刪除，只要您可以啟用 ReFS。 透過 ReFS 提升儲存效率，增加 95% 以上。
- **重複資料刪除磁碟區最佳化入口流量/出口流量的資料連接埠 API**:開發人員現在可以利用重複資料刪除包含有關如何儲存資料，有效率地以磁碟區，伺服器之間移動資料，並有效率地叢集的知識。

## <a name="remote-desktop-services-rds"></a>遠端桌面服務 (RDS)

**RDS 已與 Azure AD 整合**，因此客戶可以搭配其他使用 Azure AD 的 SaaS 應用程式運用條件式存取原則、多重要素驗證、整合式驗證，以及更多其他功能。 如需詳細資訊，請參閱[將 Azure AD 網域服務與 RDS 部署整合](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds)。

>[!TIP]
>在其他令人興奮的變更推出至 RDS 搶先預覽，請參閱[遠端桌面服務：更新與即將推出的創新](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## <a name="networking"></a>網路功能

支援 **Docker 的路由網格**。 輸入路由網格屬於[群集模式](https://docs.docker.com/engine/swarm/)的一部分，Docker 內建的容器協調流程解決方案。 如需詳細資訊，請參閱 [Windows Server 版本 1709 隨附的 Docker 路由網格](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/)。

提供**適用於 Docker 的新功能**。 如需詳細資訊，請參閱 [Windows Server 1709 提供適用於 Docker 的新功能](https://blog.docker.com/2017/09/docker-windows-server-1709/)。

**Windows 網路功能類似於 Linux kubernetes**:Windows 是現在與 Linux 同等就網路而言。 客戶可以在任何環境 (包括 Azure、內部部署) 中，以及在使用與 Linux 所支援相同網路基本項目和拓撲的協力廠商雲端堆疊上，部署混合作業系統 Kubernetes 叢集，而不需要任何工作負載或交換器擴充功能。

**核心網路堆疊**:核心網路堆疊的數個功能都已獲得改善。 如需這些功能的詳細資訊，請參閱 [Windows 10 Creators Update 中的核心網路堆疊功能](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/)。
- **TCP 快速開啟 (TFO)** :已新增支援 TFO 來最佳化 TCP 3 向交握程序。 TFO 使用標準三向交握在第一次連線上建立安全 TFO Cookie。  與相同伺服器的後續連線會使用 TFO Cookie 而不使用三向交握，以便在沒有耗費來回行程時間的情況下進行連線。
- **三次方**:實驗性 Windows 原生實作的三次方，TCP 壅塞控制演算法使用。 下列命令會分別啟用或停用 CUBIC。

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **接收視窗自動調整將**:TCP 自動調整將邏輯計算的 TCP 連線的 「 接收視窗 」 參數。  高速和/或長時間延遲連線需要這個演算法來達到良好的效能特性。  在此版本中，演算法已修改為使用步階函數來收斂到指定之連線的最大接收窗口值。
- **TCP 統計 API**:引進新的 API 呼叫 SIO_TCP_INFO。  SIO_TCP_INFO 可讓開發人員使用通訊端選項來查詢有關個別 TCP 連線的豐富資訊。
- **IPv6**:有多個改進 ipv6，在此版本中。
    - **RFC 6106**支援：RFC 6106 允許透過路由器通告 (RAs) 的 DNS 設定。 您可以使用下列命令來啟用或停用 RFC 6106 支援：

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

    - **非固定格式標籤**:開頭為 Creators Update 輸出 TCP 和 UDP 封包透過 IPv6 有此欄位設為 5 個 tuple （Src IP，Dst IP、 Src 連接埠，Dst 連接埠） 的雜湊。  這會讓僅採用 IPv6 的資料中心更有效率地進行負載平衡或流程分類。 若要啟用 flowlabels：

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

    - **ISATAP 和 6to4**:針對未來的淘汰過程 Creators Update 會有預設停用這些技術。
- **死閘道偵測 (DGD)** :DGD 演算法自動連線期間轉換到另一個閘道無法連線至目前的閘道時。 在此版本中，已將演算法改善為定期重新探查網路環境。
- [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) 是 Windows PowerShell 中執行各種網路診斷的內建 Cmdlet。  在此版本中，我們增強此 Cmdlet，以提供有關路由選取及來源位址選取的詳細資訊。

**軟體定義網路**

- **虛擬網路加密**是新的功能，可讓虛擬網路流量在彼此於標示為「加密已啟用」的子網路內通訊的虛擬機器之間進行加密。 這項功能利用虛擬子網路上的資料包傳輸層安全性 (DTLS) 來加密封包。  DTLS 會提供保護以防止任何可存取實體網路的人進行竊聽、竄改和偽造。
 
**Windows 10 VPN**

- **預先登入基礎結構通道**。 Windows 10 VPN 預設不會在使用者未登入其電腦或裝置時自動建立基礎結構通道。 您可以在 VPN 設定檔中使用裝置通道 (prelogon) 功能，將 Windows 10 VPN 設定為自動建立預先登入基礎結構通道。
- **遠端電腦及裝置管理**。  您可以藉由在 VPN 設定檔中設定裝置通道 (prelogon) 功能，管理 Windows 10 VPN 用戶端。 此外，您還必須將 VPN 連線設定為動態註冊已透過內部 DNS 服務指派給 VPN 介面的 IP 位址。
- **指定預先登入閘道**。 您可以在 VPN 設定檔中指定具有裝置通道 (prelogon) 功能的預先登入閘道，同時結合流量篩選器來控制可在公司網路上透過裝置通道存取哪些管理系統。

