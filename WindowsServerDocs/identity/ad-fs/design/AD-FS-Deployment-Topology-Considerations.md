---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: AD FS 部署拓撲考量
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8e433083ce7f1bdcfa0d950b86692662044dd1ea
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940435"
---
# <a name="ad-fs-deployment-topology-considerations"></a>AD FS 部署拓撲考量

本主題說明可協助您規劃和設計哪些 Active Directory 同盟服務 \( AD FS \) 部署拓撲以在生產環境中使用的重要考慮。 本主題是一個起點，可供您在部署 AD FS 之後，檢查及評估會影響您可使用哪些功能的考慮。 例如，視您選擇用來儲存 AD FS 設定資料庫的資料庫類型而定，最終會決定您是否可以執行某些 \( \) 需要 SQL Server 的安全性聲明標記語言 SAML 功能。

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>決定要使用的 AD FS 設定資料庫類型
AD FS 使用資料庫來儲存設定，而在某些情況下，則是與同盟服務相關的交易資料。 您可以使用 AD FS 軟體來選取內建的 \- Windows 內部資料庫 WID， \( \) 或 Microsoft SQL Server 2005 或更新版本，以將資料儲存在同盟服務中。

對於大多數用途而言，這兩個資料庫類型相對來說是一樣的。 不過，在您開始閱讀更多可與 AD FS 搭配使用的部署拓撲之前，請注意一些差異。 下表說明 WID 資料庫與 SQL Server 資料庫之間支援之功能的差異。

AD FS 功能

|功能|受到 WID 支援？|受到 SQL Server 支援？|關於這個功能的詳細資訊|
|-----------|---------------------|----------------------------|---------------------------------------|
|同盟伺服器陣列部署|是，每個伺服器陣列的限制為30部同盟伺服器|是。 沒有限制您可以在單一伺服陣列中部署的同盟伺服器數目|[決定您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|
|SAML 成品解析**注意事項：** Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例不需要這項功能。|否|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|
|SAML \/ WS \- 同盟權杖重新執行偵測|否|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|

資料庫功能

|功能|受到 WID 支援？|受到 SQL Server 支援？|關於這個功能的詳細資訊|
|-----------|---------------------|----------------------------|---------------------------------------|
|使用提取複寫的基本資料庫冗余，其中一或多部裝載資料庫的唯讀複本的伺服器， \- 會在裝載資料庫之讀取寫入複本的來源伺服器上進行變更 \/|是|否|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|
|使用高可用性解決方案的資料庫冗余 \- ，例如在資料庫層級的容錯移轉叢集或鏡像， \( 只會注意到 \) **：** 所有 AD FS 部署拓撲都支援 AD FS 服務層級的叢集。|否|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[高可用性解決方案概觀 (可能為英文網頁)](https://go.microsoft.com/fwlink/?LinkId=179853)|

### <a name="sql-server-considerations"></a>SQL Server 考量
如果您選取 SQL Server 做為 AD FS 部署的設定資料庫，您應該考量下列部署事實。

-   **SAML 功能及其對於資料庫大小與成長的影響**。 在啟用 SAML 成品解析或 SAML 權杖重新執行偵測功能時，AD FS 會在 SQL Server 設定資料庫中儲存所簽發之每個 AD FS 權杖的資訊。 因為這個活動而導致的 SQL Server 資料庫成長並不明顯，而且會根據設定之權杖的重新執行保留期間而定。 每個成品記錄的大小約為 30 kb \( \) 。

-   **部署所需的伺服器數目**。 您必須將至少一部額外的伺服器新增 \( 到部署 AD FS 基礎結構所需的伺服器總數，以 \) 作為 SQL Server 實例的專用主機。 如果您規劃使用容錯移轉叢集或鏡像來為 SQL Server 設定資料庫提供容錯和延展性，則至少需要兩部 SQL Srver。

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>您選取設定資料庫類型的方式可能會影響硬體資源
與使用 SQL Server 資料庫在伺服陣列中部署的同盟伺服器相較之下，對使用 WID 在伺服陣列中部署之同盟伺服器上的硬體資源影響並不明顯。 但是，請務必考量當您針對伺服陣列使用 WID 時，該伺服陣列中的每部同盟伺服器都必須儲存、管理及維護其 AD FS 設定資料庫之本機複本的複寫變更，同時還要繼續提供 Federation Service 所需的一般操作。

相較之下，使用 SQL Server 資料庫在伺服陣列中部署的同盟伺服器並不需要包含 AD FS 設定資料庫的本機執行個體。 因此，它們對硬體資源的要求會比較低。

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>確定您的實際執行環境可以支援 AD FS 部署
除了您要部署的同盟伺服器之外，根據現有生產環境的設定方式而定，可能還需要下列額外的伺服器來提供必要的基礎結構，以支援新的 AD FS 部署：

-   Active Directory 網域控制站

-   憑證授權單位單位 \( CA\)

-   裝載同盟中繼資料的 Web 伺服器

-   網路負載平衡 \( NLB\)

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
