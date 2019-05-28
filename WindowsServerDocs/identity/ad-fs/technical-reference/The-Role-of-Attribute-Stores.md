---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: 屬性存放區的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bec3ebf1bd12b260dbbb245a6a905277ff0d749f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188536"
---
# <a name="the-role-of-attribute-stores"></a>屬性存放區的角色
Active Directory Federation Services 使用 「 屬性存放區 」 一詞指的目錄或組織用來儲存其使用者帳戶及其相關聯的屬性值的資料庫。 設定身分識別提供者組織中之後，AD FS 從存放區擷取這些屬性的值，並建立根據該資訊，如此的 Web 應用程式或信賴憑證者的合作對象組織中裝載的服務可以進行適當的宣告當同盟使用者，授權決策\(使用者帳戶會儲存在身分識別提供者組織\)嘗試存取應用程式或服務。  
  
如需有關如何產生宣告的詳細資訊，請參閱＜ [The Role of Claims](The-Role-of-Claims.md)＞。  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>屬性存放區如何符合您的 AD FS 部署目標  
使用者屬性存放區位置和從中驗證使用者的位置會決定您如何設計來支援使用者身分識別的 AD FS。 根據屬性存放區所在的位置和使用者存取應用程式\(在內部網路或網際網路上\)，您可以使用下列的部署目標：  
  
-   [提供您的 Active Directory Users Access Your Claims-aware Applications and Services](https://technet.microsoft.com/library/dd807071.aspx)— 在此目標中，您的組織中的使用者存取的 AD FS 保護的應用程式或服務\(自己的應用程式或服務或合作夥伴的應用程式或服務\)當使用者登入至 Active Directory 公司內部網路。  
  
-   [應用程式和其他組織的服務提供您的 Active Directory Users Access](https://technet.microsoft.com/library/dd807123.aspx)— 在此目標中，您的組織中的使用者存取的 AD FS 保護的應用程式或服務\(自己的應用程式或服務或合作夥伴的應用程式或服務\)當使用者登入公司內部網路和其遠端登入來自網際網路的屬性存放區。  
  
-   [提供使用者在另一個組織存取 Your Claims-aware Applications and Services](https://technet.microsoft.com/library/dd807099.aspx)— 在此目標，另一個組織中的使用者帳戶位於該組織的公司內部網路上的屬性存放區中必須存取的 AD FS –在您的組織中受保護的應用程式。 此目標也適用於當取用者\-位於您組織的周邊網路中的屬性存放區中的基礎的使用者帳戶必須具有存取權提供給 AD FS 保護您組織中的應用程式。  
  
根據屬性存放區位置和其他組織的需求，您可以結合數個部署目標來完成您的 AD FS 部署的設計。  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>AD FS 支援的屬性存放區  
AD FS 支援各種不同的目錄和資料庫儲存了您可用於擷取系統管理員\-定義屬性值，並填入宣告這些值。 AD FS 支援任何下列目錄或資料庫做為屬性存放區：  
  
-   Windows Server 2003 Active Directory 網域服務中的 active Directory \(AD DS\) Windows Server 2012 和 2012 r2 中的 Windows Server 2008 AD DS 和 Windows Server 2016 中。 
  
-   Microsoft SQL Server 2005，SQL Server 2008，SQL Server 2012、 SQL Server 2014 和 SQL Server 2016 的所有版本  
  
-   自訂屬性存放區  
  

