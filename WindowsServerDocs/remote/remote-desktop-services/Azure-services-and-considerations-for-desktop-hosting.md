---
title: 桌面主機的 Azure 服務與考量
description: 深入了解到 Azure 中使用遠端桌面主機解決方案的唯一考量。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: 37210a5d75399309c53364f5b8ee9e06d26d6f32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849799"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>桌面主機的 Azure 服務與考量

>適用於：Windows Server （半年通道），Windows Server 2016

下列各節說明 Azure 基礎結構服務。
  
## <a name="azure-portal"></a>Azure 入口網站

此提供者建立的 Azure 訂用帳戶之後，Azure 入口網站可用來手動建立每個租用戶環境。 此程序也可以使用 PowerShell 指令碼進行自動化。  

如需詳細資訊，請瀏覽[Microsoft Azure](https://www.azure.microsoft.com)網站。
  
## <a name="azure-load-balancer"></a>Azure 負載平衡器

租用戶的元件會彼此進行通訊的虛擬機器上執行，在隔離網路上。 在部署過程中，您可以從外部存取這些虛擬機器透過 Azure 負載平衡器使用遠端桌面通訊協定端點或遠端 PowerShell 端點。 部署完成後，這些端點將通常會刪除以減少受攻擊面。 唯一的端點將會是 HTTPS 和 UDP 端點建立虛擬機器執行的 RD Web 和 RD 閘道元件。 這可讓用戶端上的網際網路連線到租用戶的桌面主機服務中執行的工作階段。 如果使用者開啟應用程式連線到網際網路，例如 web 瀏覽器中，連線會被傳遞給 Azure Load Balancer 中。  
  
如需詳細資訊，請參閱[什麼是 Azure Load Balancer？](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>安全性考量

此 Azure 桌面主機參考架構指南旨在提供高度安全且隔離的環境，每個租用戶。 系統安全性也取決於提供者所採取的託管服務部署和作業期間的保護措施。 下列清單描述提供者應該採取以保持其安全此參考架構為基礎的桌面主機解決方案的一些考量。

- 在理想情況下會隨機產生、 經常變更，而儲存在安全的集中位置只有幾個提供者選取的系統管理員才能存取，必須是強式、 所有的系統管理密碼。  
- 當複寫的新租用戶的租用戶環境，請避免使用相同或弱式系統管理密碼。
- RD Web 存取網站的 URL、 名稱和憑證必須是唯一且識別每個租用戶，以防止詐騙攻擊。  
- 一般作業期間的桌面主機服務，RD Web 和 RD 閘道虛擬機器，可讓使用者安全地連線到租用戶的桌面代管雲端服務以外的所有虛擬機器的所有公用 IP 位址都應該都刪除。 公用 IP 位址可能會暫時加入時所需的管理工作，但他們應該一律刪除之後。  
  
如需詳細資訊，請參閱下列文章：

- [安全性與保護](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [IIS 8 的安全性最佳作法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Secure Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>設計考量

請務必設計的多租用戶的桌面主機服務時，需要考量的 Microsoft Azure 基礎結構服務的條件約束。 下列清單描述提供者必須採取以達到功能且符合成本效益的桌面主機解決方案此參考架構為基礎的考量。  
  
- Azure 訂用帳戶已達上限的虛擬網路、 VM 核心和可用的雲端服務。 如果提供者需要比這更多資源，它們可能需要建立多個訂用帳戶。
- Azure 雲端服務有可用的虛擬機器的數目上限。 提供者可能需要建立超過最大值的較大型租用戶的多個雲端服務。  
- Azure 的部署成本根據部分的數目和大小的虛擬機器。 提供者應該最佳化來提供功能，並具有高度安全性的桌面主機環境成本最低的數目和每個租用戶的虛擬機器的大小。  
- 在 Azure 資料中心的實體電腦資源會使用 HYPER-V 虛擬化。 不在主機叢集中，設定 HYPER-V 主機，所以虛擬機器的可用性是取決於 Azure 基礎結構中使用個別的伺服器。 若要提供更高的可用性，每個角色服務虛擬機器的多個執行個體可以建立可用性設定組中則可以在虛擬機器內實作來賓叢集處理。  
- 在典型的儲存體設定中，服務提供者會有多個容器 （例如，一個用於每個租用戶），以及每個容器內的多個磁碟的單一儲存體帳戶。 不過，沒有限制的儲存體總計和單一儲存體帳戶可達成的效能。 對於支援大量的租用戶或高的儲存體容量或效能需求的租用戶的服務提供者，服務提供者可能需要建立多個儲存體帳戶。  
  
如需詳細資訊，請參閱下列文章：

- [雲端服務的大小](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Microsoft Azure 虛擬機器定價詳細資料](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [HYPER-V 概觀](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Azure 儲存體延展性和效能目標](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 應用程式 Proxy

Azure Active Directory (AD) 應用程式 Proxy 是付費的 Sku 的 Azure AD 可讓使用者連線到透過 Azure 自己的反向 proxy 服務的內部應用程式中提供的服務。 這可讓 RD Web 和 RD 閘道端點來隱藏內部虛擬網路，而不必對網際網路公開的公用 IP 位址。 主機服務提供者可以使用 Azure AD 應用程式 Proxy 來壓縮在租用戶的環境中的虛擬機器數目，同時仍可保有完整的部署。 Azure AD Application Proxy 也可讓 Azure AD 提供，例如條件式存取和 multi-factor authentication 的好處。

如需詳細資訊，請參閱 <<c0> [ 開始使用應用程式 Proxy 並安裝連接器](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable)。