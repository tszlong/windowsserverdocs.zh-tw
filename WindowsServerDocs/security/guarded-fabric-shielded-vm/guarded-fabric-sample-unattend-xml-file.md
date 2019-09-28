---
title: 建立 OS 特殊化回應檔案
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4920f9a90bd0190d390a9d35b3d265023d69efac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386500"
---
# <a name="create-os-specialization-answer-file"></a>建立 OS 特殊化回應檔案

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

在準備部署受防護的 Vm 時，您可能需要建立作業系統特製化回應檔案。 在 Windows 上，這通常稱為「unattend.xml」檔案。 ShieldingDataAnswerFile 的 Windows PowerShell 函**式**可協助您執行此動作。 然後，當您使用 System Center Virtual Machine Manager （或任何其他網狀架構控制器）從範本建立受防護的 Vm 時，就可以使用回應檔案。

如需受防護 Vm 之自動安裝檔案的一般指導方針，請參閱[建立回應](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)檔案。
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>下載 ShieldingDataAnswerFile 函數

您可以從[PowerShell 資源庫](https://aka.ms/gftools)取得**新的 ShieldingDataAnswerFile**函數。 如果您的電腦具有網際網路連線能力，您可以使用下列命令從 PowerShell 安裝它：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

@No__t 0 輸出可以封裝到防護資料中，以及其他成品，以便用來從範本建立受防護的 Vm。

下列各節顯示如何針對包含各種選項的 @no__t 0 檔案使用函數參數：

- [基本 Windows 回應檔案](#basic-windows-answer-file)
- [具有加入網域的 Windows 回應檔案](#windows-answer-file-with-domain-join)
- [具有靜態 IPv4 位址的 Windows 回應檔案](#windows-answer-file-with-static-ipv4-addresses)
- [具有自訂地區設定的 Windows 回應檔案](#windows-answer-file-with-a-custom-locale)
- [基本 Linux 回應檔案](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>基本 Windows 回應檔案

下列命令會建立只會設定系統管理員帳戶密碼和主機名稱的 Windows 回應檔案。
VM 網路介面卡將使用 DHCP 來取得 IP 位址，而 VM 將不會加入 Active Directory 網域。
當系統提示您輸入系統管理員認證時，請指定所需的使用者名稱和密碼。
如果您想要設定內建的系統管理員帳戶，請使用 "Administrator" 作為使用者名稱。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>具有加入網域的 Windows 回應檔案

下列命令會建立 Windows 回應檔案，該檔案會將受防護的 VM 加入 Active Directory 網域。
VM 網路介面卡將使用 DHCP 來取得 IP 位址。

第一個認證提示會要求您提供本機系統管理員帳戶資訊。
如果您想要設定內建的系統管理員帳戶，請使用 "Administrator" 作為使用者名稱。

第二個認證提示會要求有權將電腦加入 Active Directory 網域的認證。

請務必將 "-DomainName" 參數的值變更為 Active Directory 網域的 FQDN。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>具有靜態 IPv4 位址的 Windows 回應檔案

下列命令會建立 Windows 回應檔案，該檔案會使用網狀架構管理員在部署期間提供的靜態 IP 位址，例如 System Center Virtual Machine Manager。

Virtual Machine Manager 使用 IP 集區，將三個元件提供給靜態 IP 位址：IPv4 位址、IPv6 位址、閘道位址和 DNS 位址。 如果您想要包含任何其他欄位或需要自訂網路設定，您必須手動編輯腳本所產生的回應檔案。

下列螢幕擷取畫面顯示您可以在 Virtual Machine Manager 中設定的 IP 集區。 如果您想要使用靜態 IP，則需要這些集區。

目前，此函數只支援一部 DNS 伺服器。 您的 DNS 設定如下所示：

![設定具有靜態 IP 集區的 DNS 伺服器](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

建立靜態 IP 位址池的摘要如下所示。 簡言之，您必須只有一個網路路由、一個閘道和一個 DNS 伺服器，而且您必須指定您的 IP 位址。

![靜態 IP 集區建立的摘要](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

您需要為您的虛擬機器設定網路介面卡。 下列螢幕擷取畫面顯示設定該設定的位置，以及如何將它切換至靜態 IP。

![設定硬體以使用靜態 IP](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

然後，您可以使用 `-StaticIPPool` 參數，在回應檔案中包含靜態 IP 元素。 回應檔案中 `@IPAddr-1@`、`@NextHop-1-1@` 和 `@DNSAddr-1-1@` 的參數，將會取代為您在部署期間于 Virtual Machine Manager 中指定的實際值。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>具有自訂地區設定的 Windows 回應檔案

下列命令會建立具有自訂地區設定的 Windows 回應檔案。

當系統提示您輸入系統管理員認證時，請指定所需的使用者名稱和密碼。
如果您想要設定內建的系統管理員帳戶，請使用 "Administrator" 作為使用者名稱。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>基本 Linux 回應檔案

從 Windows Server 1709 版開始，您可以在受防護的 Vm 中執行某些 Linux 客體作業系統。
如果您使用 System Center Virtual Machine Manager Linux 代理程式來特殊化這些 Vm，ShieldingDataAnswerFile Cmdlet 可以為它建立相容的回應檔案。

在 Linux 回應檔案中，您通常會包含根密碼、根 SSH 金鑰，以及選擇性的靜態 IP 集區資訊。
執行下列腳本之前，請先取代 SSH 金鑰的公用部分路徑。

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>另請參閱

- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
