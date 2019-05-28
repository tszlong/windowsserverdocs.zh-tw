---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: 規劃您的 AD FS 部署拓撲
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00c43a56d9b57a2ae2c8b9aeca56807fe1d1841f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191183"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>規劃您的 AD FS 部署拓撲

規劃部署的 Active Directory Federation Services 的第一個步驟\(AD FS\)是要判斷正確的部署拓撲以符合您組織的需求。  
  
閱讀本主題之前，請檢閱 AD FS 資料如何儲存和複寫到同盟伺服器陣列中的其他同盟伺服器，並確定您了解的目的，以及可用來儲存 AD FS 中的基礎資料的複寫方法 configuration 資料庫。  
  
有兩種可用來儲存 AD FS 組態資料的資料庫類型：Windows 內部資料庫\(WID\)和 Microsoft SQL Server。 如需詳細資訊，請參閱 [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。 檢閱的各種優點與限制，為 AD FS 組態資料庫，以及他們支援，然後進行選擇的各種應用程式案例使用 WID 或 SQL Server 相關聯。  
  
> [!IMPORTANT]  
> 若要實作基本備援、 負載平衡，以及調整 Federation Service 的選項\(如有必要\)，我們建議您部署的每個同盟伺服器陣列所有生產環境中的至少兩部同盟伺服器不論您將使用的資料庫型別。  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>決定要使用的 AD FS 設定資料庫類型  
AD FS 會使用資料庫來儲存設定和 — 在某些情況下，交易資料與 Federation Service。 您可以使用 AD FS 軟體，選取任一個內建\-在 Windows 內部資料庫\(WID\)或 Microsoft SQL Server 2008 或更新版本來儲存 Federation Service 中的資料。  
  
對於大多數用途而言，這兩個資料庫類型相對來說是一樣的。 不過，有開始深入了解您可以使用 AD FS 使用的各種部署拓撲之前要注意的一些差異。 下表說明 WID 資料庫與 SQL Server 資料庫之間支援之功能的差異。  
  
||功能|受到 WID 支援？|受到 SQL Server 支援？
| --- | --- | --- |--- |
|AD FS 功能|同盟伺服器陣列部署|是的。 WID 伺服器陣列已限制為 30 的同盟伺服器，如果您有 100 或更少信賴憑證者信任。</br></br>WID 伺服器陣列不支援權杖重新執行偵測或成品解析 （安全性判斷提示標記語言 (SAML) 通訊協定的一部分）。 |是的。 沒有限制您可以在單一伺服陣列中部署的同盟伺服器數目  
|AD FS 功能|SAML 成品解析 </br></br>**注意：** 這個功能並非 Microsoft Online Services、Microsoft Office 365、Microsoft Exchange 或 Microsoft Office SharePoint 案例的必要功能。|否|是  
|AD FS 功能|SAML\/WS\-同盟權杖重新執行偵測|否|是  
|資料庫功能|基本資料庫備援使用提取複寫，其中一個或多個伺服器裝載讀取\-的資料庫要求變更裝載讀取的來源伺服器上所做的只有一個複本\/寫入資料庫的複本|是|否 
|資料庫功能|使用高的資料庫備援\-可用性的解決方案，例如容錯移轉叢集或鏡像\(在資料庫層只\)**附註：** 所有的 AD FS 部署拓撲支援 AD FS 服務層的叢集。|否|是  

  
## <a name="sql-server-considerations"></a>SQL Server 考量  
如果您選取 SQL Server 做為 AD FS 部署的設定資料庫，您應該考量下列部署事實。  
  
-   **SAML 功能及其對於資料庫大小與成長的影響**。 在啟用 SAML 成品解析或 SAML 權杖重新執行偵測功能時，AD FS 會在 SQL Server 設定資料庫中儲存所簽發之每個 AD FS 權杖的資訊。 因為這個活動而導致的 SQL Server 資料庫成長並不明顯，而且會根據設定之權杖的重新執行保留期間而定。 每個成品記錄的大小為大約 30 kb \(KB\)。  
  
-   **部署所需的伺服器數目**。 您必須新增至少一個額外的伺服器\(來部署您的 AD FS 基礎結構所需的伺服器總數\)，做為 SQL Server 執行個體的專用主機。 如果您規劃使用容錯移轉叢集或鏡像來為 SQL Server 設定資料庫提供容錯和延展性，則至少需要兩部 SQL Srver。  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>您選取設定資料庫類型的方式可能會影響硬體資源  
與使用 SQL Server 資料庫在伺服陣列中部署的同盟伺服器相較之下，對使用 WID 在伺服陣列中部署之同盟伺服器上的硬體資源影響並不明顯。 但是，請務必考量當您針對伺服陣列使用 WID 時，該伺服陣列中的每部同盟伺服器都必須儲存、管理及維護其 AD FS 設定資料庫之本機複本的複寫變更，同時還要繼續提供 Federation Service 所需的一般操作。  
  
相較之下，使用 SQL Server 資料庫在伺服陣列中部署的同盟伺服器並不需要包含 AD FS 設定資料庫的本機執行個體。 因此，它們對硬體資源的要求會比較低。  
  
## <a name="BKMK_1"></a>同盟伺服器的位置  
基於安全性最佳做法，將 AD FS 同盟伺服器的防火牆前面，並連接至您公司的網路，以避免曝露在網際網路。 這很重要，因為同盟伺服器擁有完整權限可授與安全性權杖。 因此，它們應該像網域控制站一樣受到保護。 如果同盟伺服器遭到入侵，惡意使用者能夠為所有 Web 應用程式，並且受到 AD FS 的同盟伺服器發出完整存取權杖。  
  
> [!NOTE]  
> 基於安全性最佳做法，避免您直接存取的同盟伺服器在網際網路上。 為您的同盟伺服器直接存取網際網路，您要設定測試實驗室環境，或您的組織沒有周邊網路時，才應考慮。  
  
對於一般公司網路，內部\-對向防火牆公司網路與周邊網路和網際網路之間建立\-對向防火牆之間通常會建立周邊網路，網際網路。 在此情況下，同盟伺服器位於內部公司網路，並不是由網際網路用戶端直接存取。  
  
> [!NOTE]  
> 連線到公司網路的用戶端電腦可以直接與透過 Windows 整合式驗證的同盟伺服器通訊。  
  
同盟伺服器 proxy 與 AD FS 設定防火牆伺服器供使用之前應該置於周邊網路。  
  
## <a name="supported-deployment-topologies"></a>支援的部署拓撲  
下列主題說明您可以使用 AD FS 使用的各種部署拓撲。 其中也會說明與每個部署拓撲相關聯的優點與限制，讓您能夠針對特定的商業需求選取最適當的拓撲。  
  
-   [使用 WID 的同盟伺服器陣列](Federation-Server-Farm-Using-WID.md)  
  
-   [使用 WID 和 Proxy 的同盟伺服器陣列](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [使用 SQL Server 的同盟伺服器陣列](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>另請參閱  
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

