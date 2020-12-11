---
description: 深入瞭解：舊版 AD FS 使用 WID 和 proxy 的同盟伺服器陣列
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: 使用 WID 和 Proxy 的同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 186c75d6ec7660258f8b16b93e5d35bb74669ec2
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046906"
---
# <a name="legacy-ad-fs-federation-server-farm-using-wid-and-proxies"></a>舊版 AD FS 使用 WID 和 proxy 的同盟伺服器陣列

此 Active Directory 同盟服務 (AD FS) 的部署拓撲與具有 Windows 內部資料庫 (WID) 拓撲的同盟伺服器陣列相同，但它會將 proxy 電腦新增至周邊網路以支援外部使用者。 這些 proxy 會將來自公司網路外部的用戶端驗證要求重新導向至同盟伺服器陣列。 在舊版 AD FS 中，這些 proxy 稱為同盟伺服器 proxy。

> [!IMPORTANT]
> 在 Windows Server 2012 R2 的 Active Directory 同盟服務 (AD FS) 中，同盟伺服器 proxy 的角色是由新的遠端存取角色服務（稱為 Web 應用程式 Proxy）所處理。 若要從公司網路外部啟用 AD FS 的協助工具，這是在舊版 AD FS 中部署同盟伺服器 proxy 的目的，例如 AD FS 2.0 和 Windows Server 2012 中的 AD FS，您可以在 Windows Server 2012 R2 中部署 AD FS 的一或多個 web 應用程式 proxy。
>
> 在 AD FS 的內容中，Web 應用程式 Proxy 可作為 AD FS 同盟伺服器 proxy。 此外，Web 應用程式 Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以讓任何裝置上的使用者能從公司網路外部存取這些應用程式。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 Web 應用程式 Proxy 概觀。
>
> 若要規劃 Web 應用程式 Proxy 的部署，您可以檢閱下列主題的資訊：
>
> - [規劃 Web 應用程式 Proxy 基礎結構 (WAP) ](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))
> - [規劃 Web 應用程式 Proxy 伺服器](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))

## <a name="deployment-considerations"></a>部署考量因素
本節說明與此部署拓撲相關聯之目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

- 具有100或更少設定信任關係的組織，必須提供其內部使用者和外部使用者 (登入實際位於公司網路外部的電腦) 透過單一登入 (SSO) 同盟應用程式或服務的存取權

- 需要為內部使用者和外部使用者提供 Microsoft Office 365 的 SSO 存取權的組織

- 具有外部使用者且需要重複、可擴充服務的小型組織

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點為何？

- [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID.md)陣列所列為的相同優點，以及為外部使用者提供額外存取權的優點

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲有哪些限制？

- [使用 WID 拓撲針對同盟伺服器](Federation-Server-Farm-Using-WID.md)陣列所列出的相同限制

    | 1-100 個 RP 信任 | 超過 100 個 RP 信任 |
    |--|--|
    | **1-30 個 AD FS 節點：** 支援 WID | **1-30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |
    | **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL | **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |

## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議
若要部署此拓撲，除了新增兩個 web 應用程式 proxy，您必須確定您的周邊網路也可以存取網域名稱系統 (DNS) server，以及 (NLB) 主機存取第二個網路負載平衡。 第二個 NLB 主機必須使用使用可存取網際網路之叢集 IP 位址的 NLB 叢集來設定，而且必須使用與您在公司網路上設定的先前 NLB 叢集相同的叢集 DNS 名稱設定 (fs.fabrikam.com) 。 您也應該使用網際網路可存取的 IP 位址來設定 web 應用程式 proxy。

下圖顯示現有的同盟伺服器陣列與先前所述的 WID 拓撲，以及虛構 Fabrikam、Inc.、公司如何提供周邊 DNS 伺服器的存取權、新增具有相同叢集 DNS 名稱 (fs.fabrikam.com) 的第二部 NLB 主機，以及將兩個 web 應用程式 proxy (wap1 和 wap2) 新增至周邊網路。

![WID 伺服器陣列和 proxy](media/WIDFarmADFSBlue.gif)

如需有關如何設定網路環境以用於同盟伺服器或 web 應用程式 Proxy 的詳細資訊，請參閱 [AD FS 需求](AD-FS-Requirements.md) 和 [規劃 Web 應用程式 Proxy 基礎結構 (WAP) ](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))的「名稱解析需求」一節。

## <a name="see-also"></a>另請參閱
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md) 
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)

