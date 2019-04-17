---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: "服務通訊的憑證"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="service-communications-certificates"></a>服務通訊的憑證

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

聯盟伺服器需要服務通訊憑證使用案例是 WCF 郵件安全性。  
  
## <a name="service-communication-certificate-requirements"></a>服務通訊認證需求  
服務通訊憑證必須符合下列需求，請使用 AD FS:  
  
-   服務通訊憑證必須包含伺服器的驗證增強金鑰使用 \(EKU\) 擴充功能。  
  
-   必須存取所有的憑證鏈結中的服務通訊憑證根憑證撤銷清單 \(CRLs\)。 根 CA 必須也會受信任的任何聯盟伺服器 proxy 和信任此聯盟伺服器的網頁伺服器。  
  
-   主體名稱所使用的服務通訊憑證必須符合同盟服務中的名稱同盟服務的屬性。  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>服務通訊憑證部署注意事項  
設定服務通訊的憑證，讓所有聯盟伺服器都使用相同憑證。 如果您要部署的聯盟網路 Single\-Sign\-On \(SSO\) 設計，我們建議您服務通訊的憑證，CA 公開發行。 您可以要求，並安裝這些透過 IIS 管理員 snap\ 中的憑證。  
  
您可以使用 self\ 簽署、 在實驗室測試環境聯盟伺服器成功服務通訊的憑證。 不過，production 環境中，我們建議您從公開 CA 取得服務通訊的憑證。 您應該會為何不使用 self\ 簽署服務通訊的憑證動態部署的原因如下：  
  
-   A self\ 簽章 SSL 憑證必須新增至受信任的根在市集上每個聯盟伺服器資源合作夥伴組織中。 時，這單獨不可危害資源聯盟伺服器，信任 self\ 簽署的憑證會增加電腦的攻擊，它會導致安全性弱點如果憑證簽署不可靠。  
  
-   它會建立不正確的使用者體驗。 戶端將會收到安全性警示提示嘗試存取聯盟的資源顯示以下訊息: 「 安全性憑證有不受信任的公司。 」 這會是預期的行為，是因為不受信任的 self\ 簽署的憑證。  
  
    > [!NOTE]  
    > 必要時，您可以使用群組原則來手動向下推入 self\ 簽署的憑證存放區受信任的根每個 client 電腦嘗試存取 AD FS 網站上運作周圍此條件。  
  
-   Ca 提供其他 certificate\ 為基礎的功能，例如私密金鑰保存、 續約，以及撤銷，不提供的 self\ 簽署的憑證。  
  

