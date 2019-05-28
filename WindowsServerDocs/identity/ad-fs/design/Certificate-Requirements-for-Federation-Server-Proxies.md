---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: 同盟伺服器 Proxy 的憑證需求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca0b25480eedfc6471837ab8ae83b0d1d522e61e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191660"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>同盟伺服器 Proxy 的憑證需求

在 Active Directory Federation Services 的同盟伺服器 proxy 角色中執行的伺服器\(AD FS\)才能使用安全通訊端層\(SSL\)伺服器驗證憑證。 同盟伺服器 proxy 會使用 SSL 伺服器驗證憑證，保護網頁伺服器與 Web 用戶端之間通訊流量的安全。  
  
同盟伺服器 proxy 通常會公開至網際網路上不會納入您的企業公開金鑰基礎結構的電腦\(PKI\)。 因此，使用 伺服器驗證憑證所發出的公用\(第三個\-合作對象\)憑證授權單位\(CA\)，例如 VeriSign。  
  
當您有同盟伺服器 proxy 伺服器陣列時，所有同盟伺服器 proxy 電腦必須都使用相同的伺服器驗證憑證。 如需詳細資訊，請參閱 [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md)。  
  
請務必確認 AD FS 管理嵌入式管理單元中指定的 Federation Service 名稱值，這是伺服器驗證憑證比對中的主體名稱\-中。 若要找出此值，請開啟嵌入式管理單元\-中，以滑鼠右鍵\-按一下**服務**，按一下 **編輯 Federation Service 內容**，然後尋找中的值**同盟服務名稱**文字方塊。  
  
如需使用 SSL 憑證的一般資訊，請參閱 < 設定安全通訊端層在 IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/嗎？LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544) \)和 IIS 7.0 中設定伺服器憑證\( [http:\/\/go.microsoft.com\/fwlink\/?LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\)。  
  
> [!NOTE]  
> 不需要 AD FS 同盟伺服器 proxy 的用戶端驗證憑證。  
  
如果任何憑證，您使用具有憑證撤銷清單\(Crl\)，具有已設定的憑證的伺服器必須能夠連絡發佈 Crl 的伺服器。 CRL 的類型決定了要使用的連接埠。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
