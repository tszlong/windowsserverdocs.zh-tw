---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: 主領域探索自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1198d8b76f2ecdad728e2de6ce7a5c0d053f779f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868929"
---
# <a name="home-realm-discovery-customization"></a>主領域探索自訂

>適用於：Windows Server 2016, Windows Server 2012 R2

當 AD FS 用戶端第一次要求的資源時，資源同盟伺服器會具有領域用戶端的任何資訊。 資源同盟伺服器會回應至 AD FS 用戶端**用戶端領域探索**頁面上，其中使用者從清單中選取主領域。 清單值是由宣告提供者信任的顯示名稱屬性填入。 您可以使用下列 Windows PowerShell cmdlet 來修改和自訂 AD FS 主領域探索體驗。  
  
![主領域](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> 請注意本機 Active Directory 顯示的宣告提供者名稱是 Federation Service 顯示名稱。  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>設定識別提供者以使用特定電子郵件尾碼  
組織可以與多個宣告提供者同盟。 AD FS 現在提供在\-方塊來列出的尾碼，例如，系統管理員的功能@us.contoso.com， @eu.contoso.com，也就是支援的宣告提供者並啟用供尾碼\-型探索。 此組態中，使用者可以輸入組織帳戶，與 AD FS 會自動選取對應的宣告提供者。  
  
若要設定的身分識別提供者\(IDP\)，例如`fabrikam`使用特定電子郵件尾碼，請使用下列 Windows PowerShell cmdlet 和語法。  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> 當兩個 AD FS 伺服器之間的同盟，PromptLoginFederation 屬性上設定 ForwardPromptAndHintsOverWsFederation 宣告提供者信任。  這是讓 AD FS 會轉送給 login_hint 和提示參數的 IDP。  這可以藉由執行下列 PowerShell cmdlet 來完成：
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>根據信賴憑證者設定識別提供者清單。  
在某些情況下，組織可能會想讓使用者只看到應用程式特定的宣告提供者，如此在主領域探索頁面上只會顯示宣告提供者子集。  
  
若要設定的 IDP 清單，每個信賴憑證者合作對象\(RP\)，使用下列 Windows PowerShell cmdlet 和語法。  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>略過內部網路的主領域探索  
對於從防火牆內存取的使用者，大部分組織僅支援其本機 Active Directory。 在這些情況下，系統管理員可以設定 AD FS，以略過內部網路的主領域探索。  
  
若要略過內部網路的 HRD，請使用下列 Windows PowerShell cmdlet 和語法。  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> 請注意，是否已設定信賴憑證者的合作對象的識別提供者清單，即使先前的設定已啟用，且使用者的存取是來自內部網路，而 AD FS 仍會顯示主領域探索\(HRD\)頁面。 若要在此情況下略過 HRD，您必須確定 "Active Directory" 也會新增到此信賴憑證者的 IDP 清單。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
