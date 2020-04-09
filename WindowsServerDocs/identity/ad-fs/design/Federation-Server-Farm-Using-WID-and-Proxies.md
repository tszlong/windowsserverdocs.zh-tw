---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: 使用 WID 和 Proxy 的同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 523e076ad9593f09ac2f9db5c45fa8c2e82f05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853101"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和 Proxy 的同盟伺服器陣列

適用于 Active Directory 同盟服務 \(AD FS\) 的這個部署拓撲，與 Windows Internal Database \(WID\) 拓撲的同盟伺服器陣列相同，但它會將 proxy 電腦新增至周邊網路，以支援外部使用者。 這些 proxy 會將來自公司網路外部的用戶端驗證要求重新導向至同盟伺服器陣列。 在舊版的 AD FS 中，這些 proxy 稱為同盟伺服器 proxy。  
  
> [!IMPORTANT]  
> 在 Windows Server 2012 R2 中的 Active Directory 同盟服務 \(AD FS\) 中，同盟伺服器 proxy 的角色是由新的遠端存取角色服務（稱為 Web 應用程式 Proxy）所處理。 若要讓您的 AD FS 可從公司網路外部存取（這是在舊版 AD FS 中部署同盟伺服器 proxy 的目的，例如 Windows Server 2012 中的 AD FS 2.0 和 AD FS，您可以在 Windows Server 2012 R2 中為 AD FS 部署一或多個 web 應用程式 proxy。  
>   
> 在 AD FS 的內容中，Web 應用程式 Proxy 會當做 AD FS 的同盟伺服器 proxy。 此外，Web 應用程式 Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以讓任何裝置上的使用者能從公司網路外部存取這些應用程式。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 Web 應用程式 Proxy 概觀。  
>   
> 若要規劃 Web 應用程式 Proxy 的部署，您可以檢閱下列主題的資訊：  
>   
> -   [規劃 Web 應用程式 Proxy 基礎結構（WAP）](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [規劃 Web 應用程式 Proxy 伺服器](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>部署考量  
本節說明與此部署拓撲相關聯的目標物件、優點和限制的各種考慮。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   具有100或較少已設定信任關係的組織，必須同時提供他們的內部使用者和外部使用者 \(登入位於公司\-\) 網路外部的電腦，\(SSO\) 存取同盟應用程式或服務  
  
-   需要提供內部使用者和外部使用者 Microsoft Office 365 的 SSO 存取權的組織  
  
-   具有外部使用者並需要重複、可擴充服務的小型組織  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID.md)陣列所列出的相同優點，以及為外部使用者提供額外存取權的好處  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制為何？  
  
-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID.md)陣列所列出的相同限制  

||1 \- 100 RP 信任|超過 100 RP 信任 
| ----- |-----| ------ |
|1 \- 30 AD FS 節點|支援 WID|不支援使用 WID \- SQL 需要 
|超過30個 AD FS 節點|不支援使用 WID \- SQL 需要|不支援使用 WID \- SQL 需要  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議  
若要部署此拓撲，除了新增兩個 web 應用程式 proxy 以外，您還必須確定您的周邊網路也可以提供網域名稱系統 \(DNS\) 伺服器的存取權，以及 \(NLB\) 主機的第二個網路負載平衡。 第二部 NLB 主機必須透過使用網際網路\-可存取叢集 IP 位址的 NLB 叢集來設定，而且必須使用與您在公司網路上設定的先前 NLB 叢集相同的叢集 DNS 名稱設定 \(fs.fabrikam.com\)。 Web 應用程式 proxy 也應該使用網際網路\-可存取的 IP 位址進行設定。  
  
下圖顯示具有先前所述之 WID 拓撲的現有同盟伺服器陣列，以及虛構 Fabrikam，Inc.，公司如何提供周邊 DNS 伺服器的存取權，新增具有相同叢集 DNS 名稱的第二個 NLB 主機 \(fs.fabrikam.com\)，並將兩個 web 應用程式 proxy \(wap1 和 wap2\) 新增至周邊網路。  
  
![WID 伺服器陣列和 proxy](media/WIDFarmADFSBlue.gif)  
  
如需有關如何設定網路環境以與同盟伺服器或 web 應用程式 Proxy 搭配使用的詳細資訊，請參閱[AD FS 需求](AD-FS-Requirements.md)和[規劃 Web 應用程式 Proxy 基礎結構（WAP）](https://technet.microsoft.com/library/dn383648.aspx)中的「名稱解析需求」一節。  
  
## <a name="see-also"></a>另請參閱  
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md)  
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

