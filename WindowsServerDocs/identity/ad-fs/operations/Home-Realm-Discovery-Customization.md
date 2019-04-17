---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: "家用領域探索自訂項目"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 328c313b2d5bf174f6e6af20cd91ac9620edac82
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="home-realm-discovery-customization"></a>家用領域探索自訂項目

>適用於：Windows Server 2016、Windows Server 2012 R2

AD FS client 第一次要求的資源時, 資源聯盟伺服器有領域 client 的任何資訊。 資源聯盟伺服器會 AD FS 使用**Client 領域探索**頁面上，使用者從清單選取家用領域的位置。 從宣告提供者信任的顯示名稱屬性填入清單值。 使用下列的 Windows PowerShell cmdlet 來修改及自訂 AD FS Home 領域探索體驗。  
  
![主要的領域](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> 請注意，會顯示為 [本機 Active Directory 宣告提供者名稱是同盟服務顯示名稱。  
  
## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>設定使用某些電子郵件尾碼身分提供者  
組織可以聯盟使用多個宣告提供者。 AD FS 現在提供 in\ 隨功能的系統管理員清單尾碼，例如， @us.contoso.com， @eu.contoso.com，也就是支援宣告提供者，並讓它 suffix\ 型探索。 使用此設定時，使用者可以輸入其組織帳號，並 AD FS 自動選取對應宣告提供者。  
  
若要設定的身分提供者 \(IDP\)，例如`fabrikam`、 來使用特定的電子郵件尾碼，請使用下列 Windows PowerShell cmdlet 和語法。  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
  
## <a name="configure-an-identity-provider-list-per-relying-party"></a>設定可以依據身分提供者清單派對  
某些案例中，針對組織可能會想讓使用者只會看見的特定應用程式，以便子集宣告提供者會顯示主要領域探索頁面上的宣告提供者。  
  
若要設定可以依據 IDP 清單廠商 \(RP\)，請使用下列的 Windows PowerShell cmdlet 和語法。  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>略過內部網路 Home 領域探索  
大部分的組織僅支援他們本機 Active Directory 中防火牆存取的使用者。 在這些案例中，系統管理員可以設定略過內部網路家用領域探索 AD FS。  
  
若要略過內部 HRD，使用下列的 Windows PowerShell cmdlet 和語法。  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> 請注意，如果致敬身分提供者清單廠商已設定，即使有已經支援先前的設定，並從內部網路的使用者存取權，AD FS 仍會顯示主要領域探索 \(HRD\) 頁面。 在這種情形下略過 HRD，您必須確保的 「 Active Directory 」 也會新增到清單 IDP 此信賴的。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
