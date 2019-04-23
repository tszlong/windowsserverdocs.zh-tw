---
ms.assetid: 663a2482-33d1-4c19-8607-2e24eef89fcb
title: 使用 WID 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bb4e5f88f3d62511b185a2b4317416169717c860
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851399"
---
# <a name="federation-server-farm-using-wid"></a>使用 WID 的同盟伺服器陣列

>適用於：Windows Server 2012

Active Directory Federation Services 的預設拓撲\(AD FS\)同盟伺服器陣列，使用 Windows 內部資料庫\(WID\)，其中包含裝載最多五部同盟伺服器的程式組織 Federation Service。 在此拓撲中，AD FS 會使用 WID，做為存放區的所有同盟伺服器會加入該伺服陣列的 AD FS 組態資料庫。 伺服器陣列會複寫並維護伺服器陣列中每部伺服器之設定資料庫的 Federation Service 資料。  
  
在伺服器陣列中建立第一部同盟伺服器時，也會建立新的 Federation Service。 當您使用 WID 的 AD FS 設定資料庫時，您建立伺服器陣列中第一部同盟伺服器指*主要同盟伺服器*。 這表示這台電腦，設定讀取\/寫入 AD FS 設定資料庫的複本。  
  
所有其他您設定此伺服器陣列的同盟伺服器稱為*次要同盟伺服器*因為它們必須將複寫的讀取主要同盟伺服器上所做的任何變更\-只其本機儲存的 AD FS 設定資料庫的複本。  
  
> [!NOTE]  
> 我們建議您在負載中的至少兩部同盟伺服器使用\-平衡的組態。  
  
## <a name="deployment-considerations"></a>部署考量  
本章節會描述相關的適用對象、 權益和限制，這種部署拓撲相關聯的各種考量。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   需要其內部的使用者提供的 100 或更少設定的信任關係的組織\(登入實際連接到公司網路的電腦\)單一登\-上\(SSO\)同盟應用程式或服務的存取權  
  
-   想要提供其內部使用者的 SSO 存取，Microsoft Online Services 或 Microsoft Office 365 的組織  
  
-   較小的組織需要備援、 可調整的服務  
  
> [!NOTE]  
> 具有較大的資料庫的組織應該考慮使用[同盟伺服器陣列使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)部署拓撲，稍後會說明這一節。 登入從網路外部的使用者的組織應該考慮使用其中一個[同盟伺服器陣列使用 WID 和 Proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)拓樸或[同盟伺服器陣列使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)拓撲。  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   提供內部使用者的 SSO 存取  
  
-   資料和同盟服務備援\(每部同盟伺服器會將變更複寫到相同的伺服器陣列中的其他同盟伺服器\)  
  
-   伺服器陣列可以藉由新增最多五部同盟伺服器向外延展  
  
-   WID 隨附 Windows;因此，不需要購買 SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制有哪些？  
  
-   WID 伺服器陣列的上限為 5 部同盟伺服器。 如需詳細資訊，請參閱 [AD FS 部署拓撲考量](AD-FS-Deployment-Topology-Considerations.md)。  
  
-   WID 伺服器陣列不支援權杖重新執行偵測或成品解析\(安全性聲明標記語言的一部分\(SAML\)通訊協定\)。  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器的位置和網路配置的建議  
當您準備好開始部署此拓撲，在您網路中的，您應該規劃將所有的同盟伺服器放在您的公司網路，網路負載平衡後方\(NLB\)可以設定為 NLB 叢集的主機包含網域名稱系統 」 的專用叢集\(DNS\)名稱和叢集 IP 位址。  
  
> [!NOTE]  
> 此叢集 DNS 名稱必須符合 Federation Service 名稱，例如，fs.fabrikam.com。  
  
NLB 主機可以使用用戶端將要求配置到個別的同盟伺服器到此 NLB 叢集中定義的設定。 下圖顯示 Fabrikam，Inc.，這家虛構公司如何使用兩個部署的第一個階段會設定\-電腦的同盟伺服器陣列\(fs1 和 fs2\)具有 WID 和 DNS 伺服器的位置和有線公司網路的單一 NLB 主機。  
  
![使用 WID 伺服器陣列](media/FarmWID.gif)  
  
> [!NOTE]  
> 如果沒有在這個單一 NLB 主機上的失敗，使用者將無法存取同盟應用程式或服務。 如果您的業務需求不允許有單一失敗點，請加入其他 NLB 主機。  
  
如需如何設定同盟伺服器使用的網路環境的詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)中 AD FS 設計指南。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
