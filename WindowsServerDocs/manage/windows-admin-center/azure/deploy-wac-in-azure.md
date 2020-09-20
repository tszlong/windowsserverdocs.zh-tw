---
title: 在 Azure 中部署 Windows Admin Center 閘道
description: 如何在 Azure 中部署 Windows Admin Center 閘道
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4fd03195feb275bd56c6958f8436c607829c8392
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766661"
---
# <a name="deploy-windows-admin-center-in-azure"></a>在 Azure 中部署 Windows Admin Center

## <a name="deploy-using-script"></a>使用腳本進行部署

您可以下載 [Deploy-WACAzVM.ps1](https://aka.ms/deploy-wacazvm) 將從 [Azure Cloud Shell](https://shell.azure.com) 執行，以在 Azure 中設定 Windows Admin Center 閘道。 此腳本可以建立整個環境，包括資源群組。

[跳至手動部署步驟](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Prerequisites

* 在 [Azure Cloud Shell](https://shell.azure.com)中設定您的帳戶。 如果這是您第一次使用 Cloud Shell，系統會要求您將 Azure 儲存體帳戶與 Cloud Shell 建立關聯。
* 在 **PowerShell** Cloud Shell 中，流覽至您的主目錄： ```PS Azure:\> cd ~```
* 若要上傳檔案 ```Deploy-WACAzVM.ps1``` ，請將檔案從本機電腦拖放到 Cloud Shell 視窗的任何位置。

如果指定您自己的憑證：

* 將憑證上傳至 [Azure Key Vault](/azure/key-vault/key-vault-whatis)。 首先，在 Azure 入口網站中建立金鑰保存庫，然後將憑證上傳至金鑰保存庫。 或者，您可以使用 Azure 入口網站為您產生憑證。

### <a name="script-parameters"></a>指令碼參數

* **ResourceGroupName** -[String] 指定將建立 VM 的資源組名。

* **名稱** -[String] 指定 VM 的名稱。

* **認證** -[PSCredential] 指定 VM 的認證。

* **MsiPath** -[String] 指定在現有 VM 上部署 Windows Admin Center 時 Windows Admin Center MSI 的本機路徑。 如果省略，則預設為的版本 https://aka.ms/WACDownload 。

* **VaultName** -[String] 指定包含憑證的金鑰保存庫名稱。

* **>certname** -[String] 指定要用於 MSI 安裝的憑證名稱。

* **GenerateSslCert** -[Switch] True （如果 MSI 應產生自我簽署的 ssl 憑證）。

* **PortNumber** -[Int] 指定 Windows Admin Center 服務的 ssl 埠號碼。 如果省略，則預設為443。

* **OpenPorts** -[int []] 指定 VM 的開啟埠。

* **Location** -[String] 指定 VM 的位置。

* **大小** -[字串] 指定 VM 的大小。 如果省略，則預設為 "Standard_DS1_v2"。

* **Image** -[String] 指定 VM 的映射。 如果省略，則預設為 "Win2016Datacenter"。

* **VirtualNetworkName** -[String] 指定 VM 的虛擬網路名稱。

* **SubnetName** -[String] 指定 VM 的子網名稱。

* **SecurityGroupName** -[String] 指定 VM 的安全性群組名稱。

* **PublicIpAddressName** -[String] 指定 VM 的公用 IP 位址名稱。

* **InstallWACOnly** -[Switch] 如果 WAC 應該安裝在預先存在的 Azure VM 上，則設定為 True。

有2個不同的選項可供 MSI 部署，以及用於 MSI 安裝的憑證。 MSI 可以從 aka.ms/WACDownload 下載，或者，如果部署至現有的 VM，則可以在 VM 本機上指定 MSI 的 filepath。 憑證可以在 Azure Key Vault 中找到，或 MSI 會產生自我簽署憑證。

### <a name="script-examples"></a>指令碼範例

首先，請定義腳本參數所需的通用變數。

```PowerShell
$ResourceGroupName = "wac-rg1"
$VirtualNetworkName = "wac-vnet"
$SecurityGroupName = "wac-nsg"
$SubnetName = "wac-subnet"
$VaultName = "wac-key-vault"
$CertName = "wac-cert"
$Location = "westus"
$PublicIpAddressName = "wac-public-ip"
$Size = "Standard_D4s_v3"
$Image = "Win2016Datacenter"
$Credential = Get-Credential
```

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>範例1：使用腳本，在新的虛擬網路和資源群組中的新 VM 上部署 WAC 閘道。 從 aka.ms/WACDownload 使用 MSI，並從 MSI 使用自我簽署憑證。

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>範例2：與 #1 相同，但使用 Azure Key Vault 的憑證。

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm2"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    VaultName = $VaultName
    CertName = $CertName
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>範例3：在現有的 VM 上使用本機 MSI 來部署 WAC。

```PowerShell
$MsiPath = "C:\Users\<username>\Downloads\WindowsAdminCenter<version>.msi"
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm3"
    Credential = $Credential
    MsiPath = $MsiPath
    InstallWACOnly = $true
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>執行 Windows Admin Center 閘道的 VM 需求

必須開啟埠 443 (HTTPS) 。
使用針對腳本定義的相同變數，您可以使用 Azure Cloud Shell 中的下列程式碼來更新網路安全性群組：

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>受控 Azure VM 的需求

透過 HTTP)  (WinRM 的埠5985必須開啟，並且有使用中的接聽程式。
您可以使用 Azure Cloud Shell 中的下列程式碼來更新受管理的節點。 ```$ResourceGroupName``` 並 ```$Name``` 使用與部署腳本相同的變數，但您必須使用 ```$Credential``` 您所管理 VM 的特定變數。

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>在現有的 Azure 虛擬機器上手動部署

在您想要的閘道 VM 上安裝 Windows Admin Center 之前，請先安裝 SSL 憑證以用於 HTTPS 通訊，或者您可以選擇使用 Windows Admin Center 所產生的自我簽署憑證。 但是，如果您選擇第二個選項，就會在嘗試從瀏覽器連線時收到警告。 您可以按一下 [ **詳細資料] > 移至網頁** ，或在 Chrome 中選取 **[Advanced > 繼續前往 [網頁]**，以略過此警告。 建議您只針對測試環境使用自我簽署的憑證。

> [!NOTE]
> 這些指示適用于在 Windows Server 上安裝桌面體驗，而不是在 Server Core 安裝上。

1. 將[Windows Admin Center 下載](../overview.md)到您的本機電腦。

2. 建立與 VM 的遠端桌面連線，然後從本機電腦複製 MSI 並貼到 VM 中。

3. 按兩下 MSI 以開始安裝，並遵循 wizard 中的指示。 請注意以下事項：

   - 根據預設，安裝程式會使用建議的埠 443 (HTTPS) 。 如果您想要選取不同的埠，請注意，您也必須在防火牆中開啟該埠。

   - 如果您已在 VM 上安裝 SSL 憑證，請確定您已選取該選項，並輸入指紋。

4. 啟動 Windows Admin Center 服務 (執行 C：/Program Files/Windows Admin Center/sme.exe) 

[深入瞭解如何部署 Windows Admin Center。](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>設定閘道 VM 以啟用 HTTPS 埠存取：

1. 在 Azure 入口網站中流覽至您的 VM，然後選取 [ **網路**]。

2. 選取 [**新增輸入連接埠規則**]，然後選取 [**服務**] 下的 [ **HTTPS** ]。

> [!NOTE]
> 如果您選擇預設443以外的埠，請在 [服務] 下選擇 [ **自訂** ]，然後在 [ **埠範圍**] 下的步驟3中輸入您選擇的埠。

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>存取安裝在 Azure VM 上的 Windows Admin Center 閘道

此時，您應該能夠流覽至閘道 VM 的 DNS 名稱，以從您本機電腦上的新式瀏覽器 (Edge 或 Chrome) 存取 Windows Admin Center。

> [!NOTE]
> 如果您選取了443以外的埠，您可以流覽至 HTTPs://以存取 Windows Admin Center \<DNS name of your VM\> ：\<custom port\>

當您嘗試存取 Windows Admin Center 時，瀏覽器會提示您輸入認證，以存取已安裝 Windows Admin Center 的虛擬機器。 您必須在此輸入虛擬機器的 [本機使用者] 或 [本機系統管理員] 群組中的認證。

若要在 VNet 中新增其他 Vm，請確定 WinRM 是在目標 vm 上執行，方法是在 PowerShell 中執行下列命令，或在目標 VM 上執行下列命令提示字元： `winrm quickconfig`

如果您未將 Azure VM 加入網域，VM 的行為就像是工作組中的伺服器，因此您必須確定您已考慮在 [工作組中使用 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。