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
ms.openlocfilehash: dbceb19d31738bdc5b628a9a2b069e5d3022d145
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190949"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>檢閱帳戶夥伴中的同盟伺服器 Proxy 的角色

Active Directory Federation Services 的帳戶夥伴組織的周邊網路中的同盟伺服器 proxy 的主要角色\(AD FS\)是從登入用戶端電腦收集驗證認證透過網際網路，並將這些認證傳遞給同盟伺服器，其位於帳戶夥伴組織公司網路內。 用戶端電腦的帳戶會儲存在帳戶夥伴的屬性存放區。  
  
同盟伺服器 proxy 也可以在一或多個下列角色，根據您設定帳戶夥伴組織的需求的方式運作：  
  
-   轉送安全性權杖，同盟伺服器到同盟伺服器 proxy，然後轉送至用戶端電腦的語彙基元簽發安全性權杖。 安全性權杖是用來為該用戶端電腦提供存取權給特定的信賴憑證者。  
  
-   收集認證-同盟伺服器 proxy 會使用預設用戶端登入 Web 表單\(clientlogon.aspx\)收集密碼\-基礎透過表單認證\-型驗證。 不過，您可以自訂此表單以接受其他支援的類型的驗證，例如安全通訊端層\(SSL\)用戶端驗證。 如需如何自訂此頁面的詳細資訊，請參閱 < 自訂用戶端登入和首頁領域探索頁面\( [http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)\)。 同盟伺服器 proxy 不接受透過 Windows 整合式驗證的認證。  
  
若要總而言之，帳戶夥伴中的同盟伺服器 proxy 做為用戶端登入位於公司網路中的同盟伺服器 proxy。 同盟伺服器 proxy 也協助散發安全性權杖給信賴憑證者的合作對象用於網際網路用戶端。  
  
> [!CAUTION]  
> 公開在帳戶夥伴外部網路上的同盟伺服器 proxy 將用戶端登入 Web 表單可存取網際網路的任何人存取。 這可能會讓您的組織容易遭受部分密碼\-型攻擊，例如字典攻擊或可以觸發會儲存在公司的 Active Directory 網域的使用者帳戶的帳戶鎖定的暴力密碼破解攻擊服務\(AD DS\)。  
  

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
