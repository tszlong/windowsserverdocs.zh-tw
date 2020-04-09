---
ms.assetid: 3ecb6e87-17f1-4d38-97d2-9c4d52b7cf39
title: 規劃同盟伺服器 Proxy 容量
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ec5b6e39e57539ef3de1c7bd5fed9cc8ed2e64be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858761"
---
# <a name="planning-for-federation-server-proxy-capacity"></a>規劃同盟伺服器 Proxy 容量

同盟伺服器 proxy 的容量規劃可協助您評估：  
  
-   每個同盟伺服器 proxy 的適當硬體需求。  
  
-   要在每個組織中放置的同盟伺服器和同盟伺服器 proxy 的數目。  
  
同盟伺服器 proxy 會將來自公司網路中受保護同盟伺服器的安全性權杖重新導向至同盟使用者。 部署同盟伺服器 proxy 的目的是要允許外部使用者連接到同盟伺服器。 它實際上不會簽署權杖或寫入 AD FS 設定資料庫中的資料。 因此，同盟伺服器 proxy 的硬體需求通常低於同盟伺服器的硬體需求。  
  
由於每個對同盟伺服器 proxy 的要求都會導致對同盟伺服器或同盟伺服器陣列的要求，因此必須以平行方式執行同盟伺服器和同盟伺服器 proxy 的容量規劃。  
  
針對同盟伺服器 proxy 估計每秒的尖峰正負號\-，需要瞭解將透過同盟伺服器 proxy 登入的聯盟使用者使用模式。 在許多部署中，使用同盟伺服器 proxy 登入的聯盟使用者位於網際網路上。 您可以在將受 AD FS 保護的現有 Web 應用程式上查看這些同盟使用者的使用模式，藉此預估每秒的尖峰登入\-。  
  
> [!NOTE]  
> 針對生產環境部署，建議您部署的每個同盟伺服器陣列實例至少要有兩個同盟伺服器 proxy。  
  
## <a name="estimate-the-number-of-federation-server-proxies-required-for-your-organization"></a>估計貴組織所需的同盟伺服器 proxy 數目  
您必須先決定您將在組織中部署的同盟伺服器總數，才能估計所需的 AD FS 同盟伺服器 proxy 電腦數目。 如需如何執行此動作的詳細資訊，請參閱[規劃同盟伺服器容量](Planning-for-Federation-Server-Capacity.md)。  
  
決定同盟伺服器的數目之後，請將此數目的伺服器乘以您預期的傳入同盟驗證要求百分比，而不是來自公司網路\)外部使用者 \(。 此計算的值將會提供您為外部使用者處理傳入驗證要求所估計的同盟伺服器 proxy 數目。  
  
例如，如果建議的同盟伺服器數目為3，而且您預期將從外部使用者提出的驗證要求總數大約是同盟驗證要求總數的60%，則您的計算會等於 1.8 \(3 X。 60\) 可以進位到2。  因此，在此情況下，您必須部署兩部同盟伺服器 proxy 電腦，以容納三台同盟伺服器的外部使用者驗證要求負載。  
  
在 AD FS 產品小組所執行的測試中，發現每個同盟伺服器 proxy 的整體 CPU 使用率明顯低於相同伺服器陣列的同盟伺服器上觀察到的 CPU 使用率。  在一個測試中，雖然有一部同盟伺服器 CPU 表示它是完全飽和的，但針對該相同伺服器陣列提供 proxy 服務的同盟伺服器 proxy 的 CPU，只會在20% 的使用率下觀察到。 因此，我們的測試發現，同盟伺服器 proxy 的 CPU 負載（使用本節稍早所述的類似硬體規格）可以合理地處理大約三部同盟伺服器的處理負載。  
  
不過，基於容錯目的，我們建議您部署的每個同盟伺服器陣列最少要有兩個同盟伺服器 proxy。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
