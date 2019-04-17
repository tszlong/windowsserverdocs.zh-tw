---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: "部署聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-servers"></a>部署聯盟伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要部署在 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器，完成的工作中每個[檢查清單︰ 設定好聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)。  
  
> [!NOTE]  
> 當您使用此檢查清單時，我們建議您先朗讀聯盟伺服器計劃的資訊尋找參考資料[在 Windows Server 2012 中 AD FS 程式設計指南](https://technet.microsoft.com/library/dd807036.aspx)設定伺服器的程序在您開始之前。 下列如此一來檢查清單可提供變得更好了解聯盟伺服器的設計和部署程序。  
  
## <a name="about-federation-servers"></a>有關聯盟伺服器的資訊  
聯盟伺服器的電腦安裝軟體 AD FS 使用執行 Windows Server 2008 已設定為作用中聯盟伺服器角色。 聯盟伺服器驗證，或是傳送要求在組織中其他使用者帳號，並從 client 可以隨時隨地在網際網路的電腦。  
  
在電腦上安裝 AD FS 軟體，並使用 AD FS 聯盟伺服器設定精靈聯盟伺服器角色該設定的動作可該電腦聯盟伺服器。 它也可以 AD FS 管理 snap\ 中提供在電腦上**Start\\Administrative Tools\\**功能表，您可以指定下列：  
  
-   AD FS 主機名稱位置合作夥伴和應用程式將會傳送權杖要求和回應  
  
-   AD FS 識別碼的合作夥伴公司和應用程式將會以找出的唯一名稱或位置，您的組織使用  
  
-   Token\ 簽署的憑證問題並登入權杖會使用所有聯盟伺服器伺服器  
  
-   自訂 ASP.NET 網頁 client 登入、登出，以及將愉快 client 的 account 合作夥伴探索的位置  
  
    > [!NOTE]  
    > 大部分的核心使用者介面 \(UI\) 設定，全都在 web.config 每個聯盟伺服器上。 AD FS 主機 AD FS 識別碼值不是指定名稱及 web.config 檔案中。  
  
聯盟伺服器裝載問題權杖根據認證宣告發行引擎 \（如範例、使用者名稱和 password\）的看到它。 安全性權杖是一或多個宣告的密碼編譯簽署的資料單位。 宣告伺服器會聲明 \（，例如名稱、的身分、金鑰、群組，權限或 capability\）client 有關。 聯盟伺服器上驗證認證之後 \（透過的使用者登入 process\)，使用者宣告會收集使用者屬性是儲存在指定的屬性市集中檢查透過。  
  
在聯盟網路 Single\-Sign\-On \(SSO\) 設計 \（AD FS 的設計帶有中的兩個或更多的每個組織都 involved\），適用於特定信賴理賠要求規則可以修改宣告。 宣告建置到聯盟資源合作夥伴組織的伺服器來傳送預付碼。 聯盟伺服器資源夥伴中的收到為連入宣告宣告之後，它會執行執行一組篩選、通過，或轉換那些宣告理賠要求規則宣告發行引擎。 宣告然後建置到新傳送到 Web 資源合作夥伴伺服器的憑證。  
  
在網頁 SSO 設計 \（中只有一個組織是 involved\ AD FS 設計），以讓員工可以上一次登入和仍會存取其中多個應用程式可以使用單一聯盟伺服器。  
  
