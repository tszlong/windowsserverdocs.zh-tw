---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: 使用 WID 和 Proxy 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 60072037aea4ecd81376e1334f3a89b7bb2ff851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408084"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和 Proxy 的同盟伺服器陣列

此 Active Directory 同盟服務的部署拓撲 \(AD FS @ no__t-1 等同于具有 Windows 內部資料庫 \(WID @ no__t-3 拓撲的同盟伺服器陣列，但它會將同盟伺服器 proxy 加入至周邊網路支援外部使用者。 同盟伺服器 proxy 會將來自公司網路外部的用戶端驗證要求重新導向至同盟伺服器陣列。  
  
## <a name="deployment-considerations"></a>部署考量  
本節說明與此部署拓撲相關聯的目標物件、優點和限制的各種考慮。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   若組織具有100或更少的已設定信任關係，且必須提供內部使用者和外部使用者 @no__t 0who，則會使用單一正負號 @ no__t-2on，登入實際位於公司網路外部的電腦上： @ no__t-1\(SSO @ no__t-4 對同盟應用程式或服務的存取權  
  
-   需要提供內部使用者和外部使用者 Microsoft Office 365 的 SSO 存取權的組織  
  
-   具有外部使用者並需要重複、可擴充服務的小型組織  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)陣列所列出的相同優點，以及為外部使用者提供額外存取權的好處  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制為何？  
  
-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)陣列所列出的相同限制  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議  
若要部署此拓撲，除了新增兩個同盟伺服器 proxy 以外，您還必須確定您的周邊網路也可以提供網域名稱系統 @no__t 0DNS @ no__t-1 伺服器的存取權，以及第二個網路負載平衡 \(NLB @ no__t-3 主機。 第二部 NLB 主機必須透過使用 Internet @ no__t-0accessible 叢集 IP 位址的 NLB 叢集來設定，而且它必須使用與您在公司網路上設定的先前 NLB 叢集相同的叢集 DNS 名稱設定 \(fs. fabrikam .com @ no__t-2。 同盟伺服器 proxy 也應該使用 Internet @ no__t-0accessible IP 位址進行設定。  
  
下圖顯示具有先前所述之 WID 拓撲的現有同盟伺服器陣列，以及虛構 Fabrikam，Inc.，公司如何提供周邊 DNS 伺服器的存取、新增第二個具有相同叢集 DNS 名稱 \(fs 的 NLB 主機，以及將兩個同盟伺服器 proxy \(fsp1 和 fsp2 新增 @ no__t-3 新增至周邊網路。  
  
![使用 WID 的伺服器陣列](media/FarmWIDProxies.gif)  
  
如需有關如何設定網路環境以與同盟伺服器或同盟伺服器 proxy 搭配使用的詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[同盟的名稱解析需求。伺服器](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)proxy。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
