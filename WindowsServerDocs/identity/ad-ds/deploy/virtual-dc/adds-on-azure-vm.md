---
title: 在 Azure 虛擬機器上安裝 Active Directory Domain Services
description: 如何在 Azure 虛擬機器上的虛擬機器（VM）上建立新的 Active Directory 樹系。
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 04/11/2019
ms.technology: identity-adds
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: 536c35077d402370eab7758b8b31cf1916f8ae6f
ms.sourcegitcommit: 96db7769c3be9d7534bfed942697122ce907a28a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448470"
---
# <a name="install-a-new-active-directory-forest-using-azure-cli"></a>使用 Azure CLI 安裝新的 Active Directory 樹系

AD DS 可以在 Azure 虛擬機器（VM）上執行，就像在許多內部部署實例中執行一樣。 本文會逐步引導您在使用 Azure 入口網站和 Azure CLI 的 Azure 可用性設定組中，將新的 AD DS 樹系部署在兩個新的網域控制站上。 許多客戶在建立實驗室或準備在 Azure 中部署網域控制站時，會發現此指引很有説明。

## <a name="components"></a>單元

* 用來將所有專案放入其中的資源群組。
* [Azure 虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview.md)、子網、網路安全性群組和規則，以允許對 VM 的 RDP 存取。
* Azure 虛擬機器[可用性設定組](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets)，可將兩個 Active Directory Domain Services （AD DS）網域控制站放在中。
* 要執行 AD DS 和 DNS 的兩部 Azure 虛擬機器。

### <a name="items-that-are-not-covered"></a>未涵蓋的專案

* 從內部部署位置[建立站對站 VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)連線
* [保護 Azure 中的網路流量](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices.md)
* [設計網站拓撲](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology)
* [規劃操作主機角色位置](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/planning-operations-master-role-placement)
* [部署 Azure AD Connect 以同步處理身分識別至 Azure AD](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)

## <a name="build-the-test-environment"></a>建立測試環境

我們會使用[Azure 入口網站](https://portal.azure.com)和[Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)來建立環境。

Azure CLI 可用來從命令列或在指令碼中建立和管理 Azure 資源。 本教學課程詳細說明如何使用 Azure CLI 來部署執行 Windows Server 2019 的虛擬機器。 部署完成之後，我們會連接到伺服器並安裝 AD DS。

如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free)。

### <a name="using-azure-cli"></a>使用 Azure CLI

下列腳本會將建立兩個 Windows Server 2019 Vm 的程式自動化，以便在 Azure 中建立新 Active Directory 樹系的網域控制站。 系統管理員可以修改下列變數以符合其需求，然後完成一項作業。 此腳本會使用遠端桌面、虛擬網路和子網和可用性群組的流量規則，來建立必要的資源群組、網路安全性群組。 然後，每個 Vm 都會以 20 GB 的資料磁片建立，並已停用快取以供安裝 AD DS。

您可以直接從 Azure 入口網站執行下列腳本。 如果您選擇在本機安裝和使用 CLI，本快速入門會要求您執行 Azure CLI 2.0.4 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。

| 變數名稱 | 目的 |
| :---: | :--- |
| AdminUsername | 要在每個 VM 上設定為本機系統管理員的使用者名稱。 |
| AdminPassword | 要在每個 VM 上設定為本機系統管理員密碼的純文字密碼。 |
| resourceGroupName | 要用於資源群組的名稱。 不應該複製現有的名稱。 |
| Location | 您想要部署的 Azure 位置名稱。 使用，列出目前訂用帳戶的支援區域 `az account list-locations` 。 |
| VNetName | 指派 Azure 虛擬網路的名稱不應重複現有的名稱。 |
| VNetAddress | 要用於 Azure 網路的 IP 範圍。 不應該複製現有的範圍。 |
| SubnetName | 指派 IP 子網的名稱。 不應該複製現有的名稱。 |
| SubnetAddress | 網域控制站的子網位址。 應該是 VNet 內的子網。 |
| AvailabilitySet | 網域控制站 Vm 將加入的可用性設定組名稱。 |
| VMSize | 部署位置中可用的標準 Azure VM 大小。 |
| DataDiskSize | AD DS 安裝之資料磁片的大小（以 GB 為單位）。 |
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

如果在此程式中建立的 Azure 虛擬機器將成為現有內部部署 Active Directory 基礎結構的延伸模組，則必須在部署之前，將虛擬網路上的 DNS 設定變更為包含您的內部部署 DNS 伺服器。 此步驟很重要，因為允許在 Azure 中新建立的網域控制站解析內部部署資源，並允許進行複寫。 如需 DNS、Azure 和如何設定設定的詳細資訊，請參閱[使用您自己的 DNS 伺服器的名稱解析](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)一節。

在 Azure 中升級新的網域控制站之後，必須將其設定為虛擬網路的主要和次要 DNS 伺服器，而且任何內部部署 DNS 伺服器都會降級為第三和之後。 如需變更 DNS 伺服器的詳細資訊，請參閱[建立、變更或刪除虛擬網路一](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers)文。

如需將內部部署網路擴充至 Azure 的相關資訊，請參閱[建立站對站 VPN 連接一](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal
)文。

## <a name="configure-the-vms-and-install-active-directory-domain-services"></a>設定 Vm 並安裝 Active Directory Domain Services

腳本完成之後，流覽至 [ [Azure 入口網站](https://portal.azure.com)]，然後流覽至 [**虛擬機器**]。

### <a name="configure-the-first-domain-controller"></a>設定第一個網域控制站

使用您在腳本中提供的認證連接到 AZDC01。

* 將資料磁片初始化並格式化為 F：
   * 開啟 [開始] 功能表，然後流覽至 [**電腦管理**]
   * 流覽至**儲存體**  >  **磁片管理**
   * 將磁片初始化為 MBR
   * 建立新的簡單磁片區並指派磁碟機號 F：如有需要，您可以提供磁片區標籤
* 使用伺服器管理員安裝 Active Directory Domain Services
* 將網域控制站升階為新樹系中的第一個
   * 在 [網域控制站選項] 頁面上，將 [網域名稱系統（DNS）伺服器] 和 [通用類別目錄（GC）] 保持核取狀態
   * 根據您的組織需求指定目錄服務還原模式密碼
   * 變更 C：的路徑，使其指向我們在提示輸入其位置時所建立的 F：磁片磁碟機
   * 檢查在嚮導中所做的選擇，然後選擇 **[下一步]**

> [!NOTE]
> 必要條件檢查會警告您實體網路介面卡未指派靜態 IP 位址，您可以放心地忽略此情況，因為 Azure 虛擬網路中指派了靜態 ip。

* 選擇 [**安裝**]

當 wizard 完成安裝程式時，VM 會重新開機。

當 VM 完成重新開機時，請使用之前使用的認證重新登入，但這次是您所建立之網域的成員。

   > [!NOTE]
   > 升級至網域控制站後第一次登入的時間可能會比正常情況長。 拿一杯茶、咖啡、水或其他飲料。

[Azure 虛擬網路現在支援 ipv6](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) ，但如果您想要將您的 vm 設定為偏好 Ipv6 的 IPv4，請參閱知識庫文章在[Windows 中為高階使用者設定 IPv6 中的指引](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users)，以瞭解如何完成這項工作的相關資訊。

### <a name="configure-the-second-domain-controller"></a>設定第二個網域控制站

使用您在腳本中提供的認證連接到 AZDC02。

* 將資料磁片初始化並格式化為 F：
   * 開啟 [開始] 功能表，然後流覽至 [**電腦管理**]
   * 流覽至**儲存體**  >  **磁片管理**
   * 將磁片初始化為 MBR
   * 建立新的簡單磁片區，並指派磁碟機號 F：（您可以視需要提供磁片區標籤）
* 使用伺服器管理員安裝 Active Directory Domain Services
* 升級網域控制站
   * 將網域控制站新增至現有的網域-CONTOSO.com
   * 提供認證以執行操作
   * 變更 C：的路徑，使其指向我們在提示輸入其位置時所建立的 F：磁片磁碟機
   * 確定已在 [網域控制站選項] 頁面上核取 [網域名稱系統（DNS）伺服器] 和 [通用類別目錄（GC）]
   * 根據您的組織需求指定目錄服務還原模式密碼
   * 檢查在嚮導中所做的選擇，然後選擇 **[下一步]**

> [!NOTE]
> 必要條件檢查會警告您實體網路介面卡未指派靜態 IP 位址。 您可以放心地忽略此情況，因為 Azure 虛擬網路中已指派靜態 Ip。

* 選擇 [**安裝**]

當 wizard 完成安裝程式時，VM 會重新開機。

當 VM 完成重新開機時，請使用之前使用的認證重新登入，但這次是 CONTOSO.com 網域的成員

[Azure 虛擬網路現在支援 ipv6](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#do-vnets-support-ipv6) ，但如果您想要將您的 vm 設定為偏好 Ipv6 的 IPv4，請參閱知識庫文章在[Windows 中為高階使用者設定 IPv6 中的指引](https://support.microsoft.com/help/929852/guidance-for-configuring-ipv6-in-windows-for-advanced-users)，以瞭解如何完成這項工作的相關資訊。

### <a name="configure-dns"></a>設定 DNS

在 Azure 中升級新的網域控制站之後，必須將其設定為虛擬網路的主要和次要 DNS 伺服器，而且任何內部部署 DNS 伺服器都會降級為第三和之後。 如需變更 DNS 伺服器的詳細資訊，請參閱[建立、變更或刪除虛擬網路一](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers)文。

### <a name="wrap-up"></a>總結

此時，環境會有一對網域控制站，而我們已設定 Azure 虛擬網路，以便將其他伺服器新增至環境。 Active Directory Domain Services 的後續安裝工作，例如設定網站和服務、審核、備份以及保護內建的系統管理員帳戶，此時應該會完成此作業。

## <a name="removing-the-environment"></a>移除環境

若要移除環境，當您完成測試時，可以刪除上面所建立的資源群組。 此步驟會移除屬於該資源群組的所有元件。

### <a name="remove-using-the-azure-portal"></a>使用 Azure 入口網站移除

從 Azure 入口網站中，流覽至 [**資源群組**]，然後選擇我們所建立的資源群組（在此範例中為 ADonAzureVMs），然後選取 [**刪除資源群組**]。 在刪除資源群組內包含的所有資源之前，進程會要求確認。

### <a name="remove-using-the-azure-cli"></a>使用 Azure CLI 移除

從 Azure CLI 執行下列命令：

```azurecli
az group delete --name ADonAzureVMs
```

## <a name="next-steps"></a>後續步驟

* [安全的虛擬化 Active Directory Domain Services (AD DS)](../../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)
* [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express)
* [備份和復原](https://docs.microsoft.com/azure/virtual-machines/windows/backup-recovery)
* [站對站 VPN 連線能力](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [監視](https://docs.microsoft.com/azure/virtual-machines/windows/monitor)
* [安全性與原則](https://docs.microsoft.com/azure/virtual-machines/windows/security-policy)
* [維護和更新](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
