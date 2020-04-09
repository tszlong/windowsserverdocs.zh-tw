---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: 檢閱資源夥伴中的同盟伺服器角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8cda394800bf0554e00e2897d240e2c08a72a00b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858551"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器角色

資源夥伴組織中的同盟伺服器會攔截帳戶同盟伺服器所傳送的連入安全性權杖、進行驗證和簽署，然後發出以 Web\-為基礎之應用程式的專屬安全性權杖。  
  
> [!NOTE]  
> 當同盟使用者使用其網頁瀏覽器來存取以 Web\-為基礎的應用程式時，資源夥伴組織中的同盟伺服器會建立新的驗證 cookie，並將其寫入瀏覽器。 此 cookie 可讓您使用單一\-登入\-\(SSO\) 功能，讓使用者在嘗試存取資源夥伴中不同的 Web\-應用程式時，不需要在帳戶夥伴的同盟伺服器上再次登入。  
  
在網頁 SSO 設計中，必須在周邊網路中安裝至少一部同盟伺服器。 在同盟網頁 SSO 設計中，必須在帳戶夥伴組織的公司網路中至少安裝一部同盟伺服器，並在資源夥伴組織的公司網路中至少安裝一部同盟伺服器。  
  
> [!NOTE]  
> 在資源夥伴組織中設定同盟伺服器電腦之前，您必須先將電腦加入資源夥伴組織中的任何 Active Directory 網域。 如需詳細資訊，請參閱 [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

