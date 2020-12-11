---
description: 深入瞭解：服務通訊憑證
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服務通訊憑證
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a6e17318e02cacf407148f1bf2a24c6622e465a8
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049366"
---
# <a name="service-communications-certificates"></a>服務通訊憑證

同盟伺服器需要在使用 WCF 訊息安全性的情況下使用服務通訊憑證。

## <a name="service-communication-certificate-requirements"></a>服務通訊憑證需求
服務通訊憑證必須符合下列需求，才能使用 AD FS：

-   服務通訊憑證必須包含伺服器驗證的增強金鑰使用方法 \( EKU \) 延伸模組。

-   憑證撤銷清單中 \( 的 \) 所有憑證，都必須能夠從服務通訊憑證存取根 CA 憑證。 根 CA 也必須受信任此同盟伺服器的任何同盟伺服器 proxy 和 Web 服務器所信任。

-   服務通訊憑證中使用的主體名稱必須符合同盟服務屬性中的同盟服務名稱。

## <a name="deployment-considerations-for-service-communication-certificates"></a>服務通訊憑證的部署考慮
設定服務通訊憑證，讓所有同盟伺服器都使用相同的憑證。 如果您要部署同盟 Web 單一 \- 登錄 \- \( SSO \) 設計，我們建議您的服務通訊憑證由公用 CA 發出。 您可以透過 [IIS 管理員] 嵌入式管理單元來要求和安裝這些憑證 \- 。

您可以在 \- 測試實驗室環境中的同盟伺服器上成功使用自我簽署的服務通訊憑證。 不過，在生產環境中，我們建議您從公用 CA 取得服務通訊憑證。 以下是您不應該使用自我 \- 簽署的服務通訊憑證來進行即時部署的原因：

-   自我 \- 簽署的 SSL 憑證必須新增至資源夥伴組織中每部同盟伺服器上的受根信任存放區。 雖然這不會讓攻擊者入侵資源同盟伺服器，但是信任自我簽署的 \- 憑證確實會增加電腦的受攻擊面，如果憑證簽署者不值得信任，可能會導致安全性弱點。

-   它會產生不良的使用者體驗。 當用戶端嘗試存取顯示下列訊息的同盟資源時，用戶端會收到安全性警示提示：「此安全性憑證由您未選擇信任的公司發行。」 這是預期的行為，因為自我 \- 簽署的憑證不受信任。

    > [!NOTE]
    > 如有必要，您可以使用群組原則手動 \- 將自我簽署憑證推送至將嘗試存取 AD FS 網站之每部用戶端電腦上的受根信任存放區，以解決這種情況。

-   CAs 提供其他 \- 以憑證為基礎的功能，例如私密金鑰封存、更新和撤銷，且不是由自我 \- 簽署憑證所提供。


