---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: 使用 SQL Server 的同盟伺服器陣列
description: 深入瞭解：使用 SQL Server 的舊版 AD FS 同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 781c3d99fdd063d094b85032e1ccaca0860cba9a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046986"
---
# <a name="legacy-ad-fs-federation-server-farm-using-sql-server"></a>使用 SQL Server 的舊版 AD FS 同盟伺服器陣列

Active Directory 同盟服務 AD FS 的此 \( 拓撲 \) 與使用 Windows 內部資料庫 WID 部署拓撲的同盟伺服器陣列不同 \( \) ，因為它不會將資料複寫到伺服器陣列中的每部同盟伺服器。 相反地，伺服器陣列中的所有同盟伺服器都可以讀取資料，並將其寫入儲存在執行 Microsoft SQL Server （位於公司網路中）之伺服器上的通用資料庫。

> [!IMPORTANT]
> 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。

## <a name="deployment-considerations"></a>部署考量因素
本節說明與此部署拓撲相關聯之目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

- 具有超過100個信任關係的大型組織，必須為其內部使用者和外部使用者提供同盟 \- \( \) 應用程式或服務的單一登入 SSO 存取權

- 已使用 SQL Server 並想要利用現有工具和專業知識的組織

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點為何？

- 支援超過100的信任關係數目 \(\)

- 支援權杖重新執行偵測 \( \) \( 安全性聲明標記語言 \( SAML \) 2.0 通訊協定的安全性功能和構件解析部分\)

- 支援 SQL Server 的完整優點，例如資料庫鏡像、容錯移轉叢集、報告和管理工具

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲有哪些限制？

- 此拓撲預設不會提供資料庫冗余。 雖然具有 WID 拓撲的同盟伺服器陣列會自動在伺服器陣列中的每部同盟伺服器上複寫 WID 資料庫，但具有 SQL Server 拓撲的同盟伺服器陣列只包含一個資料庫複本

    > [!NOTE]
    > SQL Server 支援許多不同的資料和應用程式的冗余選項，包括容錯移轉叢集、資料庫鏡像和數種不同類型的 SQL Server 複寫。

Microsoft 資訊技術 \( IT \) 部門使用高安全性同步模式的 SQL Server 資料庫鏡像 \- \( \) ，以及容錯移轉叢集，為 \- SQL Server 實例提供高可用性支援。 SQL Server 的交易式對等體和合併式複寫尚未 \( \- \- \) 由 Microsoft 的 AD FS 產品團隊進行測試。 如需 SQL Server 的詳細資訊，請參閱 [高可用性解決方案總覽](https://go.microsoft.com/fwlink/?LinkId=179853) 或 [選取適當](https://go.microsoft.com/fwlink/?LinkId=214648)的複寫類型。

### <a name="supported-sql-server-versions"></a>支援的 SQL Server 版本
下列 SQL server 版本支援 Windows Server 2012 R2 中的 AD FS：

- SQL Server 2008 \/ R2

- SQL Server 2012

- SQL Server 2014

## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議
與具有 WID 拓撲的同盟伺服器陣列類似，伺服器陣列中的所有同盟伺服器都設定為使用一個叢集網域名稱系統 \( DNS 名稱， \) \( 此名稱代表同盟服務名稱 \) 和一個叢集 IP 位址，做為網路負載平衡 \( NLB \) 叢集設定的一部分。 這可協助 NLB 主機將用戶端要求配置給個別的同盟伺服器。 同盟伺服器 proxy 可以用來將用戶端要求 proxy 到同盟伺服器陣列。

下圖顯示虛構的 Contoso 製藥公司如何部署其同盟伺服器陣列與公司網路中 SQL Server 拓撲。 此外，它也會顯示該公司如何設定具有 DNS 伺服器存取權的周邊網路、額外的 NLB 主機（使用在公司網路 NLB 叢集上使用的相同叢集 DNS 名稱 \( fs.contoso.com \) ），以及兩個 web 應用程式 proxy \( wap1 和 wap2 \) 。

![使用 SQL 的伺服器陣列](media/SQLFarmADFSBlue.gif)

如需有關如何設定網路環境以用於同盟伺服器或 web 應用程式 Proxy 的詳細資訊，請參閱 [AD FS 需求](AD-FS-Requirements.md) 和 [規劃 Web 應用程式 Proxy 基礎結構 (WAP) ](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))的「名稱解析需求」一節。

## <a name="high-availability-options-for-sql-server-farms"></a>SQL Server 服務器陣列的高可用性選項
在 Windows Server 2012 R2 中，AD FS 有兩個新選項可在 AD FS 伺服器陣列中使用 SQL Server 來支援高可用性。

- 支援 SQL Server AlwaysOn 可用性群組

- 使用 SQL Server 合併式複寫支援地理位置分散的高可用性

本節說明每個選項、它們各自解決的問題，以及決定要部署哪些選項的一些重要考慮。

> [!NOTE]
> 使用 Windows 內部資料庫 WID 的 AD FS 伺服器陣列會 \( \) 使用 \/ 主要同盟伺服器節點上的讀取寫入權限，以及 \- 在次要節點上的唯讀存取，提供基本資料冗余。  這可用於地理位置或地理位置分散的拓撲。
>
> 使用 WID 時，請注意下列限制：
>
> - 如果您有100或更少的信賴憑證者信任，WID 伺服器陣列的限制為30部同盟伺服器。
> - WID 伺服器陣列不支援權杖重新執行偵測或 \( 安全性聲明標記語言 \( SAML 通訊協定的構件解析 \) 部分 \) 。

下表提供使用 WID 伺服器陣列的摘要：

| 1-100 個 RP 信任 | 超過 100 個 RP 信任 |
|--|--|
| **1-30 個 AD FS 節點：** 支援 WID | **1-30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |
| **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL | **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |

### <a name="alwayson-availability-groups"></a>AlwaysOn 可用性群組
**概觀**

AlwaysOn 可用性群組是在 SQL Server 2012 中引進，並提供新的方式來建立高可用性 SQL Server 實例。  AlwaysOn 可用性群組會合並叢集和資料庫鏡像的元素，以進行 SQL 實例層和資料庫層的冗余和容錯移轉。  不同于先前的高可用性選項，AlwaysOn 可用性群組不需要在 \( 資料庫層級使用一般儲存體或存放區域網路 \) 。

可用性群組是由一組 \( 讀取 \- 寫入主資料庫 \) 和一到四個 \( 對應次要資料庫的可用性複本 \) 集合組成的主要複本所組成。  可用性群組支援單一讀取 \- 寫入複製 \( 主要複本 \) ，以及一到四個唯讀 \- 可用性複本。  每個可用性複本都必須位於單一 Windows Server 容錯移轉叢集 WSFC 叢集的不同節點上 \( \) 。  如需 AlwaysOn 可用性群組的詳細資訊，請參閱[AlwaysOn 可用性群組 \( SQL Server \) 的總覽](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)。

從 AD FS SQL Server 服務器陣列的節點觀點來看，AlwaysOn 可用性群組會將單一 SQL Server 實例取代為原則成品 \/ 資料庫。  「可用性群組」接聽程式是 \( AD FS 安全性權杖服務 \) 用來連接到 SQL 的用戶端。

下圖顯示具有 AlwaysOn 可用性群組的 AD FS SQL Server 服務器陣列。

![使用 SQL 的伺服器陣列](media/alwaysonavailabilitygroups.jpg)

> [!NOTE]
> AlwaysOn 可用性群組要求 SQL Server 實例必須位於 Windows Server 容錯移轉叢集 \( WSFC \) 節點上。

> [!NOTE]
> 只有一個可用性複本可以作為自動容錯移轉目標，而其他三個複本會依賴手動容錯移轉。

**重要部署考慮**

如果您打算搭配 SQL Server 合併式複寫來使用 AlwaysOn 可用性群組，請注意下面的 < 使用 AD FS 搭配 SQL Server 合併式複寫的主要部署考慮一節中所述的問題。  尤其是當包含複寫訂閱者資料庫的 AlwaysOn 可用性群組容錯移轉時，複寫訂閱會失敗。 若要繼續複寫，複寫管理員必須手動重新設定訂閱者。  請參閱「複寫訂閱者」端的特定問題的 SQL Server 描述，[以及 AlwaysOn 可用性群組 \( SQL Server \) ](/sql/database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server)和 AlwaysOn 可用性群組的整體支援聲明，以及複寫[、變更追蹤、變更資料捕獲和 AlwaysOn 可用性群組 \( \) ](/sql/database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability)SQL Server 的複寫選項。

**設定 AD FS 使用 AlwaysOn 可用性群組**

設定具有 AlwaysOn 可用性群組的 AD FS 伺服器陣列時，必須稍微修改 AD FS 的部署程式：

1. 您必須先建立您要備份的資料庫，才能設定 AlwaysOn 可用性群組。  AD FS 在新 AD FS SQL Server 服務器陣列的第一個 federation service 節點的設定和初始設定過程中建立其資料庫。  在 AD FS 設定的過程中，您必須指定 SQL 連接字串，因此您必須將第一個 AD FS 伺服器陣列節點設定為直接連接到 SQL 實例， \( 這只是暫時性的 \) 。   如需設定 AD FS 伺服器陣列的特定指引，包括使用 SQL server 連接字串設定 AD FS 伺服器陣列節點，請參閱 [設定同盟伺服器](../../ad-fs/deployment/Configure-a-Federation-Server.md)。

2. 建立 AD FS 資料庫之後，請將它們指派給 AlwaysOn 可用性群組，並使用 SQL Server 工具和程式建立[和設定可用性群組 \( \) SQL Server](/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server)來建立一般 TCPIP 接聽程式。

3. 最後，使用 PowerShell 來編輯 AD FS 屬性，以更新 SQL 連接字串，以使用 AlwaysOn 可用性群組接聽程式的 DNS 位址。

    用來更新 AD FS 設定資料庫之 SQL 連接字串的 PSH 命令範例：

    ```
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
    PS:\>$temp.ConfigurationdatabaseConnectionstring="data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true"
    PS:\>$temp.put()

    ```

4. 用來更新 AD FS 構件解析服務資料庫之 SQL 連接字串的 PSH 命令範例：

    ```
    PS:\> Set-AdfsProperties –artifactdbconnection "Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True"
    ```

### <a name="sql-server-merge-replication"></a>SQL Server 合併式複寫
此外，在 SQL Server 2012 中引進合併式複寫可讓您使用下列特性 AD FS 原則資料冗余：

- 不只是主要節點上的讀取和寫入功能 \(\)

- 以非同步方式複寫的資料量較少，以避免系統延遲

下圖顯示「合併式複寫 \( 1 發行者」、2個「訂閱者」 AD FS SQL Server 服務器陣列的地理位置重複 \) ：

![使用 SQL 的伺服器陣列](media/ADFSSQLGeoRedundancy3.png)

**在上圖中使用 AD FS 搭配 SQL Server 合併式複寫附注編號的重要部署考慮 \(\)**

- 散發者資料庫不支援搭配 AlwaysOn 可用性群組或資料庫鏡像使用。  請參閱複寫[、變更追蹤、變更資料捕獲和 AlwaysOn 可用性群組 \( SQL Server \) ](/sql/database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability)上的 AlwaysOn 可用性群組 SQL Server 支援語句和複寫選項。

- 當包含複寫訂閱者資料庫的 AlwaysOn 可用性群組容錯移轉時，複寫訂閱會失敗。 若要繼續複寫，複寫管理員必須手動重新設定訂閱者。  請參閱 SQL Server 描述複寫訂閱者端的特定問題[，以及 AlwaysOn 可用性群組 \( SQL Server \) ](/sql/database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server)以及 AlwaysOn 可用性群組的整體支援聲明，以及複寫選項複寫[、變更追蹤、變更資料捕獲和 AlwaysOn 可用性群組 \( SQL Server \) ](/sql/database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability)。

如需有關如何設定 AD FS 以使用 SQL Server 合併式複寫的詳細指示，請參閱 [使用 SQL Server 複寫安裝地理位置冗余](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn632406(v=ws.11))。

## <a name="see-also"></a>另請參閱
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md) 
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
