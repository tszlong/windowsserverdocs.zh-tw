---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: AD FS 部署拓撲考量
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 260d86c0feae0179620ece09e06f12729691b5a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359218"
---
# <a name="ad-fs-deployment-topology-considerations"></a>AD FS 部署拓撲考量

本主題說明可協助您規劃和設計哪些 Active Directory 同盟服務 \(AD FS 在生產環境中使用的\) 部署拓撲的重要考慮。 本主題是一個起點，可供您在部署 AD FS 之後，檢查及評估會影響您可使用哪些功能的考慮。 例如，視您選擇用來儲存 AD FS 設定資料庫的資料庫類型而定，最終會決定您是否可以執行需要 SQL Server 的某些安全性聲明標記語言 \(SAML\) 功能。  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>判斷要使用哪種類型的 AD FS 設定資料庫  
AD FS 使用資料庫來儲存設定，而在某些情況下，則是與同盟服務相關的交易資料。 您可以使用 AD FS 軟體，選取 Windows 內部資料庫中的內建\-\(WID\) 或 Microsoft SQL Server 2005 或更新版本，將資料儲存在同盟服務中。  

對於大多數用途而言，這兩個資料庫類型相對來說是一樣的。 不過，在您開始閱讀更多可與 AD FS 搭配使用的部署拓撲之前，請注意一些差異。 下表描述 WID 資料庫與 SQL Server 資料庫之間支援的功能差異。  

AD FS 功能  

|功能|受到 WID 支援？|受到 SQL Server 支援？|關於這個功能的詳細資訊|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|同盟伺服器陣列部署|是，每個伺服器陣列的限制為30部同盟伺服器|是。 沒有限制您可以在單一伺服陣列中部署的同盟伺服器數目|[決定您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|  
|SAML 成品解析**注意事項：** Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例不需要這項功能。|不可以|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-同盟權杖重新執行偵測|不可以|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

資料庫功能  

|功能|受到 WID 支援？|受到 SQL Server 支援？|關於這個功能的詳細資訊|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|使用提取複寫的基本資料庫冗余，其中一或多部裝載讀取\-的伺服器只會在裝載資料庫讀取\/寫入複本的來源伺服器上進行變更|是|不可以|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|使用高\-可用性解決方案的資料庫冗余，例如在資料庫層級的容錯移轉叢集或鏡像 \(僅\)**注意：** 所有 AD FS 部署拓撲都支援 AD FS 服務層級的叢集。|不可以|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[高可用性解決方案總覽](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>SQL Server 考量  
如果您選取 SQL Server 做為 AD FS 部署的設定資料庫，您應該考慮下列部署事實。  

-   **SAML 功能及其對於資料庫大小與成長的影響**。 當 SAML 成品解析或 SAML 權杖重新執行偵測功能已啟用時，AD FS 會針對每個發出的 AD FS 權杖，儲存 SQL Server 設定資料庫中的資訊。 由於此活動的結果，SQL Server 資料庫的成長不會被視為重要，而是取決於已設定的權杖重新執行保留期間。 每個成品記錄的大小約為 30 kb，\(KB\)。  

-   **部署所需的伺服器數目**。 您必須將至少一部額外的伺服器 \(到部署 AD FS 基礎結構\) 所需的伺服器總數，以作為 SQL Server 實例的專用主機。 如果您打算使用容錯移轉叢集或鏡像來為 SQL Server 設定資料庫提供容錯和擴充性，則至少需要兩部 SQL server。  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>您選取設定資料庫類型的方式可能會影響硬體資源  
使用 WID （而不是在使用 SQL Server 資料庫的伺服器陣列中部署的同盟伺服器）部署在伺服器陣列中的同盟伺服器上，對硬體資源的影響並不重要。 不過，請務必考慮當您使用 WID 做為伺服器陣列時，該伺服器陣列中的每部同盟伺服器都必須儲存、管理及維護其 AD FS 設定資料庫本機複本的複寫變更，同時繼續提供正常的同盟服務所需的作業。  

相較之下，在使用 SQL Server 資料庫的伺服器陣列中部署的同盟伺服器不一定包含 AD FS 設定資料庫的本機實例。 因此，它們對硬體資源的要求會比較低。  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>確定您的實際執行環境可以支援 AD FS 部署  
除了您要部署的同盟伺服器之外，根據現有生產環境的設定方式而定，可能還需要下列額外的伺服器來提供必要的基礎結構，以支援新的 AD FS 部署：  

-   Active Directory 網域控制站  

-   CA\) 的憑證授權單位單位 \(  

-   裝載同盟中繼資料的 Web 伺服器  

-   網路負載平衡 \(NLB\)  

## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
