---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: 使用 WID 和 proxy AD FS 同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 88737eade6682f7be3572b3bc7f65bfc47c8e0a1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945334"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和 Proxy 的同盟伺服器陣列

適用于 Active Directory 同盟服務 AD FS 的部署拓撲與 \( \) 具有 Windows 內部資料庫 WID 拓撲的同盟伺服器陣列相同 \( \) ，但它會將同盟伺服器 proxy 新增至周邊網路，以支援外部使用者。 同盟伺服器 proxy 會將來自公司網路外部的用戶端驗證要求重新導向至同盟伺服器陣列。

## <a name="deployment-considerations"></a>部署考量因素
本節說明與此部署拓撲相關聯的目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

-   具有100或更少已設定信任關係的組織，必須提供他們的內部使用者和外部使用者 \( 登入實際位於公司網路外部的電腦，並 \) 具有同盟 \- \( \) 應用程式或服務的單一登入 SSO 存取權

-   需要提供內部使用者和外部使用者 Microsoft Office 365 的 SSO 存取權的組織

-   具有外部使用者並需要重複、可擴充服務的小型組織

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？

-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)陣列所列出的相同優點，以及為外部使用者提供額外存取權的好處

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制為何？

-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)陣列所列出的相同限制

## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議
若要部署此拓撲，除了新增兩個同盟伺服器 proxy 以外，您還必須確定您的周邊網路也可以提供網域名稱系統 \( DNS \) 伺服器和第二個網路負載平衡 \( NLB 主機的存取權 \) 。 第二部 NLB 主機必須透過使用可存取網際網路的叢集 IP 位址的 NLB 叢集來設定 \- ，而且必須使用與您在公司網路 fs.fabrikam.com 上設定的先前 NLB 叢集相同的叢集 DNS 名稱設定 \( \) 。 同盟伺服器 proxy 也應該使用網際網路 \- 可存取的 IP 位址進行設定。

下圖顯示具有先前所述之 WID 拓撲的現有同盟伺服器陣列，以及虛構 Fabrikam，Inc.，公司如何提供周邊 DNS 伺服器的存取權，新增第二個具有相同叢集 DNS 名稱 fs.fabrikam.com 的 NLB 主機 \( \) ，並將兩個同盟伺服器 proxy \( fsp1 和 fsp2 新增新增 \) 至周邊網路。

![使用 WID 的伺服器陣列](media/FarmWIDProxies.gif)

如需有關如何設定網路環境以與同盟伺服器或同盟伺服器 proxy 搭配使用的詳細資訊，請參閱同盟伺服器的[名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)或[同盟伺服器 Proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
