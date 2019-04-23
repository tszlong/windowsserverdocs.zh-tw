---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: 使用 SQL Server 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863949"
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 的同盟伺服器陣列

>適用於：Windows Server 2012

Active Directory Federation Services 使用此拓撲\(AD FS\)不同於使用 Windows 內部資料庫的同盟伺服器陣列\(WID\)部署拓撲中，它不會複寫到資料伺服器陣列中每部同盟伺服器。 相反地，伺服器陣列中的所有同盟伺服器可以讀取，並將資料寫入至儲存在執行 Microsoft SQL Server 位於公司網路中的伺服器的通用資料庫。  
  
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
安裝 Windows Server 2012 的 AD FS 支援下列的 SQL server 版本：  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器的位置和網路配置的建議  
類似於同盟伺服器陣列使用 WID 拓撲，同盟伺服器陣列中的所有已設定為使用一個叢集網域名稱系統\(DNS\)名稱\(代表 Federation Service 名稱\)和一個叢集一部分的網路負載平衡的 IP 位址\(NLB\)叢集組態。 這可協助用戶端將要求配置到個別的同盟伺服器的 NLB 主機。 同盟伺服器 proxy 可用 proxy 的同盟伺服器陣列用戶端要求。  
  
下圖顯示虛構的 Contoso Pharmaceuticals 公司部署其公司網路中的 SQL Server 拓撲的同盟伺服器陣列的方式。 它也會顯示該公司設定周邊網路與 DNS 伺服器，會使用相同的叢集 DNS 名稱的其他 NLB 主機的存取權的方式\(fs.contoso.com\)用在公司網路 NLB 叢集，並具有兩個同盟伺服器 proxy \(fsp1 和 fsp2\)。  
  
![使用 SQL server 伺服器陣列](media/FarmSQLProxies.gif)  
  
如需如何設定您的網路環境使用與同盟伺服器或同盟伺服器 proxy 的詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[名稱同盟伺服器 Proxy 的解析度需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
