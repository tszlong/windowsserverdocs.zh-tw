---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: AD FS 部署拓撲考量
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cf646dedef85add8607c7940275e3c3fae90a661
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445338"
---
# <a name="ad-fs-deployment-topology-considerations"></a>AD FS 部署拓撲考量

本主題描述可協助您規劃和設計的 Active Directory Federation Services 的重要考量事項\(AD FS\)部署拓撲，以在生產環境中使用。 本主題會檢閱和評估會影響哪些功能可供您之後部署 AD FS 的考量事項的起始點。 例如，視資料庫而定您選擇儲存 AD FS 設定資料庫的型別，最終將決定您是否可以實作特定安全性聲明標記語言\(SAML\)需要 SQL 的功能伺服器。  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>決定要使用的 AD FS 設定資料庫的類型  
AD FS 會使用資料庫來儲存設定和 — 在某些情況下，交易資料與 Federation Service。 您可以使用 AD FS 軟體，選取任一個內建\-在 Windows 內部資料庫\(WID\)或 Microsoft SQL Server 2005 或更新版本來儲存 Federation Service 中的資料。  

對於大多數用途而言，這兩個資料庫類型相對來說是一樣的。 不過，有開始深入了解您可以使用 AD FS 使用的各種部署拓撲之前要注意的一些差異。 下表說明 WID 資料庫與 SQL Server 資料庫之間的差異在支援的功能。  

AD FS 功能  

|功能|受到 WID 支援？|受到 SQL Server 支援？|關於這個功能的詳細資訊|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|同盟伺服器陣列部署|是，限制為 30 的同盟伺服器，每個伺服陣列|是的。 沒有限制您可以在單一伺服陣列中部署的同盟伺服器數目|[決定您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|  
|SAML 成品解析**附註：** 這個功能並非 Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例的必要功能。|否|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-同盟權杖重新執行偵測|否|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[安全規劃和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

資料庫功能  

|功能|受到 WID 支援？|受到 SQL Server 支援？|關於這個功能的詳細資訊|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|基本資料庫備援使用提取複寫，其中一個或多個伺服器裝載讀取\-的資料庫要求變更裝載讀取的來源伺服器上所做的只有一個複本\/寫入資料庫的複本|是|否|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|使用高的資料庫備援\-可用性的解決方案，例如容錯移轉叢集或鏡像\(在資料庫層只\)**附註：** 所有的 AD FS 部署拓撲支援 AD FS 服務層的叢集。|否|是|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[高可用性解決方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>SQL Server 考量  
如果您的 AD FS 部署選取 SQL Server 做為設定資料庫，您應該考慮下列部署事實。  

-   **SAML 功能及其對於資料庫大小與成長的影響**。 SAML 成品解析或 SAML 權杖重新執行偵測功能啟用時，AD FS 會在每個 AD FS 權杖所發出的 SQL Server 組態資料庫中儲存資訊。 此活動的結果的 SQL Server 資料庫的成長並不明顯，並已設定的權杖重新執行保留期間而定。 每個成品記錄的大小為大約 30 kb \(KB\)。  

-   **部署所需的伺服器數目**。 您必須新增至少一個額外的伺服器\(來部署您的 AD FS 基礎結構所需的伺服器總數\)，做為 SQL Server 執行個體的專用主機。 如果您打算使用容錯移轉叢集或鏡像 SQL Server 設定資料庫提供容錯和延展性，兩部 SQL server 至少需要。  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>您選取設定資料庫類型的方式可能會影響硬體資源  
而不在使用 SQL Server 資料庫的伺服器陣列中部署同盟伺服器使用 WID 伺服器陣列中部署的同盟伺服器上的硬體資源影響並不重要。 不過，很重要的考量，當您使用 WID 伺服器陣列時，該伺服陣列中的每部同盟伺服器必須儲存、 管理及維護 AD FS 設定資料庫的本機複本的複寫變更，同時還要繼續提供一般Federation Service 所需的作業。  

相較之下，使用 SQL Server 資料庫的伺服器陣列中部署的同盟伺服器並不會需要包含 AD FS 設定資料庫的本機執行個體。 因此，它們對硬體資源的要求會比較低。  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>確定您的實際執行環境可以支援 AD FS 部署  
除了同盟伺服器，您將部署，並根據您現有的生產環境的設定中，您就可能需要下列額外的伺服器提供必要的基礎結構，以支援新的 AD FS 部署：  

-   Active Directory 網域控制站  

-   憑證授權單位\(CA\)  

-   裝載同盟中繼資料的 Web 伺服器  

-   網路負載平衡\(NLB\)  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
