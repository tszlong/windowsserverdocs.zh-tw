---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服務通訊憑證
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 624d2e26bc0277129e44eee3fdce7c7396b735a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407901"
---
# <a name="service-communications-certificates"></a>服務通訊憑證

同盟伺服器需要在使用 WCF 訊息安全性的案例中使用服務通訊憑證。  
  
## <a name="service-communication-certificate-requirements"></a>服務通訊憑證需求  
服務通訊憑證必須符合下列需求，才能與 AD FS 搭配使用：  
  
-   服務通訊憑證必須包含伺服器驗證的增強金鑰使用方法 \(EKU @ no__t-1 延伸模組。  
  
-   從服務通訊憑證到根 CA 憑證的鏈中的所有憑證，都必須能夠存取憑證撤銷清單 \(CRLs @ no__t-1。 根 CA 也必須受到信任此同盟伺服器的任何同盟伺服器 proxy 和 Web 服務器所信任。  
  
-   服務通訊憑證中所用的主體名稱必須符合同盟服務屬性中的同盟服務名稱。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>服務通訊憑證的部署考慮  
設定服務通訊憑證，讓所有同盟伺服器都使用相同的憑證。 如果您要部署同盟 Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 設計，我們建議您的服務通訊憑證是由公用 CA 所發行。 您可以透過 IIS 管理員 snap @ no__t-0in 來要求並安裝這些憑證。  
  
您可以在測試實驗室環境中的同盟伺服器上，順利使用自我 @ no__t-0signed、服務通訊憑證。 不過，在生產環境中，我們建議您從公用 CA 取得服務通訊憑證。 以下是您不應該使用自我 @ no__t-0signed、適用于即時部署的服務通訊憑證的原因：  
  
-   您必須在資源夥伴組織中的每部同盟伺服器上，將自我 @ no__t-0signed、SSL 憑證新增至受信任的根存放區。 雖然這本身並不會讓攻擊者危害資源同盟伺服器，但信任的自我 @ no__t 0signed 憑證會增加電腦的攻擊面，如果憑證簽署者不是，則可能會導致安全性弱點trustworthy.  
  
-   這會造成不正確的使用者體驗。 當用戶端嘗試存取顯示下列訊息的同盟資源時，將會收到安全性警示提示：「安全性憑證是由您未選擇信任的公司所發行。」 這是預期的行為，因為 self @ no__t-0signed 憑證不受信任。  
  
    > [!NOTE]  
    > 如有需要，您可以使用群組原則，手動將0signed 的自我 @ no__t 的憑證向下一部嘗試存取 AD FS 網站的用戶端電腦上的受根信任存放區，以解決這種情況。  
  
-   CAs 提供了自我 @ no__t 1signed 憑證未提供的額外憑證 @ no__t 0based 功能，例如私密金鑰封存、更新及撤銷。  
  

