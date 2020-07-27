---
title: 桌面主機的 Azure 服務與考量
description: 了解 Azure 對於遠端桌面主機解決方案的唯一考量。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: lizross
ms.openlocfilehash: 434d4910e8718747c07fc7378eb37d9ff4e85710
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966700"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>桌面主機的 Azure 服務與考量

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

下列各節描述 Azure 基礎結構服務。
  
## <a name="azure-portal"></a>Azure 入口網站

提供者建立 Azure 訂用帳戶之後，可以使用 Azure 入口網站手動建立每個租用戶的環境。 此程序也可以使用 PowerShell 指令碼來自動化。  

如需詳細資訊，請瀏覽 [Microsoft Azure](https://www.azure.microsoft.com) 網站。
  
## <a name="azure-load-balancer"></a>Azure Load Balancer

租用戶元件會在於隔離網路上彼此通訊的虛擬機器上執行。 在部署程序中，您可以使用遠端桌面通訊協定端點或遠端 PowerShell 端點，透過 Azure Load Balancer 從外部存取這些虛擬機器。 部署完成之後，通常會刪除這些端點以縮小受攻擊面。 為執行 RD Web 和 RD 閘道元件的虛擬機器所建立唯一端點會是 HTTPS 和 UDP 端點。 這可讓網際網路上的用戶端連線到租用戶桌面主機服務中執行的工作階段。 如果使用者開啟連線到網際網路的應用程式 (例如網頁瀏覽器)，則會透過 Azure Load Balancer 傳遞連線。  
  
如需詳細資訊，請參閱[什麼是 Azure Load Balancer？](/azure/load-balancer/load-balancer-overview)
  
## <a name="security-considerations"></a>安全性考量

此《Azure 桌面主機參考架構指南》旨在為每個租用戶提供高度安全且隔離的環境。 系統安全性也取決於提供者在託管服務部署和作業期間所採取的保護措施。 下列清單描述提供者應該進行的一些考量，以確保基於此參考架構的桌面主機解決方案為安全。

- 所有系統管理密碼都必須為強式、適時隨機產生、經常變更，且儲存在僅供一些特定提供者系統管理員存取的安全集中位置中。  
- 複寫新租用戶的租用戶環境時，請避免使用相同或弱式的系統管理密碼。
- RD Web 存取網站 URL、名稱和憑證對每個租用戶都必須是唯一且可辨識，以防止詐騙攻擊。  
- 在桌面主機服務的一般作業期間，除了可讓使用者安全連線到租用戶桌面主機雲端服務的 RD Web 和 RD 閘道虛擬機器以外，應該刪除其他所有虛擬機器的所有公用 IP 位址。 如果管理工作需要，可暫時新增公用 IP 位址，但之後一律應刪除。  
  
如需詳細資訊，請參閱下列文章：

- [Security and protection](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831778(v=ws.11)) (安全性與保護)  
- [Security best practices for IIS 8](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj635855(v=ws.11)) (IIS 8 的安全性最佳做法)  
- [Secure Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831360(v=ws.11)) (保護 Windows Server 2012 R2)  
  
## <a name="design-considerations"></a>設計考量

設計多租用戶的桌面主機服務時，請務必考量 Microsoft Azure 基礎結構服務的限制。 下列清單描述提供者必須進行的考量，以達成基於此參考架構之運作正常且符合成本效益的桌面主機解決方案。  
  
- Azure 訂用帳戶已達可用的虛擬網路、VM 核心和雲端服務數目上限。 如果提供者需要比此上限更多的資源，其可能需要建立多個訂用帳戶。
- Azure 雲端服務具有可用的虛擬機器數目上限。 提供者可能需要為超過上限的更大型租用戶建立多個雲端服務。  
- Azure 部署成本部分取決於虛擬機器的數目和大小。 提供者應該針對每個租用戶將虛擬機器的數目和大小最佳化，以最低成本來提供運作正常且高度安全的桌面主機環境。  
- Azure 資料中心的實體電腦資源使用 Hyper-V 虛擬化。 Hyper-V 主機不是在主機叢集中設定，因此虛擬機器可用性取決於 Azure 基礎結構中所使用的個別伺服器可用性。 若要提供更高的可用性，您可以在可用性設定組中建立每個角色服務虛擬機器的多個執行個體，之後就可以在虛擬機器中實作客體叢集。  
- 在一般儲存體設定中，服務提供者會有單一儲存體帳戶，其中包含多個容器 (例如每個租用戶一個)，且每個容器中包含多個磁碟。 不過，單一儲存體帳戶可以達到的總儲存空間和效能有限。 如果服務提供者支援大量租用戶，或是支援具有高儲存容量或效能需求的租用戶，則服務提供者可能需要建立多個儲存體帳戶。  
  
如需詳細資訊，請參閱下列文章：

- [Sizes for Cloud Services](/azure/cloud-services/cloud-services-sizes-specs) (雲端服務的大小)  
- [Microsoft Azure 虛擬機器定價詳細資料](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Hyper-V overview](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v=ws.11)) (Hyper-V 概觀)  
- [Azure Storage scalability and performance targets](/azure/storage/common/storage-scalability-targets) (Azure 儲存體延展性和效能目標)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 應用程式 Proxy

Azure Active Directory (AD) 應用程式 Proxy 是 Azure AD 付費 SKU 中所提供的一項服務，可讓使用者透過 Azure 自己的反向 Proxy 服務連線到內部應用程式。 這讓 RD Web 和 RD 閘道端點可以在虛擬網路內隱藏，而不需要透過公用 IP 位址對網際網路公開。 主機服務提供者可以使用 Azure AD 應用程式 Proxy 來壓縮租用戶環境中的虛擬機器數目，同時仍保有完整的部署。 Azure AD 應用程式 Proxy 也支援 Azure AD 所提供的多項優點，例如條件式存取和多重要素驗證。

如需詳細資訊，請參閱[開始使用應用程式 Proxy 並安裝連接器](/azure/active-directory/manage-apps/application-proxy-enable)。
