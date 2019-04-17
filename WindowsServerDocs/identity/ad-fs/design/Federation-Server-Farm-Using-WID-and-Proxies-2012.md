---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: "使用 WID 與 Proxy 聯盟伺服器陣列"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 與 Proxy 聯盟伺服器陣列

>適用於：Windows Server 2012

這個 Active Directory 同盟服務 \(AD FS\) 部署拓撲聯盟伺服器發電廠相同的 Windows 內部資料庫 \(WID\) 拓撲，但加入周邊網路支援外部使用者聯盟的 proxy 伺服器。 聯盟伺服器 proxy 重新導向來自您的企業網路外聯盟伺服器陣列 client 驗證要求。  
  
## <a name="deployment-considerations"></a>部署注意事項  
本節各種考量有關的目標對象、優點和這部署拓撲相關聯的限制。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   100 或較少設定的信任關係需要他們內部使用者和外部使用者提供的組織 \（誰登入電腦，實際上以外的公司 network\）的單一 sign\ 上 \(SSO\) 存取聯盟應用程式或服務  
  
-   組織必須為其內部使用者和外部使用者提供 SSO 存取與 Microsoft Office 365  
  
-   較小的組織外部使用者且需要備援可縮放服務  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用這個拓撲的好處為何？  
  
-   相同好處所列的[聯盟伺服器發電廠使用 WID](Federation-Server-Farm-Using-WID-2012.md)拓撲，再加上的優點外部使用者提供額外的存取  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用這個拓撲限制為何？  
  
-   限制相同為列出適用於[聯盟伺服器發電廠使用 WID](Federation-Server-Farm-Using-WID-2012.md)拓撲  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器配置建議位置與網路  
若要部署這個拓撲，除了新增兩個聯盟伺服器 proxy，您必須確定周邊網路，也可以提供及存取權的網域名稱系統 \(DNS\) 伺服器第二個網路負載平衡 \(NLB\) 主機。 必須設定使用 Internet\ 無障礙叢集 IP 位址，NLB 叢集 NLB 第二部主機和必須使用先前 NLB 叢集您企業網路 \(fs.fabrikam.com\) 上設定為相同的叢集 DNS 名稱設定。 聯盟伺服器 proxy 也應該 Internet\ 存取 IP 位址設定。  
  
下圖顯示現有聯盟伺服器陣列與 WID 拓撲之前所述的方式虛構 Fabrikam，Inc.公司提供存取周邊 DNS 伺服器，將新增第二個 NLB 主機的相同叢集 DNS 名稱 \(fs.fabrikam.com\)，並將有兩個聯盟伺服器 proxy \(fsp1 and fsp2\) 周邊網路。  
  
![使用 WID 伺服器陣列](media/FarmWIDProxies.gif)  
  
如需有關如何使用您的網路環境設定聯盟伺服器或聯盟的 proxy 伺服器，查看任一個[聯盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[聯盟的 Proxy 伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
