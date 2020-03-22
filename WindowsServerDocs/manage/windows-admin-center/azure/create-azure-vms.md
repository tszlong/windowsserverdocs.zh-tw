---
title: 使用 Windows 管理中心部署 Azure 虛擬機器
description: 使用 Windows 系統管理中心部署 Azure 虛擬機器。 將 Azure 虛擬機器設定為 Windows 管理中心管理案例的一部分。
ms.technology: manage
ms.topic: article
author: nedpyle
ms.author: nedpyle
manager: jgerend
ms.date: 01/28/2020
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 08135ed3454bb22db1c2b0fa3a14a8342fbc2dab
ms.sourcegitcommit: 8b801bd86e2ddf8255899b11f547daa920e5f651
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80110661"
---
# <a name="deploy-azure-virtual-machines-from-within-windows-admin-center"></a>在 Windows 系統管理中心內部署 Azure 虛擬機器

>適用于： Windows 系統管理中心、Windows 系統管理中心預覽

Windows Admin Center 1910 版可讓您部署 Azure 虛擬機器。 這會將 VM 部署整合到 Windows 管理中心管理的工作負載，例如[儲存體遷移服務](../../../storage/storage-migration-service/overview.md)和[儲存體複本](../../../storage/storage-replica/storage-replica-overview.md)。 不要在部署您的工作負載之前以手動方式在 Azure 入口網站中建立新的伺服器和 Vm，而是可能遺失必要的步驟和設定-Windows 管理中心可以部署 Azure VM、設定其儲存體、將其加入您的網域、安裝角色，以及然後設定您的分散式系統。 您也可以在沒有工作負載的情況下，從 Windows 管理中心的 [連線] 頁面部署新的 Azure Vm。

Windows 管理中心也會管理各種不同的 Azure 服務。 [深入瞭解 Windows 管理中心提供的 Azure 整合選項](../plan/azure-integration-options.md)。

## <a name="scenarios"></a>案例

Windows 管理中心1910版 Azure VM 部署支援下列案例：

- [儲存體遷移服務](../../../storage/storage-migration-service/overview.md)
- [儲存體複本](../../../storage/storage-replica/storage-replica-overview.md)
- [新的獨立伺服器（不含角色）](index.md#extend-on-premises-capacity-with-azure)

## <a name="requirements"></a>需求

若要從 Windows 系統管理中心建立新的 Azure VM，您必須具備：

- [Azure 訂](https://azure.microsoft.com)用帳戶。
- 向[Azure 註冊的 Windows 管理中心閘道](azure-integration.md)
- 您擁有建立許可權的現有[Azure 資源群組](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)。
- 現有的[Azure 虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)和子網。
- 系結至虛擬網路和子網的[Azure Express Route](https://azure.microsoft.com/services/expressroute/)或[azure VPN 解決方案](https://azure.microsoft.com/services/vpn-gateway/)，允許從 Azure vm 連線到您的內部部署用戶端、網域控制站、Windows 系統管理中心電腦，以及需要與此 VM 通訊的任何伺服器，做為工作負載部署的一部分。 比方說，若要使用儲存體遷移服務將儲存體遷移至 Azure VM，協調器電腦和來源電腦必須能夠與您要遷移的目的地 Azure VM 連線。

## <a name="usage"></a>使用方式

Azure VM 部署步驟和嚮導會因案例而異。 如需整體案例的詳細資訊，請參閱工作負載的檔。

### <a name="deploying-azure-vms-as-part-of-storage-migration-service"></a>將 Azure Vm 部署為儲存體遷移服務的一部分

1. 從 Windows 系統管理中心內的*儲存體遷移服務*工具，執行一或多個來源伺服器的清查。
2. 在*傳輸資料*階段中，選取 [*指定目的地*] 頁面上的 [**建立新的 Azure VM** ]，然後按一下 [**建立 VM**]。<br><br>
這會開始一個逐步建立工具，其會選取 Windows Server 2012 R2、Windows Server 2016 或 Windows Server 2019 Azure VM 作為遷移目的地。 儲存體遷移服務提供建議的 VM 大小以符合您的來源，但您可以按一下 [**查看所有大小**] 來覆寫它們。
<br><br>來源伺服器資料也會用來自動設定您的受控磁片和其檔案系統，以及將新的 Azure VM 加入您的 Active Directory 網域。 如果 VM 是 Windows Server 2019 （建議採用），Windows 系統管理中心會安裝儲存體遷移服務 proxy 功能。 建立 Azure VM 之後，Windows 管理中心會回到正常的儲存體遷移服務傳輸工作流程。  

### <a name="deploying-azure-vms-as-part-of-storage-replica"></a>將 Azure Vm 部署為儲存體複本的一部分

1. 從 Windows 系統管理中心內的 *儲存體複本* 工具，在 *合作關係* 索引標籤底下選取 **新增**，然後在 *使用其他伺服器*複寫 底下選取**使用新的 Azure VM**
2. 指定來源伺服器資訊和複寫組名，然後選取 **[下一步]** 。<br><br>
這會開始一種程式，自動選取 Windows Server 2016 或 Windows Server 2019 Azure VM 作為遷移來源的目的地。 儲存體遷移服務會建議 VM 大小以符合您的來源，但您可以選取 [**查看所有大小**] 來覆寫。 清查資料是用來自動設定您的受控磁片和其檔案系統，以及將您的新 Azure VM 加入您的 Active Directory 網域。 
3. 在 Windows 系統管理中心建立 Azure VM 之後，請提供複寫組名，然後選取 [**建立**]。 接著，Windows 管理中心會開始正常的儲存體複本初始同步處理常式，以開始保護您的資料。

以下影片顯示如何使用儲存體複本來遷移至 Azure Vm。

> [!VIDEO https://www.youtube-nocookie.com/embed/_VqD7HjTewQ] 

### <a name="deploying-a-new-standalone-azure-vm"></a>部署新的獨立 Azure VM

1. 從 Windows 管理中心的 [*所有*連線] 頁面中，選取 [**新增**]。
2. 在 [ *AZURE VM* ] 區段中，選取 [**新建**]。<br><br> 這是一項逐步建立的工具，可讓您選取 Windows Server 2012 R2、Windows Server 2016 或 Windows Server 2019 Azure VM、挑選大小、新增受控磁片，以及選擇性地加入您的 Active Directory 網域。

以下影片顯示如何使用 Windows 管理中心來建立 Azure Vm。

> [!VIDEO https://www.youtube-nocookie.com/embed/__A8J9aC_Jk] 
