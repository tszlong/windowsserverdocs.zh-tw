---
title: 部署在 Azure 中的 Windows Admin Center 閘道
description: 如何部署在 Azure 中的 Windows Admin Center 閘道
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a3fa1838096d910505faf9a2c5bd819b3a256fe2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280414"
---
# <a name="deploy-windows-admin-center-in-azure"></a>部署在 Azure 中的 Windows Admin Center

## <a name="deploy-using-script"></a>使用指令碼進行部署

您可以下載[部署 WACAzVM.ps1](https://aka.ms/deploy-wacazvm)您會從執行[Azure Cloud Shell](https://shell.azure.com)設為在 Azure 中的 Windows Admin Center 閘道。 此指令碼可以建立整個環境，包括資源群組。

[跳到手動部署步驟](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>先決條件

* 設定您的帳戶[Azure Cloud Shell](https://shell.azure.com)。 如果這是您第一次使用 Cloud Shell，您會要求您建立的關聯，或使用 Cloud Shell 中建立 Azure 儲存體帳戶。
* 在  **PowerShell** Cloud Shell 中，瀏覽至您的主目錄： ```PS Azure:\> cd ~```
* 若要上傳```Deploy-WACAzVM.ps1```檔案、 拖曳和卸除它從本機電腦到任何地方在 Cloud Shell 視窗。

如果要指定您自己的憑證：

* 若要將憑證上傳[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)。 首先，在 Azure 入口網站中建立金鑰保存庫，然後將憑證上傳至金鑰保存庫。 或者，您可以使用 Azure 入口網站來為您產生的憑證。

### <a name="script-parameters"></a>指令碼參數

* **ResourceGroupName** -[String] 指定將要建立 VM 的資源群組的名稱。

* **名稱**-[String] 指定 VM 的名稱。

* **認證**-[PSCredential] 為 vm 指定的認證。

* **MsiPath** -部署的現有 VM 上的 Windows Admin Center 時，[String] 指定的 Windows Admin Center MSI 的本機路徑。 預設為從版本 http://aka.ms/WACDownload 如果省略。

* **VaultName** -[String] 指定包含憑證的金鑰保存庫的名稱。

* **CertName** -[String] 指定要用於 MSI 安裝憑證的名稱。

* **GenerateSslCert** -[交換器] 如果 MSI 應該產生自我簽署的 ssl 憑證則為 True。

* **連接埠號碼**-[int] 指定的 Windows Admin Center 服務的 ssl 連接埠號碼。 預設為 443，如果省略。

* **OpenPorts** -[int []] 指定 VM 開啟連接埠。

* **位置**-[String] 指定 VM 的位置。

* **大小**-[String] 指定 VM 的大小。 如果省略"Standard_DS1_v2"的預設值。

* **映像**-[String] 指定 VM 映像。 預設為 「 將 Win2016Datacenter"如果省略。

* **VirtualNetworkName** -[String] 指定 vm 的虛擬網路的名稱。

* **SubnetName** -[String] 指定 VM 子網路的名稱。

* **SecurityGroupName** -[String] 指定安全性群組的成員名稱的 vm。

* **PublicIpAddressName** -[String] 指定 VM 的公用 IP 位址的名稱。

* **InstallWACOnly** -[參數] 設定為 True，如果 WAC 應該安裝在現有的 Azure VM 上。

有 2 個不同的選項，MSI 部署和使用 MSI 安裝的憑證。 MSI 是您可以從 aka.ms/WACDownload 下載，或者，如果部署至現有的 VM，可以指定 VM 本機上的 msi 檔案路徑。 憑證可在其中一個 Azure 金鑰保存庫，或將 MSI 產生的自我簽署的憑證。

### <a name="script-examples"></a>指令碼範例

首先，定義常用的參數，指令碼的所需的變數。

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>範例 1：您可以使用 指令碼來部署新的虛擬網路和資源群組中的新 VM 上的 WAC 閘道。 使用 MSI 從 aka.ms/WACDownload 和 MSI 從自我簽署的憑證。

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>範例 2：與 #1，但使用 Azure 金鑰保存庫的憑證相同。

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>範例 3：使用現有的 VM 上的本機 MSI，來部署 WAC。

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>執行 Windows Admin Center 閘道 vm 的需求

您必須開啟連接埠 443 (HTTPS)。
使用相同的變數定義的指令碼，您可以使用下列程式碼在 Azure Cloud Shell 中更新的網路安全性群組：

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>受控 Azure VM 的需求

連接埠 5985 (WinRM over HTTP) 必須是開啟，並具有作用中的接聽程式。
您可以使用 Azure Cloud Shell 中的下列程式碼來更新受管理的節點。 ```$ResourceGroupName``` 並```$Name```為部署指令碼中，使用相同的變數，但您必須使用```$Credential```您要管理特定的 vm。

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>現有的 Azure 虛擬機器上手動部署

您所需的閘道 VM 上安裝 Windows Admin Center 之前, 安裝用來進行 HTTPS 通訊的 SSL 憑證，或您可以選擇使用 Windows Admin Center 所產生的自我簽署的憑證。 不過，您會收到警告時，如果您選擇第二種選項，從瀏覽器連線嘗試。 您可以按一下連線，略過 Edge 中的這項警告**詳細資料 > 請移至網頁**或在 Chrome 中，選取**進階 > 繼續進行 [網頁]** 。 我們建議您只有使用自我簽署的憑證作為測試環境。

> [!NOTE]
> 這些指示是針對不在 Server Core 安裝上的 含桌面體驗的 Windows Server 上安裝。 

1. [下載 Windows Admin Center](https://aka.ms/windowsadmincenter)到本機電腦。

2. 建立遠端桌面連線到 VM，然後複製您的本機電腦的 MSI 並貼到 VM。

3. 按兩下 MSI 以開始安裝，並遵循精靈中的指示。 請注意下列：

   - 根據預設，安裝程式會使用建議的連接埠 443 (HTTPS)。 如果您想要選取不同的連接埠，請注意，您必須也在防火牆中開啟該連接埠。 

   - 如果您已在 VM 上安裝 SSL 憑證，請確定您選取該選項，然後輸入憑證指紋。

4. 啟動 Windows Admin Center 服務 （執行 c: / Program 檔案和 Windows 系統管理員 Center/sme.exe）

[深入了解部署 Windows Admin Center。](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>設定閘道以啟用 HTTPS 連接埠存取的 VM: 

1. 瀏覽至您的 VM 在 Azure 入口網站，然後選取**網路**。 

2. 選取 **新增輸入連接埠規則**，然後選取**HTTPS**之下**服務**。 

> [!NOTE]
> 如果您選擇預設為 443 以外的連接埠，請選擇**自訂**服務下，輸入您在下的步驟 3 所選擇的連接埠**連接埠範圍**。 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>存取 Azure VM 上安裝的 Windows Admin Center 閘道

此時，您應該能夠存取 Windows Admin Center （Edge 或 Chrome） 的現代瀏覽器從您本機電腦上瀏覽至您的閘道 VM 的 DNS 名稱。 

> [!NOTE]
> 如果您選取的連接埠 443 以外，您可以存取 Windows Admin Center 瀏覽至 https://\<VM 的 DNS 名稱\>:\<自訂連接埠\>

當您嘗試存取 Windows Admin Center 時，在瀏覽器會提示輸入認證來存取 Windows Admin Center 安裝所在的虛擬機器。 這裡，您必須輸入本機使用者或本機 administrators 群組的虛擬機器中的認證。 

若要新增其他 Vm 在 VNet 中，請確定 WinRM 目標 Vm 上執行目標 VM 上，在 PowerShell 或命令提示字元中執行下列命令： `winrm quickconfig`

如果您還沒有已加入網域的 Azure VM，VM 行為會類似的伺服器位於工作群組，因此您必須先確定您負責[使用工作群組中的 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。