---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: 服務通訊憑證
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825649"
---
# <a name="service-communications-certificates"></a>服務通訊憑證

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

同盟伺服器需要服務通訊憑證的使用案例中使用 WCF 訊息安全性。  
  
## <a name="service-communication-certificate-requirements"></a>服務通訊憑證需求  
服務通訊憑證必須符合下列需求，才能與 AD FS 搭配運作：  
  
-   服務通訊憑證必須包含伺服器驗證增強金鑰使用方式\(EKU\)延伸模組。  
  
-   憑證撤銷清單\(Crl\)必須能夠在鏈結中的所有憑證從服務通訊憑證的根 CA 憑證。 也必須受到任何同盟伺服器 proxy 和信任此同盟伺服器的 Web 伺服器的信任根 CA。  
  
-   用於服務通訊憑證的主體名稱必須符合 Federation Service 屬性中的 Federation Service 名稱。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>服務通訊憑證的部署考量  
設定服務通訊憑證，讓所有的同盟伺服器會使用相同的憑證。 如果您要部署同盟網頁單一\-號\-上\(SSO\)設計中，我們建議您的公用 CA 所簽發的您的服務通訊憑證。 您可以要求並安裝這些憑證，透過 IIS 管理員 嵌入式管理單元\-中。  
  
您可以使用自我\-簽署，服務通訊憑證，順利在測試實驗室環境中的同盟伺服器上。 不過，針對生產環境中，我們建議您從公用 CA 取得服務通訊憑證。 以下是的原因為何您不應該使用自我\-簽署，服務通訊憑證，進行即時部署：  
  
-   自我\-簽署，SSL 憑證必須新增至每個資源夥伴組織中的同盟伺服器上的受信任的根存放區。 雖然這單獨不會啟用攻擊者危害資源同盟伺服器，信任自我\-簽署的憑證會增加電腦的攻擊面，以及它可能會導致安全性弱點如果憑證簽署人不值得信任。  
  
-   它會建立不正確的使用者體驗。 用戶端將收到安全性警示的提示，當使用者試著存取同盟的資源會顯示下列訊息："安全性憑證是由您尚未選擇要信任公司核發。 」 這是預期的行為，因為自我\-簽署的憑證不受信任。  
  
    > [!NOTE]  
    > 如果有必要，您可以解決這種情況使用群組原則，以手動方式將資料放入自我\-簽署的憑證來存取 AD FS 網站會嘗試每個用戶端電腦上的受信任的根存放區。  
  
-   CAs 提供額外的憑證\-架構功能，例如私密金鑰保存、 更新，以及撤銷，不提供自我\-簽署憑證。  
  

