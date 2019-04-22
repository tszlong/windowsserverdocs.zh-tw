---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: 使用 WID 和 Proxy 的同盟伺服器陣列
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817619"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和 Proxy 的同盟伺服器陣列

>適用於：Windows Server 2012

Active Directory Federation Services 使用此部署拓撲\(AD FS\)等同於同盟伺服器陣列含有 Windows Internal Database \(WID\)拓撲，但它將同盟伺服器 proxy若要支援外部使用者的周邊網路。 同盟伺服器 proxy 重新導向至同盟伺服器陣列均來自公司網路外部的用戶端驗證要求。  
  
## <a name="deployment-considerations"></a>部署考量  
本章節會描述相關的適用對象、 權益和限制，這種部署拓撲相關聯的各種考量。  
  
### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？  
  
-   需要其內部使用者以及外部使用者所提供的 100 或更少設定的信任關係的組織\(誰登入實際上位於公司網路外部的電腦\)單一登\-上\(SSO\)同盟應用程式或服務的存取權  
  
-   必須提供其內部使用者以及外部使用者的 SSO 存取至 Microsoft Office 365 的組織  
  
-   較小的組織有外部使用者，而且需要備援、 可調整的服務  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？  
  
-   相同有益於如列[伺服陣列使用 WID 的同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)拓樸，再加上外部使用者提供額外的存取權的優點  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制有哪些？  
  
-   列出針對相同的限制[伺服陣列使用 WID 的同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)拓樸  
  
## <a name="server-placement-and-network-layout-recommendations"></a>伺服器的位置和網路配置的建議  
若要部署此拓撲中的，除了新增兩個同盟伺服器 proxy，您必須確定您的周邊網路，可以同時提供存取權的網域名稱系統\(DNS\)伺服器和第二個網路負載平衡\(NLB\)主應用程式。 第二部 NLB 主機都必須設有會使用網際網路 NLB 叢集\-可存取的叢集 IP 位址，而且必須使用相同的叢集 DNS 名稱設定為先前您在公司網路上設定 NLB 叢集\(fs.fabrikam.com\)。 也必須設定同盟伺服器 proxy 與網際網路\-可存取的 IP 位址。  
  
下圖顯示使用先前所述的 WID 拓撲和 Fabrikam，Inc.，這家虛構公司如何提供周邊 DNS 伺服器，以存取現有的同盟伺服器陣列新增第二個 NLB 主機具有相同的叢集 DNS 名稱\(fs.fabrikam.com\)，並將兩個同盟伺服器 proxy \(fsp1 和 fsp2\)至周邊網路。  
  
![使用 WID 伺服器陣列](media/FarmWIDProxies.gif)  
  
如需如何設定您的網路環境使用與同盟伺服器或同盟伺服器 proxy 的詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[名稱同盟伺服器 Proxy 的解析度需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
