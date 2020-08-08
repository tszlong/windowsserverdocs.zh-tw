---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: 在資源夥伴組織中部署舊版 AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a129423c4d646549b0b569fb9920e0b550a1fdbb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942892"
---
# <a name="deploying-legacy-ad-fs-in-the-resource-partner-organization"></a>在資源夥伴組織中部署舊版 AD FS

Active Directory 同盟服務 AD FS 中的資源夥伴 \( 組織 \) 代表其 Web 服務器可能受到資源 \- 端同盟伺服器保護的組織。 資源夥伴的同盟伺服器會使用帳戶夥伴所產生的安全性權杖，向位於資源夥伴的 Web 服務器提供宣告。

在您需要將同盟服務或應用程式的存取權提供給許多不同的使用者（在某些使用者位於不同的組織時），您可以設定資源同盟伺服器，讓您可以部署多個帳戶夥伴。

如需有關如何安裝和設定資源夥伴組織的詳細資訊，請參閱 [Checklist: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)。

## <a name="in-this-section"></a>本節內容

-   [檢閱資源夥伴中的同盟伺服器角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)

-   [檢閱資源夥伴中的同盟伺服器 Proxy 角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)

-   [決定資源夥伴的同盟應用程式策略](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)


## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
