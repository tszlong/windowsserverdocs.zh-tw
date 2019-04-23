---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: 檢閱帳戶夥伴中的同盟伺服器角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835129"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>檢閱帳戶夥伴中的同盟伺服器角色

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

同盟伺服器在 Active Directory Federation Services \(AD FS\)做為安全性權杖簽發者。 同盟伺服器產生宣告式帳戶位於區域屬性的值存放區並將其封裝到安全性權杖，以便使用者可以順暢地存取 Web\-瀏覽器\-架構的應用程式\(使用單一登\-上\(SSO\) \)資源夥伴組織中所裝載。  
  
> [!NOTE]  
> 當使用者使用網頁瀏覽器存取同盟應用程式時，同盟伺服器會自動將 cookie 發行給使用者，以維護其登入狀態為該 Web\-瀏覽器\-基礎的應用程式。 這些 Cookie 包含使用者的宣告。 Cookie 會啟用 SSO 功能，讓使用者不必每次輸入認證瀏覽不同的網頁\-瀏覽器\-資源夥伴中的應用程式。  
  
在網頁 SSO 設計中，具有周邊網路的組織想要網際網路使用者能夠存取應用程式必須安裝在周邊網路中的同盟伺服器 proxy。 在 同盟網頁 SSO 設計中，必須有至少一個帳戶夥伴組織公司網路中安裝的同盟伺服器和資源夥伴組織公司網路中安裝至少一部同盟伺服器。  
  
> [!NOTE]  
> 您可以設定帳戶夥伴組織中的同盟伺服器電腦之前，您必須先將電腦加入至其中的同盟伺服器將用來驗證來自該樹系的使用者的 Active Directory 樹系中任何網域。 如需詳細資訊，請參閱[檢查清單：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
