---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: "AD FS 部署拓撲計劃"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="plan-your-ad-fs-deployment-topology"></a>AD FS 部署拓撲計劃

>適用於：Windows Server 2016、Windows Server 2012 R2

規劃部署 \(AD FS\) Active Directory 同盟服務的第一個步驟是以判斷正確部署拓撲貴組織的需求。  
  
您讀取本主題之前，檢視如何儲存和複製到其他聯盟伺服器聯盟伺服器 AD FS 資料，請確定您知道的目的和複寫方法可用的基礎 AD FS 設定資料庫中儲存的資料。  
  
有兩種資料庫類型，您可以用來儲存 AD FS 設定的資料：Windows 內部資料庫 \(WID\) 與 Microsoft SQL Server。 如需詳細資訊，請查看[的角色 AD FS 設定資料庫的](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。 檢視不同的優點和 AD FS 設定資料庫，以及各種不同的應用程式案例他們支援，請選擇使用 WID 或 SQL Server 相關聯的限制。  
  
> [!IMPORTANT]  
> 實作基本冗餘、負載平衡和縮放同盟服務 \(if required\) 的選項，我們建議您部署至少兩部聯盟伺服器每聯盟伺服器陣列所有 production 環境，無論您將會使用資料庫類型。  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>判斷哪一種 AD FS 使用的設定資料庫  
AD FS 使用儲存設定資料庫和 — 有時候 — 交易資料相關同盟服務。 您可以使用 AD FS 軟體選取 built\ 中 Windows 內部資料庫 \(WID\) 或 2008 年，或較新版本的 Microsoft SQL Server 同盟服務中儲存資料。  
  
大多數用途，資料庫兩種類型的相當等。 但是，有一些開始朗讀詳細資訊，您可以使用 AD FS 使用的各種部署拓撲之前會注意到不同。 下表描述 WID 資料庫和 edition 是不同中支援的功能。  
  
||功能|支援 WID 嗎？|支援 SQL Server 嗎？
| --- | --- | --- |--- |
|AD FS 功能|聯盟伺服器發電廠部署|[是]。 如果您依賴 100 或較少廠商信任，WID 發電廠的 30 聯盟伺服器的上限。</br></br>WID 發電廠不支援權杖重播偵測或成品的解析度（部安全性判斷提示標記的語言 (SAML) 通訊協定）。 |[是]。 聯盟伺服器，您可以在單一發電廠部署的數目無執行限制  
|AD FS 功能|SAML 成品解析度 </br></br>**注意：**這個功能不需 Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例。|否]|[是]  
|AD FS 功能|聯盟-WS\ SAML\ 日權杖重播偵測|否]|[是]  
|資料庫功能|基本資料庫重複使用提取複寫，其中一或多個伺服器裝載 read\ 僅限來源的伺服器上的資料庫要求變更的複本，主控資料庫 read\/寫入複本|[是]|否] 
|資料庫功能|使用 high\ 可用性方案，例如錯誤後的移轉叢集或鏡像資料庫冗餘 \（在資料庫層 only\)**請注意：**所有 AD FS 部署拓撲都支援叢集 AD FS 服務層級。|否]|[是]  

  
## <a name="sql-server-considerations"></a>SQL Server 注意事項  
如果您 AD FS 部署選取設定資料庫 SQL Server，考慮下列部署實用資訊。  
  
-   **SAML 功能，並其資料庫大小和成長**。 SAML 成品解析度或 SAML 權杖重播偵測功能的支援，AD FS 會所發行的每個 AD FS 標記 SQL Server 設定資料庫中儲存的資訊。 SQL Server 資料庫根據這項活動的成長並不會很大，並設定權杖重播保留期間而定。 每個成品記錄具有約 30 kb \(KB\) 的大小。  
  
-   **伺服器部署所需的數字**。 您將需要新增至少一個其他伺服器 \（若要將您 AD FS infrastructure\ 部署所需的伺服器總數）可做為專用主機 SQL Server 執行個體。 如果您打算使用錯誤後的移轉叢集或鏡像提供 SQL Server 設定資料庫容錯和延展性的兩個 SQL server 至少需要。  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>如何設定資料庫類型，您可能會影響硬體資源  
不很大的影響，而不聯盟伺服器部署陣列使用 SQL Server 資料庫中使用 WID 發電廠中部署聯盟伺服器上的硬體資源。 不過，請務必現在，當您使用 WID 陣列時，在農地的每個聯盟伺服器必須市集、管理及維護複寫本機複本 AD FS 資料庫設定的變更時也會持續提供同盟服務需要正常運作。  
  
相較之下，使用 SQL Server 資料庫發電廠中部署聯盟伺服器不一定包含 AD FS 設定資料庫本機執行個體。 因此，它們可以製作硬體資源稍微較少的要求。  
  
## <a name="BKMK_1"></a>放置聯盟伺服器的位置  
基於安全性最佳練習、AD FS 聯盟伺服器前面防火牆然後將它們連接到您的企業網路，以避免遭受從網際網路。 因為聯盟伺服器有完整的授權以授與的安全性權杖，這很重要。 因此，它們應該會有為網域控制站的相同的保護。 受到聯盟伺服器，惡意使用者所有 Web 應用程式，並聯盟伺服器受到 AD FS 發出權杖完整存取權的能力。  
  
> [!NOTE]  
> 安全性與最佳做法，請避免在網際網路上遇到聯盟伺服器直接存取。 請考慮實驗室測試或組織不具有周邊網路時，您的設定時，只提供您聯盟伺服器直接存取網際網路。  
  
一般的企業網路，intranet\ 面向防火牆建立公司網路和周邊網路，並在 Internet\ 面向防火牆通常會建立周邊網路與網際網路之間。 此時，聯盟伺服器位於中的企業網路，並不是用網際網路直接存取。  
  
> [!NOTE]  
> Client 電腦連接到企業網路，可以直接與透過 Windows 整合式驗證聯盟伺服器通訊。  
  
您在設定使用您防火牆伺服器 AD FS 進行之前，聯盟 proxy 伺服器應該會放在周邊網路。  
  
## <a name="supported-deployment-topologies"></a>支援的部署拓撲  
下列主題描述，您可以使用 AD FS 使用的各種部署拓撲。 它們也描述的優點和，因此您可以選取最適合拓撲針對特定企業需求的相關每個部署拓撲限制。  
  
-   [使用 WID 聯盟伺服器陣列](Federation-Server-Farm-Using-WID.md)  
  
-   [使用 WID 與 Proxy 聯盟伺服器陣列](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [使用 SQL Server 聯盟伺服器陣列](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>也了  
[在 Windows Server 2012 R2 的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

