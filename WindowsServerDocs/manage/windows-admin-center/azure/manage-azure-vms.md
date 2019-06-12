---
title: 管理 Azure IaaS 虛擬機器
description: 管理 Windows Admin Center （專案檀香山） 與 Azure IaaS Vm
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ac98f42c4ad5606cc8d2b142f209f9bdb2b9611c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445907"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>管理 Azure IaaS 虛擬機器與 Windows Admin Center

您可以使用 Windows Admin Center 來管理您的 Azure Vm 以及內部部署機器。 有數個不同的組態可能-選擇適合您環境的組態：
- [管理 Azure Vm 從內部部署 Windows Admin Center 閘道](#manage-with-an-on-premises-windows-admin-center-gateway)
- [從 Azure VM 上安裝的 Windows Admin Center 閘道管理的 Azure Vm](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>管理與內部部署 Windows Admin Center 閘道

如果您已經安裝 Windows Admin Center （可能是 Windows 10 或 Windows Server 2016） 在內部部署閘道上，您可以使用這個相同的閘道，來管理 Windows 10 或 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2在 Azure 中的 Vm。 

### <a name="connecting-to-vms-with-a-public-ip"></a>連接到使用公用 IP 的 Vm

如果您的目標 Vm (您想要使用 Windows Admin Center 來管理 Vm) 有公用 Ip，將它們加入您的 Windows Admin Center 閘道 IP 位址或 FQDN。 有幾個考量納入考量：

- 您必須啟用到您的目標 VM 的 WinRM 存取目標 VM 上，在 PowerShell 或命令提示字元中執行下列命令： `winrm quickconfig`
- 如果您還沒有已加入網域的 Azure VM，VM 行為會類似的伺服器位於工作群組，因此您必須先確定您負責[使用工作群組中的 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。
- 您也必須啟用透過 HTTP 的 winrm 連接埠 5985 的輸入的連線，以便管理目標 VM 的 Windows Admin Center:
  1. 目標 VM，以啟用輸入的連線在客體 OS 上的連接埠 5985 上執行下列 PowerShell 指令碼：   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. 您也必須在 Azure 的網路開啟連接埠：

     - 選取您的 Azure VM 中，選取**Networking**，然後**新增輸入連接埠規則**。 
     - 請確定**基本**選取頂端**新增輸入的安全性規則**窗格。
     - 在 **連接埠範圍**欄位中，輸入**5985**。
    
     如果您的 Windows Admin Center 閘道有靜態 ip 位址，您可以選擇僅允許輸入的 WinRM 存取從您的 Windows Admin Center 閘道來提高安全性。
     若要這樣做，請選取**進階**頂端**新增輸入的安全性規則**窗格。

     針對**來源**，選取**IP 位址**，然後輸入對應到您的 Windows Admin Center 閘道的來源 IP 位址。

     - 針對**通訊協定**選取**TCP**。
     - Rest 可保持為預設值。

> [!NOTE]
> 您必須建立自訂連接埠規則。 Azure 網路功能所提供的 WinRM 連接埠規則會使用連接埠 5986 （透過 HTTPS) 而不是 5985 （透過 HTTP)。 

### <a name="connecting-to-vms-without-a-public-ip"></a>連線至沒有公用 IP 的 Vm

如果您的目標 Azure Vm 沒有公用 Ip，而且您想要管理這些 Vm 從內部部署網路中部署的 Windows Admin Center 閘道，您需要設定您的內部部署網路，才能連線至目標 Vm 所在的 VNet連接。 有 3 種方式執行這項操作：ExpressRoute、 VPN 站對站或點對站 VPN。 [了解的連線選項的意義在您的環境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>如果您想要使用點對站 VPN 來連線至 Azure VNet 來管理 Azure Vm，您可以使用 VNet，您的 Windows Admin Center 閘道[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)Windows Admin Center 中的功能。 若要這樣做，請連接到伺服器已安裝的 Windows Admin Center、 瀏覽至 [網路] 工具並選取 「 新增 Azure 網路介面卡 」。 當您提供必要的詳細資料，並按一下 [設定]，Windows Admin Center 將會設定至您指定，在那之後，您可以連接到及管理 Azure Vm 從內部部署 Windows Admin Center 閘道 Azure VNet 的點對站 VPN。

請確定 WinRM 目標 Vm 上執行目標 VM 上，在 PowerShell 或命令提示字元中執行下列命令： `winrm quickconfig`

如果您還沒有已加入網域的 Azure VM，VM 行為會類似的伺服器位於工作群組，因此您必須先確定您負責[使用工作群組中的 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。

如果您遇到任何問題，請參閱[疑難排解 Windows Admin Center](../support/troubleshooting.md) ，看看 （比方說，如果您使用本機系統管理員帳戶連接，或不是已加入網域），是否必須針對設定額外的步驟。

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>使用在 Azure 中部署的 Windows Admin Center 閘道

您可以部署在目標 Vm，已連線的 VNet 中的 Windows Admin Center 而不需要任何內部部署的相依性的檔案，以管理 Azure Vm。 

若要管理的 Windows Admin Center 閘道部署所在的 VNet 外部的 Vm，您必須建立 Windows Admin Center 閘道的 VNet 與目標伺服器的 VNet 之間的 VNet 對 VNet 連線。 您可以建立此 VNet 對等互連、 VNet 對 VNet 連線，或站對站連線的連線。 [深入了解 VNet 對 VNet 連線的選項有意義您環境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center 可以安裝在現有或新部署的 VM，您的環境中。 您選擇的 Windows Admin Center 安裝的 VM 必須有公用 IP 和 DNS 名稱。

[深入了解部署在 Azure 中的 Windows Admin Center 閘道](deploy-wac-in-azure.md)