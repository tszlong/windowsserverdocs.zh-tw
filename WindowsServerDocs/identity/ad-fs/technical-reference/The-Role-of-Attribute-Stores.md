---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: "此屬性存放區的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
 >適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>此屬性存放區的角色
Active Directory 同盟服務使用的字詞 」 屬性商店 「 參考目錄或資料庫組織用來儲存其帳號，其相關聯的屬性的值。 它的身分提供者組織中設定之後，AD FS 從存放區擷取這些屬性的值，並建立宣告，根據該資訊，以便 Web 應用程式或服務裝載信賴的派對組織中可以做出適當的授權，只要聯盟使用者 \ 嘗試應用程式或服務存取 （account 其會儲存在身分提供者 organization\ 使用者）。  
  
如需有關如何專宣告，請查看[宣告角色](The-Role-of-Claims.md)。  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>如何儲存屬性符合使用 AD FS 部署目標  
使用者屬性存放區的位置，以及位置從中驗證使用者判斷您如何設計支援的使用者身分 AD FS。 根據屬性存放區的位置，讓使用者將會存取應用程式 \ （內部網路或 Internet\），您可以使用其中一項下列部署目標：  
  
-   [提供您 Active Directory 使用者存取您宣告感知應用程式和服務](https://technet.microsoft.com/library/dd807071.aspx)-這個目標，在組織中的使用者存取 AD FS – 保護的應用程式或服務 \ （您自己的應用程式或服務或合作夥伴的應用程式或 service\） 時，使用者在公司 intranet 登入 Active Directory。  
  
-   [其他公司的服務和應用程式提供您 Active Directory 使用者存取](https://technet.microsoft.com/library/dd807123.aspx)-這個目標，在組織中的使用者存取 AD FS – 保護的應用程式或服務 \ （您自己的應用程式或服務或合作夥伴的應用程式或 service\） 時，使用者登入公司內部網路並登入時遠端從網際網路屬性存放區。  
  
-   [在另一部組織存取的使用者提供您宣告感知應用程式和服務](https://technet.microsoft.com/library/dd807099.aspx)-這個目標，在另一部在組織中的使用者帳號位於屬性存放在該組織公司的企業網路，必須存取 AD FS – 保護您的組織中的應用程式。 Consumer\ 為基礎的使用者帳號，您組織的周邊網路屬性存放區位於必須 ad FS – 保護您的組織中的應用程式提供的存取權時，也可以運作這個目標。  
  
您可以根據屬性存放區的位置，您的組織其他需求結合幾個部署目標以完成部署 AD FS 的設計。  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>屬性 AD FS 所支援的商店  
AD FS 支援各種 directory 和資料庫儲存，您可以使用解壓縮 administrator\ 定義屬性的值與填入主張使用這些值。 AD FS 支援的任何下列目錄或資料庫屬性商店：  
  
-   在 Windows Server 2003 active Directory Active Directory Domain 服務 \(AD DS\) Windows Server 2008、 Windows Server 2012 和 2012 R2 中 AD DS 和 Windows Server 2016。 
  
-   所有的 Microsoft SQL Server 2005、 SQL Server 2008、 SQL Server 2012、 SQL Server 2014，以及 SQL Server 2016 的版本  
  
-   自訂屬性存放區  
  

