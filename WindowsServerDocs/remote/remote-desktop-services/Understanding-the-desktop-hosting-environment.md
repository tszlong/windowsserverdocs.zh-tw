---
title: 了解桌面主機環境
description: 使用 Azure IaaS RDS deployhment 的概觀。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 880fc8f9fa2db5ec56d2117e02c069650c61584a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877919"
---
# <a name="understanding-the-desktop-hosting-environment"></a>了解桌面主機環境

>適用於：Windows Server （半年通道），Windows Server 2016

下列資訊描述桌面主機服務的元件。  
  
## <a name="tenant-environment"></a>租用戶環境  
提供者的桌面主機服務會實作為一組隔離的租用戶環境中。 每個租用戶的環境是由儲存體容器、 一組的虛擬機器，以及透過隔離的虛擬網路的所有通訊的 Azure 服務，組合所組成。 每個虛擬機器包含一或多個元件組成的租用戶裝載的桌面環境。 下列各小節會描述組成每個租用戶裝載的桌面環境的元件。

## <a name="remote-desktop-services"></a>遠端桌面服務
在桌面的裝載環境中，會安裝在各種不同的虛擬機器之間的下列遠端桌面服務角色：

  - 遠端桌面連線代理人
  - 遠端桌面閘道
  - 遠端桌面授權
  - 遠端桌面工作階段主機
  - 遠端桌面 Web 存取

如需每個這些角色，以及它們如何彼此互動的完整說明，請檢閱[了解 RDS 角色](Understanding-RDS-roles.md)文件。
  
##  <a name="azure-active-directory-domain-services"></a>(Azure)Active Directory 網域服務  
有多種方式來連接和管理桌面的主機代管環境，在 Azure 中的 Active Directory 網域服務 (AD DS):

1. 在執行 AD DS 角色的租用戶的環境中建立虛擬機器
2. 使用租用戶的內部部署環境，以使用現有的 AD DS 中建立站對站 VPN 連線
3. 使用 Azure AD Domain Services PaaS 角色，以建立租用戶的租用戶的 Azure Active Directory 為基礎的虛擬網路上的網域

使用遠端桌面服務，租用戶必須有 Active Directory 來管理存取到環境中，使用者設定檔的儲存體，以及監視部署中。 當使用標準的 (非 Azure) AD DS，租用戶的樹系不需要任何與提供者的管理樹系的信任關係。 網域系統管理員帳戶可以設定租用戶的網域中允許 （例如，監視系統狀態和套用軟體更新） 的租用戶的環境中執行管理工作，並協助提供者的技術人員疑難排解和組態。  
    
其他資訊：  
[Azure Active Directory Domain Services 文件](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Azure 虛擬網路上安裝新的 Active Directory 樹系](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[具有站對站 VPN 連線使用 Azure 入口網站建立資源管理員 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Azure SQL Database  
Azure SQL Database 用來擴充其遠端桌面服務部署，而不需要部署和維護完整的 SQL Server Always-On 叢集中的主機服務提供者。 Azure SQL Database 使用遠端桌面連線代理人來儲存部署資訊，例如目前的使用者連線終端主機伺服器的對應。 如同其他 Azure 服務，Azure SQL DB 會遵循耗用量的模型，為適用於任何大小的部署。   
  
其他資訊：  
[什麼是 SQL Database？](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 應用程式 Proxy  
Azure Active Directory 應用程式 Proxy 是付費的 Sku 的 Azure Active Directory 可讓使用者連線到透過 Azure 自己的反向 proxy 服務的內部應用程式中提供的服務。 這可讓 RD Web 和 RD 閘道端點來隱藏內部虛擬網路，不需要對公用 IP 位址透過網際網路公開。 更進一步，這可讓主機服務提供者，同時仍可保有完整的部署濃縮在租用戶的環境中的虛擬機器數目。
  
其他資訊：  
[啟用 Azure AD 應用程式 Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>檔案伺服器  
檔案伺服器會使用伺服器訊息區塊 (SMB) 3.0 通訊協定，以提供共用的資料夾。 共用的資料夾用來建立及儲存使用者設定檔磁碟檔案 (.vhdx)，來備份資料，並允許使用者與租用戶的虛擬網路中的其他使用者共用資料的位置。
  
用來部署檔案伺服器 VM 必須有連接並使用 共用資料夾設定的 Azure 資料磁碟。 Azure 資料磁碟會使用直接寫入式快取寫入磁碟可確保保留到重新啟動之後的 VM。  
  
對於小型的租用戶，結合在租用戶的環境中的單一虛擬機器上執行的 RD 連線代理人及 RD 授權的角色的虛擬機器中的檔案伺服器可降低成本。  
  
其他資訊  
[檔案和存放服務概觀](https://technet.microsoft.com/library/hh831487.aspx)  
[如何將資料磁碟連接至虛擬機器](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>使用者設定檔磁碟  
使用者設定檔磁碟讓使用者儲存個人設定和檔案，當他們登入在集合中，RD 工作階段主機伺服器上的工作階段，並登入不同集合中的 RD 工作階段主機伺服器時，則有相同的設定和檔案存取權。 當使用者第一次登入時，租用戶的檔案伺服器上，建立使用者設定檔磁碟，且該磁碟掛接到使用者已連線的 RD 工作階段主機伺服器。 針對每個後續登入時，使用者設定檔磁碟會掛接到適當的 RD 工作階段主機伺服器，而且每個登出，取消掛接。 只能由該使用者存取設定檔磁碟的內容。  
  


