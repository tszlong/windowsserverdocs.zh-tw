---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: "AD FS 部署拓撲注意事項"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eee14ee7bb50e1a82f35caf9fbacda0b86d3a1ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-deployment-topology-considerations"></a>AD FS 部署拓撲注意事項

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述，協助您計畫並設計 production 環境中使用的 Active Directory 同盟服務 \(AD FS\) 部署拓撲重要的注意事項。 此主題是「檢視及評估考量影響哪些功能將會提供給 AD FS 部署之後的起點。 例如根據的資料庫輸入您選擇儲存 AD FS 設定資料庫將最終來判斷您是否可以實作需要 SQL Server 特定安全性判斷提示標記語言 \(SAML\) 功能。  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>判斷哪一種 AD FS 使用的設定資料庫  
AD FS 使用儲存設定資料庫和 — 有時候 — 交易資料相關同盟服務。 您可以使用 AD FS 軟體選取 built\ 中 Windows 內部資料庫 \(WID\) 或 Microsoft SQL Server 2005 或較新版本同盟服務中儲存資料。  
  
大多數用途，資料庫兩種類型的相當等。 但是，有一些開始朗讀詳細資訊，您可以使用 AD FS 使用的各種部署拓撲之前會注意到不同。 下表描述 WID 資料庫和 edition 是不同中支援的功能。  
  
AD FS 功能  
  
|功能|支援 WID 嗎？|支援 SQL Server 嗎？|此功能的相關詳細資訊|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|聯盟伺服器發電廠部署|是，使用的每個農場五個聯盟伺服器限制|[是]。 聯盟伺服器，您可以在單一發電廠部署的數目無執行限制|[判斷您 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)|  
|SAML 成品解析度**請注意：**此功能不需 Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例。|否]|[是]|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[規劃安全和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|聯盟-WS\ SAML\ 日權杖重播偵測|否]|[是]|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[規劃安全和部署 AD FS 的最佳做法](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
資料庫功能  
  
|功能|支援 WID 嗎？|支援 SQL Server 嗎？|此功能的相關詳細資訊|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|基本資料庫重複使用提取複寫，其中一或多個伺服器裝載 read\ 僅限來源的伺服器上的資料庫要求變更的複本，主控資料庫 read\/寫入複本|[是]|否]|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|使用 high\ 可用性方案，例如錯誤後的移轉叢集或鏡像資料庫冗餘 \（在資料庫層 only\)**請注意：**所有 AD FS 部署拓撲都支援叢集 AD FS 服務層級。|否]|[是]|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[可用性方案概觀](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>SQL Server 注意事項  
如果您 AD FS 部署選取設定資料庫 SQL Server，考慮下列部署實用資訊。  
  
-   **SAML 功能，並其資料庫大小和成長**。 SAML 成品解析度或 SAML 權杖重播偵測功能的支援，AD FS 會所發行的每個 AD FS 標記 SQL Server 設定資料庫中儲存的資訊。 SQL Server 資料庫根據這項活動的成長並不會很大，並設定權杖重播保留期間而定。 每個成品記錄具有約 30 kb \(KB\) 的大小。  
  
-   **伺服器部署所需的數字**。 您將需要新增至少一個其他伺服器 \（若要將您 AD FS infrastructure\ 部署所需的伺服器總數）可做為專用主機 SQL Server 執行個體。 如果您打算使用錯誤後的移轉叢集或鏡像提供 SQL Server 設定資料庫容錯和延展性的兩個 SQL server 至少需要。  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>如何設定資料庫類型，您可能會影響硬體資源  
不很大的影響，而不聯盟伺服器部署陣列使用 SQL Server 資料庫中使用 WID 發電廠中部署聯盟伺服器上的硬體資源。 不過，請務必現在，當您使用 WID 陣列時，在農地的每個聯盟伺服器必須市集、管理及維護複寫本機複本 AD FS 資料庫設定的變更時也會持續提供同盟服務需要正常運作。  
  
相較之下，使用 SQL Server 資料庫發電廠中部署聯盟伺服器不一定包含 AD FS 設定資料庫本機執行個體。 因此，它們可以製作硬體資源稍微較少的要求。  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>確認您的實際執行環境，可支援 AD FS 部署  
除了將部署聯盟伺服器和您現有的 production 環境設定方式而定，下列其他伺服器可能需要提供支援新 AD FS 部署所需的基礎結構：  
  
-   Active Directory 網域控制站  
  
-   憑證授權單位 \(CA\)  
  
-   網頁伺服器主機聯盟中繼資料  
  
-   網路負載平衡 \(NLB\)  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
