---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: "判斷您 AD FS 部署拓撲"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="determine-your-ad-fs-deployment-topology"></a>判斷您 AD FS 部署拓撲

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

規劃部署 \(AD FS\) Active Directory 同盟服務的第一個步驟是組織的以判斷正確部署拓撲符合單一 sign\ 上 \(SSO\) 需要您。 在本區段中的主題描述，您可以使用 AD FS 使用的各種部署拓撲。 它們也描述的優點和，因此您可以選取最適合拓撲針對特定企業需求的相關每個部署拓撲限制。  
  
您在朗讀本部署拓撲主題之前，我們建議您先完成的工作順序，如下表所示。  
  
|建議使用的工作|描述|參考資料|  
|--------------------|---------------|-------------|  
|檢查 AD FS 資料如何儲存和複製到其他聯盟伺服器聯盟伺服器。|了解的目的和複寫方法可用的基礎 AD FS 設定資料庫中所儲存資料。 本主題介紹設定資料庫的概念，並描述資料庫兩種類型：Windows 內部資料庫 \(WID\) 與 Microsoft SQL Server。|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|選取 AD FS，您將會在組織中部署設定資料庫的類型。|檢視不同的優點和使用 AD FS 設定資料庫，以及各種他們支援的應用程式案例 WID 或 SQL Server 相關聯的限制。|[AD FS 部署拓撲注意事項](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> 實作基本冗餘、負載平衡和縮放同盟服務 \(if required\) 的選項，我們建議您部署至少兩部聯盟伺服器每聯盟伺服器陣列所有 production 環境，無論您將會使用資料庫類型。  
  
當您已經檢視中的上一個表格 content 時，進行下列主題在本區段中：  
  
-   [使用 WID 獨立聯盟伺服器](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [使用 WID 聯盟伺服器陣列](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [使用 WID 與 Proxy 聯盟伺服器陣列](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [使用 SQL Server 聯盟伺服器陣列](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
選取您的 AD FS 部署拓撲完成之後，我們建議您檢視主題[規劃 AD FS 伺服器容量](Planning-for-AD-FS-Server-Capacity.md)來查看建議的伺服器，您將需要支援這種拓撲部署。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

