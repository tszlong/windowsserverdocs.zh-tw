---
title: 將 AD FS Powershell Cmdlet 存取權委派給非系統管理員使用者
description: 本檔 descirbes 如何將許可權委派給 AD FS PowerShell Cmdlet 的非系統管理員。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 485b36299727b25787b1ac46f77ef1222e01ad68
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960420"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>將 AD FS Powershell Cmdlet 存取權委派給非系統管理員使用者 
根據預設，透過 PowerShell 的 AD FS 管理只能由 AD FS 系統管理員完成。 對於許多大型組織而言，處理其他角色（例如技術支援人員）時，這可能不是可行的營運模式。  

只要有足夠的系統管理（JEA），客戶現在就可以將特定 commandlet 委派給不同的人員群組。  
此使用案例的一個良好範例，是讓技術支援人員在通過使用者時，查詢 AD FS 帳戶鎖定狀態，並在 AD FS 中重設帳戶鎖定狀態。 在此情況下，需要委派的 commandlet 如下： 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

我們會在本檔的其餘部分使用此範例。 不過，您可以自訂此功能，以允許委派設定信賴憑證者的屬性，並將此內容交給組織內的應用程式擁有者。  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>建立將許可權授與使用者所需的群組 
1. 建立[群組受管理的服務帳戶](../../../security/group-managed-service-accounts/group-managed-service-accounts-overview.md)。 GMSA 帳戶是用來允許 JEA 使用者以其他電腦或 web 服務的方式存取網路資源。 它會提供網域身分識別，可用來對網域內任何電腦上的資源進行驗證。 GMSA 帳戶會在稍後的安裝程式中被授與必要的系統管理許可權。 在此範例中，我們會呼叫帳戶**gMSAContoso**。 
2. 建立 Active Directory 群組可以填入需要授與委派命令之許可權的使用者。 在此範例中，技術支援人員會被授與讀取、更新及重設 ADFS 鎖定狀態的許可權。 在整個範例中，我們將此群組稱為**JEAContoso**。 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>在 ADFS 伺服器上安裝 gMSA 帳戶： 
建立具有 ADFS 伺服器系統管理許可權的服務帳戶。 只要已安裝 AD RSAT 套件，就可以在網域控制站或遠端執行此作業。服務帳戶必須建立在與 ADFS 伺服器相同的樹系中。 將範例值修改為伺服器陣列的設定。 

```powershell
 # This command should only be run if this is the first time gMSA accounts are enabled in the forest 
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))  
 
# Run this on every node that you want to have JEA configured on  
$adfsServer = Get-ADComputer server01.contoso.com  
 
# Run targeted at domain controller  
$serviceaccount = New-ADServiceAccount gMSAcontoso -DNSHostName <FQDN of the domain containing the KDS key> - PrincipalsAllowedToRetrieveManagedPassword $adfsServer –passthru 
 
# Run this on every node 
Add-ADComputerServiceAccount -Identity server01.contoso.com -ServiceAccount $ServiceAccount 
```

在 ADFS 伺服器上安裝 gMSA 帳戶。這必須在伺服器陣列中的每個 ADFS 節點上執行。 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>授與 gMSA 帳戶系統管理員許可權 
如果伺服器陣列使用委派系統管理，請將其新增至具有委派系統管理員存取權的現有群組，以授與 gMSA 帳戶系統管理員許可權。  
 
如果伺服器陣列不使用委派系統管理，請將其設為所有 ADFS 伺服器上的本機管理群組，以授與 gMSA 帳戶系統管理員許可權。 
 
 
### <a name="create-the-jea-role-file"></a>建立 JEA 角色檔案 
 
在 AD FS 伺服器上，于 [記事本] 檔案中建立 JEA 角色。 [JEA 角色功能](/powershell/jea/role-capabilities)提供建立角色的指示。 
 
在此範例中委派的 commandlet 是 `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity` 。 

委派「重設-ADFSAccountLockout」、「ADFSAccountActivity」和「ADFSAccountActivity」 commandlet 存取權的範例 JEA 角色：

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>建立 JEA 會話設定檔 
遵循指示來建立[JEA 會話配置](/powershell/jea/session-configurations)檔。 設定檔會決定誰可以使用 JEA 端點，以及他們有權存取的功能。 

角色功能是由角色功能檔案的一般名稱（不含副檔名的檔案名）所參考。 如果系統上有多個具有相同一般名稱的角色功能，則 PowerShell 會使用其隱含搜尋順序來選取有效的角色功能檔案。 它不會授與所有具有相同名稱的角色功能檔案的存取權。 

若要指定具有路徑的角色功能檔案，請使用 `RoleCapabilityFiles` 引數。 針對子資料夾，JEA 會尋找包含子資料夾的有效 Powershell 模組 `RoleCapabilities` ，其中 `RoleCapabilityFiles` 引數應修改為 `RoleCapabilities` 。 

會話設定檔範例： 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

儲存會話設定檔。 
 
如果您已使用文字編輯器手動編輯 .pssc 檔案，以確保語法正確，則強烈建議您[測試您的會話設定檔](/powershell/module/microsoft.powershell.core/test-pssessionconfigurationfile?view=powershell-5.1)。 如果會話設定檔案未通過此測試，就不會成功地在系統上註冊。  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>在 AD FS 伺服器上安裝 JEA 會話設定 

在 AD FS 伺服器上安裝 JEA 會話設定 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>操作指示 
設定好之後，如果正確的使用者有權存取 JEA 端點，就可以使用 JEA 記錄和審核來判斷。 

若要使用委派的命令： 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 


```
