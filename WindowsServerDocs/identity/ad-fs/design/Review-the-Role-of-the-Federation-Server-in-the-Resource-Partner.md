---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: 檢閱資源夥伴中的同盟伺服器角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b2ed7a09bbc50c83d3bf6f8f2688152ed5202abc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190824"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器角色

資源夥伴組織中的同盟伺服器會攔截帳戶同盟伺服器所傳送的連入安全性權杖、 驗證和簽署，並接著發出用於 Web 的專屬安全性權杖\-基礎應用程式。  
  
> [!NOTE]  
> 當同盟的使用者使用他們的 Web 瀏覽器來存取 Web\-基礎的應用程式，資源夥伴組織中的同盟伺服器建立新的驗證 cookie，並將它寫入至瀏覽器。 此 cookie 可啟用單一\-號\-上\(SSO\)功能，讓使用者不需要再次登入帳戶夥伴中的同盟伺服器在使用者嘗試存取不同的網頁時\-架構資源夥伴中的應用程式。  
  
在網頁 SSO 設計中，至少一部同盟伺服器都必須安裝在周邊網路中。 在 同盟網頁 SSO 設計中，必須有至少一個帳戶夥伴組織公司網路中安裝的同盟伺服器和資源夥伴組織公司網路中安裝至少一部同盟伺服器。  
  
> [!NOTE]  
> 您可以設定資源夥伴組織中的同盟伺服器電腦之前，您必須先將電腦加入至資源夥伴組織中的任何 Active Directory 網域。 如需詳細資訊，請參閱[檢查清單：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

