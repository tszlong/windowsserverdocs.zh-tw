---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: "使用 WID 聯盟伺服器陣列"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 41c2179cbd8bf2c6032f233335099b512c02f880
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid"></a>使用 WID 聯盟伺服器陣列

>適用於：Windows Server 2016、Windows Server 2012 R2

Active Directory 同盟服務 \(AD FS\) 的預設拓撲是使用 Windows 內部資料庫 \(WID\) 聯盟伺服器發電廠。 在這個拓撲，AD FS 使用 WID 在市集中為所有加入農地聯盟伺服器設定資料庫 AD FS。 發電廠複製和維護同盟服務資料庫中的資料設定陣列中每個伺服器上。 在 Windows Server 2012 R2 AD FS 可讓您組織的 100 或較少信賴廠商信任設定聯盟伺服器農場 WID 使用最多 30 伺服器。  
  
建立的第一個聯盟伺服器發電廠中的動作也會建立新的同盟服務。 AD FS 設定資料庫中使用 WID，當您建立陣列中的第一個聯盟伺服器稱為*主要聯盟伺服器*。 這表示這部電腦已使用 AD FS 設定資料庫 read\/寫入複本。  
  
所有您設定的這個發電廠其他聯盟伺服器稱為*第二個聯盟伺服器]*因為他們必須複寫 read\ 僅限複本儲存本機 AD FS 設定資料庫主要聯盟伺服器上所做的任何變更。  
  
> [!IMPORTANT]  
> 我們建議使用 load\ 平衡設定在至少兩部聯盟伺服器。  
  
## <a name="deployment-considerations"></a>部署注意事項  
本節各種考量有關的目標對象、優點和這部署拓撲相關聯的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   100 或較少設定的信任關係需要提供內部使用者與組織 \（登入電腦的實際連接到企業 network\）的單一 sign\ 上 \(SSO\) 存取聯盟應用程式或服務  
  
-   想要讓他們內部使用者 SSO 存取 Microsoft Online Services 或 Microsoft Office 365 組織  
  
-   較小的組織需要備援，可縮放服務  
  
> [!NOTE]  
> 使用較大型資料庫組織考慮使用[聯盟伺服器發電廠使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)拓撲部署。 從登入網路以外的使用者與組織考慮使用[聯盟伺服器發電廠使用 WID 與 Proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)拓撲或[聯盟伺服器發電廠使用 SQL Server](Federation-Server-Farm-Using-SQL-Server.md)拓撲。  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用這個拓撲的好處為何？  
  
-   內部使用者提供 SSO 存取  
  
-   資料和同盟服務冗餘 \（每個聯盟伺服器會複寫變更相同 farm\ 其他聯盟伺服器）  
  
-   WID 已隨附 Windows。因此，不需要購買 SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用這個拓撲限制為何？  
  
-   如果您依賴 100 或較少廠商信任，WID 發電廠的 30 聯盟伺服器的上限。  
  
-   WID 發電廠不支援權杖重播偵測或成品解析度 \（的安全性判斷提示標記語言 \(SAML\) protocol\ 一部分）。  
  
下表使用 WID 發電廠提供摘要。  使用它來規劃實作。  
  
|| 1 \-100 資源點數信任 | 超過 100 資源點數信任 |
| --- | --- | --- |
|1 \-30 AD FS 節點|WID 支援|不支援使用 WID-SQL 需要 
|超過 30 AD FS 節點|不支援使用 WID-SQL 需要|不支援使用 WID-SQL 需要  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器配置建議位置與網路  
當您準備好部署這個拓撲您網路中的 [開始]，您應該計劃的所有聯盟伺服器置於背後的網路負載平衡 \(NLB\) 主機，可以針對 NLB 叢集專用的叢集網域名稱系統 \(DNS\) 名稱及叢集 IP 位址設定您的公司網路。  
  
> [!NOTE]  
> 這個叢集 DNS 名稱必須符合同盟服務名稱，例如 fs.fabrikam.com。  
  
NLB 主機可以使用此 NLB 配置 client 要求的個人聯盟伺服器叢集定義的設定。 下圖顯示虛構 Fabrikam，Inc.公司如何設定使用 two\ 電腦聯盟伺服器陣列其部署第一階段 \(fs1 and fs2\) WID 和 DNS 伺服器和已企業網路單一 NLB 主機的位置。  
  
![使用 WID 伺服器陣列](media/FarmWID.gif)  
  
> [!NOTE]  
> 如果此 NLB 的單一主機失敗，使用者將無法聯盟應用程式或服務存取。 新增其他 NLB 主機，如果您的企業需求不允許時發生失敗單點。  
  
如需有關如何使用您的網路環境設定與聯盟伺服器，查看的名稱解析需求一節[AD FS 需求](AD-FS-Requirements.md)。  
  
## <a name="see-also"></a>也了  
[AD FS 部署拓撲計劃](Plan-Your-AD-FS-Deployment-Topology.md)  
[在 Windows Server 2012 R2 的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

