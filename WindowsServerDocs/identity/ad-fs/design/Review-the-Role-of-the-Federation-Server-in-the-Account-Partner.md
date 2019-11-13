---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: 檢閱帳戶夥伴中的同盟伺服器角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ead8868f38faa570a0e524630e23d99e276a7c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407925"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>檢閱帳戶夥伴中的同盟伺服器角色

Active Directory 同盟服務中的同盟伺服器 \(AD FS\) 做為安全性權杖簽發者。 同盟伺服器會根據位於本機屬性存放區的帳戶值來產生宣告，並將其封裝到安全性權杖，讓使用者可以順暢地存取 Web\-瀏覽器\-應用程式，\(在資源夥伴組織中裝載 \(SSO\)\) 上使用單一登入\-。  
  
> [!NOTE]  
> 當您的使用者使用網頁瀏覽器存取同盟應用程式時，同盟伺服器會自動將 cookie 發行給使用者，以維護其 Web\-瀏覽器\-應用程式的登入狀態。 這些 Cookie 包含使用者的宣告。 Cookie 可啟用 SSO 功能，讓使用者不需要在每次造訪資源夥伴中不同的 Web\-瀏覽器\-應用程式時輸入認證。  
  
在網頁 SSO 設計中，具有周邊網路且想要網際網路使用者能夠存取應用程式的組織，必須在周邊網路中安裝同盟伺服器 proxy。 在同盟網頁 SSO 設計中，必須在帳戶夥伴組織的公司網路中至少安裝一部同盟伺服器，並在資源夥伴組織的公司網路中至少安裝一部同盟伺服器。  
  
> [!NOTE]  
> 在帳戶夥伴組織中設定同盟伺服器電腦之前，您必須先將電腦加入 Active Directory 樹系中的任何網域，以使用同盟伺服器來驗證來自該樹系的使用者。 如需詳細資訊，請參閱 [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
