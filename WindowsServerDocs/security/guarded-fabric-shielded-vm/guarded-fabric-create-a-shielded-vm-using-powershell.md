---
title: 建立受防護的 VM，使用 PowerShell
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 76be26e107bd16165367d5432e1dd757dea2f9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855409"
---
# <a name="create-a-shielded-vm-using-powershell"></a>建立受防護的 VM，使用 PowerShell

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

在生產環境中，您通常會使用網狀架構管理員 (例如 VMM) 部署受防護的 Vm。 不過，如下所示的步驟可讓您部署並驗證整個案例，而不需要網狀架構管理員。

簡單的說，您會建立範本磁碟、 防護資料檔案、 自動的安裝回應檔案和其他安全性成品，任何在電腦上，然後將這些檔案複製到受防護主機和佈建受防護的 VM。

## <a name="create-a-signed-template-disk"></a>建立已簽署的範本磁碟

若要建立新的受防護的 VM，您必須是其作業系統磁碟區 （或在 Linux 上的開機和根磁碟分割） 簽署與加密之前已受防護的 VM 的範本磁碟。
如何建立範本磁碟，請遵循下列連結，如需詳細資訊。

- [準備好 Windows 範本磁碟](guarded-fabric-create-a-shielded-vm-template.md)
- [準備 Linux 範本磁碟](guarded-fabric-create-a-linux-shielded-vm-template.md)

您也將需要一份磁碟的磁碟區簽章目錄，若要建立防護資料檔案。
若要儲存此檔案，執行下列命令在電腦上建立的範本磁碟的位置：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>下載守護者中繼資料

針對每個您想要用來執行您的受防護的 VM 的虛擬化網狀架構中，您必須取得網狀架構 HGS 叢集的守護者中繼資料。
主機供應商應該能夠提供這項資訊。

如果您是在企業環境中，可以與 HGS 伺服器進行通訊的守護者中繼資料會位於*http://\<HGSCLUSTERNAME\>/KeyProtection/service/metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>建立防護資料 (PDK) 檔案

防護資料會建立並由租用戶 VM 擁有者所擁有並包含建立受防護網狀架構系統管理員，例如受防護的 VM 系統管理員密碼必須防範的 Vm 所需的機密資料。
防護資料已加密，只有 HGS 伺服器和租用戶可以將它解密。
一旦建立租用戶/VM 擁有者，所產生的 PDK 檔案必須複製到受防護網狀架構。
如需詳細資訊，請參閱 <<c0> [ 什麼防護資料，以及它為何如此必要？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

此外，您將需要自動的安裝回應檔案 （unattend.xml 的 Windows，適用於 Linux 的而異）。 請參閱[建立回應檔案](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)如要包含在回應檔案中的指引。

受防護的 Vm 安裝遠端伺服器管理工具的電腦上執行下列 cmdlet。
如果您要建立 PDK 的 Linux 虛擬機器，您必須執行 Windows Server 1709 版或更新版本的伺服器上將此動作。

 
```powershell
# Create owner certificate, don't lose this!
# The certificate is stored at Cert:\LocalMachine\Shielded VM Local Certificates
$Owner = New-HgsGuardian –Name 'Owner' –GenerateCertificates
 
# Import the HGS guardian for each fabric you want to run your shielded VM
$Guardian = Import-HgsGuardian -Path C:\HGSGuardian.xml -Name 'TestFabric'
 
# Create the PDK file
# The "Policy" parameter describes whether the admin can see the VM's console or not
# Use "EncryptionSupported" if you are testing out shielded VMs and want to debug any issues during the specialization process
New-ShieldingDataFile -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Owner $Owner –Guardian $guardian –VolumeIDQualifier (New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\MyTemplateDiskCatalog.vsc' -VersionRule Equals) -WindowsUnattendFile 'C:\unattend.xml' -Policy Shielded
```
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>在受守護的主機上佈建受防護的 VM
複製範本磁碟檔案 (ServerOS.vhdx) 和 PDK 檔案 (contoso.pdk) 至受防護主機，以準備開始部署。

在受防護主機上，安裝受防護網狀架構工具 PowerShell 模組，其中包含新增 Microsoft-nanoserver-shieldedvm-package cmdlet，來簡化佈建程序。 如果您的受防護的主機可以存取網際網路，請執行下列命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

您也可以下載另一部具有網際網路存取，並將產生的模組，若要複製的電腦上的模組`C:\Program Files\WindowsPowerShell\Modules`受防護主機上。

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

一旦安裝模組，您即可佈建受防護的 VM。

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

如果您的範本磁碟會包含在以 Linux 為基礎的作業系統，包括`-Linux`旗標時執行命令：

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

請說明內容使用`Get-Help New-ShieldedVM -Full`若要深入了解其他選項，您可以傳遞至 cmdlet。

一旦 VM 完成佈建，它會進入特定 OS 的特製化階段之後, 會被可供使用。
請務必將 VM 連接到有效的網路，讓它開始執行 （使用 RDP、 PowerShell、 透過 ssh 連線或您慣用的管理工具） 之後，您可以連接到它。

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>在 HYPER-V 叢集上執行受防護的 Vm

如果您嘗試要部署受防護的 Vm （使用 Windows 容錯移轉叢集） 的受防護主機叢集上，您可以設定受防護的 VM，才能使用下列 cmdlet 提供高可用性：

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

受防護的 VM 現在可即時叢集內移轉。

## <a name="next-step"></a>後續步驟

>[!div class="nextstepaction"]
[部署受防護使用 VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)