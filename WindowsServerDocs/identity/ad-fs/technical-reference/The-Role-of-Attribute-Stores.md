---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: 屬性存放區的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0a1543f2c935c2ef76ea014567b18bfc778c7401
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407381"
---
# <a name="the-role-of-attribute-stores"></a>屬性存放區的角色
Active Directory 同盟服務使用「屬性存放區」一詞來參照組織用來儲存其使用者帳戶及其相關屬性值的目錄或資料庫。 在身分識別提供者組織中設定之後，AD FS 從存放區中抓取這些屬性值，並根據該資訊建立宣告，讓信賴憑證者組織中裝載的 Web 應用程式或服務可以適當地進行當同盟使用者\(的帳戶儲存在身分識別提供者組織\)中的使用者嘗試存取應用程式或服務時，就會進行授權決策。  
  
如需有關如何產生宣告的詳細資訊，請參閱＜ [The Role of Claims](The-Role-of-Claims.md)＞。  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>屬性存放區如何符合您的 AD FS 部署目標  
使用者屬性存放區的位置，以及使用者驗證的位置，會決定您設計 AD FS 以支援使用者身分識別的方式。 根據屬性存放區的所在位置，以及使用者在內部網路或網際網路\( \)上存取應用程式的位置而定，您可以使用下列其中一個部署目標：  
  
-   為[您的 Active Directory 使用者提供您的宣告感知應用程式和服務的存取權](https://technet.microsoft.com/library/dd807071.aspx)-在此目標中，組織中的使用者可存取受\(AD FS 保護的應用程式或服務，無論您自己的應用程式或服務或合作夥伴的當使用者登\)入公司內部網路中的 Active Directory 時的應用程式或服務。  
  
-   [為您的 Active Directory 使用者提供其他組織的應用程式和服務存取權](https://technet.microsoft.com/library/dd807123.aspx)-在此目標中，組織中的使用者可存取 AD FS 保護的\(應用程式或服務，無論您自己的應用程式或服務或當使用者登入公司\)內部網路中的屬性存放區，以及從網際網路遠端登入時，合作夥伴的應用程式或服務。  
  
-   為[其他組織的使用者提供您的宣告感知應用程式和服務的存取權](https://technet.microsoft.com/library/dd807099.aspx)，在此目標中，另一個組織中的使用者帳戶位於該組織的公司內部網路上的屬性存放區，必須存取 AD FS –貴組織中的安全應用程式。 此目標也適用于在\-組織的周邊網路中的屬性存放區中，以取用者為基礎的使用者帳戶，必須能夠存取貴組織中受 AD FS 保護的應用程式。  
  
視屬性存放區的位置和組織的其他需求而定，您可以結合其中數個部署目標來完成 AD FS 部署的設計。  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>AD FS 支援的屬性存放區  
AD FS 支援各種不同的目錄和資料庫存放區，可供您用來解壓縮\-系統管理員定義的屬性值，並以這些值填入宣告。 AD FS 支援下列任何目錄或資料庫做為屬性存放區：  
  
-   Windows server 2003 中的 Active Directory、 \(windows\) server 2008 中的 Active Directory Domain Services AD DS、windows server 2012 和 2012 R2 中的 AD DS，以及 windows server 2016。 
  
-   所有版本的 Microsoft SQL Server 2005、SQL Server 2008、SQL Server 2012、SQL Server 2014 和 SQL Server 2016  
  
-   自訂屬性存放區  
  

