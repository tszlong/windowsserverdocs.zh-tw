---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: 權杖簽署憑證
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9db69cfb2eb42af90b392433a6e05eaab9978160
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190813"
---
# <a name="token-signing-certificates"></a>權杖簽署憑證

同盟伺服器需要的語彙基元\-簽署憑證讓攻擊者無法改變或盜用安全性權杖，以嘗試取得未經授權的存取同盟資源。 私用\/配對的公開金鑰來使用語彙基元\-簽章憑證是最重要的驗證任何的機制同盟的合作關係，因為這些金鑰可讓您驗證安全性權杖由有效的夥伴發出同盟伺服器，並在傳輸期間未修改的語彙基元。  
  
## <a name="token-signing-certificate-requirements"></a>語彙基元\-簽署的憑證需求  
語彙基元\-簽署憑證必須符合下列需求，才能與 AD FS 搭配運作：  
  
-   語彙基元\-簽署憑證來成功登入安全性權杖，權杖\-簽署憑證必須包含私密金鑰。  
  
-   AD FS 服務帳戶必須具有存取此語彙基元\-簽署憑證的私密金鑰，本機電腦的個人存放區中。 這是由負責安裝程式。 您也可以使用 AD FS 管理嵌入式管理單元\-才可確保此存取權，如果您接著變更權杖\-簽署憑證。  
  
> [!NOTE]  
> 它是公開金鑰基礎結構\(PKI\)針對多種目的私密金鑰不共用最佳做法。 因此，不會使用您在做為權杖的同盟伺服器安裝的服務通訊憑證\-簽署憑證。  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>如何語彙基元\-跨合作夥伴使用的簽署憑證  
每個語彙基元\-簽署憑證包含私密金鑰的密碼編譯金鑰和用來數位簽章的公開金鑰\(透過私密金鑰\)安全性權杖。 稍後，夥伴同盟伺服器收到後，這些金鑰會驗證真實性\(透過公開金鑰\)加密的安全性權杖。  
  
因為每個安全性權杖數位簽章由帳戶夥伴中，資源夥伴可以確認事實上帳戶夥伴所簽發的安全性權杖，並且它未經修改。 數位簽章必須經過協力廠商的語彙基元的公開金鑰部分\-簽署憑證。 驗證簽章後，在資源同盟伺服器會產生自己的安全性權杖為其組織則是針對與它自己的語彙基元的安全性權杖\-簽署憑證。  
  
同盟夥伴的環境下，當權杖\-由 CA 簽署的憑證發出時，請確定：  
  
1.  憑證撤銷清單\(Crl\)的憑證都可存取信賴憑證者的合作對象和信任的同盟伺服器的 Web 伺服器。  
  
2.  信任的根 CA 憑證的信賴憑證者的合作對象 」 和 「 信任的同盟伺服器的 Web 伺服器。  
  
資源夥伴中的 Web 伺服器使用的公開金鑰語彙基元\-簽署的憑證，以驗證已簽署資源同盟伺服器的安全性權杖。 然後，Web 伺服器可讓適當的存取權給用戶端。  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>語彙基元的部署考量\-簽署憑證  
當您部署新的 AD FS 安裝的第一個同盟伺服器時，您必須取得 token\-簽署憑證，並將它安裝在該同盟伺服器上的本機電腦個人憑證存放區。 您可以取得 token\-CA 或公用 CA，被要求從企業簽署憑證或建立自我\-簽署憑證。  
  
當您部署 AD FS 伺服器陣列時，k\-以不同的方式，取決於您要如何建立伺服器陣列安裝簽署憑證。  
  
有兩個伺服器陣列選項，您可以考慮當您取得權杖\-簽署憑證，讓您的部署：  
  
-   從一個語彙基元的私用金鑰\-簽署憑證在伺服陣列中的所有同盟伺服器之間共用。  
  
    在同盟伺服器陣列環境中，我們建議所有同盟伺服器都共用\(或重複使用\)相同的語彙基元\-簽署憑證。 您可以安裝單一語彙基元\-簽署從同盟伺服器上的 CA 憑證和只要發行的憑證會標示為可匯出，然後匯出私密金鑰。  
  
    如所示在下圖中，從單一的私用金鑰語彙基元\-可以共用到伺服陣列中的所有同盟伺服器的簽章憑證。 此選項 — 相較於下列 「 唯一的語彙基元\-簽署憑證 」 選項 — 降低成本，如果您計劃取得的權杖\-簽署從公用 CA 的憑證。  
  
![權杖簽署](media/adfs2_fedserver_certstory_3.gif)  
  
-   沒有唯一的語彙基元\-簽署的伺服器陣列中每部同盟伺服器的憑證。  
  
    當您使用多個時，唯一的憑證，在您的伺服器陣列，該伺服陣列中的每一部伺服器會使用它自己唯一的私密金鑰簽署權杖。  
  
    下圖所示，您可以取得個別的語彙基元\-簽署每一部伺服器陣列中的單一同盟伺服器的憑證。 此選項會比較昂貴，如果您想取得您的權杖\-簽署從公用 CA 的憑證。  
  
![權杖簽署](media/adfs2_fedserver_certstory_4.gif)  
  
當您使用 Microsoft Certificate Services 做為企業 CA 安裝憑證的相關資訊，請參閱[IIS 7.0:在 IIS 7.0 中建立網域的伺服器憑證](https://go.microsoft.com/fwlink/?LinkId=108548)。  
  
如需有關從公用 CA 安裝憑證的詳細資訊，請參閱[要求網際網路伺服器憑證 (IIS 7)](https://go.microsoft.com/fwlink/?LinkId=108549)。  
  
如需安裝自我\-簽署憑證，請參閱[IIS 7.0:建立自我\-簽署伺服器憑證，在 IIS 7.0 中的](https://go.microsoft.com/fwlink/?LinkID=108271)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
