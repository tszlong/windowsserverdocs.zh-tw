---
description: 深入瞭解：使用 WID 的舊版 AD FS 同盟伺服器陣列
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: 使用 WID 的同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c92cc5bcd779bba4e75f70e9555aa6f5cf7092c9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046886"
---
# <a name="legacy-ad-fs-federation-server-farm-using-wid"></a>舊版 AD FS 使用 WID 的同盟伺服器陣列

Active Directory 同盟服務 AD FS 的預設拓撲 \( \) 是使用 Windows 內部資料庫 WID 的同盟伺服器陣列 \( \) 。 在此拓撲中，AD FS 會使用 WID 作為已加入該伺服器陣列之所有同盟伺服器的 AD FS 設定資料庫存放區。 伺服器陣列會複寫並維護伺服器陣列中每部伺服器之設定資料庫的 Federation Service 資料。 Windows Server 2012 R2 中的 AD FS 可讓具有100或較少信賴憑證者信任的組織使用最多30部伺服器的 WID 來設定同盟伺服器陣列。

在伺服器陣列中建立第一部同盟伺服器時，也會建立新的 Federation Service。 當您將 WID 用於 AD FS 設定資料庫時，您在伺服器陣列中建立的第一部同盟伺服器稱為「 *主要同盟伺服器*」。 這表示此電腦已設定 AD FS 設定資料庫的讀 \/ 寫複本。

您為此伺服器陣列設定的其他所有同盟伺服器稱為「 *次要同盟伺服器* 」，因為它們必須將在主要同盟伺服器上所做的任何變更複寫到 \- 其儲存在本機的 AD FS 設定資料庫的唯讀複本。

> [!IMPORTANT]
> 建議您在負載平衡設定中使用至少兩部同盟伺服器 \- 。

## <a name="deployment-considerations"></a>部署考量因素
本節說明與此部署拓撲相關聯之目標物件、優點和限制的各種考慮。

### <a name="who-should-use-this-topology"></a>誰應該使用此拓撲？

- 具有100或更少設定之信任關係的組織，需要提供使用者 \( 登入的內部使用者登入的電腦，這些電腦已實際連接到公司網路， \) 並具有同盟 \- \( \) 應用程式或服務的單一登入 SSO 存取權

- 想要為其內部使用者提供 Microsoft Online Services 或 Microsoft Office 365 的 SSO 存取權的組織

- 需要重複、可擴充服務的小型組織

> [!NOTE]
> 具有較大資料庫的組織應考慮使用 SQL Server 部署拓撲的 [同盟伺服器](Federation-Server-Farm-Using-SQL-Server.md) 陣列。 如果組織的使用者是從網路外部登入，則應該考慮使用 [使用 WID 和](Federation-Server-Farm-Using-WID-and-Proxies.md) proxy 拓撲的同盟伺服器陣列，或是 [使用 SQL Server 拓撲的同盟伺服器](Federation-Server-Farm-Using-SQL-Server.md) 陣列。

### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓撲的優點為何？

- 提供內部使用者的 SSO 存取權

- 資料和同盟服務冗余-每部 \( 同盟伺服器會將變更複寫到相同伺服器陣列中的其他同盟伺服器\)

- WID 隨附于 Windows;因此，不需要購買 SQL Server

### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓撲有哪些限制？

- 如果您有100或更少的信賴憑證者信任，WID 伺服器陣列的限制為30部同盟伺服器。

- WID 伺服器陣列不支援權杖重新執行偵測或 \( 安全性聲明標記語言 \( SAML 通訊協定的構件解析 \) 部分 \) 。

下表提供使用 WID 伺服器陣列的摘要。 您可以使用它來規劃您的執行。

| 1-100 個 RP 信任 | 超過 100 個 RP 信任 |
|--|--|
| **1-30 個 AD FS 節點：** 支援 WID | **1-30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |
| **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL | **超過 30 個 AD FS 節點：** 不支援使用 WID - 需要 SQL |


## <a name="server-placement-and-network-layout-recommendations"></a>伺服器放置和網路設定建議
當您準備好要在網路中開始部署此拓撲時，您應該規劃將公司網路中的所有同盟伺服器放置在網路負載平衡 \( NLB 主機後方， \) 該主機可針對具有專用叢集網功能變數名稱稱系統 \( DNS \) 名稱和叢集 IP 位址的 nlb 叢集進行設定。

> [!NOTE]
> 此叢集 DNS 名稱必須符合同盟服務名稱，例如，fs.fabrikam.com。

NLB 主機可以使用此 NLB 叢集中定義的設定，將用戶端要求配置給個別的同盟伺服器。 下圖顯示虛構 Fabrikam，Inc. 的公司如何使用兩部電腦同盟伺服器陣列 fs1 和 WID 來設定其部署的第一個階段，以及如何使用 \- \( \) WID 和 DNS 伺服器的位置，以及連接到公司網路的單一 NLB 主機。

![使用 WID 的伺服器陣列](media/FarmWID.gif)

> [!NOTE]
> 如果這個單一 NLB 主機上發生失敗，使用者將無法存取同盟應用程式或服務。 如果您的業務需求不允許有單一失敗點，請加入其他 NLB 主機。

如需有關如何設定網路環境以與同盟伺服器搭配使用的詳細資訊，請參閱 [AD FS 需求](AD-FS-Requirements.md)中的名稱解析需求一節。

## <a name="see-also"></a>另請參閱
[規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md) 
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
