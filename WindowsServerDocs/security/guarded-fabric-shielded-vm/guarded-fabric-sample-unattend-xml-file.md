---
title: 建立 OS 特殊化回應檔案
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 299aa38e-28d2-4cbe-af16-5b8c533eba1f
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1d9e91ec8f4c998f34e324b5d551a387eba5a310
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823629"
---
# <a name="create-os-specialization-answer-file"></a>建立 OS 特殊化回應檔案

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

在準備来部署受防護的 Vm 時，您可能需要建立的作業系統特製化的回應檔案。 在 Windows，這通常稱為 「 unattend.xml"檔案。 **新增 ShieldingDataAnswerFile** Windows PowerShell 函式可協助您執行這項操作。 接著，您可以使用回應檔案，當您建立受防護的 Vm 範本中使用 System Center Virtual Machine Manager （或任何其他的網狀架構控制器）。

如需受防護的 Vm 的自動安裝檔案的一般指導方針，請參閱 <<c0> [ 建立回應檔案](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)。
 
## <a name="downloading-the-new-shieldingdataanswerfile-function"></a>下載新 ShieldingDataAnswerFile 函式

您可以取得**新增 ShieldingDataAnswerFile**函式[PowerShell 資源庫](https://aka.ms/gftools)。 如果您的電腦有網際網路連線，您可以從 PowerShell 安裝使用下列命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

`unattend.xml`輸出可以封裝到虛擬機器防護資料，以及其他構件，以便它可以用來從範本建立受防護的 Vm。

下列各節將示範如何使用的函式參數`unattend.xml`檔案，其中包含各種不同的選項：

- [基本 Windows 回應檔案](#basic-windows-answer-file)
- [Windows 回應檔案以加入網域](#windows-answer-file-with-domain-join)
- [Windows 回應檔案，以及將靜態 IPv4 位址](#windows-answer-file-with-static-ipv4-addresses)
- [使用自訂地區設定的 Windows 回應檔案](#windows-answer-file-with-custom-locale)
- [基本的 Linux 回應檔案](#basic-linux-answer-file)

您也可以檢閱[函式參數](#function-parameters)稍後在本主題中。

## <a name="basic-windows-answer-file"></a>基本 Windows 回應檔案

下列命令會建立只需設定系統管理員帳戶密碼和主機名稱的 Windows 回應檔案。
VM 網路介面卡會使用 DHCP 取得 IP 位址和 VM 不會加入至 Active Directory 網域。
當系統提示您輸入系統管理員認證時，指定所需的使用者名稱和密碼。
如果您想要設定的內建的系統管理員帳戶，請使用 「 系統管理員 」 使用者名稱。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred
```

## <a name="windows-answer-file-with-domain-join"></a>Windows 回應檔案以加入網域

下列命令會建立受防護的 VM 加入 Active Directory 網域的 Windows 回應檔案。
VM 網路介面卡會使用 DHCP 取得 IP 位址。

第一次認證提示會要求本機系統管理員帳戶資訊。
如果您想要設定的內建的系統管理員帳戶，請使用 「 系統管理員 」 使用者名稱。

第二個認證提示會要求提供有權將電腦加入 Active Directory 網域的認證。

請務必變更的值"-DomainName"參數，以您的 Active Directory 網域的 FQDN。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"
$domainCred = Get-Credential -Prompt "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -DomainName 'my.contoso.com' -DomainJoinCredentials $domainCred
```
## <a name="windows-answer-file-with-static-ipv4-addresses"></a>Windows 回應檔案，以及將靜態 IPv4 位址

下列命令會建立網狀架構管理員，例如 System Center Virtual Machine Manager 在部署期間提供的靜態 IP 位址的 Windows 回應檔案。

Virtual Machine Manager 使用的 IP 集區，以提供靜態 IP 位址的三個元件：IPv4 位址、 IPv6 位址、 閘道位址和 DNS 位址。 如果您想要包含或需要自訂網路設定的任何其他欄位時，您必須手動編輯回應檔案所產生的指令碼。

下列螢幕擷取畫面顯示的 IP 集區，您可以設定在 Virtual Machine Manager。 這些集區是如果您想要使用靜態 IP。

目前，此函式支援只有一部 DNS 伺服器。 以下是您的 DNS 設定會如下所示：

![使用靜態 IP 集區設定 DNS 伺服器](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-dns-settings.png)

以下是您建立靜態 IP 位址集區的摘要會如下所示。 簡單地說，您必須只有一個網路路由、 一個閘道，以及一部 DNS 伺服器-，而且您必須指定您的 IP 位址。

![建立靜態 IP 集區的摘要](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-summary.png)

您需要設定虛擬機器的網路介面卡。 下列螢幕擷取畫面顯示如何設定該組態，以及如何改為使用靜態 IP。

![設定為使用靜態 IP 的硬體](../media/Guarded-Fabric-Shielded-VM/guarded-host-unattend-static-ip-address-pool-network-adapter-settings.png)

然後，您可以使用`-StaticIPPool`參數，以回應檔案中包含靜態的 IP 項目。 參數`@IPAddr-1@`， `@NextHop-1-1@`，和`@DNSAddr-1-1@`答案檔案將會取代您在部署期間指定 Virtual Machine Manager 中的實際值。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -StaticIPPool IPv4Address
```

## <a name="windows-answer-file-with-a-custom-locale"></a>使用自訂地區設定的 Windows 回應檔案

下列命令會建立 Windows 回應檔案的自訂地區設定。

當系統提示您輸入系統管理員認證時，指定所需的使用者名稱和密碼。
如果您想要設定的內建的系統管理員帳戶，請使用 「 系統管理員 」 使用者名稱。

```powershell
$adminCred = Get-Credential -Prompt "Local administrator account"
$domainCred = Get-Credential -Prompt "Domain join credentials"

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -AdminCredentials $adminCred -Locale es-ES
```

## <a name="basic-linux-answer-file"></a>基本的 Linux 回應檔案

從 Windows Server 1709 版開始，您可以在受防護的 Vm 中執行特定的 Linux 客體 Os。
如果您使用 System Center Virtual Machine Manager Linux 代理程式特製化這些 Vm，新增 ShieldingDataAnswerFile 指令程式可以為其建立相容的回應檔案。

在 Linux 回應檔案中，您通常會包含根密碼、 根 SSH 金鑰，並選擇性地靜態 IP 集區資訊。
請先執行下列指令碼取代您的 SSH 金鑰的公開部分的路徑。

```powershell
$rootPassword = Read-Host -Prompt "Root password" -AsSecureString

New-ShieldingDataAnswerFile -Path '.\ShieldedVMAnswerFile.xml' -RootPassword $rootPassword -RootSshKey '~\.ssh\id_rsa.pub'
```

## <a name="see-also"></a>另請參閱

- [部署受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
