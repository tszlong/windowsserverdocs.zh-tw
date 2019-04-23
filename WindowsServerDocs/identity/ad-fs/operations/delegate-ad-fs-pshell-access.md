---
title: AD FS Powershell Commandlet 存取權委派給非系統管理員使用者
description: 此文件 descirbes 如何委派給非系統管理員的 AD FS PowerShell cmdlt 的權限。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 265d22b045011e34192e56bdb970955b601cda56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883559"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>AD FS Powershell Commandlet 存取權委派給非系統管理員使用者 
根據預設，AD FS 系統管理，透過 PowerShell 僅可透過 AD FS 系統管理員。 對於許多大型組織，這不可能是可行的營運模式處理其他人物代表，例如技術支援人員時。  

使用 Just Enough Administration (JEA)，客戶現在可以委派給不同的人員群組的特定指令程式。  
這個使用案例的理想範例允許查詢 AD FS 支援服務中心人員帳戶鎖定狀態，與重設帳戶鎖定狀態，在 AD FS 中的，一旦已驗證使用者。 在此情況下，想要委派的 commandlet 如下： 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

我們在本文的其餘部分中使用此範例。 不過，其中可以自訂這也允許設定的信賴憑證者的合作對象的屬性，以及這項功能交給組織內的應用程式擁有者的委派。  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>建立所需的群組授與使用者權限 
1. 建立[群組受控的服務帳戶](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。 GMSA 帳戶用來讓 JEA 使用者做為其他電腦或 web 服務存取網路資源。 它會提供可用來對網域內的任何電腦上的資源進行驗證的網域身分識別。 稍後在安裝程式所需的系統管理權限授與 gMSA 帳戶。 此範例中，我們呼叫帳戶**gMSAContoso**。 
2. 建立 Active Directory 群組可以填入要授與委派命令的權限的使用者。 在此範例中，支援服務中心人員有權讀取、 更新和重設的 ADFS 鎖定狀態。 我們將此群組的範例為整個**JEAContoso**。 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>在 ADFS 伺服器上安裝的 gMSA 帳戶： 
建立具有系統管理權限的 ADFS 伺服器的服務帳戶。 這可以是網域控制站上或遠端執行長 AD RSAT 套件安裝。  必須在 ADFS 伺服器相同的樹系中建立的服務帳戶。 修改您的伺服器陣列組態的範例值。 

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

在 ADFS 伺服器上安裝的 gMSA 帳戶。  這需要在伺服器陣列中的每個 ADFS 節點上執行。 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>授與 gMSA 帳戶系統管理員權限 
如果伺服器陣列使用委派的系統管理，授與 gMSA 帳戶系統管理員權限新增至現有的群組，其已委派系統管理員存取權。  
 
如果伺服器陣列不使用委派的系統管理，授與 gMSA 帳戶系統管理員權限，以便本機管理群組的所有 ADFS 伺服器上。 
 
 
### <a name="create-the-jea-role-file"></a>建立 JEA 角色檔案 
 
在 [記事本] 檔案中建立 JEA 角色。 若要建立角色的指示會提供[JEA 角色功能](https://docs.microsoft.com/powershell/jea/role-capabilities)。 
 
在此範例中所委派的 commandlet 是`Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`。 

範例 JEA 角色來委派存取權的 '重設 ADFSAccountLockout'、' Get-ADFSAccountActivity' 和 ' Set ADFSAccountActivity' cmdlet:

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>建立 JEA 工作階段設定檔 
請依照下列指示來建立[JEA 工作階段設定](https://docs.microsoft.com/powershell/jea/session-configurations)檔案。 組態檔會決定誰可以使用 JEA 端點，以及他們可以存取哪些功能。 

角色功能會參考角色功能檔案的一般名稱 （不含副檔名的檔名）。 如果多個角色功能具有相同的一般名稱的系統上可用，則 PowerShell 會使用其隱含搜尋順序來選取有效的角色功能檔案。 它並不授予所有具有相同名稱的角色功能檔案的存取。 

若要指定角色功能檔案的路徑，請使用`RoleCapabilityFiles`引數。 為子資料夾中，JEA 會尋找包含的有效 Powershell 模組`RoleCapabilities`子資料夾，其中`RoleCapabilityFiles`引數應該修改成`RoleCapabilities`。 

範例工作階段設定檔： 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

儲存工作階段設定檔。 
 
強烈建議[測試您的工作階段設定檔](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1)您已經手動編輯 pssc 檔案使用文字編輯器，以確定語法是否正確。 如果工作階段設定檔未通過此測試，它未成功註冊系統上。  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>安裝 AD FS 伺服器上的 JEA 工作階段設定 

安裝 AD FS 伺服器上的 JEA 工作階段設定 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>操作的指示 
設定好之後，記錄與稽核 JEA 可用來判斷如果正確的使用者可以存取 JEA 端點。 

若要使用的委派的命令： 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 
```
