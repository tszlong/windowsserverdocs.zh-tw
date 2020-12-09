---
title: 在 Azure 虛擬機器上安裝 Active Directory Domain Services
description: 如何在 Azure 虛擬機器上 (VM) 的虛擬機器上建立新的 Active Directory 樹系。
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 04/11/2019
ms.topic: article
ms.openlocfilehash: 3da3a66a6d414c5e80b5f84dc14c5e739a7f788a
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866337"
---
# <a name="install-a-new-active-directory-forest-using-azure-cli"></a>使用 Azure CLI 安裝新的 Active Directory 樹系

AD DS 可以在 Azure 虛擬機器上執行 (VM) 的方式，與在許多內部部署實例中執行的方式相同。 本文將逐步引導您在 Azure 可用性設定組中，使用 Azure 入口網站和 Azure CLI，在兩個新的網域控制站上部署新的 AD DS 樹系。 當您建立實驗室或準備在 Azure 中部署網域控制站時，許多客戶都會發現此指導方針很有用。

## <a name="components"></a>單元

* 要將所有專案放在其中的資源群組。
* [Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview.md)、子網、網路安全性群組和規則，可允許對 VM 進行 RDP 存取。
* Azure 虛擬機器 [可用性設定組](/azure/virtual-machines/windows/regions-and-availability#availability-sets) ，可將兩個 Active Directory Domain Services (AD DS) 網域控制站。
* 兩部要執行 AD DS 和 DNS 的 Azure 虛擬機器。

### <a name="items-that-are-not-covered"></a>未涵蓋的專案

* 從內部部署位置[建立站對站 VPN](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)連線
* [保護 Azure 中的網路流量](/azure/security/azure-security-network-security-best-practices.md)
* [設計網站拓撲](../../plan/designing-the-site-topology.md)
* [規劃操作主機角色放置](../../plan/planning-operations-master-role-placement.md)
* [部署 Azure AD Connect，以將身分識別同步處理至 Azure AD](/azure/active-directory/hybrid/how-to-connect-install-express)

## <a name="build-the-test-environment"></a>建立測試環境

我們會使用 [Azure 入口網站](https://portal.azure.com) 和 [Azure CLI](/cli/azure/overview) 來建立環境。

Azure CLI 可用來從命令列或在指令碼中建立和管理 Azure 資源。 本教學課程詳細說明如何使用 Azure CLI 來部署執行 Windows Server 2019 的虛擬機器。 部署完成後，我們會連接到伺服器，並安裝 AD DS。

如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free)。

### <a name="using-azure-cli"></a>使用 Azure CLI

下列腳本會將建立兩個 Windows Server 2019 Vm 的程式自動化，以針對 Azure 中新的 Active Directory 樹系建立網域控制站。 系統管理員可以修改下列變數，以符合其需求，然後再完成一項作業。 此腳本會建立必要的資源群組、具有遠端桌面、虛擬網路和子網的流量規則和可用性群組的網路安全性群組。 接著，每個 Vm 都會以 20 GB 的資料磁片建立，並停用快取以供安裝 AD DS。

您可以直接從 Azure 入口網站執行下列腳本。 如果您選擇在本機安裝和使用 CLI，本快速入門會要求您執行 Azure CLI 2.0.4 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI 2.0](/cli/azure/install-azure-cli)。

| 變數名稱 | 目的 |
| :---: | :--- |
| AdminUsername | 要在每個 VM 上設定為本機系統管理員的使用者名稱。 |
| AdminPassword | 要在每個 VM 上設定為本機系統管理員密碼的純文字密碼。 |
| resourceGroupName | 要用於資源群組的名稱。 不應複製現有的名稱。 |
| 位置 | 您想要部署的 Azure 位置名稱。 使用列出目前訂用帳戶的支援區域 `az account list-locations` 。 |
| VNetName | 指派 Azure 虛擬網路的名稱不應複製現有的名稱。 |
| VNetAddress | 要用於 Azure 網路的 IP 範圍。 不應複製現有的範圍。 |
| SubnetName | 指派 IP 子網的名稱。 不應複製現有的名稱。 |
| SubnetAddress | 網域控制站的子網位址。 應該是 VNet 內的子網。 |
| AvailabilitySet | 網域控制站 Vm 將加入之可用性設定組的名稱。 |
| VMSize | 部署位置中可用的標準 Azure VM 大小。 |
| DataDiskSize | AD DS 安裝的資料磁片大小（以 GB 為單位）。 |
| DomainController1 | 第一個網域控制站的名稱。 |
| DC1IP | 第一個網域控制站的 IP 位址。 |
| DomainController2 | 第二個網域控制站的名稱。 |
| DC2IP | 第二個網域控制站的 IP 位址。 |

```azurecli
#Update based on your organizational requirements
Location=westus2
ResourceGroupName=ADonAzureVMs
NetworkSecurityGroup=NSG-DomainControllers
VNetName=VNet-AzureVMsWestUS2
VNetAddress=10.10.0.0/16
SubnetName=Subnet-AzureDCsWestUS2
SubnetAddress=10.10.10.0/24
AvailabilitySet=DomainControllers
VMSize=Standard_DS1_v2
DataDiskSize=20
AdminUsername=azureuser
AdminPassword=ChangeMe123456
DomainController1=AZDC01
DC1IP=10.10.10.11
DomainController2=AZDC02
DC2IP=10.10.10.12

# Create a resource group.
az group create --name $ResourceGroupName \
                --location $Location

# Create a network security group
az network nsg create --name $NetworkSecurityGroup \
                      --resource-group $ResourceGroupName \
                      --location $Location

# Create a network security group rule for port 3389.
az network nsg rule create --name PermitRDP \
                           --nsg-name $NetworkSecurityGroup \
                           --priority 1000 \
                           --resource-group $ResourceGroupName \
                           --access Allow \
                           --source-address-prefixes "*" \
                           --source-port-ranges "*" \
                           --direction Inbound \
                           --destination-port-ranges 3389

# Create a virtual network.
az network vnet create --name $VNetName \
                       --resource-group $ResourceGroupName \
                       --address-prefixes $VNetAddress \
                       --location $Location \

# Create a subnet
az network vnet subnet create --address-prefix $SubnetAddress \
                              --name $SubnetName \
                              --resource-group $ResourceGroupName \
                              --vnet-name $VNetName \
                              --network-security-group $NetworkSecurityGroup

# Create an availability set.
az vm availability-set create --name $AvailabilitySet \
                              --resource-group $ResourceGroupName \
                              --location $Location

# Create two virtual machines.
az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController1 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC1IP \
    --no-wait

az vm create \
    --resource-group $ResourceGroupName \
    --availability-set $AvailabilitySet \
    --name $DomainController2 \
    --size $VMSize \
    --image Win2019Datacenter \
    --admin-username $AdminUsername \
    --admin-password $AdminPassword \
    --data-disk-sizes-gb $DataDiskSize \
    --data-disk-caching None \
    --nsg $NetworkSecurityGroup \
    --private-ip-address $DC2IP
```

## <a name="dns-and-active-directory"></a>DNS 和 Active Directory

如果在此程式中建立的 Azure 虛擬機器將是現有內部部署 Active Directory 基礎結構的延伸模組，則在部署之前，必須將虛擬網路上的 DNS 設定變更為包含您的內部部署 DNS 伺服器。 此步驟很重要，可讓 Azure 中新建立的網域控制站解析內部部署資源，並允許進行複寫。 如需有關 DNS、Azure 及如何設定設定的詳細資訊，請參閱 [使用您自己的 DNS 伺服器的名稱解析](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)一節。

在 Azure 中升級新的網域控制站之後，必須將這些網域控制站設定為虛擬網路的主要和次要 DNS 伺服器，而且任何內部部署的 DNS 伺服器都會降級為第三個或更高。 如需變更 DNS 伺服器的詳細資訊，請參閱 [建立、變更或刪除虛擬網路一](/azure/virtual-network/manage-virtual-network#change-dns-servers)文。

有關將內部部署網路擴充至 Azure 的資訊，請參閱 [建立站對站 VPN 連線一](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)文。

## <a name="configure-the-vms-and-install-active-directory-domain-services"></a>設定 Vm 並安裝 Active Directory Domain Services

腳本完成之後，請流覽至 [Azure 入口網站](https://portal.azure.com)，然後流覽至 [ **虛擬機器**]。

### <a name="configure-the-first-domain-controller"></a>設定第一個網域控制站

使用您在腳本中提供的認證連接到 AZDC01。

* 將資料磁片初始化並格式化為 F：
   * 開啟 [開始] 功能表並流覽至 [**電腦管理**]
   * 流覽至 **存放裝置**  >  **磁片管理**
   * 將磁片初始化為 MBR
   * 建立新的簡單磁片區並指派磁碟機號 F：如有需要，您可以提供磁片區標籤
* 使用伺服器管理員安裝 Active Directory Domain Services
* 將網域控制站升級為新樹系中的第一個網域控制站
   * 在 [網域控制站選項] 頁面上，將網域名稱系統 (DNS) 伺服器和通用類別目錄 (GC) 核取
   * 根據您的組織需求指定目錄服務還原模式密碼
   * 變更 C：的路徑，以指向我們在提示您輸入位置時所建立的 F：磁片磁碟機
   * 查看在嚮導中所做的選擇，然後選擇 **[下一步**]

> [!NOTE]
> 必要條件檢查會警告您，實體網路介面卡沒有 (es) 指派的靜態 IP 位址，您可以放心地忽略此情況，因為在 Azure 虛擬網路中指派靜態 ip。

* 選擇 **安裝**

當 wizard 完成安裝程式時，VM 會重新開機。

當 VM 完成重新開機時，請使用之前使用的認證重新登入，但這次是您建立之網域的成員。

   > [!NOTE]
   > 升級至網域控制站之後的第一次登入可能會花費較長的時間，而且這是正常的。 抓住一杯茶、咖啡、水或其他選擇的飲料。

[Azure 虛擬網路現在支援 ipv6](/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) ，但如果您想要將 vm 設定為優先于 ipv6，請參閱知識庫文章指引，以在 [Windows 中為 Advanced users 設定 IPv6](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users)。

### <a name="configure-dns"></a>設定 DNS

升級 Azure 中的第一部伺服器之後，必須將伺服器設定為虛擬網路的主要和次要 DNS 伺服器，而且任何內部部署 DNS 伺服器都會降級為第三個或更高。 如需變更 DNS 伺服器的詳細資訊，請參閱 [建立、變更或刪除虛擬網路一](/azure/virtual-network/manage-virtual-network#change-dns-servers)文。

### <a name="configure-the-second-domain-controller"></a>設定第二個網域控制站

使用您在腳本中提供的認證連接到 AZDC02。

* 將資料磁片初始化並格式化為 F：
   * 開啟 [開始] 功能表並流覽至 [**電腦管理**]
   * 流覽至 **存放裝置**  >  **磁片管理**
   * 將磁片初始化為 MBR
   * 建立新的簡單磁片區並指派磁碟機號 F： (如果想要的話，您可以提供磁片區標籤) 
* 使用伺服器管理員安裝 Active Directory Domain Services
* 升級網域控制站
   * 將網域控制站新增至現有網域-CONTOSO.com
   * 提供用來執行操作的認證
   * 變更 C：的路徑，以指向我們在提示您輸入位置時所建立的 F：磁片磁碟機
   * 確定已在 [網域控制站選項] 頁面上檢查網域名稱系統 (DNS) 伺服器和通用類別目錄 (GC) 
   * 根據您的組織需求指定目錄服務還原模式密碼
   * 查看在嚮導中所做的選擇，然後選擇 **[下一步**]

> [!NOTE]
> 必要條件檢查會警告您，實體網路介面卡沒有 (es) 指派的靜態 IP 位址。 您可以放心地忽略這種情況，因為靜態 Ip 是在 Azure 虛擬網路中指派的。

* 選擇 **安裝**

當 wizard 完成安裝程式時，VM 會重新開機。

當 VM 完成重新開機時，請使用之前使用的認證重新登入，但這次以 CONTOSO.com 網域的成員身分登入。

[Azure 虛擬網路現在支援 ipv6](/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6)，但如果您想要將 vm 設定為優先于 ipv6，請參閱知識庫文章指引，以在 [Windows 中為 Advanced users 設定 IPv6](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users)。

### <a name="wrap-up"></a>總結

此時，環境具有一對網域控制站，而我們已設定 Azure 虛擬網路，以便將額外的伺服器新增至環境。 Active Directory Domain Services 的後續安裝工作（例如設定網站和服務、審核、備份和保護內建的系統管理員帳戶）應該在此時完成。

## <a name="removing-the-environment"></a>移除環境

若要移除環境，當您完成測試時，可以刪除我們在上面建立的資源群組。 此步驟會移除屬於該資源群組的所有元件。

### <a name="remove-using-the-azure-portal"></a>使用 Azure 入口網站移除

在 Azure 入口網站中，流覽至 [ **資源群組** ]，然後選擇我們在此範例 ADonAzureVMs) 所建立的資源群組 (，然後選取 [ **刪除資源群組**]。 此程式會在刪除資源群組內包含的所有資源之前，先要求確認。

### <a name="remove-using-the-azure-cli"></a>使用 Azure CLI 移除

從 Azure CLI 執行下列命令：

```azurecli
az group delete --name ADonAzureVMs
```

## <a name="next-steps"></a>後續步驟

* [安全的虛擬化 Active Directory Domain Services (AD DS)](../../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)
* [Azure AD Connect](/azure/active-directory/connect/active-directory-aadconnect-get-started-express)
* [備份和復原](/azure/virtual-machines/windows/backup-recovery)
* [站對站 VPN 連線能力](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [監視](/azure/virtual-machines/windows/monitor)
* [安全性與原則](/azure/virtual-machines/windows/security-policy)
* [維護和更新](/azure/virtual-machines/windows/maintenance-and-updates)
