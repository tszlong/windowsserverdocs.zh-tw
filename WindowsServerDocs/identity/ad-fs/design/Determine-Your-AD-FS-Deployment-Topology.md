---
description: 深入瞭解：判斷您的 AD FS 部署拓撲
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: 決定您的 AD FS 部署拓撲
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1c5bd925318a6f18513fa7267c2dfe2ab5381281
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047696"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>決定您的 AD FS 部署拓撲

規劃 Active Directory 同盟服務 AD FS 部署的第一步， \( \) 就是決定正確的部署拓撲，以符合組織的單一 \- 登錄 \( SSO \) 需求。 本節中的主題描述您可以搭配 AD FS 使用的各種部署拓撲。 其中也會說明與每個部署拓撲相關聯的優點與限制，讓您能夠針對特定的商業需求選取最適當的拓撲。

在您閱讀這個部署拓撲主題之前，建議您先依照下表所示的順序完成工作。

|建議的工作|描述|參考|
|--------------------|---------------|-------------|
|複習 AD FS 資料如何儲存及複寫到同盟伺服器陣列中的其他同盟伺服器。|了解可用於 AD FS 設定資料庫中所儲存之基礎資料的複寫用途與複寫方法。 本主題介紹設定資料庫的概念，並說明這兩種資料庫類型： Windows 內部資料庫 \( WID \) 和 Microsoft SQL Server。|[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|
|選取您將在組織中部署的 AD FS 設定資料庫類型。|檢閱使用 WID 或 SQL 伺服器做為 AD FS 設定資料庫的各種相關優點與限制，以及它們支援的各種應用程式案例。|[AD FS 部署拓撲考量](AD-FS-Deployment-Topology-Considerations.md)|

> [!NOTE]
> 若要執行基本的冗余、負載平衡，以及視需要調整同盟服務的選項 \( \) ，建議您在所有生產環境中，為每個同盟伺服器陣列部署至少兩部同盟伺服器，不論您將使用的資料庫類型為何。

當您檢閱上述表格中的內容之後，請繼續閱讀本節中的下列主題：

-   [使用 WID 的獨立同盟伺服器](Stand-Alone-Federation-Server-Using-WID.md)

-   [使用 WID 的同盟伺服器陣列](Federation-Server-Farm-Using-WID-2012.md)

-   [使用 WID 和 Proxy 的同盟伺服器陣列](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)

-   [使用 SQL Server 的同盟伺服器陣列](Federation-Server-Farm-Using-SQL-Server-2012.md)

當您完成選取 AD FS 部署拓撲之後，建議您參閱 [規劃 AD FS 伺服器容量](Planning-for-AD-FS-Server-Capacity.md) 的主題，以判斷您需要部署以支援此拓撲的建議伺服器數目。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

