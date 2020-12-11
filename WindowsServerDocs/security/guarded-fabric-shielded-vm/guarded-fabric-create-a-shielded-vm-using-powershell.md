---
description: 深入瞭解：使用 PowerShell 建立受防護的 VM
title: 使用 PowerShell 建立受防護的 VM
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 51a91559c808792d0be71a5a96573c96799822f7
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047416"
---
# <a name="create-a-shielded-vm-using-powershell"></a>使用 PowerShell 建立受防護的 VM

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

在生產環境中，您通常會使用網狀架構管理員 (例如，VMM) 來部署受防護的 Vm。
不過，以下說明的步驟可讓您在沒有網狀架構管理員的情況下，部署和驗證整個案例。

總而言之，您將會在任何電腦上建立範本磁片、防護資料檔案、自動安裝回應檔案和其他安全性構件，然後將這些檔案複製到受防護的主機，並布建受防護的 VM。

## <a name="create-a-signed-template-disk"></a>建立已簽署的範本磁碟

若要建立新的受防護 VM，您必須先使用其 OS 磁片區預先加密的受防護 VM 範本磁片， (或 Linux 上的開機和根磁碟分割) 簽署。
如需有關如何建立範本磁片的詳細資訊，請遵循下列連結。

- [準備 Windows 範本磁片](guarded-fabric-create-a-shielded-vm-template.md)
- [準備 Linux 範本磁片](guarded-fabric-create-a-linux-shielded-vm-template.md)

您也需要磁片的磁片區簽章目錄複本來建立防護資料檔案。
若要儲存此檔案，請在您建立範本磁片的電腦上執行下列命令：

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>下載守護者中繼資料

針對您想要在其中執行受防護 VM 的每個虛擬化網狀架構，您將需要取得網狀架構的 HGS 叢集的守護者中繼資料。
您的主機服務提供者應該能夠為您提供此資訊。

如果您是在企業環境中，而且可以與 HGS 伺服器通訊，則可在 *Http:// \<HGSCLUSTERNAME\> /KeyProtection/service/metadata/2014-07/* 取得守護者中繼資料 metadata.xml

## <a name="create-shielding-data-pdk-file"></a> (PDK) 檔案建立防護資料

防護資料是由租使用者 VM 擁有者所建立和擁有，並包含建立受防護 Vm 所需的秘密，而這些 Vm 必須受到網狀架構系統管理員的保護，例如受防護 VM 的系統管理員密碼。
防護資料會經過加密，因此只有 HGS 伺服器和租使用者可以將它解密。
一旦租使用者/VM 擁有者建立之後，就必須將產生的 PDK 檔案複製到受防護網狀架構。
如需詳細資訊，請參閱 [何謂防護資料，以及為何需要它？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)。

此外，您還需要 ( # A0 for Windows 的自動安裝回應檔案，Linux) 會有所不同。 請參閱 [建立回應](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) 檔案，以取得要包含在回應檔案中的指引。

在已安裝受防護 Vm 遠端伺服器管理工具的電腦上執行下列 Cmdlet。
如果您要建立適用于 Linux VM 的 PDK，您必須在執行 Windows Server 1709 版或更新版本的伺服器上執行此作業。


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

## <a name="provision-shielded-vm-on-a-guarded-host"></a>在受防護主機上布建受防護的 VM
在執行 Windows Server 2016 的主機上，您可以監視 VM 是否關閉以發出布建工作的信號，如果布建失敗，請參閱 Hyper-v 事件記錄檔中的錯誤資訊。

您也可以在建立新的受防護 VM 之前，每次建立新的範本磁片。

將範本磁片檔案 (ServerOS) 和 PDK 檔案（)  (contoso.com）複製到受防護主機，以準備進行部署。

在受防護主機上安裝受防護網狀架構工具 PowerShell 模組，其中包含 New-ShieldedVM Cmdlet，以簡化布建程式。 如果您的受防護主機具有網際網路的存取權，請執行下列命令：

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

您也可以在另一部具有網際網路存取權的電腦上下載模組，並將產生的模組複製到 `C:\Program Files\WindowsPowerShell\Modules` 受防護主機上的。

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

安裝模組之後，您就可以開始布建受防護的 VM。

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

如果您的防護資料回應檔案包含特製化值，您可以提供取代值給新的-ShieldedVM。 在此範例中，回應檔案會設定為靜態 IPv4 位址的預留位置值。

```powershell
$specializationValues = @{
    "@IP4Addr-1@" = "192.168.1.10/24"
    "@MacAddr-1@" = "Ethernet"
    "@Prefix-1-1@" = "24"
    "@NextHop-1-1@" = "192.168.1.254"
}
New-ShieldedVM -Name 'MyStaticIPVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -SpecializationValues $specializationValues -Wait

```

如果您的範本磁片包含以 Linux 為基礎的作業系統，請 `-Linux` 在執行命令時包含旗標：

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

使用來查看說明內容， `Get-Help New-ShieldedVM -Full` 以深入瞭解您可以傳遞給 Cmdlet 的其他選項。

VM 完成布建之後，它會進入作業系統專用的特製化階段，之後將可供使用。
請務必將 VM 連線到有效的網路，讓您可以在執行 (使用 RDP、PowerShell、SSH 或您慣用的管理工具) 來進行連接。

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>在 Hyper-v 叢集上執行受防護的 Vm

如果您嘗試使用 Windows 容錯移轉叢集)  (在叢集受防護主機上部署受防護的 Vm，您可以使用下列 Cmdlet 將受防護的 VM 設定為高可用性：

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

受防護的 VM 現在已可在叢集中進行即時移轉。

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [使用 VMM 部署受防護](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)
