---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: "使用 SQL Server 聯盟伺服器陣列"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2333f79c733415833b1d54afc8c385700ac5581e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 聯盟伺服器陣列

>適用於：Windows Server 2016、Windows Server 2012 R2

這個 Active Directory 同盟服務 \(AD FS\) 拓撲與它不會複寫發電廠每個聯盟伺服器資料，使用 Windows 內部資料庫 \(WID\) 部署拓撲聯盟伺服器陣列不同。 改所有聯盟伺服器可讀取和寫入通用資料庫會儲存在位於公司網路中的 Microsoft SQL server 的伺服器上的資料。  
  
> [!IMPORTANT]  
> 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012，以及 SQL Server 2014。  
  
## <a name="deployment-considerations"></a>部署注意事項  
本節各種考量有關的目標對象、優點和這部署拓撲相關聯的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   大型的組織超過 100 信任關係，必須為其內部使用者和外部使用者單一 sign\ 上 \(SSO\) 存取聯盟應用程式或服務提供使用  
  
-   組織已經使用 SQL Server，並且想要利用其現有的工具和專業  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用這個拓撲的好處為何？  
  
-   支援信任關係的數字越大 \(more than 100\)  
  
-   支援權杖重播偵測 \(a security feature\) 和成品解析度 \ (安全性判斷提示標記語言 \(SAML\) 2.0 的一部分 protocol\)  
  
-   支援 SQL Server 的完整優點資料庫鏡像、容錯、報告] 下方，和管理工具  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用這個拓撲限制為何？  
  
-   這個拓撲預設不提供資料庫冗餘。 聯盟伺服器陣列有 SQL Server 拓撲聯盟伺服器陣列有 WID 拓撲會自動複製 WID 資料庫陣列中每個聯盟伺服器上的，但包含份資料庫  
  
> [!NOTE]  
> SQL Server 支援許多不同的資料與應用程式冗餘選項，包括容錯，請稍後及 SQL Server 複寫數種不同類型。  
  
Microsoft 的資訊技術 \(IT\) 部門使用 SQL Server 資料庫鏡像 high\ 安全 \(synchronous\) 模式和容錯提供 high\ 可用性支援 SQL Server 執行個體。 Microsoft AD FS product 小組尚未經過測試 SQL Server 交易 \(peer\-to\-peer\) 及合併複寫。 如需 SQL Server 的詳細資訊，請查看[高可用性方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)或[選取適當的︰ 複寫輸入](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>支援的 SQL Server 版本  
AD FS 在 Windows Server 2012 R2 的支援下列 SQL server 版本：  
  
-   SQL Server 2008 \ 日 R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器配置建議位置與網路  
類似的 WID 拓撲聯盟伺服器陣列聯盟伺服器的所有設定為使用一個叢集網域名稱系統 \(DNS\) 名稱 \（代表同盟服務 name\）和網路負載平衡 \(NLB\) 叢集組態的一部分一個叢集 IP 位址。 這可協助 NLB 主機配置 client 要求的個人聯盟伺服器。 聯盟伺服器 proxy 可用於 proxy 伺服器聯盟陣列 client 請求。  
  
下圖顯示虛構 Contoso 醫藥公司部署其聯盟具有伺服器陣列 SQL Server 拓撲公司網路中的方式。 它也示範如何該公司設定周邊網路存取權的 DNS 伺服器，使用適用於企業網路 NLB 叢集、相同叢集 DNS 名稱 \(fs.contoso.com\) 其他 NLB 主機與兩個 web 應用程式 proxy \(wap1 and wap2\)。  
  
![使用 SQL server 陣列](media/SQLFarmADFSBlue.gif)  
  
如需有關如何聯盟伺服器或網路應用程式的 proxy 設定使用您的網路環境，查看 [名稱解析需求」一節中[AD FS 需求](AD-FS-Requirements.md)和[計劃 Web 應用程式 Proxy 基礎結構 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="high-availability-options-for-sql-server-farms"></a>可用性選項 SQL Server 農田  
在 Windows Server 2012 R2，AD FS 有兩個新選項支援可用性 AD FS 農場使用 SQL Server 中。  
  
-   支援 SQL Server AlwaysOn 可用性群組  
  
-   支援使用 SQL Server 合併複寫分散可用性  
  
本節每個選項，它們分別克服哪些問題，以及一些重要事項決定部署的選項。  
  
> [!NOTE]  
> 使用 Windows 內部資料庫 \(WID\) AD FS 農場備援基本資料 read\ 寫主要聯盟伺服器節點上的存取權與 read\ 存取第二個節點上。  這可用於地理位置本機或分散的拓撲。  
>   
> 當使用 WID 會注意到以下限制︰  
>   
> -   如果您依賴 100 或較少廠商信任，WID 發電廠的 30 聯盟伺服器的上限。  
> -   WID 發電廠不支援權杖重播偵測或成品解析度 \（的安全性判斷提示標記語言 \(SAML\) protocol\ 一部分）。  
  
下表使用 WID 發電廠提供摘要。  
  
||||  
|-|-|-|  
||1 \-100 資源點數信任|超過 100 資源點數信任|  
|1 \-30 AD FS 節點|WID 支援|不支援使用 WID \-SQL 需要|  
|超過 30 AD FS 節點|不支援使用 WID \-SQL 需要|不支援使用 WID \-SQL 需要|  
  
### <a name="alwayson-availability-groups"></a>AlwaysOn 可用性群組  
**概觀**  
  
AlwaysOn 可用性群組帶來 SQL Server 2012 和提供新的方式來建立的可用性 SQL Server 執行個體。  AlwaysOn 可用性群組結合叢集和資料庫鏡像冗餘和容錯移轉 SQL 執行個體層和資料庫層級兩者的項目。  然而先前的可用性選項 AlwaysOn 可用性群組不需要一般的儲存空間 \（或存放區 network\）資料庫層級。  
  
可用性群組所組成主要複本 \（read\ 寫主要 databases\ 一組）和 1 到 4 個可用性複本 \（的對應次要 databases\ 集）。  可用性群組支援單一 read\ 寫複製 \ (主要 replica\)，以及 1 到 4 個僅限 read\ 可用性複本。  每個可用性複本必須位於不同的單一的 Windows Server 容錯 \(WSFC\) 叢集節點。  如需有關 AlwaysOn 可用性群組查看[AlwaysOn 可用性群組概觀 \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx)。  
  
AD FS SQL Server 發電廠節點觀點，AlwaysOn 可用性群組會取代為原則的單一 SQL Server 執行個體 \ 日成品資料庫。  可用性群組其實是何種 client \ 連接 SQL (AD FS 的安全性權杖 service\) 使用。  
  
下圖顯示 AD FS SQL Server 發電廠 AlwaysOn 可用性群組。  
  
![使用 SQL server 陣列](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> AlwaysOn 可用性群組需要 SQL Server 執行個體存在於 Windows Server 容錯 \(WSFC\) 節點。  
  
> [!NOTE]  
> 只有一個可用性複本可做為自動容錯移轉目標，其他三個將會依賴手動錯誤後的移轉。  
  
**主要部署注意事項**  
  
如果您打算 SQL Server 合併複寫搭配使用的可用性 AlwaysOn 群組，請記下在」SQL Server 合併複寫 AD FS 使用的按鍵部署考量」如下所述的問題。  尤其當包含是複寫訂閱資料庫 AlwaysOn 可用性群組失敗，複寫裝機費失敗。 若要繼續複寫，複寫系統管理員必須手動重新設定訂戶。  看 SQL Server 特定問題的[複寫訂閱和 AlwaysOn 可用性群組 \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx)和整體支援聲明 AlwaysOn 可用性群組複寫選項，在[複寫、歷程、變更擷取的資料，及 AlwaysOn 可用性群組 \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
**設定 AD FS 使用 AlwaysOn 可用性群組**  
  
AD FS 發電廠設定 AlwaysOn 可用性群組需要稍微 AD FS 部署程序修改：  
  
1.  必須先建立的資料庫您想要備份，您可以設定 AlwaysOn 可用性群組。  AD FS 建立它資料庫設定與初始設定新的 AD FS SQL Server 發電廠的第一個同盟服務節點的一部分。  AD FS 組態的一部分，您必須指定 SQL 連接字串，因此您將需要設定直接連接至 SQL 執行個體的第一個 AD FS 發電廠節點 \（這是只 temporary\）。   特定指導方針設定 AD FS 發電廠，包括設定 AD FS 發電廠節點 SQL server 連接字串，請查看[設定聯盟伺服器](../../ad-fs/deployment/Configure-a-Federation-Server.md)。  
  
2.  一旦建立 AD FS 資料庫，指派給群組 AlwaysOn 可用性建立使用 SQL Server 工具常見 TCPIP 其實及處理[建立和設定的可用性群組 \(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx)。  
  
3.  最後，使用 PowerShell 編輯 AD FS 屬性更新 SQL 連接字串使用 AlwaysOn 可用性群組其實 DNS 地址。  
  
    範例更新 SQL 連接字串 AD FS 原則資料庫 PSH 命令：  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  範例更新 SQL 連接字串 AD FS 原則資料庫 PSH 命令：  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>SQL Server 合併複寫  
AD FS 使用下列特性原則資料冗餘也引進了 SQL Server 2012，可讓合併複寫：  
  
-   讀取和寫入所有節點上的功能 \ (而不只是 primary\)  
  
-   小大量非同步複製到避免延遲系統資料  
  
下圖顯示地理位置備援 AD FS SQL Server 農場合併複寫 \（發行者 1、2 subscribers\）：  
  
![使用 SQL server 陣列](media/ADFSSQLGeoRedundancy3.png)  
  
**AD FS 使用 SQL Server 合併複寫鍵部署考量 \（請注意圖表 above\ 中的數字）**  
  
-   不支援搭配 AlwaysOn 可用性群組或資料庫鏡像代理商資料庫。  查看 SQL Server 支援聲明 AlwaysOn 可用性群組複寫選項，在[複寫、歷程、變更擷取的資料，及 AlwaysOn 可用性群組 \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
-   當包含是複寫訂閱資料庫 AlwaysOn 可用性群組無法透過時，便會失敗複寫裝機費。 若要繼續複寫，複寫系統管理員必須手動重新設定訂戶。  看 SQL Server 特定問題的[複寫訂閱和 AlwaysOn 可用性群組 \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx)和整體 AlwaysOn 可用性群組複寫選項的支援聲明[複寫、歷程、變更擷取的資料，及 AlwaysOn 可用性群組 \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx)。  
  
使用 SQL Server 合併複寫如何設定 AD FS 詳細指示，請查看[安裝地理備援 SQL Server 複寫與](https://technet.microsoft.com/en-us/library/dn632406.aspx)。  
  
## <a name="see-also"></a>也了  
[AD FS 部署拓撲計劃](Plan-Your-AD-FS-Deployment-Topology.md)  
[在 Windows Server 2012 R2 的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

