---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: "建立聯盟 Proxy 伺服器的時機"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server-proxy"></a>建立聯盟 Proxy 伺服器的時機

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您在組織中建立聯盟 proxy 伺服器 Active Directory 同盟服務 \(AD FS\) 部署新增額外的安全性層級。 請考慮部署聯盟伺服器您組織的周邊網路 proxy，當您想要的事項：  
  
-   直接存取聯盟伺服器避免外部 client 電腦。 部署聯盟伺服器周邊網路 proxy，您有效隔離聯盟伺服器使其可以存取僅供 client 電腦的登入透過聯盟伺服器 proxy 代表的外部 client 電腦所做的公司網路。 聯盟伺服器 proxy 不能用製作權杖私密金鑰存取。 如需詳細資訊，請查看[放置聯盟 Proxy 伺服器](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   提供方便的方式來區分即將網際網路，而不來自您使用 Windows 整合驗證的企業網路使用者的使用者 sign\ 中的體驗。 聯盟 proxy 伺服器的認證或主領域詳細資料會從網際網路 client 電腦收集使用登入，登出和身分提供者探索 \(homerealmdiscovery.aspx\) 頁面會儲存在聯盟 proxy 伺服器上。  
  
    相較之下，client 電腦隨附的企業網路遭遇不同的體驗，從依據聯盟伺服器的設定。 適用於 Windows 整合式驗證的企業網路上的使用者提供順暢的 sign\ 中體驗公司網路聯盟伺服器通常設定。  
  
您在組織中播放的聯盟 proxy 伺服器角色是否放置聯盟 proxy 伺服器 account 合作夥伴公司或資源合作夥伴組織中而定。 例如，當聯盟 proxy 伺服器位於 account 協力廠商周邊網路，其的角色是從瀏覽器會收集使用者的認證資訊。 聯盟 proxy 伺服器位於周邊網路資源協力廠商，它轉送資源聯盟伺服器的安全性權杖要求，並產生組織的安全性權杖中所提供的其 account 合作夥伴的安全性權杖回應。  
  
如需詳細資訊，請查看[檢視聯盟伺服器 Proxy Account 合作夥伴中的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)和[檢視聯盟伺服器 Proxy 資源夥伴中的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>如何建立聯盟 proxy 伺服器  
您可以建立聯盟伺服器 proxy 使用 AD FS 聯盟伺服器 Proxy 設定精靈或 Fsconfig.exe command\ 列工具。 如需如何執行此動作，請查看[設定電腦聯盟 Proxy 角色](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
了解如何設定的必要條件所有所需部署聯盟 proxy 伺服器的一般資訊，請查看[檢查清單︰ 設定好聯盟伺服器 Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
