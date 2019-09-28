---
title: 使用 PowerShell 建立受防護的 VM
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 888177c1288216c28f7d4c0a667fd81e93bdce8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402397"
---
# <a name="create-a-shielded-vm-using-powershell"></a>使用 PowerShell 建立受防護的 VM

>適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

在生產環境中，您通常會使用網狀架構管理員（例如 VMM）來部署受防護的 Vm。 不過，下面所述的步驟可讓您在不使用網狀架構管理員的情況下，部署及驗證整個案例。

簡言之，您會在任何電腦上建立範本磁片、防護資料檔案、自動安裝回應檔案和其他安全性成品，然後將這些檔案複製到受防護的主機，並布建受防護的 VM。

## <a name="create-a-signed-template-disk"></a>建立已簽署的範本磁片

若要建立新的受防護 VM，您必須先將受防護的 VM 範本磁片預先加密，並簽署其作業系統磁片區（或 Linux 上的開機和根磁碟分割）。
如需如何建立範本磁片的詳細資訊，請遵循下列連結。

- [準備 Windows 範本磁片](guarded-fabric-create-a-shielded-vm-template.md)
- [準備 Linux 範本磁片](guarded-fabric-create-a-linux-shielded-vm-template.md)

您也需要一份磁片的磁片區簽章目錄，以建立防護資料檔案。
若要儲存此檔案，請在您建立範本磁片的電腦上執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>下載守護者中繼資料

針對您想要執行受防護 VM 的每個虛擬化網狀架構，您必須取得網狀架構的 HGS 叢集的守護者中繼資料。
您的主機服務提供者應該能夠為您提供這種資訊。

如果您在企業環境中，而且可以與 HGS 伺服器通訊，則守護者中繼資料可在*HTTP://\<HGSCLUSTERNAME @ no__t-2/KeyProtection/service/metadata/2014-07/metadata .xml 取得。*

## <a name="create-shielding-data-pdk-file"></a>建立防護資料（PDK）檔案

防護資料是由租使用者 VM 擁有者所建立和擁有，而且包含建立受防護的 Vm 所需的秘密，而這些虛擬機器必須受到網狀架構系統管理員的保護，例如受防護的 VM 系統管理員密碼。
防護資料會加密，因此只有 HGS 伺服器和租使用者可以將其解密。
一旦由租使用者/VM 擁有者建立之後，所產生的 PDK 檔案就必須複製到受防護的網狀架構。
如需詳細資訊，請參閱[什麼是防護資料，以及為何需要它？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

此外，您還需要自動安裝回應檔案（適用于 Windows 的 unattend.xml，會因 Linux 而異）。 如需如何在回應檔案中包含的指引，請參閱[建立回應](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file)檔案。

在已安裝受防護 Vm 的遠端伺服器管理工具的電腦上執行下列 Cmdlet。
如果您要建立適用于 Linux VM 的 PDK，您必須在執行 Windows Server 1709 版或更新版本的伺服器上進行此動作。

 
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
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>在受保護的主機上布建受防護的 VM
將範本磁片檔案（ServerOS）和 PDK 檔案（contoso. pdk）複製到受防護的主機，以準備開始進行部署。

在受防護的主機上，安裝受防護的網狀架構工具 PowerShell 模組，其中包含 ShieldedVM Cmdlet，可簡化布建程式。 如果您的受防護主機具有網際網路存取權，請執行下列命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

您也可以在另一部具有網際網路存取的電腦上下載模組，並將產生的模組複製到受防護主機上的 `C:\Program Files\WindowsPowerShell\Modules`。

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

安裝模組之後，您就可以開始布建受防護的 VM。

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

如果您的範本磁片包含以 Linux 為基礎的作業系統，請在執行命令時包含 `-Linux` 旗標：

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

請使用 `Get-Help New-ShieldedVM -Full` 來檢查說明內容，以深入瞭解您可以傳遞給 Cmdlet 的其他選項。

一旦 VM 完成布建，它會進入 OS 特定的特製化階段，之後就可以開始使用。
請務必將 VM 連線到有效的網路，如此一來，您就可以在執行（使用 RDP、PowerShell、SSH 或您偏好的管理工具）時連線到該虛擬機器。

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>在 Hyper-v 叢集上執行受防護的 Vm

如果您嘗試在叢集受防護主機（使用 Windows 容錯移轉叢集）上部署受防護的 vm，您可以使用下列 Cmdlet 將受防護的 VM 設定為高可用性：

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

受防護的 VM 現在可以在叢集中即時移轉。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [使用 VMM 部署受防護的](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)