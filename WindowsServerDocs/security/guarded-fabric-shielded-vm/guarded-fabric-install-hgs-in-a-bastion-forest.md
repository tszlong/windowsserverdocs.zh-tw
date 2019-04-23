---
title: 在 現有的防禦樹系中安裝 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 147610d9dcb36dfedab3aca11ee1a64731715519
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842219"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>在 現有的防禦樹系中安裝 HGS 

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>HGS 伺服器加入現有的網域

在現有的防禦樹系 HGS 必須新增至根網域。 使用 伺服器管理員或[Add-computer](https://go.microsoft.com/fwlink/?LinkId=821564) HGS 伺服器加入根網域。

## <a name="add-the-hgs-server-role"></a>新增 HGS 伺服器角色

執行本主題中的所有命令，在提升權限的 PowerShell 工作階段。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

如果您的資料中心有您要加入 HGS 節點的安全防禦樹系，請遵循下列步驟。
您也可以使用下列步驟，設定 2 個或更獨立的 HGS 叢集加入相同的網域。

## <a name="join-the-hgs-server-to-the-existing-domain"></a>HGS 伺服器加入現有的網域

使用 伺服器管理員或[Add-computer](https://go.microsoft.com/fwlink/?LinkId=821564) HGS 伺服器加入所需的網域。

## <a name="prepare-active-directory-objects"></a>準備 Active Directory 物件

建立群組受控服務帳戶和 2 個安全性群組。
您可以也預先設置叢集物件如果您要初始化 HGS 使用的帳戶沒有在網域中建立電腦物件的權限。

## <a name="group-managed-service-account"></a>群組受控服務帳戶

群組受控服務帳戶 (gMSA) 是由 HGS 用來擷取並使用其憑證的身分識別。 使用[New-adserviceaccount](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adserviceaccount)才能建立 gMSA。
如果這是第一個網域中的 gMSA，您必須將金鑰發佈服務根金鑰。

每一個 HGS 節點必須能夠存取 gMSA 密碼。
最簡單的方式進行此設定是要建立的安全性群組，其中包含所有您的 HGS 節點，並授與該安全性群組存取權，以擷取 gMSA 密碼。

您必須重新啟動您的 HGS 伺服器之後將它新增至安全性群組，以確保它會取得新的群組成員資格。

```powershell
# Check if the KDS root key has been set up
if (-not (Get-KdsRootKey)) {
    # Adds a KDS root key effective immediately (ignores normal 10 hour waiting period)
    Add-KdsRootKey -EffectiveTime ((Get-Date).AddHours(-10))
}

# Create a security group for HGS nodes
$hgsNodes = New-ADGroup -Name 'HgsServers' -GroupScope DomainLocal -PassThru

# Add your HGS nodes to this group
# If your HGS server object is under an organizational unit, provide the full distinguished name instead of "HGS01"
Add-ADGroupMember -Identity $hgsNodes -Members "HGS01"

# Create the gMSA
New-ADServiceAccount -Name 'HGSgMSA' -DnsHostName 'HGSgMSA.yourdomain.com' -PrincipalsAllowedToRetrieveManagedPassword $hgsNodes
```

GMSA 將需要在每一部 HGS 伺服器的安全性記錄中產生事件的權限。
如果您使用群組原則來設定使用者權限指派時，請確定會授與 gMSA 帳戶[產生稽核事件的特殊權限](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29)HGS 伺服器上。

> [!NOTE]
> 群組受管理的服務帳戶是 Windows Server 2012 Active Directory 結構描述的版本開始。
> 如需詳細資訊，請參閱 <<c0> [ 群組受控服務帳戶需求](https://technet.microsoft.com/library/jj128431.aspx)。

## <a name="jea-security-groups"></a>JEA 安全性群組

當您設定 HGS [Just Enough Administration (JEA)](https://aka.ms/JEAdocs) PowerShell 端點設定為允許系統管理員管理 HGS，而不需要完整的本機系統管理員權限。
您不需要管理 HGS 使用 JEA，但它仍必須設定執行初始化 HgsServer 時。
JEA 端點的組態包含指定 2 個安全性群組，其中包含您的 HGS 系統管理員和 HGS 檢閱者。
屬於系統管理員群組的使用者可以新增、 變更或移除 HGS; 上的原則檢閱者只能檢視目前的組態。

建立使用 Active Directory 系統管理工具的這些 JEA 群組 2 個安全性群組或[New-adgroup](https://technet.microsoft.com/itpro/powershell/windows/addsadministration/new-adgroup)。

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>叢集物件

如果您要用來設定 HGS 帳戶並沒有在網域中建立新的電腦物件的權限，您必須預先設置叢集物件。
說明這些步驟[Active Directory 網域服務中的預先設置叢集電腦物件](https://technet.microsoft.com/library/dn466519(v=ws.11).aspx)。

若要設定您的第一個 HGS 節點，您必須建立一個叢集名稱物件 (CNO) 和一個虛擬電腦物件 (VCO)。
CNO 代表叢集的名稱，而且主要是在內部由容錯移轉叢集。
VCO 代表 HGS 服務位在叢集之上，且會註冊與 DNS 伺服器的名稱。

> [!IMPORTANT]
> 將執行的使用者`Initialize-HgsServer`需要**完全控制**CNO 和 VCO 物件的 Active Directory。

若要快速地預先設定您的 CNO 和 VCO，具有 Active Directory 系統管理員執行下列 PowerShell 命令：

```powershell
# Create the CNO
$cno = New-ADComputer -Name 'HgsCluster' -Description 'HGS CNO' -Enabled $false -Passthru

# Create the VCO
$vco = New-ADComputer -Name 'HgsService' -Description 'HGS VCO' -Passthru

# Give the CNO full control over the VCO
$vcoPath = Join-Path "AD:\" $vco.DistinguishedName
$acl = Get-Acl $vcoPath
$ace = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $cno.SID, "GenericAll", "Allow"
$acl.AddAccessRule($ace)
Set-Acl -Path $vcoPath -AclObject $acl

# Allow time for your new CNO and VCO to replicate to your other Domain Controllers before continuing
```

## <a name="security-baseline-exceptions"></a>安全性基準例外狀況

如果您要部署 HGS 到高鎖定的環境，特定的群組原則設定可能會阻止 HGS 正常運作。
檢查您的群組原則物件的下列設定，並遵循的指導方針，如果您受到影響：

### <a name="network-logon"></a>網路登入

**原則的路徑：** 電腦設定 \windows 設定 \ 安全性設定 \ 本機原則 \ 使用者權限指派

**原則名稱：** 拒絕從網路存取這台電腦

**必要的值：** 請確定值不會封鎖所有的本機帳戶的網路登入。 您可以不過安全地封鎖本機系統管理員帳戶。

**原因：** 容錯移轉叢集，依賴於非系統管理員的本機帳戶，稱為 CLIUSR 來管理叢集節點。 這位使用者的封鎖網路登入將會防止叢集運作正常。

### <a name="kerberos-encryption"></a>Kerberos 加密

**原則的路徑：** 電腦設定\Windows 設定\安全性設定\本機原則\安全性選項

**原則名稱：** 網路安全性：設定 Kerberos 允許的加密類型

**動作**:如果設定此原則，您必須更新的 gMSA 帳戶[Set-adserviceaccount](https://docs.microsoft.com/powershell/module/addsadministration/set-adserviceaccount?view=win10-ps)此原則中使用支援的加密類型。 比方說，如果您的原則只允許 AES128\_HMAC\_SHA1，AES256\_HMAC\_SHA1，您應該執行`Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256`。



## <a name="next-steps"></a>後續步驟

- 若要設定 TPM 型證明的下一個步驟，請參閱[初始化現有的防禦樹系中使用 TPM 模式 HGS 叢集](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)。
- 設定主機金鑰證明的下一個步驟，請參閱 <<c0> [ 初始化 HGS 叢集使用現有的防禦樹系中的索引鍵模式](guarded-fabric-initialize-hgs-key-mode-bastion.md)。
- 下一個步驟來設定 以系統管理員為基礎的證明，（在 Windows Server 2019 中已被取代），請參閱[初始化現有的防禦樹系中使用 AD 模式 HGS 叢集](guarded-fabric-initialize-hgs-ad-mode-bastion.md)。

