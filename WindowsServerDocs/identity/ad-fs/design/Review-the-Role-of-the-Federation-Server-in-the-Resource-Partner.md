---
description: 深入瞭解：檢查資源夥伴中的同盟伺服器角色
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: 檢閱資源夥伴中的同盟伺服器角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e01c291fe3e8d96cb81cfee9c62dd1abb2f34662
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049386"
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>檢閱資源夥伴中的同盟伺服器角色

資源夥伴組織中的同盟伺服器會攔截帳戶同盟伺服器所傳送的連入安全性權杖、驗證並簽署它們，然後發出自己的安全性權杖，而該權杖是以 Web 為 \- 基礎的應用程式為目標。

> [!NOTE]
> 當同盟使用者使用其網頁瀏覽器來存取 Web \- 應用程式時，資源夥伴組織中的同盟伺服器會建立新的驗證 cookie，並將其寫入瀏覽器。 此 cookie 可啟用 \- 單一 \- 登錄 \( SSO \) 功能，讓使用者在使用者嘗試存取資源夥伴中的不同 Web 應用程式時，不需要在帳戶夥伴的同盟伺服器上再次登入 \- 。

在網頁 SSO 設計中，至少必須在周邊網路中安裝一部同盟伺服器。 在同盟網頁 SSO 設計中，至少必須在帳戶夥伴組織的公司網路中安裝一部同盟伺服器，而且至少必須在資源夥伴組織的公司網路中安裝一部同盟伺服器。

> [!NOTE]
> 在資源夥伴組織中設定同盟伺服器電腦之前，您必須先將電腦加入資源夥伴組織中的任何 Active Directory 網域。 如需詳細資訊，請參閱 [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

