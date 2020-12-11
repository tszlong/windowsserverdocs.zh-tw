---
description: 深入瞭解：檢查帳戶夥伴中的同盟伺服器角色
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: 檢閱帳戶夥伴中的同盟伺服器角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ccde30456f09d2bb86053db457db5758fd03185b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049396"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>檢閱帳戶夥伴中的同盟伺服器角色

Active Directory 同盟服務中的同盟伺服器 \( AD FS \) 做為安全性權杖簽發者。 同盟伺服器會根據位於本機屬性存放區的帳戶值產生宣告，並將它們封裝到安全性權杖中，讓使用者可以 \- \- \( 使用 \- \( \) \) 資源夥伴組織中裝載的單一登入 SSO，順暢地存取以網頁瀏覽器為基礎的應用程式。

> [!NOTE]
> 當您的使用者使用網頁瀏覽器存取同盟應用程式時，同盟伺服器會自動將 cookie 發出給使用者，以維護其以網頁 \- 瀏覽器為基礎之應用程式的登入狀態 \- 。 這些 Cookie 包含使用者的宣告。 Cookie 可啟用 SSO 功能，讓使用者不需要在每次造訪 \- 資源夥伴中的不同網頁瀏覽器應用程式時輸入認證 \- 。

在網頁 SSO 設計中，有周邊網路的組織想要讓網際網路使用者存取應用程式，必須在周邊網路中安裝同盟伺服器 proxy。 在同盟網頁 SSO 設計中，至少必須在帳戶夥伴組織的公司網路中安裝一部同盟伺服器，而且至少必須在資源夥伴組織的公司網路中安裝一部同盟伺服器。

> [!NOTE]
> 設定帳戶夥伴組織中的同盟伺服器電腦之前，您必須先將電腦加入 Active Directory 樹系中的任何網域，而同盟伺服器將用來驗證該樹系中的使用者。 如需詳細資訊，請參閱 [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
