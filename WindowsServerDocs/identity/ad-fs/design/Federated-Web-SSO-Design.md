---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: "聯盟的網路 SSO 設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="federated-web-sso-design"></a>聯盟的網路 SSO 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) 的聯盟網路 Single\-Sign\-On \(SSO\) 設計涉及安全通訊跨多個防火牆、周邊網路及 name\ 高解析度的伺服器，除了整個網際網路路由基礎結構。  
  
建立聯盟信任關係，允許使用者在組織中的兩個組織同意時，此設計通常會使用 \ (account 合作夥伴 organization\) Web\ 為基礎的應用程式或服務，AD FS 其他公司會受到 \ (資源合作夥伴 organization\)。  
  
囉聯盟信任關係是 business\ 層級合約或合作關係之間兩個組織都更能襯托。 下圖所示，您可以建立兩個企業而言，會導致 end\ to\ 高階聯盟案例中的有聯盟信任關係。  
  
![聯盟的網路 sso](media/adfs2_FederatedWebSSODesign.gif)  
  
One\ 向中的箭號圖示表示聯盟的方向信任的-喜歡 Windows 信任的方向，隨時 account 側邊的樹系點。 這表示驗證流量 account 合作夥伴公司的資源合作夥伴組織。  
  
在這種聯盟網路 SSO 設計，有兩個聯盟伺服器 \（中 Fabrikam 和到其他 Contoso\）路由來自帳號驗證要求 Fabrikam Web\ 為基礎的應用程式或服務 Contoso 中的。  
  
> [!NOTE]  
> 取得額外安全性時，您可以使用聯盟伺服器 proxy 轉送要求聯盟伺服器的不是從網際網路直接存取。  
  
在此範例中，Fabrikam 是的身分或帳號，提供者。 聯盟網路 SSO 設計的 Fabrikam 部分使用下列 AD FS 部署目標：  
  
-   [提供您的 Active Directory 使用者存取權的應用程式與其他公司的服務](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是資源提供者。 聯盟網路 SSO 設計的部分 Contoso 達成下列 AD FS 部署目標：  
  
-   [在另一部組織存取的使用者提供您宣告感知應用程式與服務](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [提供您 Active Directory 的使用者存取您的宣告感知應用程式與服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需詳細的工作，您可以使用計劃和部署的聯盟網路 SSO 設計的清單，請查看[檢查清單︰ 實作聯盟網路 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
