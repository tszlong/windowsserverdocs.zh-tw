---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服務通訊憑證
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 91ef716f518676fe9cf42b2136afcbcd42c50946
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858521"
---
# <a name="service-communications-certificates"></a>服務通訊憑證

同盟伺服器需要在使用 WCF 訊息安全性的案例中使用服務通訊憑證。  
  
## <a name="service-communication-certificate-requirements"></a>服務通訊憑證需求  
服務通訊憑證必須符合下列需求，才能與 AD FS 搭配使用：  
  
-   服務通訊憑證必須包含伺服器驗證的增強金鑰使用方法 \(EKU\) 延伸模組。  
  
-   憑證撤銷清單 \(Crl\) 必須可從服務通訊憑證到根 CA 憑證的鏈中的所有憑證存取。 根 CA 也必須受到信任此同盟伺服器的任何同盟伺服器 proxy 和 Web 服務器所信任。  
  
-   服務通訊憑證中所用的主體名稱必須符合同盟服務屬性中的同盟服務名稱。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>服務通訊憑證的部署考慮  
設定服務通訊憑證，讓所有同盟伺服器都使用相同的憑證。 如果您要在 \(SSO\) 設計上部署同盟 Web 單一\-登入\-，建議您將服務通訊憑證由公用 CA 發行。 您可以透過中的 [IIS 管理員] 嵌入式\-管理單元來要求並安裝這些憑證。  
  
您可以在測試實驗室環境中的同盟伺服器上，順利使用自我\-簽署的服務通訊憑證。 不過，在生產環境中，我們建議您從公用 CA 取得服務通訊憑證。 以下是您不應該使用自我\-簽署的服務通訊憑證來進行即時部署的原因：  
  
-   您必須在資源夥伴組織中的每部同盟伺服器上，將自我\-簽署的 SSL 憑證新增至受信任的根存放區。 雖然這本身並不會讓攻擊者危害資源同盟伺服器，但信任自我\-簽署的憑證會增加電腦的攻擊面，如果憑證簽署者不可靠，可能會導致安全性弱點。  
  
-   這會造成不正確的使用者體驗。 當用戶端嘗試存取顯示下列訊息的同盟資源時，會收到安全性警示提示：「安全性憑證是由您未選擇信任的公司所發行。」 這是預期的行為，因為自我\-簽署的憑證不受信任。  
  
    > [!NOTE]  
    > 如有需要，您可以使用群組原則將自我\-簽署的憑證手動推送至每部將嘗試存取 AD FS 網站的用戶端電腦上的受根信任存放區，以解決這種情況。  
  
-   CAs 提供了額外的憑證\-功能，例如私密金鑰封存、更新和撤銷，不是由自我\-簽署的憑證所提供。  
  

