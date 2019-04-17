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
ms.openlocfilehash: af766c2e0502d9fe633cae42d999db5cbffc32c8
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296921"
---
# 部署在 Azure 中的 Windows Admin Center

## 使用指令碼部署

您可以下載[部署 WACAzVM.ps1](https://aka.ms/deploy-wacazvm)您將會從[Azure 雲端殼層](https://shell.azure.com)設定 Windows Admin Center 閘道在 Azure 中執行。 這個指令碼可以建立整個環境，包括資源群組。

[跳至手動部署步驟](#deploy-manually-on-an-existing-azure-virtual-machine)

### 必要條件

* 將您的帳戶， [Azure 雲端](https://shell.azure.com)shell 設定。 如果這是您第一次使用雲端殼層，將會要求您您建立關聯，或使用雲端殼層中建立的 Azure 儲存體帳戶。
* 在**PowerShell**雲端 Shell 中，瀏覽至您的主目錄： ```PS Azure:\> cd ~```
* 若要上傳```Deploy-WACAzVM.ps1```檔案、 將拖放從您的本機電腦到任何地方雲端殼層視窗上。

如果指定您自己的憑證：

* 憑證上傳至[Azure 金鑰保存庫](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)中。 首先，在 Azure 入口網站中，建立金鑰的保存庫，然後上傳到索引鍵的保存庫的憑證。 或者，您可以使用 Azure 入口網站來為您產生的憑證。

### 指令碼參數

* **ResourceGroupName** [字串] 指定將要建立 VM 資源群組的名稱。

* **名稱**[字串] 指定 VM 的名稱。

* **認證**[PSCredential] 會指定之 vm 的認證。

* 部署 Windows Admin Center 上現有 VM 時， **MsiPath** [字串] 指定 Windows Admin Center MSI 的本機路徑。 預設為從版本http://aka.ms/WACDownload如果省略。

* **VaultName** [字串] 指定索引鍵的保存庫包含憑證的名稱。

* **CertName** [字串] 指定要用於 MSI 安裝憑證的名稱。

* **GenerateSslCert** [切換] 如果 MSI 應該產生自我簽署的 ssl 憑證，則為 True。

* **PortNumber** [int] 指定 Windows Admin Center 服務的 ssl 連接埠號碼。 如果省略 443 預設值。

* **OpenPorts** -[int []] 指定之 vm 的開啟的連接埠。

* **位置**[字串] 指定 VM 的位置。

* **大小**[字串] 指定的 VM 大小。 如果省略 」 Standard_DS1_v2 」 預設值。

* **映像**[字串] 指定的 vm 的影像。 如果省略 」 Win2016Datacenter 」 預設值。

* **VirtualNetworkName** [字串] 指定之 vm 的虛擬網路的名稱。

* **SubnetName** [字串] 指定之 vm 的子網路的名稱。

* **SecurityGroupName** [字串] 指定之 vm 的安全性群組的名稱。

* **PublicIpAddressName** [字串] 指定之 vm 的公用 IP 位址的名稱。

* **InstallWACOnly** -[切換] 設定為 True，如果 WAC 應該預先存在的 Azure VM 上安裝。

有 2 個不同的選項來部署 MSI 和用於 MSI 安裝的憑證。 可以是從 aka.ms/WACDownload 下載 MSI，或者，如果部署到現有的 VM，您可以提供的 MSI VM 在本機上 filepath。 憑證可以找到在任一種 Azure 金鑰保存庫或會由 MSI 產生自我簽署的憑證。

### 指令碼範例

首先，定義所需的參數的指令碼的常見變數。

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

#### 範例 1： 使用指令碼部署新的 VM 中新的虛擬網路和資源群組上的 WAC 閘道。 使用從 aka.ms/WACDownload MSI 和從 MSI 的自我簽署的憑證。

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

#### 範例 2： 相同 #1，但使用 Azure 金鑰保存庫中的憑證。

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

#### 範例 3： 使用現有 VM 在本機的 MSI，部署 WAC。

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

### 適用於執行 Windows Admin Center 閘道 VM 需求

您必須開啟連接埠 443 (HTTPS)。
使用相同的指令碼定義的變數，您可以使用下列程式碼中 Azure 雲端殼層更新網路安全性群組：

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### 適用於受管理 Azure VM 的需求

連接埠 5985 (透過 HTTP WinRM) 必須為開啟狀態，而且有作用中的接聽程式。
您可以使用 Azure 雲端殼層中下方的程式碼來更新受管理的節點。 ```$ResourceGroupName``` 和```$Name```使用相同的變數做為部署指令碼，但您將需要使用```$Credential```您正在管理特定的 vm。

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## 現有的 Azure 虛擬機器上手動部署

您想要的閘道 VM 上安裝 Windows Admin Center 之前, 安裝 SSL 憑證，以使用 HTTPS 通訊，或您可以選擇使用 Windows Admin Center 所產生的自我簽署的憑證。 不過，您會收到警告，嘗試連線從瀏覽器，如果您選擇後者的選項時。 您可以略過此警告，在 Edge 中的按一下 [**詳細資料 > 移至網頁**，或在 Chrome，透過選取**進階的 > 前進到 [網頁]**。 我們建議僅用於測試環境的自我簽署的憑證。

> [!NOTE]
> 這些指示僅適用於含有桌面體驗的 Windows Server 上安裝不在 Server Core 安裝上。 

1. [下載 Windows Admin Center](https://aka.ms/windowsadmincenter)到本機電腦。

2. 建立遠端桌面連線到 VM 中，然後複製 MSI 從您的本機電腦並貼到 VM。

3. 按兩下以開始安裝，MSI，然後依照精靈中的指示。 請注意下列：

   - 根據預設，安裝程式會使用建議的連接埠 443 (HTTPS)。 如果您想要選取不同的連接埠，請注意，您需要在您的防火牆也中開啟該連接埠。 

   - 如果您已經在 VM 上安裝的 SSL 憑證，請確定您選取該選項，並輸入指紋。

4. 啟動 Windows Admin Center 服務 （以執行程式 c: / / Windows 檔案系統管理員 Center/sme.exe）

[深入了解部署 Windows Admin Center。](../deploy/install.md)

### 設定 VM 以啟用 HTTPS 連接埠存取閘道： 

1. 瀏覽至您的 VM 在 Azure 入口網站，然後選取 [**網路功能**。 

2. 選取 [**新增輸入連接埠規則**，然後選取 [ **HTTPS**下**服務**。 

> [!NOTE]
> 如果您選擇的連接埠 443 預設以外，選擇 [**自訂**] 底下服務，然後輸入您在步驟 3**連接埠範圍**下選擇的連接埠。 

### 存取 Azure VM 上安裝 Windows Admin Center 閘道

到目前為止，您應該能夠從 （Edge 或 Chrome） 的現代化瀏覽器存取 Windows Admin Center 本機電腦上瀏覽至您的閘道 VM 的 DNS 名稱。 

> [!NOTE]
> 如果您選取的連接埠 443 以外，您可以存取 Windows Admin Center 瀏覽至您 VM\>:\<custom port\> https://\<DNS 名稱

當您嘗試存取 Windows Admin Center 時，瀏覽器會提示輸入認證，才能存取 Windows Admin Center 安裝所在的虛擬機器。 這裡，您將需要輸入都是以本機的使用者或虛擬機器的本機系統管理員群組的認證。 

為了 VNet 中新增其他 Vm，請確定 WinRM 目標 Vm 上執行 PowerShell 或命令提示字元中執行下列目標 VM 上： `winrm quickconfig`

如果您還沒有加入網域的 Azure VM，VM 行為類似的伺服器工作群組中，因此您需要確定您考慮[使用 Windows Admin Center 在工作群組中](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。