---
ms.assetid: 46725afe-8652-4cd7-928c-93b98f7fbae3
title: 建立沒有網域系統管理員許可權的 AD FS 伺服器陣列
description: 使用 AdfsFarm 指令程式和腳本，以使用委派的系統管理員認證來建立 AD FS 伺服器陣列
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/01/2020
ms.topic: article
ms.openlocfilehash: 7dff0b19b4d8783dcd43344c6152be9d2c36441d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972185"
---
# <a name="creating-an-ad-fs-farm-without-domain-admin-privileges"></a>建立沒有網域系統管理員許可權的 AD FS 伺服器陣列

>適用于： Windows Server 2019 和2016

## <a name="overview"></a>概觀
從 Windows Server 2016 中的 AD FS 開始，您可以在同盟伺服器上以本機系統管理員身分執行 Cmdlet AdfsFarm，前提是您的網域系統管理員已準備好 Active Directory。  本文中的下列腳本可用於準備 AD。  步驟如下：

1) 身為網域系統管理員，請 (執行腳本，或手動建立 Active Directory 物件和許可權) 。
2) 腳本會傳回 AdminConfiguration 物件，其中包含新建立之 AD 物件的 DN。
3) 在同盟伺服器上，請在以本機系統管理員身分登入時執行 AdfsFarm Cmdlet，並從上述 #2 傳遞物件作為 AdminConfiguration 參數

## <a name="assumptions"></a>假設
- Contoso\localadmin 是同盟伺服器上的非網域系統管理員內建管理員
- Contoso\FsSvcAcct 是將會是 AD FS 服務帳戶的網域帳戶
- Contoso\FsGmsaAcct $ 是 gMSA 帳戶，將會是 AD FS 服務帳戶
- $svcCred 是 AD FS 服務帳戶的認證
- $localAdminCred 是同盟伺服器上本機 (非 DA) 系統管理員帳戶的認證

## <a name="using-a-domain-account-as-ad-fs-service-account"></a>使用網域帳戶做為 AD FS 服務帳戶
### <a name="prepare-ad"></a>準備 AD
以網域系統管理員身分執行下列動作
```
PS:\>$adminConfig=(.\New-AdfsDkmContainer.ps1 -ServiceAccount contoso\fssvcacct -AdfsAdministratorAccount contoso\localadmin)
```
### <a name="sample-output"></a>範例輸出
```
$adminconfig.DkmContainerDN
CN=9530440c-bc84-4fe6-a3f9-8d60162a7bcf,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com
```
### <a name="create-the-ad-fs-farm"></a>建立 AD FS 伺服器陣列
在以本機系統管理員身分的同盟伺服器上，于提升許可權的 PowerShell 命令視窗中執行下列動作。

首先，如果同盟伺服器管理員未使用與上述網域系統管理員相同的 PowerShell 會話，請使用上述的輸出重新建立 Adminconfig.ps1 物件。
```
PS:\>$adminConfig = @{"DKMContainerDn"="CN=9530440c-bc84-4fe6-a3f9-8d60162a7bcf,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com"}
```

接下來，建立伺服器陣列：
```
PS:\>$svcCred = (get-credential)
PS:\>$localAdminCred = (get-credential)
PS:\>Install-AdfsFarm -CertificateThumbprint 270D041785C579D75C1C981DA0F9C36ECFDB65E0 -FederationServiceName "fs.contoso.com" -ServiceAccountCredential $svcCred -Credential $localAdminCred -OverwriteConfiguration -AdminConfiguration $adminConfig -Verbose
```
## <a name="using-a-gmsa-as-the-ad-fs-service-account"></a>使用 gMSA 做為 AD FS 服務帳戶
### <a name="prepare-ad"></a>準備 AD
```
PS:\>$adminConfig=(.\New-AdfsDkmContainer.ps1 -ServiceAccount contoso\FsGmsaAcct$ -AdfsAdministratorAccount contoso\localadmin)
```

### <a name="sample-output"></a>範例輸出
```
$adminconfig.DkmContainerDN
CN=8065f653-af9d-42ff-aec8-56e02be4d5f3,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com
```

### <a name="create-the-ad-fs-farm"></a>建立 AD FS 伺服器陣列
在以本機系統管理員身分的同盟伺服器上，于提升許可權的 PowerShell 命令視窗中執行下列動作。

首先，如果同盟伺服器管理員未使用與上述網域系統管理員相同的 PowerShell 會話，請使用上述的輸出重新建立 Adminconfig.ps1 物件。
```
PS:\>$adminConfig = @{"DKMContainerDn"="CN=8065f653-af9d-42ff-aec8-56e02be4d5f3,CN=ADFS,CN=Microsoft,CN=Program Data,DC=contoso,DC=com"}
```

接下來，建立伺服器陣列：請注意，必須將本機電腦帳戶和 ADFS 系統管理員帳戶授與 gMSA 的「取得密碼」和「委派」給帳戶許可權。
```
PS:\>$localadminobj = get-aduser "localadmin"
PS:\>$adfsnodecomputeracct = get-adcomputer "contoso_adfs_node"
PS:\>Set-ADServiceAccount -Identity fsgmsaacct -PrincipalsAllowedToRetrieveManagedPassword @( add=$localadmin.sid.value, $computeracct.sid.value) -PrincipalsAllowedToDelegateToAccount @( add=$localadmin.sid.value, $computeracct.sid.value)
PS:\>$localAdminCred = (get-credential)
PS:\>Install-AdfsFarm -CertificateThumbprint 270D041785C579D75C1C981DA0F9C36ECFDB65E0 -FederationServiceName "fs.contoso.com" -Credential $localAdminCred -GroupServiceAccountIdentifier "contoso\fsgmsaacct$" -OverwriteConfiguration -AdminConfiguration $adminConfig
```

## <a name="script-for-preparing-ad"></a>準備 AD 的腳本
下列 PowerShell 腳本可以用來完成上述範例

```
[CmdletBinding(SupportsShouldProcess=$true)]
param (
   [Parameter(Mandatory=$True)]
   [string]$ServiceAccount,
   [Parameter(Mandatory=$True)]
   [string]$AdfsAdministratorAccount
)

$ServiceAccountSplit = $ServiceAccount.Split("\");
if ($ServiceAccountSplit.Length -ne 2)
{
    Write-error "Specify the ServiceAccount identifier in 'domain\username' format"
    exit 1
}

$AdfsAdministratorAccountSplit = $AdfsAdministratorAccount.Split("\");
if ($AdfsAdministratorAccountSplit.Length -ne 2)
{
    Write-error "Specify the AdfsAdministratorAccount identifier in 'domain\username' format"
    exit 1
}

#######################################
## Verify AD module is installed
#######################################
$m = "ActiveDirectory"
if (Get-Module | Where-Object {$_.Name -eq $m})
{
    write-verbose "Module $m is already imported."
}
else
{
    if (Get-Module -ListAvailable | Where-Object {$_.Name -eq $m})
    {
        Import-Module $m -Verbose
    }
    else
    {
        write-error "Module $m was not imported, install the Active Directory RSAT package and retry."
        exit 1
    }
}

push-location ad:

#######################################
## Generate random DKM container name
## The OU Name is a randomly generated Guid
#######################################
[string]$guid = [Guid]::NewGuid()
write-verbose ("OU Name" + $guid)

$ouName = $guid
$initialPath = "CN=Microsoft,CN=Program Data," + (Get-ADDomain).DistinguishedName
$ouPath = "CN=ADFS," + $initialPath
$ou = "CN=" + $ouName + "," + $ouPath

#######################################
## Create DKM container and assign default ACE which allows adfs admin read access
#######################################

if ($pscmdlet.ShouldProcess("$ou", "Creating DKM container and assinging access"))
{
    Write-Verbose ("Creating organizational unit with DN: " + $ou)

    if ($AdfsAdministratorAccount.EndsWith("$"))
    {
        write-verbose "ADFS administrator account passed with $ suffix indicating a computer account"
        $userNameSplit = $AdfsAdministratorAccount.Split("\");
        $strSID = (Get-ADServiceAccount -Identity $userNameSplit[1]).SID
    }
    else
    {
        write-verbose "ADFS administrator account is a standard AD user"
        $objUser = New-Object System.Security.Principal.NTAccount($AdfsAdministratorAccount)
        $strSID = $objUser.Translate([System.Security.Principal.SecurityIdentifier])
    }

    if ($null -eq (Get-ADObject -Filter {distinguishedName -eq $ouPath}))
    {
        Write-Verbose ("First creating initial path " + $ouPath)
        New-ADObject -Name "ADFS" -Type Container -Path $initialPath
    }

    $acl = get-acl -Path $ouPath
    [System.DirectoryServices.ActiveDirectorySecurityInheritance]$adSecInEnum = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::All
    $ace1 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"GenericRead","Allow",$adSecInEnum

    $acl.AddAccessRule($ace1)
    set-acl -Path $ouPath -AclObject $acl

    New-ADObject -Name $ouName -Type Container -Path $ouPath
}


#######################################
## Grant the following permission to the service account
# Read
# Create Child
# Write Owner
# Delete Tree
# Write DACL
# Write Property
#######################################
if ($ServiceAccount.EndsWith("$"))
{
    write-verbose "service account passed with $ suffix indicating a gMSA"
    $userNameSplit = $ServiceAccount.Split("\");
    $strSID = (Get-ADServiceAccount -Identity $userNameSplit[1]).SID
}
else
{
    write-verbose "service account is a standard AD user"
    $objUser = New-Object System.Security.Principal.NTAccount($ServiceAccount)
    $strSID = $objUser.Translate([System.Security.Principal.SecurityIdentifier])
}

if ($pscmdlet.ShouldProcess("$strSID", "Granting GenericRead, CreateChild, WriteOwner, DeleteTree, WriteDacl and WriteProperty"))
{
    [System.DirectoryServices.ActiveDirectorySecurityInheritance]$adSecInEnum = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::All
    $ace1 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"GenericRead","Allow",$adSecInEnum
    $ace2 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"CreateChild","Allow",$adSecInEnum
    $ace3 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteOwner","Allow",$adSecInEnum
    $ace4 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"DeleteTree","Allow",$adSecInEnum
    $ace5 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteDacl","Allow",$adSecInEnum
    $ace6 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteProperty","Allow",$adSecInEnum

    $acl = get-acl -Path $ou

    $acl.AddAccessRule($ace1)
    $acl.AddAccessRule($ace2)
    $acl.AddAccessRule($ace3)
    $acl.AddAccessRule($ace4)
    $acl.AddAccessRule($ace5)
    $acl.AddAccessRule($ace6)

    $acl.SetOwner($strSID)

    set-acl -Path $ou -AclObject $acl
}

#######################################
## Grant the following permission to the adfs admin account
# Read
# Create Child
# Write Owner
# Delete Tree
# Write DACL
# Write Property
#######################################

if ($AdfsAdministratorAccount.EndsWith("$"))
{
    write-verbose "ADFS administrator account passed with $ suffix indicating a gMSA"
    $userNameSplit = $AdfsAdministratorAccount.Split("\");
    $strSID = (Get-ADServiceAccount -Identity $userNameSplit[1]).SID
}
else
{
    write-verbose "ADFS administrator account is a standard AD user"
    $objUser = New-Object System.Security.Principal.NTAccount($AdfsAdministratorAccount)
    $strSID = $objUser.Translate([System.Security.Principal.SecurityIdentifier])
}

if ($pscmdlet.ShouldProcess("$strSID", "Granting GenericRead, CreateChild, WriteOwner, DeleteTree, WriteDacl and WriteProperty"))
{
    [System.DirectoryServices.ActiveDirectorySecurityInheritance]$adSecInEnum = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::All
    $ace1 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"GenericRead","Allow",$adSecInEnum
    $ace2 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"CreateChild","Allow",$adSecInEnum
    $ace3 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteOwner","Allow",$adSecInEnum
    $ace4 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"DeleteTree","Allow",$adSecInEnum
    $ace5 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteDacl","Allow",$adSecInEnum
    $ace6 = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $strSID,"WriteProperty","Allow",$adSecInEnum

    $acl = get-acl -Path $ou

    $acl.AddAccessRule($ace1)
    $acl.AddAccessRule($ace2)
    $acl.AddAccessRule($ace3)
    $acl.AddAccessRule($ace4)
    $acl.AddAccessRule($ace5)
    $acl.AddAccessRule($ace6)

    $acl.SetOwner($strSID)

    set-acl -Path $ou -AclObject $acl

    $adminConfig = @{"DKMContainerDn"=$ou}

    Write-Output $adminConfig
}

pop-location

```
