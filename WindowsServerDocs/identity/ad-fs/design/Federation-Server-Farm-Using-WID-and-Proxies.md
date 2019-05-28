---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: 使用 WID 和 Proxy 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d49ae34d83d4a0b912bd92dbb9de16e18cc5b7ff
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191345"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和 Proxy 的同盟伺服器陣列

Active Directory Federation Services 使用此部署拓撲\(AD FS\)等同於同盟伺服器陣列含有 Windows Internal Database \(WID\)拓撲，但它加入至 proxy 電腦若要支援外部使用者的周邊網路。 這些 proxy 重新導向至同盟伺服器陣列均來自公司網路外部的用戶端驗證要求。 在舊版的 AD FS 中，這些 proxy 呼叫同盟伺服器 proxy。  
  
> [!IMPORTANT]  
> 在 Active Directory Federation Services \(AD FS\) Windows Server 2012 r2 中的同盟伺服器 proxy 角色由稱為 Web Application Proxy 的新遠端存取角色服務。 若要啟用從公司網路外部的也就是部署舊版 AD FS 中，例如 AD FS 2.0 和 Windows Server 2012 中的 AD FS 同盟伺服器 proxy 的目的的協助工具的 AD FS 中，您可以部署為 A 的一或多個 web 應用程式 proxyWindows Server 2012 R2 中的 D FS。  
>   
> 在 AD FS 內容中，Web 應用程式 Proxy 做為 AD FS 同盟伺服器 proxy。 此外，Web 應用程式 Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以讓任何裝置上的使用者能從公司網路外部存取這些應用程式。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 Web 應用程式 Proxy 概觀。  
>   
> 若要規劃 Web 應用程式 Proxy 的部署，您可以檢閱下列主題的資訊：  
>   
> -   [規劃 Web Application Proxy 基礎結構 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [規劃 Web Application Proxy 伺服器](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>部署考量  
本章節會描述相關的適用對象、 權益和限制，這種部署拓撲相關聯的各種考量。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   需要其內部使用者以及外部使用者所提供的 100 或更少設定的信任關係的組織\(誰登入實際上位於公司網路外部的電腦\)單一登\-上\(SSO\)同盟應用程式或服務的存取權  
  
-   必須提供其內部使用者以及外部使用者的 SSO 存取至 Microsoft Office 365 的組織  
  
-   較小的組織有外部使用者，而且需要備援、 可調整的服務  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   相同有益於如列[伺服陣列使用 WID 的同盟伺服器](Federation-Server-Farm-Using-WID.md)拓樸，再加上外部使用者提供額外的存取權的優點  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制有哪些？  
  
-   列出針對相同的限制[伺服陣列使用 WID 的同盟伺服器](Federation-Server-Farm-Using-WID.md)拓樸  

||1 \- 100 的 RP 信任|100 個以上的 RP 信任 
| ----- |-----| ------ |
|1 \- 30 AD FS 節點|WID 支援|不支援使用 WID\-所需的 SQL 
|30 多個 AD FS 節點|不支援使用 WID\-所需的 SQL|不支援使用 WID\-所需的 SQL  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器的位置和網路配置的建議  
若要部署此拓撲中的，除了新增兩個 web 應用程式 proxy，您必須確定您的周邊網路，可以同時提供存取權的網域名稱系統\(DNS\)伺服器和第二個的網路負載平衡\(NLB\)主應用程式。 第二部 NLB 主機都必須設有會使用網際網路 NLB 叢集\-可存取的叢集 IP 位址，而且必須使用相同的叢集 DNS 名稱設定為先前您在公司網路上設定 NLB 叢集\(fs.fabrikam.com\)。 也必須設定 web 應用程式 proxy 與網際網路\-可存取的 IP 位址。  
  
下圖顯示使用先前所述的 WID 拓撲和 Fabrikam，Inc.，這家虛構公司如何提供周邊 DNS 伺服器，以存取現有的同盟伺服器陣列新增第二個 NLB 主機具有相同的叢集 DNS 名稱\(fs.fabrikam.com\)，並將兩個 web 應用程式 proxy \(wap1 和 wap2\)至周邊網路。  
  
![WID 伺服器陣列和 Proxy](media/WIDFarmADFSBlue.gif)  
  
如需如何設定您的網路環境使用與同盟伺服器或 web 應用程式 proxy 的詳細資訊，請參閱 「 名稱解析需求 」 一節中[AD FS 需求](AD-FS-Requirements.md)和[規劃網路應用程式 Proxy 基礎結構 (WAP)](https://technet.microsoft.com/library/dn383648.aspx)。  
  
## <a name="see-also"></a>另請參閱  
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md)  
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

