---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: 規劃您的 AD FS 部署拓撲
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 156a7c451038fc40cf22037138bbcf446c93de1e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972105"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>規劃您的 AD FS 部署拓撲

規劃 Active Directory 同盟服務 (AD FS) 部署的第一步，就是決定適當的部署拓撲，以符合您組織的需求。

閱讀本主題之前，請先參閱如何將 AD FS 資料儲存及複寫到同盟伺服器陣列中的其他同盟伺服器，並確定您瞭解的用途，以及可用於儲存在 AD FS 設定資料庫中基礎資料的複寫方法。

您可以使用兩種資料庫類型來儲存 AD FS 設定資料： Windows 內部資料庫 (WID) 和 Microsoft SQL Server。 如需詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。 請參閱使用 WID 或 SQL Server 做為 AD FS 設定資料庫相關聯的各種優點和限制，以及它們支援的各種應用程式案例，然後進行選取。

> [!IMPORTANT]
> 若要實作基本備援、負載平衡，以及調整 Federation Service (如果需要) 的選項，無論您將使用的資料庫類型為何，都建議您針對所有實際執行環境中的每個同盟伺服器陣列至少部署兩部同盟伺服器。

## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>決定要使用的 AD FS 設定資料庫類型
AD FS 使用資料庫來儲存設定，而在某些情況下，則是與同盟服務相關的交易資料。 您可以使用 AD FS 軟體來選取內建的 Windows 內部資料庫 (WID) 或 Microsoft SQL Server 2008 或更新版本，以將資料儲存在同盟服務中。

對於大多數用途而言，這兩個資料庫類型相對來說是一樣的。 不過，在您開始閱讀更多可與 AD FS 搭配使用的部署拓撲之前，請注意一些差異。 下表說明 WID 資料庫與 SQL Server 資料庫之間支援之功能的差異。

|描述|功能|受到 WID 支援？|受到 SQL Server 支援？
| --- | --- | --- |--- |
|AD FS 功能|同盟伺服器陣列部署|是。 如果您有100或較少的信賴憑證者信任，WID 伺服器陣列的限制為30部同盟伺服器。</br></br>WID 伺服器陣列不支援權杖重新執行偵測或成品解析 (安全性聲明標記語言 (SAML) 通訊協定) 的一部分。 |是。 沒有限制您可以在單一伺服陣列中部署的同盟伺服器數目
|AD FS 功能|SAML 成品解析 </br></br>**注意：** Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例不需要這項功能。|否|是
|AD FS 功能|SAML/WS-同盟權杖重新執行偵測|否|是
|資料庫功能|使用提取複寫的基本資料庫備援，其中裝載資料庫唯讀複本的一或多台伺服器會要求在裝載資料庫讀取/寫入複本之來源伺服器上所做的變更|是|否
|資料庫功能|使用高可用性解決方案（例如，容錯移轉叢集或資料庫層級的鏡像 (）的資料庫冗余僅) **注意：** 所有 AD FS 部署拓撲都支援 AD FS 服務層級的叢集。|否|是

## <a name="sql-server-considerations"></a>SQL Server 考量
如果您選取 SQL Server 做為 AD FS 部署的設定資料庫，您應該考量下列部署事實。

- **SAML 功能及其對於資料庫大小與成長的影響**。 在啟用 SAML 成品解析或 SAML 權杖重新執行偵測功能時，AD FS 會在 SQL Server 設定資料庫中儲存所簽發之每個 AD FS 權杖的資訊。 因為這個活動而導致的 SQL Server 資料庫成長並不明顯，而且會根據設定之權杖的重新執行保留期間而定。 每個成品記錄的大小約為 30 KB。

- **部署所需的伺服器數目**。 您必須將至少一部額外的伺服器 (到部署 AD FS 基礎結構) 所需的伺服器總數，以作為 SQL Server 實例的專用主機。 如果您規劃使用容錯移轉叢集或鏡像來為 SQL Server 設定資料庫提供容錯和延展性，則至少需要兩部 SQL Srver。

## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>您選取設定資料庫類型的方式可能會影響硬體資源
與使用 SQL Server 資料庫在伺服陣列中部署的同盟伺服器相較之下，對使用 WID 在伺服陣列中部署之同盟伺服器上的硬體資源影響並不明顯。 但是，請務必考量當您針對伺服陣列使用 WID 時，該伺服陣列中的每部同盟伺服器都必須儲存、管理及維護其 AD FS 設定資料庫之本機複本的複寫變更，同時還要繼續提供 Federation Service 所需的一般操作。

相較之下，使用 SQL Server 資料庫在伺服陣列中部署的同盟伺服器並不需要包含 AD FS 設定資料庫的本機執行個體。 因此，它們對硬體資源的要求會比較低。

## <a name="where-to-place-a-federation-server"></a><a name="BKMK_1"></a>放置同盟伺服器的位置
基於安全性最佳作法，請將 AD FS 同盟伺服器放在防火牆前面，並將它們連線到您的公司網路，以避免暴露在網際網路上。 這很重要，因為同盟伺服器具有授與安全性權杖的完整授權。 因此，它們應該像網域控制站一樣受到保護。 如果同盟伺服器遭到入侵，惡意使用者就能夠對所有 Web 應用程式和受 AD FS 保護的同盟伺服器發出完整存取權杖。

> [!NOTE]
> 基於安全性最佳作法，請避免在網際網路上直接存取您的同盟伺服器。 請考慮在您設定測試實驗室環境時，或當您的組織沒有周邊網路時，讓您的同盟伺服器直接存取網際網路。

對於一般的公司網路而言，公司網路與周邊網路之間會建立內部網路對向防火牆，而周邊網路與網際網路之間通常會建立網際網路對向防火牆。 在此情況下，同盟伺服器位於公司網路內，且不能由網際網路用戶端直接存取。

> [!NOTE]
> 連線到公司網路的用戶端電腦可以透過 Windows 整合式驗證直接與同盟伺服器通訊。

您必須先將同盟伺服器 proxy 放在周邊網路中，才能設定防火牆伺服器以與 AD FS 搭配使用。

## <a name="supported-deployment-topologies"></a>支援的部署拓撲
下列主題說明您可以搭配 AD FS 使用的各種部署拓撲。 其中也會說明與每個部署拓撲相關聯的優點與限制，讓您能夠針對特定的商業需求選取最適當的拓撲。

- [使用 WID 的同盟伺服器陣列](Federation-Server-Farm-Using-WID.md)

- [使用 WID 和 Proxy 的同盟伺服器陣列](Federation-Server-Farm-Using-WID-and-Proxies.md)

- [使用 SQL Server 的同盟伺服器陣列](Federation-Server-Farm-Using-SQL-Server.md)

## <a name="see-also"></a>另請參閱
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
