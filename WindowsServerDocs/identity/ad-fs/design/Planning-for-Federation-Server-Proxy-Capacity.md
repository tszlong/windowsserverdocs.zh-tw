---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: 規劃同盟伺服器 Proxy 容量
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c3efbb4081336ebfdfe9d3ab8a2b91412aa82dee
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191085"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>規劃同盟伺服器 Proxy 容量

容量規劃同盟伺服器 proxy 可協助您評估：  
  
-   每個同盟伺服器 proxy 適當的硬體需求。  
  
-   同盟伺服器和同盟伺服器 proxy 放置在每個組織中的數字。  
  
同盟伺服器 proxy 重新導向至同盟使用者從公司網路中受保護的同盟伺服器的安全性權杖。 部署同盟伺服器 proxy 的目的是為了允許外部使用者連接到同盟伺服器。 它不會不會實際簽署權杖或 AD FS 組態資料庫中的資料寫入。 因此，同盟伺服器 proxy 的硬體需求是通常低於同盟伺服器的硬體需求。  
  
因為每個同盟伺服器 proxy 的要求會導致同盟伺服器或同盟伺服器陣列的要求，必須以平行方式執行的同盟伺服器和同盟伺服器 proxy 容量規劃。  
  
估計尖峰登\-集每秒的同盟伺服器 proxy 需要了解將登入，透過同盟伺服器 proxy 的同盟使用者的使用模式。 在許多部署中，使用同盟伺服器 proxy 登入的同盟的使用者位於網際網路上。 您可以估計尖峰登\-集查看的使用模式，其中每秒的同盟使用者，將受到 AD FS 的現有 Web 應用程式。  
  
> [!NOTE]  
> 生產環境部署中，我們建議針對每個同盟伺服器陣列部署的執行個體的兩個同盟伺服器 proxy 的最小值。  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估計同盟伺服器 proxy 所需的組織的數目  
您可以估計數目之前所需 AD FS 同盟伺服器 proxy 電腦，您必須先判斷您組織中，您將部署的同盟伺服器的總數。 如需如何執行這項操作的詳細資訊，請參閱[同盟伺服器容量規劃](Planning-for-Federation-Server-Capacity.md)。  
  
一旦您決定的同盟伺服器數目乘以下列數量之伺服器的連入的同盟驗證的百分比會要求您預期會從外部使用者進行\(位於公司網路外部\). 這項計算的值會提供您將會處理連入驗證要求的外部使用者的同盟伺服器 proxy 的估計數目。  
  
比方說，如果建議的同盟伺服器數目是 3，而您預期會從外部使用者進行的驗證要求的總數會大約 60%的同盟的驗證要求的總數您計算就會等於 1.8 \(3 X.60\)其中您可以藉由四捨五入最多 2 個。  因此，在此情況下，您必須部署兩個同盟伺服器 proxy 電腦，以容納之三個同盟伺服器的外部使用者驗證要求的負載。  
  
在 AD FS 產品小組所執行的測試，每個同盟伺服器 proxy 上的整體 CPU 使用率找不到要大幅低於觀察到相同的伺服陣列的同盟伺服器的 CPU 使用率。  一項測試，而一部同盟伺服器 CPU 所指出，它已完全飽和，提供該相同的伺服陣列的 proxy 服務的同盟伺服器 proxy 的 CPU 已觀察到在只有 20%的使用率。 因此，我們的測試會顯示同盟伺服器 proxy，它會使用類似的硬體規格所述稍早在本節中，CPU 的負載可以合理地處理大約三個同盟伺服器的處理負載。  
  
不過，基於容錯功能，我們建議您部署的每個同盟伺服器陣列的兩個同盟伺服器 proxy 的最小值。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
