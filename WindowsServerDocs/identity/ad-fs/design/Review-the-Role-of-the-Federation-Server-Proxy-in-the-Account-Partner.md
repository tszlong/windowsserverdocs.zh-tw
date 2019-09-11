---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: 檢閱帳戶夥伴中的同盟伺服器 Proxy 的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eb0547ac1d84fffed31360f0ee510504510c24ce
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867664"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>檢閱帳戶夥伴中的同盟伺服器 Proxy 的角色

在 Active Directory 同盟服務\(AD FS\)的帳戶夥伴組織周邊網路中，同盟伺服器 proxy 的主要角色是從登入的用戶端電腦收集驗證認證透過網際網路，並將這些認證傳遞到同盟伺服器，此伺服器位於帳戶夥伴組織的公司網路內。 用戶端電腦的帳戶會儲存在帳戶夥伴的屬性存放區中。  
  
同盟伺服器 proxy 也可以在下列一或多個角色中運作，視您設定它以符合帳戶夥伴組織需求的方式而定：  
  
-   轉送安全性權杖-同盟伺服器會將安全性權杖發行到同盟伺服器 proxy，然後將權杖轉送至用戶端電腦。 安全性權杖是用來為該用戶端電腦提供存取權給特定的信賴憑證者。  
  
-   收集認證—同盟伺服器 proxy 會使用預設的用戶端登入\(Web 表單\) clientlogon.aspx，透過\-以表單\-為基礎的驗證來收集密碼認證。 不過，您可以自訂此表單以接受其他支援的驗證類型，例如\(安全通訊端層\) SSL 用戶端驗證。 如需如何自訂此頁面的詳細資訊，請參閱自訂用戶端登入\(和主領域探索頁面[HTTP：\/ \/ \/go.microsoft.com\/fwlink？LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)。\) 同盟伺服器 proxy 不接受透過 Windows 整合式驗證的認證。  
  
總而言之，帳戶夥伴中的同盟伺服器 proxy 會作為 proxy，讓用戶端登入位於公司網路的同盟伺服器。 同盟伺服器 proxy 也有助於將安全性權杖散發給以信賴憑證者為目標的網際網路用戶端。  
  
> [!CAUTION]  
> 在帳戶夥伴外部網路上公開同盟伺服器 proxy，可供具有網際網路存取權的任何人存取用戶端登入 Web 表單。 這可能會讓您的組織容易遭受一些\-以密碼為基礎的攻擊，例如字典攻擊或暴力破解攻擊，以觸發儲存在公司 Active Directory 網域的使用者帳戶的帳戶鎖定。服務\(AD DS\)。  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
