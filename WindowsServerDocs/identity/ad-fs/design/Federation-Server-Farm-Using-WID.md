---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: 使用 WID 的同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c91b24313f3bb11592da1dcd25e8e9330fcfd250
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945355"
---
# <a name="legacy-ad-fs-federation-server-farm-using-wid"></a>舊版 AD FS 使用 WID 的同盟伺服器陣列

Active Directory 同盟服務 AD FS 的預設拓撲 \( \) 是同盟伺服器陣列，使用的是 Windows 內部資料庫 \( WID \) 。 在此拓撲中，AD FS 會針對加入該伺服器陣列的所有同盟伺服器，使用 WID 做為 AD FS 設定資料庫的存放區。 伺服器陣列會複寫並維護伺服器陣列中每部伺服器之設定資料庫的 Federation Service 資料。 Windows Server 2012 R2 中的 AD FS 可讓具有100或較少信賴憑證者信任的組織，使用最多30部伺服器的 WID 來設定同盟伺服器陣列。

在伺服器陣列中建立第一部同盟伺服器時，也會建立新的 Federation Service。 當您使用 WID 做為 AD FS 設定資料庫時，您在伺服器陣列中建立的第一部同盟伺服器稱為「*主要同盟伺服器*」。 這表示這部電腦是使用 AD FS 設定資料庫的讀取 \/ 寫入複本進行設定。

您為此伺服器陣列所設定的其他所有同盟伺服器稱為「*次要同盟伺服器*」，因為它們必須將在主要同盟伺服器上所做的任何變更複寫到 \- 其本機儲存的 AD FS 設定資料庫的唯讀複本。

> [!IMPORTANT]
> 建議您在負載平衡設定中至少使用兩部同盟伺服器 \- 。

## <a name="deployment-considerations"></a>部署考量因素
本節說明與此部署拓撲相關聯的目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

- 具有100或更少已設定信任關係的組織，必須提供他們的內部使用者 \( 登入實際連線到公司網路的電腦， \) 並具有 \- 同盟 \( \) 應用程式或服務的單一登入 SSO 存取權

- 想要為其內部使用者提供 Microsoft Online Services 或 Microsoft Office 365 的 SSO 存取權的組織

- 需要重複、可擴充服務的小型組織

> [!NOTE]
> 具有較大資料庫的組織應該考慮使用 SQL Server 部署拓撲的[同盟伺服器](Federation-Server-Farm-Using-SQL-Server.md)陣列。 具有從網路外部登入之使用者的組織，應考慮使用[使用 WID 和](Federation-Server-Farm-Using-WID-and-Proxies.md)proxy 拓撲的同盟伺服器陣列，或使用 SQL Server 拓撲的[同盟伺服器](Federation-Server-Farm-Using-SQL-Server.md)陣列。

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點有哪些？

- 提供內部使用者的 SSO 存取權

- 資料和同盟服務冗余 \( 每一部同盟伺服器都會將變更複寫到相同伺服器陣列中的其他同盟伺服器\)

- WID 隨附在 Windows 中;因此，不需要購買 SQL Server

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲的限制為何？

- 如果您有100或較少的信賴憑證者信任，WID 伺服器陣列的限制為30部同盟伺服器。

- WID 伺服器陣列不支援 \( 安全性聲明標記語言 \( SAML \) 通訊協定的權杖重新執行偵測或成品解析部分 \) 。

下表提供使用 WID 伺服器陣列的摘要。 使用它來規劃您的實施。

| 1-100 個 RP 信任 | 超過 100 個 RP 信任 |
|--|--|
| **1-30 個 AD FS 節點：** 支援 WID | **1-30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |
| **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL | **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |


## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議
當您準備好要開始在網路中部署此拓撲時，應該規劃將公司網路中的所有同盟伺服器放置在網路負載平衡 \( nlb 主機後方， \) 其可針對具有專用叢集網域名稱系統 \( DNS 名稱和叢集 IP 位址的 nlb 叢集進行設定 \) 。

> [!NOTE]
> 此叢集 DNS 名稱必須符合同盟服務名稱，例如 fs.fabrikam.com。

NLB 主機可以使用此 NLB 叢集中定義的設定，將用戶端要求配置給個別的同盟伺服器。 下圖顯示虛構 Fabrikam，Inc.，公司如何使用兩部 \- 電腦同盟伺服器陣列 \( fs1 和 FS2 搭配 \) WID，以及連接到公司網路的 DNS 伺服器和單一 NLB 主機的位置，來設定其部署的第一個階段。

![使用 WID 的伺服器陣列](media/FarmWID.gif)

> [!NOTE]
> 如果此單一 NLB 主機失敗，使用者將無法存取同盟應用程式或服務。 如果您的業務需求不允許有單一失敗點，請加入其他 NLB 主機。

如需有關如何設定網路環境以與同盟伺服器搭配使用的詳細資訊，請參閱[AD FS 需求](AD-FS-Requirements.md)中的名稱解析需求一節。

## <a name="see-also"></a>另請參閱
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md) 
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
