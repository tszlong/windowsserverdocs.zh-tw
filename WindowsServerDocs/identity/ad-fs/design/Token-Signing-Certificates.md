---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: "權杖簽署的憑證"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 84e106ec87861960d7de651b4aaa0e88c952aa79
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="token-signing-certificates"></a>權杖簽署的憑證

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

聯盟伺服器需要 token\ 簽署的憑證，以避免攻擊者變更或仿冒的安全性權杖嘗試聯盟資源未經授權的存取。 搭配的公用 private\ 日鍵 token\ 簽署的憑證以用於是最重要的任何聯盟合作關係驗證機制，因為這些按鍵驗證的安全性權杖核發有效協力廠商聯盟伺服器，並在傳送時，未修改預付碼。  
  
## <a name="token-signing-certificate-requirements"></a>Token\ 簽署憑證需求  
Token\ 簽署的憑證必須符合下列需求，請使用 AD FS:  
  
-   成功登入的安全性權杖 token\ 簽署的憑證，token\ 簽署的憑證能私密金鑰。  
  
-   AD FS 服務 account 必須 token\ 簽署的憑證私密金鑰存取在本機電腦的個人的市集。 這是處理的安裝程式。 您也可以使用 AD FS 管理 snap\ 中，以確保如果後續變更 token\ 簽署的憑證的存取權。  
  
> [!NOTE]  
> 最好公用基礎結構 \(PKI\) 不分享的私密金鑰多個項目的。 因此，不使用您聯盟 token\ 簽署的憑證的伺服器安裝的服務通訊憑證。  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>如何用所有合作夥伴 token\ 簽署的憑證  
每個 token\ 簽署的憑證包含密碼編譯私人和公開鍵是用來簽署 \（藉由私人 key\) 的安全性權杖。 之後，它們會接收到透過協力廠商聯盟伺服器之後，這些按鍵驗證真確性 \（藉由公用 key\) 加密的安全性權杖。  
  
每個的安全性權杖數位簽章 account 合作夥伴，因為資源合作夥伴可以檢查的安全性權杖的處理機都會確實由 account 合作夥伴發出與它不修改。 數位簽章的合作夥伴的 token\-仙蹤憑證的公用部分來確認。 簽章驗證之後，資源聯盟伺服器產生自己的安全性權杖對其組織，並且它簽章自己 token\ 簽署的憑證的安全性權杖。  
  
聯盟合作夥伴環境中，當 ca 發行後 token\ 簽署的憑證，確保：  
  
1.  憑證撤銷清單 \(CRLs\) 的憑證的存取信賴派對和信任聯盟伺服器的網頁伺服器。  
  
2.  CA 根憑證會受信任的依賴派對和信任聯盟伺服器的網頁伺服器。  
  
資源合作夥伴的網頁伺服器使用的 token\ 簽署的憑證來驗證您的安全性權杖已資源聯盟伺服器。 然後，網頁伺服器可讓 client 適當存取。  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>部署考量 token\ 簽署的憑證  
當您部署新 AD FS 安裝的第一個聯盟伺服器時，您必須取得 token\ 簽署的憑證，並安裝該聯盟伺服器上的本機電腦個人化的憑證存放區。 您可以取得 token\ 簽署 CA 或公用 CA 憑證被要求的企業版或來建立 self\ 簽署的憑證。  
  
部署時 AD FS 發電廠，會有不同，根據您要如何建立伺服器陣列安裝 token\ 簽署的憑證。  
  
有兩個 token\ 簽署的憑證取得您的部署時，您可以考慮伺服器發電廠選項：  
  
-   所有聯盟伺服器在之間共用私密金鑰從一個 token\ 簽署的憑證。  
  
    在聯盟伺服器發電廠環境中，我們建議您所有的聯盟伺服器分享 \(or reuse\) 相同 token\ 簽署的憑證。 您可以從 CA 聯盟的伺服器上安裝的單一 token\ 簽署的憑證，然後匯出私密金鑰，只要標示為匯出發行的憑證。  
  
    如下所示，即可共用私密金鑰從單一 token\ 簽署的憑證，以發電廠中的所有聯盟伺服器。 此選項，相較於下列」唯一 token\ 簽署的憑證」選項，如果您想要從公開 CA 取得 token\ 簽署的憑證降低成本。  
  
![權杖登入](media/adfs2_fedserver_certstory_3.gif)  
  
-   還有陣列中每個聯盟伺服器的唯一 token\ 簽署憑證。  
  
    當您使用多個時，唯一整個您發電廠，在農地的每個伺服器的憑證會有自己的唯一私密金鑰簽署權杖。  
  
    如下所示，您可以取得陣列中每個單一聯盟伺服器的獨立 token\ 簽署憑證。 這個選項會在價格如果您想要從公開 CA 取得您 token\ 簽署的憑證。  
  
![權杖登入](media/adfs2_fedserver_certstory_4.gif)  
  
安裝憑證，當您使用 Microsoft 憑證服務為您的企業 CA 相關資訊，請查看[IIS 7.0：建立網域伺服器憑證 IIS 7.0 在](https://go.microsoft.com/fwlink/?LinkId=108548)。  
  
安裝憑證從公用 CA 相關的資訊，請查看[IIS 7.0：要求網際網路伺服器的憑證](https://go.microsoft.com/fwlink/?LinkId=108549)。  
  
安裝 self\ 簽署的憑證相關資訊，請查看[IIS 7.0：建立 Self\-Signed 伺服器憑證 IIS 7.0 在](https://go.microsoft.com/fwlink/?LinkID=108271)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
