---
description: 深入瞭解：使用 WID 和 proxy 的同盟伺服器陣列
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: 使用 WID 和 proxy AD FS 同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 673aeabc3cb390e3001880f0a4174291d8b84871
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046926"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>使用 WID 和 Proxy 的同盟伺服器陣列

Active Directory 同盟服務 AD FS 的部署拓撲 \( 與 \) 具有 Windows 內部資料庫 WID 拓撲的同盟伺服器陣列相同 \( \) ，但它會將同盟伺服器 proxy 新增至周邊網路，以支援外部使用者。 同盟伺服器 proxy 會將來自公司網路外部的用戶端驗證要求重新導向至同盟伺服器陣列。

## <a name="deployment-considerations"></a>部署考量因素
本節說明與此部署拓撲相關聯之目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

-   具有100或更少設定之信任關係的組織，需要提供他們的內部使用者，以及 \( 登入實際位於公司網路外部的外部使用者，且 \) 具有同盟 \- \( \) 應用程式或服務的單一登入 SSO 存取權

-   需要為內部使用者和外部使用者提供 Microsoft Office 365 的 SSO 存取權的組織

-   具有外部使用者且需要重複、可擴充服務的小型組織

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點為何？

-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)陣列所列為的相同優點，以及為外部使用者提供額外存取權的優點

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲有哪些限制？

-   [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID-2012.md)陣列所列出的相同限制

## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議
若要部署此拓撲，除了新增兩個同盟伺服器 proxy 之外，您必須確定您的周邊網路也可以提供網域名稱系統 DNS 伺服器的存取權， \( \) 以及第二個網路負載平衡 \( NLB 主機的存取權 \) 。 第二部 NLB 主機必須使用使用可存取網際網路之叢集 IP 位址的 NLB 叢集來設定 \- ，而且必須使用與您在公司網路 fs.fabrikam.com 上設定的先前 NLB 叢集相同的叢集 DNS 名稱設定 \( \) 。 同盟伺服器 proxy 也應使用網際網路 \- 可存取的 IP 位址進行設定。

下圖顯示現有的同盟伺服器陣列與先前所述的 WID 拓撲，以及虛構 Fabrikam，Inc. 的公司如何提供周邊 DNS 伺服器的存取權、新增具有相同叢集 DNS 名稱 fs.fabrikam.com 的第二部 NLB 主機 \( \) ，以及將兩個同盟伺服器 proxy \( fsp1 和 fsp2 新增 \) 至周邊網路。

![使用 WID 的伺服器陣列](media/FarmWIDProxies.gif)

如需有關如何設定網路環境以與同盟伺服器或同盟伺服器 proxy 搭配使用的詳細資訊，請參閱同盟伺服器的 [名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md) 或 [同盟伺服器 Proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
