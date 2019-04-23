---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: 決定您的 AD FS 部署拓撲
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834519"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>決定您的 AD FS 部署拓撲

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

規劃部署的 Active Directory Federation Services 的第一個步驟\(AD FS\)是要判斷正確的部署拓撲以符合的單一登\-上\(SSO\)需要的程式組織。 在本節中的主題描述您可以使用 AD FS 使用的各種部署拓撲。 其中也會說明與每個部署拓撲相關聯的優點與限制，讓您能夠針對特定的商業需求選取最適當的拓撲。  
  
在您閱讀這個部署拓撲主題之前，建議您先依照下表所示的順序完成工作。  
  
|建議的工作|描述|參考資料|  
|--------------------|---------------|-------------|  
|檢閱 AD FS 資料如何儲存和複寫到同盟伺服器陣列中的其他同盟伺服器。|了解可用於 AD FS 設定資料庫中所儲存之基礎資料的複寫用途與複寫方法。 本主題介紹設定資料庫的概念，並說明這兩個資料庫類型：Windows 內部資料庫\(WID\)和 Microsoft SQL Server。|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|選取您將在組織中部署的 AD FS 設定資料庫類型。|檢閱使用 WID 或 SQL 伺服器做為 AD FS 設定資料庫的各種相關優點與限制，以及它們支援的各種應用程式案例。|[AD FS 部署拓撲考量](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> 若要實作基本備援、 負載平衡，以及調整 Federation Service 的選項\(如有必要\)，我們建議您部署的每個同盟伺服器陣列所有生產環境中的至少兩部同盟伺服器不論您將使用的資料庫型別。  
  
當您檢閱上述表格中的內容之後，請繼續閱讀本節中的下列主題：  
  
-   [使用 WID 的獨立同盟伺服器](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [使用 WID 的同盟伺服器陣列](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [使用 WID 和 Proxy 的同盟伺服器陣列](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [使用 SQL Server 的同盟伺服器陣列](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
您完成選取您的 AD FS 部署拓撲之後，我們建議您檢閱本主題[AD FS 伺服器容量規劃](Planning-for-AD-FS-Server-Capacity.md)來判斷您要部署到支援此拓撲的伺服器建議的數目。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

