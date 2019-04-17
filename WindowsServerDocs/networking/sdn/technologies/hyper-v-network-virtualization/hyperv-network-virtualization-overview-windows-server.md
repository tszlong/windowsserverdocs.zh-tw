---
title: Windows Server 2016 HYPER-V 網路模擬概觀
description: 本主題提供 Windows Server 2016 中 HYPER-V 網路模擬的概觀
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c20f9b2d81ac2d49ed0bbea5f3aca48dea0c50
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Windows Server 2016 HYPER-V 網路模擬概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

在 Windows Server 2016 和一樣管理員中，Microsoft 提供的端點網路模擬方案。  有組成 Microsoft 的網路模擬方案的五個主要元件：  
  
-   **Windows Azure 套件適用於 Windows Server**提供房客入口網站，以建立 virtual 網路及系統管理員入口網站管理 virtual 網路面對。  
  
-   **一樣管理員]** (VMM) 提供的網路 fabric 的集中的管理。  
  
-   **Microsoft Network Controller**提供的集中、 程式化自動化管理、 設定、 監視，以及疑難排解 virtual 和實體網路基礎結構資料中心的點。  
  
-   **HYPER-V 網路模擬**提供虛擬化網路流量所需的基礎結構。  
  
-   **HYPER-V 網路模擬閘道**提供之間 virtual 和實體網路的連接。  
  
本主題介紹概念，並解釋主要優點和 Windows Server 2016 中 HYPER-V 網路模擬 （一部整體網路模擬方案） 的功能。 它解釋網路模擬好處這兩個私人雲朵尋找適用於企業的工作負載彙總和公用雲端服務提供者的基礎結構即服務 (IaaS) 的方式。  
  
Windows Server 2016 中網路模擬有關的更詳細資訊，請查看[HYPER-V 網路模擬技術的詳細資料 Windows Server 2016 在](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md)。  
  
**您表示嗎**  
  
-   [HYPER-V 網路模擬概觀](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b)(Windows Server 2012 R2)  
  
-   [HYPER-V 概觀](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [HYPER-V Virtual 切換概觀](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>描述的功能  
HYPER-V 網路模擬虛擬類似如何伺服器模擬 (hypervisor) 提供 「 虛擬機器 「 作業系統的電腦提供 「 virtual 的網路 」 （稱為 「 VM 網路功能）。 網路模擬分離從實體網路基礎結構 virtual 網路，並移除的 VLAN 以及階層 IP 位址指派限制提供一樣。 這種彈性可讓您輕鬆移到 IaaS 雲朵針對有效率主機服務提供者和 datacenter 系統管理員，管理其基礎結構，同時也可維護必要多承租人隔離、 安全性需求，以及支援重疊一樣 IP 位址。  
  
針對想要順暢延伸到雲端的資料中心。 現今有技術挑戰讓這類順暢混合雲端架構。 最大針對表面上的其中一個是重複使用他們現有網路拓撲 （子網路、 IP 位址、 網路的服務，及等） 在雲端中，其先資源和他們雲端資源橋接。  HYPER-V 網路模擬提供，不受影響的基礎實體網路 VM 網路的概念。 此 VM 網路的一或多個 Virtual 子網路、 撰寫的概念的連接至 virtual 網路中的實體網路的確切位置時與 virtual 網路拓撲分離。 如此一來，針對可以輕鬆地移動他們 virtual 子網路至雲端同時保留現有的 IP 位址和拓撲在雲端中的繼續運作現有的服務，不知道的所在位置的子網路。 是的 HYPER-V 網路模擬可讓順暢混合雲端。  
  
除了混合雲端，許多組織的整合他們資料中心，並建立私人雲朵內部取得雲端架構的效率和延展性優點。 HYPER-V 網路模擬可較佳的效率和彈性的私人雲朵來聯繫營業網路拓撲 （，讓您 virtual） 的實際實體網路拓撲。 如此一來的業務單位輕鬆時所隔離彼此分享內部的私人雲端和保留現有的網路拓撲繼續。 Datacenter 作業小組已部署及動態將不提供更好的操作效率伺服器被迫中斷作業 datacenter 和整體更有效率資料中心中的任何位置點一下工作負載的彈性。  
  
主要優點工作負載的擁有者，是，他們可以現在前往他們工作負載 」 拓撲 「 雲端變更其 IP 位址或重新寫入自己的應用程式。 例如，一般三層 LOB 應用程式由前端層、 業務邏輯層級和資料庫層。 原則、 透過 HYPER-V 網路模擬可讓客戶訓練所有或部分的三個層到雲端，同時保留路由拓撲及 IP 位址的服務 （例如一樣 IP 位址），而不需要應用程式，以變更。  
  
基礎結構的擁有者，處於一樣位置中的其他可讓而不變更虛擬電腦或網路重新設定工作負載任何位置點一下移動中的資料中心。 例如 HYPER-V 網路模擬一樣可以即時 datacenter 服務中斷不在任何地方移轉，可以讓跨子網路即時移轉。 先前即時移轉限於相同子限制虛擬電腦找不到網路。 子網路跨即時移轉允許整合依據動態資源需求能源效率，工作負載的系統管理員，可也基礎結構維護而不干擾客戶工作負載時間。  
  
## <a name="BKMK_APP"></a>實用的應用程式  
Datacenter 模擬的成功、 IT 組織和裝載提供者 （提供者會提供共置或實體伺服器出租） 開始提供更具彈性模擬基礎結構，讓您更輕鬆地提供其針對視伺服器執行個體。 此服務新一代稱為基礎結構即服務 (IaaS)。 Windows Server 2016 提供所有所需的平台功能，可讓企業針對建置私人雲朵和轉換到 IT 即服務操作模型。 Windows Server 2016 2016年也可讓主機創造建置公用雲朵並提供其針對 IaaS 方案。 結合時一樣管理員和 Windows Azure 套件管理 HYPER-V 網路模擬原則，Microsoft 提供的雲端強大方案。  
  
Windows Server 2016 HYPER-V 網路模擬提供型原則、 軟體控制網路模擬減少負荷企業所面臨時它們擴充專用的 IaaS 雲朵，並提供雲端主機服務提供者好彈性與延展性管理達成高資源使用量虛擬電腦管理。  
  
IaaS 案例中有虛擬 （專用雲端） 不同組織部門或其他針對 （裝載的雲端） 的電腦需要隔離的安全。 目前的方案，區域網路 (Vlan)，可以在本案例中顯示重大缺點。  
  
**Vlan**  
  
目前 Vlan 是，大部分的組織用來支援位址空間重複使用和承租人隔離的機制。 VLAN 使用明確標記 (VLAN ID) 乙太網路框架標頭，以及它依賴乙太網路回到執行隔離，並具有相同的 VLAN 收到網路節點限制流量 主要的 Vlan 缺點如下所示：  
  
-   所造成的風險因為麻煩重新設定 production 參數虛擬電腦或隔離邊界動態 datacenter 向時的非故意中斷。  
  
-   由於最大的 4094 Vlan 與一般參數支援不會超過 1000 VLAN Id 有限延展性。  
  
-   在單一的 IP 子網路，限制的單一 VLAN 中節點，限制根據實體的位置虛擬電腦的位置限制。 即使 Vlan 可以展開的網站上，整個 VLAN 必須是相同的子網路上。  
  
**IP 位址指派**  
  
缺點所提出的 Vlan，除了一樣 IP 位址指派提出問題，其中包括：  
  
-   Datacenter 網路基礎結構的所在位置判斷一樣 IP 位址。 如此一來，移動至雲端通常需要變更服務工作負載的 IP 位址。  
  
-   原則受限於 IP 位址，例如免、 資源探索及 directory 服務等等。 變更 IP 位址需要更新相關的原則。  
  
-   一樣部署及流量隔離的拓撲而定。  
  
Datacenter 網路系統管理員計劃的資料中心的實體配置，當他們必須請判斷位置子網路將實體放置並傳送。 這些決策技術為基礎 IP 和乙太網路影響潛在允許上指定的伺服器或讓連接到特定架 datacenter 中執行虛擬電腦的 IP 位址。 當一樣是提供，並放入 datacenter 時，它必須遵守這些選擇和相關的 IP 位址限制。 因此，一般的結果是，datacenter 系統管理員將新的 IP 位址指派給虛擬的電腦。  
  
這項需求的問題是正在地址，除了語意 IP 位址相關聯的資訊。 例如，一個子網路可能包含提供的服務，或在不同的所在位置。 免、 存取控制原則和 IPsec 安全性關聯的通常關聯的 IP 位址。 變更 IP 位址強制調整所有原始的 IP 位址所依據他們原則一樣擁有者。 這個重新編號費用是高許多企業中，選擇新只服務部署到雲端，舊版應用程式保留。  
  
HYPER-V 網路模擬分離 virtual 網路上的實體網路基礎結構客戶虛擬電腦。 如此一來，它可以讓客戶虛擬電腦維持其原始的 IP 位址，同時讓 datacenter 系統管理員而不需要重新設定實體 IP 位址或 VLAN Id 提供客戶虛擬電腦中的任何資料中心。 下一節摘要的按鍵的功能。  
  
## <a name="BKMK_NEW"></a>重要的功能  
以下是清單鍵的功能、 益處與 HYPER-V 網路模擬 Windows Server 2016 中的功能：  
  
-   **可讓彈性工作負載位置-網路隔離及 IP 位址重新使用 Vlan 不**  
  
    HYPER-V 網路模擬分離顧客 virtual 網路從主機服務提供者，自由提供工作負載位置資料中心中的實體網路基礎結構。 一樣工作負載位置不再受限於 VLAN 隔離需求實體網路的 IP 位址指派因為它從依據軟體定義、 multitenant 模擬原則 HYPER-V 主機執行。  
  
    現在在同一部主機伺服器不需要麻煩 VLAN 設定違反 IP 位址階層部署虛擬電腦的其他針對重疊的 IP 位址。 這可以簡化至主機服務提供者、 將修改，包括您保持不變一樣 IP 位址，而這些工作負載針對可讓共用 IaaS 客戶工作負載的移轉。 主機的供應商，支援許多針對想要延伸到共用 IaaS 資料中心他們現有網路位址空間是設定和維護隔離的 Vlan 每個客戶確保的潛在重疊的地址空格並存複雜履行。 使用 HYPER-V 網路模擬，支援重疊的地址做了更容易和裝載者較網路重新設定。  
  
    此外，維護實體基礎結構升級可以完成不會導致向下客戶工作負載的時間。 使用 HYPER-V 網路模擬，可以而不需要實際 IP 位址變更或主要重新設定移轉虛擬特定主機、 架、 子網路，VLAN 或整個叢集上的電腦。  
  
-   **可讓共用 IaaS 雲端工作負載的變得更容易移**  
  
    使用 HYPER-V 網路模擬、 IP 位址和一樣設定維持不變。 這可以讓 IT 組織更輕鬆地將工作負載從他們的資料中心共用 IaaS 裝載提供者的工作負載或工具基礎架構與原則的最低重新設定。 萬一兩個資料中心是連接，IT 系統管理員可以繼續使用他們的工具而不需要重新這些設定。  
  
-   **可讓子網路上的動態移轉**  
  
    即時移轉一樣工作負載的傳統已限制相同的 IP 子網路或 VLAN 因為交叉子網路一樣的客體作業系統變更它的 IP 位址。 此變更地址中斷現有的通訊，中斷一樣上執行之服務。 使用 HYPER-V 網路模擬，工作負載可以動態從執行 Windows Server 2016 一個子網路中，而無須更動工作負載的 IP 位址，在不同的子網路中執行 Windows Server 2016 伺服器移轉。 HYPER-V 網路模擬確保一樣的位置變更因為即時移轉的更新，而且之間的移轉一樣持續進行通訊的主機保持同步。  
  
-   **可讓變得更容易管理的伺服器與網路的分離管理**  
  
    因為移轉和工作負載的位置的基礎實體網路組態的獨立簡化伺服器工作負載的位置。 伺服器管理員可對焦於管理服務，伺服器，並網路系統管理員可以專注於整體網路基礎結構及流量管理。 這可讓資料中心部署，及移轉虛擬電腦，而無須更動虛擬電腦的 IP 位址的伺服器管理員。 那里減少負荷因為 HYPER-V 網路模擬允許一樣位置網路拓撲，而發生減少的網路系統管理員可能會變更隔離邊界的定位參與。  
  
-   **簡化網路，並改善伺服器或網路資源使用量**  
  
    Rigidity Vlan 和相依性 overprovisioning 和 underutilization 實體網路基礎結構結果一樣的位置。 中斷相依性，一樣工作負載位置的提高的彈性可以簡化網路管理，並改善伺服器及網路資源使用量。 請注意，HYPER-V 網路模擬支援 Vlan 實體 datacenter 環境中。 例如，datacenter 可能會想將特定的 VLAN 上的所有 HYPER-V 網路模擬傳輸。  
  
-   **與現有的基礎結構和新興技術相容**  
  
    HYPER-V 網路模擬可以部署在今天的資料中心，但是新興 datacenter 」 一般的網路 」 的技術與相容。  
  
    例如，HNV Windows Server 2016 中的支援 VXLAN 封裝格式與開放 vSwitch 資料庫管理通訊協定 (OVSDB) 為 SouthBound 介面 (SBI)...  
  
-   **提供交互操作以及生態系統整備**  
  
    HYPER-V 網路模擬支援多個組態通訊的現有的資源，例如跨前提連接、 存放區網路 （舊）、 非擬化檔案資源的存取等等。 Microsoft 會致力於處理生態系統合作夥伴支援及提升效能、 延展性及管理 HYPER-V 網路模擬的體驗。  
  
-   **原則為基礎的設定**  
  
    Windows Server 2016 中的網路模擬原則是透過 Microsoft Network Controller 設定。 網路控制器有一套 RESTful northbound API，及 Windows PowerShell 介面設定原則。 如需 Microsoft Network Controller 的詳細資訊，請查看[Network Controller](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
使用 Microsoft Network Controller HYPER-V 網路模擬需要 Windows Server 2016 和 HYPER-V 角色。  
  
## <a name="BKMK_LINKS"></a>也了  
若要了解更多有關 Windows Server 2016 中 HYPER-V 網路模擬查看下列連結：  
  
|內容類型|資訊尋找參考資料|  
|----------------|--------------|  
|**社群資源**|-   [私人雲端架構部落格](http://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-詢問問題：[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN- [RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**相關的技術**|-   [Network Controller](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [HYPER-V 網路模擬概觀](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b)(Windows Server 2012 R2)|  
  


