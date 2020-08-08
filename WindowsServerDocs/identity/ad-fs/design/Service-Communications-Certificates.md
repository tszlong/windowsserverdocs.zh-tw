---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服務通訊憑證
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 377b6f9053a5805f6a13f6a865185da6965f9966
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972075"
---
# <a name="service-communications-certificates"></a>服務通訊憑證

同盟伺服器需要在使用 WCF 訊息安全性的案例中使用服務通訊憑證。

## <a name="service-communication-certificate-requirements"></a>服務通訊憑證需求
服務通訊憑證必須符合下列需求，才能與 AD FS 搭配使用：

-   服務通訊憑證必須包含伺服器驗證的增強金鑰使用 \( EKU \) 延伸模組。

-   憑證撤銷清單 \( crl \) 必須可供從服務通訊憑證到根 CA 憑證的鏈中的所有憑證存取。 根 CA 也必須受到信任此同盟伺服器的任何同盟伺服器 proxy 和 Web 服務器所信任。

-   服務通訊憑證中所用的主體名稱必須符合同盟服務屬性中的同盟服務名稱。

## <a name="deployment-considerations-for-service-communication-certificates"></a>服務通訊憑證的部署考慮
設定服務通訊憑證，讓所有同盟伺服器都使用相同的憑證。 如果您要部署同盟 Web 單一 \- 登錄 \- \( SSO \) 設計，建議您將服務通訊憑證由公用 CA 發行。 您可以透過 [IIS 管理員] 嵌入式管理單元來要求並安裝這些憑證 \- 。

您可以 \- 在測試實驗室環境中的同盟伺服器上，順利使用自我簽署的服務通訊憑證。 不過，在生產環境中，我們建議您從公用 CA 取得服務通訊憑證。 以下是您不應該將自我 \- 簽署的服務通訊憑證用於即時部署的原因：

-   自我 \- 簽署的 SSL 憑證必須新增至資源夥伴組織中每部同盟伺服器上的受根信任存放區。 雖然這種情況並不會讓攻擊者危害資源同盟伺服器，但信任的自我 \- 簽署憑證會增加電腦的受攻擊面，如果憑證簽署者不可靠，可能會導致安全性弱點。

-   這會造成不正確的使用者體驗。 當用戶端嘗試存取顯示下列訊息的同盟資源時，會收到安全性警示提示：「安全性憑證是由您未選擇信任的公司所發行。」 這是預期的行為，因為自我 \- 簽署憑證不受信任。

    > [!NOTE]
    > 如有需要，您可以使用群組原則，手動將自我簽署憑證向下推 \- 至每部將嘗試存取 AD FS 網站的用戶端電腦上的受根信任存放區，以解決這種情況。

-   Ca \- 會提供自我簽署憑證未提供的額外憑證功能，例如私密金鑰封存、更新及撤銷 \- 。


