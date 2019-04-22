---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: 使用 SQL Server 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e26b7cac971f472bc8b5e48e3dc8cd2592dc22ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814779"
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 的同盟伺服器陣列

>適用於：Windows Server 2016, Windows Server 2012 R2

Active Directory Federation Services 使用此拓撲\(AD FS\)不同於使用 Windows 內部資料庫的同盟伺服器陣列\(WID\)部署拓撲中，它不會複寫到資料伺服器陣列中每部同盟伺服器。 相反地，伺服器陣列中的所有同盟伺服器可以讀取，並將資料寫入至儲存在執行 Microsoft SQL Server 位於公司網路中的伺服器的通用資料庫。  
  
> [!IMPORTANT]  
> 如果您想要建立 AD FS 伺服器陣列，並使用 SQL Server 來儲存設定資料，您可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012 和 SQL Server 2014。  
  
## <a name="deployment-considerations"></a>部署考量  
本章節會描述相關的適用對象、 權益和限制，這種部署拓撲相關聯的各種考量。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   大型組織中具有 100 個以上的信任關係，必須為其內部使用者以及外部使用者提供單一登\-上\(SSO\)同盟應用程式或服務的存取權  
  
-   組織已使用 SQL Server，而且想要充分利用其現有的工具和專業知識  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   支援較大的數字的信任關係\(超過 100 個\)  
  
-   支援權杖重新執行偵測\(的安全性功能\)和 成品解析\(安全性聲明標記語言的一部分\(SAML\) 2.0 通訊協定\)  
  
-   SQL Server 的完整優點，例如資料庫鏡像、 容錯移轉叢集、 報告及管理支援工具  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制有哪些？  
  
-   根據預設，此拓撲不提供資料庫備援。 雖然使用 WID 拓撲的同盟伺服器陣列會自動複寫 WID 資料庫在伺服陣列中每部同盟伺服器上的，SQL Server 拓撲的同盟伺服器陣列會包含只有一個資料庫的副本  
  
> [!NOTE]  
> SQL Server 支援許多不同的資料和應用程式的備援選項包括容錯移轉叢集、 資料庫鏡像，以及許多不同類型的 SQL Server 複寫。  
  
Microsoft Information Technology \(IT\)部門使用的 SQL Server 資料庫鏡像中學\-安全\(同步\)模式與容錯移轉叢集以提供最高\-SQL Server 執行個體的可用性支援。 SQL Server 異動\(對等\-要\-對等\)和合併式複寫有尚未經過 Microsoft 的 AD FS 產品小組。 如需有關 SQL Server 的詳細資訊，請參閱 <<c0> [ 高可用性解決方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)或是[選取適當類型的複寫](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>支援的 SQL Server 版本  
Windows Server 2012 R2 中 AD FS 支援下列的 SQL server 版本：  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器的位置和網路配置的建議  
類似於同盟伺服器陣列使用 WID 拓撲，同盟伺服器陣列中的所有已設定為使用一個叢集網域名稱系統\(DNS\)名稱\(代表 Federation Service 名稱\)和一個叢集一部分的網路負載平衡的 IP 位址\(NLB\)叢集組態。 這可協助用戶端將要求配置到個別的同盟伺服器的 NLB 主機。 同盟伺服器 proxy 可用 proxy 的同盟伺服器陣列用戶端要求。  
  
下圖顯示虛構的 Contoso Pharmaceuticals 公司部署其公司網路中的 SQL Server 拓撲的同盟伺服器陣列的方式。 它也會顯示該公司設定周邊網路與 DNS 伺服器，會使用相同的叢集 DNS 名稱的其他 NLB 主機的存取權的方式\(fs.contoso.com\)用在公司網路 NLB 叢集，並使用兩個 web應用程式 proxy \(wap1 和 wap2\)。  
  
![使用 SQL server 伺服器陣列](media/SQLFarmADFSBlue.gif)  
  
如需如何設定您的網路環境使用與同盟伺服器或 web 應用程式 proxy 的詳細資訊，請參閱 「 名稱解析需求 」 一節中[AD FS 需求](AD-FS-Requirements.md)和[規劃網路應用程式 Proxy 基礎結構 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="high-availability-options-for-sql-server-farms"></a>SQL Server 伺服器陣列的高可用性選項  
Windows Server 2012 r2 中有 AD FS 是以支援使用 SQL Server 的 AD FS 伺服器陣列中的高可用性的兩個新選項。  
  
-   針對 SQL Server AlwaysOn 可用性群組的支援  
  
-   使用 SQL Server 合併式複寫地理位置分散的高可用性支援  
  
本章節描述每個選項，它們分別解決哪些問題和一些重要的考量，來決定所要部署選項。  
  
> [!NOTE]  
> 使用 Windows Internal Database 的 AD FS 伺服器陣列\(WID\)具有讀取提供基本的資料備援\/主要同盟伺服器節點上寫入存取和讀取\-只能存取在次要節點上。  這可用在地理區域或地理位置分散的拓樸中。  
>   
> 使用 WID 時請注意下列限制：  
>   
> -   WID 伺服器陣列已限制為 30 的同盟伺服器，如果您有 100 或更少信賴憑證者信任。  
> -   WID 伺服器陣列不支援權杖重新執行偵測或成品解析\(安全性聲明標記語言的一部分\(SAML\)通訊協定\)。  
  
下表提供摘要使用 WID 伺服器陣列。  
  
||||  
|-|-|-|  
||1 \- 100 的 RP 信任|100 個以上的 RP 信任|  
|1 \- 30 AD FS 節點|WID 支援|不支援使用 WID\-所需的 SQL|  
|30 多個 AD FS 節點|不支援使用 WID\-所需的 SQL|不支援使用 WID\-所需的 SQL|  
  
### <a name="alwayson-availability-groups"></a>AlwaysOn 可用性群組  
**概觀**  
  
AlwaysOn 可用性群組 SQL Server 2012 中引進，並提供新的方式，來建立高可用性 SQL Server 執行個體。  AlwaysOn 可用性群組會將叢集和資料庫鏡像的備援和容錯移轉，在 SQL 執行個體層級和資料庫層級的項目。  不同於先前的高可用性選項，AlwaysOn 可用性群組不需要通用的儲存體\(或存放區域網路\)在資料庫層級。  
  
可用性群組由主要複本組成\(讀取一組\-寫入主要資料庫\)以及一到四個可用性複本\(對應的次要資料庫設定\)。  可用性群組支援單一讀取\-寫入複製\(主要的複本\)，以及一到四個讀取\-只可用性複本。  每個可用性複本必須位於單一 Windows Server 容錯移轉叢集的不同節點上\(WSFC\)叢集。  如需有關 AlwaysOn 可用性群組，請參閱[AlwaysOn 可用性群組概觀\(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx)。  
  
AD FS 的 SQL Server 伺服器陣列節點的觀點來看，從 AlwaysOn 可用性群組會取代為原則的單一 SQL Server 執行個體\/成品資料庫。  可用性群組接聽程式是用戶端\(AD FS 安全性權杖服務\)用來連接到 SQL。  
  
下圖顯示 AD FS SQL Server 伺服器陣列與 AlwaysOn 可用性群組。  
  
![使用 SQL server 伺服器陣列](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> AlwaysOn 可用性群組需要 SQL Server 執行個體位於 Windows Server Failover Clustering \(WSFC\)節點。  
  
> [!NOTE]  
> 一個可用性複本可以做為自動容錯移轉目標，其他三個將會依賴手動容錯移轉。  
  
**金鑰部署考量因素**  
  
如果您打算使用 SQL Server 合併式複寫搭配 AlwaysOn 可用性群組，請記下面 < 使用 SQL Server 合併式複寫中使用 AD FS 的金鑰部署考量 > 底下所述的問題。  特別是，當包含複寫訂閱者資料庫的 AlwaysOn 可用性群組容錯移轉，複寫訂用帳戶將會失敗。 若要繼續複寫，複寫管理員必須手動重新設定 「 訂閱者 」。  請參閱特定問題的 SQL Server 說明[複寫訂閱者及 AlwaysOn 可用性群組\(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx)和整體的 AlwaysOn 可用性群組支援陳述式複寫選項位於[複寫、 變更追蹤、 異動資料擷取和 AlwaysOn 可用性群組\(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
**設定 AD FS 使用 AlwaysOn 可用性群組**  
  
使用 AlwaysOn 可用性群組設定 AD FS 伺服器陣列需要稍微修改一下 AD FS 部署程序：  
  
1.  必須先建立您想要備份的資料庫，您可以設定 AlwaysOn 可用性群組。  AD FS 會建立其資料庫安裝程式與新的 AD FS 的 SQL Server 伺服器陣列的第一個同盟服務節點的初始組態的一部分。  AD FS 組態的一部分，您必須指定 SQL 連接字串，因此您必須設定第一個 AD FS 伺服器陣列節點，直接連接到 SQL 執行個體\(這只是暫時\)。   如需設定 AD FS 伺服器陣列，其中包括使用 SQL server 連接字串，設定 AD FS 伺服器陣列節點的特定指引[Configure a Federation Server](../../ad-fs/deployment/Configure-a-Federation-Server.md)。  
  
2.  當 AD FS 資料庫已建立之後時，將它們指派給 AlwaysOn 可用性群組，以及建立常見的 TCPIP 接聽程式使用 SQL Server 工具和處理[建立和設定可用性群組\(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  最後，使用 PowerShell 來編輯 AD FS 內容，以更新 SQL 連接字串，若要使用 AlwaysOn 可用性群組的接聽程式的 DNS 位址。  
  
    若要更新的 AD FS 設定資料庫的 SQL 連接字串的範例 PSH 命令：  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  若要更新 AD FS 成品解析服務資料庫的 SQL 連接字串的範例 PSH 命令：  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>SQL Server 合併式複寫  
也引進了 SQL Server 2012 中，合併式複寫可讓 AD FS 原則資料冗餘，具有下列特性：  
  
-   讀取和寫入功能，在所有節點上\(不只是主要\)  
  
-   較少量的資料以非同步方式複寫，以避免產生系統的延遲  
  
下圖顯示異地備援的 AD FS 的 SQL Server 伺服器陣列與合併式複寫\(1 的 「 發行者 」，2 的訂閱者\):  
  
![使用 SQL server 伺服器陣列](media/ADFSSQLGeoRedundancy3.png)  
  
**使用 AD FS 搭配 SQL Server 合併式複寫的金鑰部署考量\(請注意上圖中的數字\)**  
  
-   散發者資料庫不支援搭配 AlwaysOn 可用性群組或資料庫鏡像。  請參閱支援陳述式複寫選項，在 AlwaysOn 可用性群組的 SQL Server[複寫、 變更追蹤、 異動資料擷取和 AlwaysOn 可用性群組\(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
-   當包含複寫訂閱者資料庫的 AlwaysOn 可用性群組容錯移轉時，複寫訂用帳戶將會失敗。 若要繼續複寫，複寫管理員必須手動重新設定 「 訂閱者 」。  請參閱特定問題的 SQL Server 說明[複寫訂閱者及 AlwaysOn 可用性群組\(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx)和整體的 AlwaysOn 可用性群組支援陳述式複寫選項[複寫、 變更追蹤、 異動資料擷取和 AlwaysOn 可用性群組\(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
如需有關如何設定 AD FS 使用 SQL Server 合併式複寫的詳細指示，請參閱[安裝程式使用 SQL Server 複寫的地理備援](https://technet.microsoft.com/library/dn632406.aspx)。  
  
## <a name="see-also"></a>另請參閱  
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md)  
[Windows Server 2012 R2 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

