---
description: 深入瞭解：建立 OS 特製化回應檔案
title: 建立 OS 特殊化回應檔案
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 3bc7149bd8129c5fcac7d683b8327cfa5959791b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043926"
---
# <a name="create-os-specialization-answer-file"></a>建立 OS 特殊化回應檔案

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

若要準備部署受防護的 Vm，您可能需要建立作業系統特製化回應檔案。 在 Windows 中，這通常稱為「unattend.xml」檔案。 **ShieldingDataAnswerFile** Windows PowerShell 函數可協助您這樣做。 然後，當您使用 System Center Virtual Machine Manager (或任何其他網狀架構控制器) ，從範本建立受防護的 Vm 時，您就可以使用回應檔案。

如需受防護 Vm 的自動安裝檔案的一般指導方針，請參閱 [建立回應](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)檔案。

## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>下載 New-ShieldingDataAnswerFile 函數

您可以從 [PowerShell 資源庫](https://aka.ms/gftools)取得 **新的 ShieldingDataAnswerFile** 函數。 如果您的電腦具有網際網路連線能力，您可以使用下列命令從 PowerShell 進行安裝：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

`unattend.xml`輸出可以封裝至防護資料，以及額外的構件，以便用來從範本建立受防護的 vm。

下列各節會示範如何針對包含各種選項的檔案使用函式參數 `unattend.xml` ：

- [基本 Windows 回應檔案](#basic-windows-answer-file)
- [具有加入網域的 Windows 回應檔案](#windows-answer-file-with-domain-join)
- [具有靜態 IPv4 位址的 Windows 回應檔案](#windows-answer-file-with-static-ipv4-addresses)
- [具有自訂地區設定的 Windows 回應檔案](#windows-answer-file-with-a-custom-locale)
- [基本 Linux 回應檔案](#basic-linux-answer-file)

## <a name="basic-windows-answer-file"></a>基本 Windows 回應檔案

下列命令會建立 Windows 回應檔案，該檔案只會設定系統管理員帳戶密碼和主機名稱。
VM 網路介面卡將會使用 DHCP 來取得 IP 位址，而 VM 將不會加入 Active Directory 網域。
當系統提示您輸入系統管理員認證時，請指定所需的使用者名稱和密碼。
如果您想要設定內建的系統管理員帳戶，請使用 "Administrator" 作為使用者名稱。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>具有加入網域的 Windows 回應檔案

下列命令會建立 Windows 回應檔案，以將受防護的 VM 加入至 Active Directory 網域。
VM 網路介面卡將會使用 DHCP 來取得 IP 位址。

第一個認證提示會要求本機系統管理員帳戶資訊。
如果您想要設定內建的系統管理員帳戶，請使用 "Administrator" 作為使用者名稱。

第二個認證提示會要求有權將電腦加入 Active Directory 網域的認證。

請務必將 "-DomainName" 參數的值變更為您 Active Directory 網域的 FQDN。

```powershell
$adminCred = Get-Credential -Message "Local administrator account"
$domainCred = Get-Credential -Message "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>具有靜態 IPv4 位址的 Windows 回應檔案

下列命令會建立 Windows 回應檔案，該檔案會使用網狀架構管理員在部署期間提供的靜態 IP 位址，例如 System Center Virtual Machine Manager。

Virtual Machine Manager 使用 IP 集區提供三個元件給靜態 IP 位址： IPv4 位址、IPv6 位址、閘道位址和 DNS 位址。 如果您想要包含任何其他欄位或需要自訂網路設定，則必須手動編輯腳本所產生的回應檔案。

下列螢幕擷取畫面顯示您可以在 Virtual Machine Manager 中設定的 IP 集區。 如果您想要使用靜態 IP，則需要這些集區。

目前，此函數僅支援一部 DNS 伺服器。 以下是您的 DNS 設定看起來的樣子：

![使用靜態 IP 集區設定 DNS 伺服器](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

建立靜態 IP 位址池的摘要如下所示。 簡單地說，您必須只有一個網路路由、一個閘道和一部 DNS 伺服器，而且您必須指定 IP 位址。

![靜態 IP 集區建立的摘要](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

您必須設定虛擬機器的網路介面卡。 下列螢幕擷取畫面顯示設定該設定的位置，以及如何將其切換為靜態 IP。

![設定硬體以使用靜態 IP](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

然後，您可以使用參數，將  `-StaticIPPool` 靜態 IP 元素包含在回應檔案中。 然後，回應檔案中的參數 `@IPAddr-1@` 、 `@NextHop-1-1@` 和 `@DNSAddr-1-1@` 會以您在部署時 Virtual Machine Manager 中指定的實際值取代。

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
如果您使用 System Center Virtual Machine Manager Linux 代理程式來特製化這些 Vm，則 New-ShieldingDataAnswerFile Cmdlet 可以為其建立相容的回應檔案。

在 Linux 回應檔案中，您通常會包含根密碼、根 SSH 金鑰，以及選擇性的靜態 IP 集區資訊。
執行下列腳本之前，請先取代 SSH 金鑰的公開半形路徑。

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="additional-references"></a>其他參考資料

- [部署受防護的 VM](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)
