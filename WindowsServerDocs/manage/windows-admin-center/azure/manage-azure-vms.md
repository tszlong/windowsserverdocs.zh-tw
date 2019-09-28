---
title: 管理 Azure IaaS 虛擬機器
description: 使用 Windows 系統管理中心管理 Azure IaaS Vm （Project 檀香山）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7b85f64d108283d4865b718b565ad3ba40f14f02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357336"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>使用 Windows 系統管理中心管理 Azure IaaS 虛擬機器

您可以使用 Windows Admin Center 來管理您的 Azure VM 及內部部署機器。 可能有數個不同的設定-選擇對您的環境有意義的設定：
- [從內部部署 Windows 管理中心閘道管理 Azure Vm](#manage-with-an-on-premises-windows-admin-center-gateway)
- [從安裝在 Azure VM 上的 Windows 管理中心閘道管理 Azure Vm](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>使用內部部署 Windows 管理中心閘道管理

如果您已在內部部署閘道上安裝 Windows 系統管理中心（在 Windows 10 或 Windows Server 2016 上），您可以使用這個相同的閘道來管理 Windows 10 或 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2Azure 中的 Vm。 

### <a name="connecting-to-vms-with-a-public-ip"></a>連接至具有公用 IP 的 Vm

如果您的目標 Vm （您想要使用 Windows 管理中心管理的 Vm）具有公用 Ip，請以 IP 位址或 FQDN 將它們新增至您的 Windows Admin Center 閘道。 有幾個要考慮的事項：

- 您必須在目標 VM 上的 PowerShell 或命令提示字元中執行下列命令，以啟用目標 VM 的 WinRM 存取： `winrm quickconfig`
- 如果您尚未加入 Azure VM 的網域，VM 的行為就像工作組中的伺服器，因此您必須確定您[是在工作組中使用 Windows 系統管理中心](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)的帳戶。
- 您也必須針對 WinRM over HTTP 啟用埠5985的輸入連線，Windows 管理中心才能管理目標 VM：
  1. 在目標 VM 上執行下列 PowerShell 腳本，以啟用虛擬作業系統上端口5985的輸入連線：   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. 您也必須在 Azure 網路中開啟埠：

     - 選取您的 Azure VM，選取 [**網路**]，然後按一下 [**新增輸入連接埠規則**]。 
     - 確定已選取 [**新增輸入安全性規則**] 窗格頂端的 [**基本**]。
     - 在 [**埠範圍**] 欄位中，輸入**5985**。
    
     如果您的 Windows 管理中心閘道有靜態 IP，您可以選擇只允許來自 Windows 管理中心閘道的輸入 WinRM 存取，以提高安全性。
     若要這麼做，請選取 [**新增輸入安全性規則**] 窗格頂端的 [ **Advanced** ]。

     針對 [**來源**]，選取 [ **IP 位址**]，然後輸入與您的 Windows 管理中心閘道相對應的來源 IP 位址。

     - 針對 [**通訊協定**]，選取 [ **TCP**]。
     - 其餘部分可以保留為預設值。

> [!NOTE]
> 您必須建立自訂連接埠規則。 Azure 網路提供的 WinRM 連接埠規則會使用埠5986（透過 HTTPS），而不是5985（透過 HTTP）。 

### <a name="connecting-to-vms-without-a-public-ip"></a>連接至沒有公用 IP 的 Vm

如果您的目標 Azure Vm 沒有公用 Ip，而您想要從部署在內部部署網路中的 Windows 管理中心閘道管理這些 Vm，您必須設定您的內部部署網路，使其能夠連線到目標 Vm 所在的 VNet聯繫. 有三種方式可執行此動作：ExpressRoute、站對站 VPN 或點對站 VPN。 [瞭解在您的環境中有意義的連線選項。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>如果您想要使用點對站 VPN 將您的 Windows 管理中心閘道連線至 Azure VNet，以管理該 VNet 中的 Azure Vm，您可以使用 Windows 系統管理中心的[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)功能。 若要這麼做，請連接到已安裝 Windows 系統管理中心的伺服器，流覽至 [網路] 工具，然後選取 [新增 Azure 網路介面卡]。 當您提供必要的詳細資料，並按一下 [設定] 時，Windows 管理中心會將點對站 VPN 設定為您指定的 Azure VNet，之後您就可以從內部部署 Windows 管理中心閘道連線並管理 Azure Vm。

請在 PowerShell 中執行下列命令，或在目標 VM 上使用命令提示字元，以確定 WinRM 正在您的目標 Vm 上執行： `winrm quickconfig`

如果您尚未加入 Azure VM 的網域，VM 的行為就像工作組中的伺服器，因此您必須確定您[是在工作組中使用 Windows 系統管理中心](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)的帳戶。

如果您遇到任何問題，請參閱針對[Windows 系統管理中心進行疑難排解](../support/troubleshooting.md)，以查看設定是否需要額外的步驟（例如，如果您使用本機系統管理員帳戶連接，或未加入網域）。

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>使用在 Azure 中部署的 Windows 管理中心閘道

您可以在目標 Vm 連線的 VNet 中部署 Windows 管理中心，而不需要任何內部部署相依性來管理 Azure Vm。 

若要在部署 Windows 管理中心閘道的 VNet 以外管理 Vm，您必須在 Windows 管理中心閘道的 VNet 與目標伺服器的 vnet 之間，建立 VNet 對 VNet 連線能力。 您可以使用 VNet 對等互連、VNet 對 VNet 連線或站對站連線來建立此連線能力。 [深入瞭解在您的環境中有意義的 VNet 對 VNet 連線能力選項。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows 管理中心可以安裝在您環境中現有或新部署的 VM 上。 您為 Windows 管理中心安裝選擇的 VM 必須具有公用 IP 和 DNS 名稱。

[深入瞭解在 Azure 中部署 Windows 管理中心閘道](deploy-wac-in-azure.md)