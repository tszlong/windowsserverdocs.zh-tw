---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: 屬性存放區的角色
description: 深入瞭解：屬性存放區的角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: dc23e15893672633c7e175d2b0313771d327fccc
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045306"
---
# <a name="the-role-of-attribute-stores"></a>屬性存放區的角色
Active Directory 同盟服務使用「屬性存放區」一詞來指稱組織用來儲存其使用者帳戶及其相關屬性值的目錄或資料庫。 在身分識別提供者組織中設定之後，AD FS 會從存放區中抓取這些屬性值，並根據該資訊建立宣告，如此一來，當同盟使用者的 \( 帳戶儲存在身分識別提供者組織中的使用者 \) 嘗試存取應用程式或服務時，裝載于信賴憑證者組織的 Web 應用程式或服務就可以進行適當的授權決策。

如需有關如何產生宣告的詳細資訊，請參閱＜ [The Role of Claims](The-Role-of-Claims.md)＞。

## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>屬性存放區如何符合您的 AD FS 部署目標
使用者屬性存放區的位置，以及使用者驗證的位置，會決定您設計 AD FS 以支援使用者身分識別的方式。 根據屬性存放區的位置，以及使用者將在內部網路或網際網路存取應用程式的位置而定 \( \) ，您可以使用下列其中一個部署目標：

-   [提供您的 Active Directory 使用者對宣告感知應用程式和服務的存取權] —在此目標中，您組織中 \( \) 的使用者會在使用者登入公司內部網路中的 Active Directory 時，存取 AD FS 安全的應用程式或服務，或是您自己的應用程式或服務。

-   [為您的 Active Directory 使用者提供其他組織的應用程式和服務存取權] —在此目標中，您組織中 \( \) 的使用者會在使用者登入公司內部網路中的屬性存放區，以及從網際網路遠端登入時，存取 AD FS 安全的應用程式或服務，以存取您自己的應用程式或服務。

-   [為其他組織中的使用者提供您宣告感知應用程式和服務的存取權] —在此目標中，另一個組織中的使用者帳戶位於該組織的公司內部網路上的屬性存放區，必須存取您組織中受 AD FS 保護的應用程式。 當 \- 位於組織周邊網路中屬性存放區的取用者型使用者帳戶必須能夠存取您組織中受 AD FS 保護的應用程式時，此目標也適用。

視您組織的屬性存放區位置和其他需求而定，您可以合併數個部署目標來完成 AD FS 部署的設計。

## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>AD FS 支援的屬性存放區
AD FS 支援廣泛的目錄和資料庫存放區，可供您用來解壓縮系統管理員 \- 定義的屬性值，並以這些值填入宣告。 AD FS 支援下列任何目錄或資料庫做為屬性存放區：

-   Windows server 2003 中的 Active Directory、windows server 2008 中的 Active Directory Domain Services \( AD DS \) 、windows server 2012 和 2012 R2 中 AD DS，以及 windows server 2016 和更新版本;

-   所有版本的 Microsoft SQL Server 2005、SQL Server 2008、SQL Server 2012、SQL Server 2014 以及 SQL Server 2016 和更新版本;

-   自訂屬性存放區
