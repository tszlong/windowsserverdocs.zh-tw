---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: "檢視聯盟伺服器 Proxy Account 合作夥伴中的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>檢視聯盟伺服器 Proxy Account 合作夥伴中的角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

主要聯盟伺服器 proxy 周邊網路的 Active Directory 同盟服務 \(AD FS\) account 合作夥伴組織中的角色是從網際網路上登入時 client 的電腦收集驗證憑證，並將這些認證傳遞至聯盟伺服器，這位於 account 合作夥伴公司的企業網路。 負責 client 電腦會儲存在 account 合作夥伴的屬性存放區。  
  
聯盟 proxy 伺服器也可以在下列一或多個下列的角色，根據您的設定需求 account 合作夥伴公司的功能：  
  
-   轉送的安全性權杖-聯盟伺服器問題的安全性權杖給聯盟伺服器 proxy，然後才轉送 client 電腦預付碼。 安全性權杖用來提供特定信賴該 client 電腦的存取。  
  
-   收集認證 — 聯盟 proxy 伺服器會使用預設 client 登入 Web 表單 \(clientlogon.aspx\) 收集 password\ 認證透過 forms\ 為基礎的驗證。 不過，您可以自訂若接受其他受支援的類型的驗證，例如安全通訊端層 \(SSL\) client 驗證此表單。 如需如何自訂此頁面，查看自訂 Client 登入及 Home 領域探索頁面 \ ([http:///\/go.microsoft.com\/fwlink\/ 嗎？LinkId\ = 104275](https://go.microsoft.com/fwlink/?LinkId=104275)\)。 聯盟 proxy 伺服器不接受透過 Windows 整合式驗證認證。  
  
總結聯盟伺服器 proxy account 合作夥伴中的扮演 client 登入聯盟伺服器位於公司網路 proxy。 聯盟 proxy 伺服器也可以協助安全性權杖給網際網路戶端目的地信賴的對象為的分配。  
  
> [!CAUTION]  
> 公開聯盟伺服器上 account 合作夥伴外部網路 proxy 將 Web 表單可以存取網際網路的任何人 client 登入來存取。 這可能會讓您的組織某些 password\ 型攻擊，例如字典攻擊或可以設定觸發儲存在公司的 Active Directory Domain Services \(AD DS\) 帳號鎖定暴力攻擊。  
  

## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
