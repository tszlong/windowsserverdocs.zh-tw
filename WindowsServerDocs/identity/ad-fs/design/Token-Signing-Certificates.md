---
description: 深入瞭解： Token-Signing 憑證
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: 權杖簽署憑證
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: aed5fac9cf53ec41d79828ef94eb194b6dfb1d84
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716944"
---
# <a name="token-signing-certificates"></a>權杖簽署憑證

同盟伺服器需要權杖 \- 簽署憑證，以防止攻擊者改變或偽造安全性權杖，以嘗試取得同盟資源的未經授權存取權。 \/與權杖簽署憑證搭配使用的私用公開金鑰配對 \- 是任何同盟合作關係的最重要驗證機制，因為這些金鑰會驗證安全性權杖是否由有效的夥伴同盟伺服器發出，以及在傳輸期間未修改權杖。

## <a name="token-signing-certificate-requirements"></a>權杖 \- 簽署憑證需求
權杖 \- 簽署憑證必須符合下列需求，才能使用 AD FS：

-   \-若要讓權杖簽署憑證成功簽署安全性權杖，權杖 \- 簽署憑證必須包含私密金鑰。

-   AD FS 服務帳戶必須具有 \- 本機電腦個人存放區中權杖簽署憑證私密金鑰的存取權。 這會由安裝程式負責。 如果您之後變更權杖簽署憑證，您也可以使用 AD FS 管理嵌入式管理單元 \- 來確保此存取權 \- 。

> [!NOTE]
> 這是一個公開金鑰基礎結構 \( PKI \) 最佳做法，不會基於多個用途共用私密金鑰。 因此，請勿使用您在同盟伺服器上安裝的服務通訊憑證作為權杖 \- 簽署憑證。

## <a name="how-token-signing-certificates-are-used-across-partners"></a>如何 \- 跨夥伴使用權杖簽署憑證
每個權杖 \- 簽署憑證都包含密碼編譯私密金鑰和公開金鑰，這些金鑰是用來透過 \( 私密金鑰以安全性權杖來以數位方式簽署 \) 。 之後，合作夥伴同盟伺服器收到這些金鑰之後，這些金鑰會透過 \( 加密安全性權杖的公開金鑰來驗證真實性 \) 。

由於帳戶夥伴會數位簽章每個安全性權杖，所以資源夥伴可驗證安全性權杖確實是由帳戶夥伴發出，並且未經修改。 數位簽章會由合作夥伴權杖簽署憑證的公開金鑰部分進行驗證 \- 。 驗證簽章之後，資源同盟伺服器會為其組織產生自己的安全性權杖，並使用它自己的權杖簽署憑證來簽署安全性權杖 \- 。

針對同盟夥伴環境，當 \- CA 發出權杖簽署憑證時，請確定：

1.  憑證撤銷會列出 \( \) 信任同盟伺服器的信賴憑證者和 Web 服務器可存取憑證的 crl。

2.  信任同盟伺服器的信賴憑證者和 Web 服務器會根信任 CA 憑證。

資源夥伴中的網頁伺服器會使用權杖簽署憑證的公開金鑰， \- 來確認安全性權杖是否由資源同盟伺服器簽署。 然後，Web 服務器會允許適當的用戶端存取。

## <a name="deployment-considerations-for-token-signing-certificates"></a>權杖簽署憑證的部署考慮 \-
當您在新的 AD FS 安裝中部署第一部同盟伺服器時，您必須取得權杖 \- 簽署憑證，並將它安裝在該同盟伺服器上的本機電腦個人憑證存儲中。 您可以 \- 從企業 ca 或公用 ca 要求一個權杖簽署憑證，或建立自我簽署憑證來取得權杖簽署憑證 \- 。

-   一個權杖簽署憑證的私密金鑰 \- 會在伺服器陣列中的所有同盟伺服器之間共用。

    在同盟伺服器陣列環境中，我們建議所有同盟伺服器共用 \( 或重複使用 \) 相同的權杖 \- 簽署憑證。 您可以 \- 從同盟伺服器上的 CA 安裝單一權杖簽署憑證，然後匯出私密金鑰，只要已發行的憑證標示為可匯出。

    如下圖所示，單一權杖簽署憑證中的私密金鑰 \- 可以共用到伺服器陣列中的所有同盟伺服器。 此選項（相較于下列 [唯一權杖 \- 簽署憑證] 選項），如果您打算從公用 CA 取得權杖簽署憑證，則會降低成本 \- 。

    ![顯示單一權杖簽署憑證之私密金鑰的圖例， \- 可以與伺服器陣列中的所有同盟伺服器共用。](media/adfs2_fedserver_certstory_3.gif)


如需使用 Microsoft 憑證服務做為企業 CA 時安裝憑證的相關資訊，請參閱 [iis 7.0：在 iis 7.0 中建立網域伺服器憑證](https://go.microsoft.com/fwlink/?LinkId=108548)。

如需從公用 CA 安裝憑證的詳細資訊，請參閱 [IIS 7.0：要求網際網路伺服器憑證](https://go.microsoft.com/fwlink/?LinkId=108549)。

如需安裝自我簽署憑證的相關資訊 \- ，請參閱 [iis 7.0： \- 在 Iis 7.0 中建立自我簽署的伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108271)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
