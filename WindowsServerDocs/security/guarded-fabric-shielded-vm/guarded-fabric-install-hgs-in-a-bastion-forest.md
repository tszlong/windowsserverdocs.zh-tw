---
title: 在現有的防禦樹系中安裝 HGS
description: 深入瞭解：在現有的防禦樹系中安裝 HGS
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 1d25127b974c6b379e4fd52433ea74252801c166
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047276"
---
# <a name="install-hgs-in-an-existing-bastion-forest"></a>在現有的防禦樹系中安裝 HGS

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016


## <a name="join-the-hgs-server-to-the-existing-domain"></a>將 HGS 伺服器加入現有的網域

在現有的防禦樹系中，必須將 HGS 新增至根域。 使用伺服器管理員或 [新增電腦](https://go.microsoft.com/fwlink/?LinkId=821564) ，將 HGS 伺服器加入根域。

## <a name="add-the-hgs-server-role"></a>新增 HGS 伺服器角色

在提高許可權的 PowerShell 會話中執行本主題中的所有命令。

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)]

如果您的資料中心具有安全的防禦樹系，而您想要在其中加入 HGS 節點，請遵循下列步驟。
您也可以使用這些步驟來設定2個或更多個加入相同網域的獨立 HGS 叢集。

## <a name="join-the-hgs-server-to-the-existing-domain"></a>將 HGS 伺服器加入現有的網域

使用伺服器管理員或 [新增電腦](https://go.microsoft.com/fwlink/?LinkId=821564) ，將 HGS 伺服器加入至所需的網域。

## <a name="prepare-active-directory-objects"></a>準備 Active Directory 物件

建立群組受管理的服務帳戶和2個安全性群組。
如果您要初始化 HGS 的帳戶沒有在網域中建立電腦物件的許可權，您也可以預先設置叢集物件。

## <a name="group-managed-service-account"></a>群組受控服務帳戶

群組受管理的服務帳戶 (gMSA) 是 HGS 用來取出和使用其憑證的身分識別。 使用 [uninstall-adserviceaccount](/powershell/module/addsadministration/new-adserviceaccount) 建立 gMSA。
如果這是網域中的第一個 gMSA，您將需要新增金鑰發佈服務根金鑰。

每個 HGS 節點都必須允許存取 gMSA 密碼。
設定此設定的最簡單方式是建立包含所有 HGS 節點的安全性群組，並授與該安全性群組存取權以抓取 gMSA 密碼。

您必須在將 HGS 伺服器新增至安全性群組之後，將它重新開機，以確保它會取得新的群組成員資格。

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

GMSA 需要在每部 HGS 伺服器的安全性記錄中產生事件的許可權。
如果您使用群組原則設定使用者權限指派，請確定 gMSA 帳戶已獲得 HGS 伺服器上的「 [產生 audit 事件」許可權](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn221956%28v=ws.11%29) 。

> [!NOTE]
> 從 Windows Server 2012 Active Directory 架構開始，可以使用群組受管理的服務帳戶。
> 如需詳細資訊，請參閱 [群組受管理的服務帳戶需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj128431(v=ws.11))。

## <a name="jea-security-groups"></a>JEA 安全性群組

當您設定 HGS 時，系統會將足夠的系統 [管理 (JEA) ](/powershell/scripting/learn/remoting/jea/overview) PowerShell 端點設定為允許系統管理員管理 HGS，而不需要完整的本機系統管理員許可權。
您不一定要使用 JEA 來管理 HGS，但它仍然必須在執行 Initialize-HgsServer 時進行設定。
JEA 端點的設定包含指定包含 HGS 系統管理員和 HGS 審核者的2個安全性群組。
屬於系統管理員群組的使用者可以新增、變更或移除 HGS 上的原則;審核者只能查看目前的設定。

使用 Active Directory 管理工具或 [new-adgroup](/powershell/module/addsadministration/new-adgroup)，為這些 JEA 群組建立2個安全性群組。

```powershell
New-ADGroup -Name 'HgsJeaReviewers' -GroupScope DomainLocal
New-ADGroup -Name 'HgsJeaAdmins' -GroupScope DomainLocal
```

## <a name="cluster-objects"></a>叢集物件

如果您用來設定 HGS 的帳戶沒有在網域中建立新電腦物件的許可權，您將需要預先設置叢集物件。
這些步驟會在 Active Directory Domain Services 的預先設置叢集 [電腦物件](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466519(v=ws.11))中說明。

若要設定您的第一個 HGS 節點，您將需要建立一個叢集名稱物件 (CNO) 和一個 (VCO) 的虛擬電腦物件。
CNO 代表叢集的名稱，主要是由容錯移轉叢集在內部使用。
VCO 代表位於叢集頂端的 HGS 服務，而且將會是在 DNS 伺服器中註冊的名稱。

> [!IMPORTANT]
> 將執行的使用者 `Initialize-HgsServer` 需要 Active Directory 中的 CNO 和 VCO 物件的 **完整控制權** 。

若要快速預先設置 CNO 和 VCO，請 Active Directory 系統管理員執行下列 PowerShell 命令：

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

如果您要將 HGS 部署到高度鎖定的環境中，某些群組原則設定可能會導致 HGS 無法正常運作。
檢查您的群組原則物件中是否有下列設定，並在受影響時遵循指導方針：

### <a name="network-logon"></a>網路登入

**原則路徑：** 電腦設定 \Windows 設定 \ 安全性設定 \ 本機 \ 授權許可權指派

**原則名稱：** 拒絕從網路存取這部電腦

**必要值：** 請確定此值不會封鎖所有本機帳戶的網路登入。 不過，您可以安全地封鎖本機系統管理員帳戶。

**原因：** 容錯移轉叢集依賴稱為 CLIUSR 的非系統管理員本機帳戶來管理叢集節點。 封鎖此使用者的網路登入，將會導致叢集無法正常運作。

### <a name="kerberos-encryption"></a>Kerberos 加密

**原則路徑：** 電腦設定 \Windows 設定 \ 安全性設定 \ 本機原則選項

**原則名稱：** 網路安全性：設定 Kerberos 允許的加密類型

**動作**：如果已設定此原則，您必須將 gMSA 帳戶更新為 [uninstall-adserviceaccount](/powershell/module/addsadministration/set-adserviceaccount) ，才能只在此原則中使用支援的加密類型。 比方說，如果您的原則只允許 AES128 \_ hmac \_ SHA1 和 AES256 \_ hmac \_ sha1，您應該執行 `Set-ADServiceAccount -Identity HGSgMSA -KerberosEncryptionType AES128,AES256` 。



## <a name="next-steps"></a>後續步驟

- 如需設定以 TPM 為基礎的證明的後續步驟，請參閱 [在現有的防禦樹系中使用 TPM 模式初始化 HGS](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)叢集。
- 如需設定主機金鑰證明的後續步驟，請參閱 [使用現有的防禦樹系中的金鑰模式初始化 HGS](guarded-fabric-initialize-hgs-key-mode-bastion.md)叢集。
- 如需設定以系統管理員為基礎的證明 (在 Windows Server 2019) 中淘汰的後續步驟，請參閱 [在現有的防禦樹系中使用 AD 模式初始化 HGS](guarded-fabric-initialize-hgs-ad-mode-bastion.md)叢集。
