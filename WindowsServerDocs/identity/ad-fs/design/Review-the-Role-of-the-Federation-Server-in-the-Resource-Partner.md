---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: 檢閱資源夥伴中的同盟伺服器角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fd9f20eb7559f5862ee50bdd8364fa1604d3c1b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358974"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器角色

資源夥伴組織中的同盟伺服器會攔截帳戶同盟伺服器所傳送的連入安全性權杖、進行驗證和簽署，然後發出自己的安全性權杖，其目標為 Web @ no__t-0based 應用程式.  
  
> [!NOTE]  
> 當同盟使用者使用其網頁瀏覽器來存取 Web @ no__t 0based 應用程式時，資源夥伴組織中的同盟伺服器會建立新的驗證 cookie，並將其寫入瀏覽器。 此 cookie 可啟用單一 @ no__t-0sign @ no__t-1on \(SSO @ no__t-3 功能，讓使用者在嘗試存取資源中的不同 Web @ no__t-4based 應用程式時，不需要再次登入帳戶夥伴中的同盟伺服器搭檔.  
  
在網頁 SSO 設計中，必須在周邊網路中安裝至少一部同盟伺服器。 在同盟網頁 SSO 設計中，必須在帳戶夥伴組織的公司網路中至少安裝一部同盟伺服器，並在資源夥伴組織的公司網路中至少安裝一部同盟伺服器。  
  
> [!NOTE]  
> 在資源夥伴組織中設定同盟伺服器電腦之前，您必須先將電腦加入資源夥伴組織中的任何 Active Directory 網域。 如需詳細資訊，請參閱 [Checklist：設定同盟伺服器 @ no__t-0。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

