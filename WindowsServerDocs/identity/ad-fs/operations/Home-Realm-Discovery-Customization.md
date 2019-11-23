---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: 主領域探索自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5a151e46e566d9f5459419771cbd476bb26c248d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357959"
---
# <a name="home-realm-discovery-customization"></a>主領域探索自訂


當 AD FS 用戶端第一次要求資源時，資源同盟伺服器沒有用戶端領域的相關資訊。 資源同盟伺服器會以 [**用戶端領域探索**] 頁面回應 AD FS 用戶端，而使用者會從清單中選取主領域。 清單值是由宣告提供者信任的顯示名稱屬性填入。 使用下列 Windows PowerShell Cmdlet 來修改和自訂 AD FS 主領域探索體驗。  
  
![主領域](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> 請注意本機 Active Directory 顯示的宣告提供者名稱是 Federation Service 顯示名稱。  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>設定識別提供者以使用特定電子郵件尾碼  
組織可以與多個宣告提供者同盟。 AD FS 現在提供\-box 功能，可讓系統管理員列出宣告提供者支援的尾碼，例如 @us.contoso.com、@eu.contoso.com，並針對尾碼\-為基礎的探索啟用它。 透過此設定，使用者可以輸入其組織帳戶，AD FS 會自動選取對應的宣告提供者。  
  
若要設定身分識別提供者 \(IDP\)（例如 `fabrikam`）使用特定電子郵件尾碼，請使用下列 Windows PowerShell Cmdlet 和語法。  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> 在兩個 AD FS 伺服器之間進行同盟時，請將宣告提供者信任上的 PromptLoginFederation 屬性設定為 ForwardPromptAndHintsOverWsFederation。  如此一來，AD FS 就會轉送 login_hint 並提示參數到 IDP。  這可以藉由執行下列 PowerShell Cmdlet 來完成：
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>根據信賴憑證者設定識別提供者清單。  
在某些情況下，組織可能會想讓使用者只看到應用程式特定的宣告提供者，如此在主領域探索頁面上只會顯示宣告提供者子集。  
  
若要設定每個信賴憑證者 \(RP\)的 IDP 清單，請使用下列 Windows PowerShell Cmdlet 和語法。  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>略過內部網路的主領域探索  
對於從防火牆內存取的使用者，大部分組織僅支援其本機 Active Directory。 在這些情況下，系統管理員可以設定 AD FS 略過內部網路的主領域探索。  
  
若要略過內部網路的 HRD，請使用下列 Windows PowerShell Cmdlet 和語法。  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> 請注意，如果已設定信賴憑證者的身分識別提供者清單，即使已啟用先前的設定且使用者從內部網路存取，AD FS 仍會顯示主領域探索 \(HRD\) 頁面。 若要在此情況下略過 HRD，您必須確定 "Active Directory" 也會新增到此信賴憑證者的 IDP 清單。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
