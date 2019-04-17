---
title: 管理 Azure IaaS 虛擬機器
description: 管理 Azure IaaS 虛擬機器使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6f162294076d7e12df09f31b0bbd7d9244abe679
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296931"
---
# 管理 Azure IaaS 虛擬機器使用 Windows Admin Center

您可以使用 Windows Admin Center 來管理您的 Azure Vm 以及內部部署電腦。 有數個不同的設定可能-選擇適合您環境的設定：
- [從內部部署的 Windows Admin Center 閘道管理 Azure Vm](#manage-with-an-on-premises-windows-admin-center-gateway)
- [從 Azure VM 上安裝 Windows Admin Center 閘道管理 Azure Vm](#use-a-windows-admin-center-gateway-deployed-in-azure)

## 使用內部部署的 Windows Admin Center 閘道來管理

如果您已經安裝 Windows Admin Center 在內部部署的閘道 （無論是在 Windows 10 或 Windows Server 2016 上），您可以使用這個相同的閘道來管理 Windows 10 或 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2在 Azure 中的虛擬機器。 

### 連接至具有公用 IP 的 Vm

如果您的目標 Vm (您想要使用 Windows Admin Center 管理的 Vm) 有公用 Ip，將它們新增到您 Windows Admin Center 閘道 IP 位址或 FQDN。 有幾個列入考量：

- 您必須啟用 WinRM 存取到您的目標 VM，藉由在目標 VM 上執行下列 PowerShell 或命令提示字元： `winrm quickconfig`
- 如果您還沒有加入網域的 Azure VM，VM 行為類似的伺服器工作群組中，因此您需要確定您考慮[使用 Windows Admin Center 在工作群組中](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。
- 您也必須啟用透過 HTTP 的輸入的連線至連接埠 5985 winrm，為了讓 Windows Admin Center 來管理 VM 的目標：
   1. 若要啟用客體 OS 上的連接埠 5985 輸入的連線的 VM 的目標上執行下列 PowerShell 指令碼：   
`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

   2. 您也必須在 Azure 網路中開啟連接埠：

    - 選取您的 Azure VM 中，選取 [**網路功能**，然後**新增輸入連接埠規則**。 
    - 請確定**基本**選取頂端的 [**新增輸入的安全性規則**] 窗格。
    - 在**連接埠範圍**欄位中輸入**5985**。
    
    如果您的 Windows Admin Center 閘道具有靜態 ip 位址，您可以選取僅允許輸入的 WinRM 存取從您的 Windows Admin Center 閘道增加安全性。
    若要這樣做，選取頂端的 [**新增輸入的安全性規則**窗格中的**進階**。

    針對**來源**，請選取**IP 位址**，然後輸入對應到您 Windows Admin Center 閘道的來源 IP 位址。

    - **通訊協定**選取**TCP**。
    - 其餘部分可以保留為預設值。

> [!NOTE]
> 您必須建立自訂的連接埠規則。 Azure 網路功能所提供的 WinRM 連接埠規則 （透過 HTTP) 會使用連接埠 5986 （透過 HTTPS) 而不是 5985。 

### 連線到虛擬機器不需要的公用 IP

如果您的目標 Azure Vm 不需要公用 Ip，而且您想要管理這些虛擬機器從您的內部網路中部署 Windows Admin Center 閘道，您需要設定您的內部網路連線到目標 Vm 都 VNet連接。 有 3 種方式，您可以這麼做： ExpressRoute、 網站對站 VPN 或點對站 VPN。 [了解哪一個連線選項適合您環境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>如果您想要使用點對站 VPN，來將 Windows Admin Center 閘道連線至 Azure VNet，來管理該 VNet Azure 虛擬機器，您可以使用 Windows Admin Center 中的[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)功能。 若要這樣做，連線到伺服器安裝 Windows Admin Center 所在，瀏覽至 [網路] 工具並選取 「 新增 Azure 網路介面卡 」。 當您提供必要的詳細資訊，並按一下 「 設定 」，Windows Admin Center 將會依據您指定，在那之後，您可以連線到，並從您的內部部署 Windows Admin Center 閘道管理 Azure Vm Azure vnet 點對站 VPN。

請確定 WinRM 正在執行您的目標 Vm 上在目標 VM 上執行下列 PowerShell 或命令提示字元： `winrm quickconfig`

如果您還沒有加入網域的 Azure VM，VM 行為類似的伺服器工作群組中，因此您需要確定您考慮[使用 Windows Admin Center 在工作群組中](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。

如果您遇到任何問題，請參閱[疑難排解 Windows Admin Center](../support/troubleshooting.md) ，以查看如果需要額外步驟的組態 （例如，如果您使用本機系統管理員帳戶進行連線，或未加入網域）。

## 使用 Azure 中部署 Windows Admin Center 閘道

您可以管理 Azure Vm 不需要任何內部相依性，藉由部署 Windows Admin Center 中您的目標 Vm 連線的位置 VNet。 

若要管理 Vm 在部署 Windows Admin Center 閘道時 VNet 之外，您必須建立 VNet-VNet 連線的 Windows Admin Center 閘道 VNet 之間的目標伺服器 VNet。 您可以建立此連線使用對等互連 VNet、 VNet-VNet 連線或網站-連線。 [了解更多關於哪些 VNet-VNet 連線選項適合您環境中。](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

可以在您的環境中的現有或最新部署 VM 上安裝 Windows Admin Center。 您選擇的 Windows Admin Center 安裝在 VM 必須有公用 IP 和 DNS 名稱。

[深入了解部署在 Azure 中的 Windows Admin Center 閘道](deploy-wac-in-azure.md)