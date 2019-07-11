---
title: 了解桌面主機環境
description: 使用 Azure IaaS RDS 的部署概觀。
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
ms.openlocfilehash: 1bdf4e3e25facfa8cc49459ada8d9b1b6309a724
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63744037"
---
# <a name="understanding-the-desktop-hosting-environment"></a>了解桌面主機環境

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

下列資訊描述桌面主機服務的元件。  
  
## <a name="tenant-environment"></a>租用戶環境  
提供者的桌面主機服務，會實作為一組隔離的租用戶環境。 每個租用戶的環境都由儲存體容器、一組虛擬機器和 Azure 服務所組成，這些服務全都透過隔離的虛擬網路進行通訊。 每部虛擬機器都包含組成租用戶裝載桌面環境的一或多個元件。 下列各小節介紹組成每個租用戶裝載桌面環境的元件。

## <a name="remote-desktop-services"></a>遠端桌面服務
在桌面主機環境中，下列遠端桌面服務角色會安裝於各種不同的虛擬機器中：

  - 遠端桌面連線代理人
  - 遠端桌面閘道
  - 遠端桌面授權
  - 遠端桌面工作階段主機
  - 遠端桌面 Web 存取

如需這些角色的完整描述並了解它們如何彼此互動，請檢閱[了解 RDS 角色](Understanding-RDS-roles.md)文件。
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Active Directory 網域服務  
有多種方式可連線並管理 Azure 中桌面主機環境的 Active Directory 網域服務 (AD DS)：

1. 在執行 AD DS 角色的租用戶環境中建立虛擬機器
2. 以租用戶的內部部署環境建立站對站 VPN 連線，以使用現有的 AD DS
3. 使用 Active Directory 網域服務 PaaS 角色，該角色會根據租用戶的 Azure Active Directory 在租用戶虛擬網路上建立網域

使用遠端桌面服務時，租用戶必須具有 Active Directory 以管理部署內的環境、使用者設定檔儲存體和監視的存取。 使用標準 (非 Azure) AD DS 時，租用戶樹系不需要與提供者的管理樹系具有任何信任關係。 您可以在租用戶的網域中設定網域系統管理員帳戶，以允許提供者的技術人員在租用戶環境中執行管理工作 (例如監視系統狀態與套用軟體更新)，並協助進行疑難排解和設定。  
    
其他資訊：  
[Azure Active Directory 網域服務文件](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[在 Azure 虛擬網路上安裝新的 Active Directory 樹系](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[使用 Azure 入口網站建立具有站對站 VPN 連線的資源管理員 VNet](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Azure SQL Database  
Azure SQL Database 允許主機服務提供者擴充其遠端桌面服務部署，而不需部署及維護完整的 SQL Server Always-On 叢集。 Azure SQL Database 使用遠端桌面連線代理人來儲存部署資訊，例如目前使用者連線與終端主機伺服器的對應。 如同其他 Azure 服務，Azure SQL DB 遵循耗用量模型，適用於任何大小的部署。   
  
其他資訊：  
[什麼是 SQL Database？](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 應用程式 Proxy  
Azure Active Directory 應用程式 Proxy 是 Azure Active Directory 付費 SKU 中所提供的一項服務，可讓使用者透過 Azure 自己的反向 Proxy 服務連線到內部應用程式。 這讓 RD Web 和 RD 閘道端點可以在虛擬網路內隱藏，而不需要透過公用 IP 位址對網際網路公開。 這可讓主機服務提供者壓縮租用戶環境中的虛擬機器數目，同時仍保有完整的部署。
  
其他資訊：  
[啟用 Azure AD 應用程式 Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>檔案伺服器  
檔案伺服器使用伺服器訊息區 (SMB) 3.0 通訊協定來提供共用資料夾。 共用資料夾用於建立並儲存使用者設定檔磁碟檔案 (.vhdx) 以備份資料，並提供使用者與租用戶虛擬網路中其他使用者共用資料的位置。
  
用於部署檔案伺服器的 VM 必須已連結 Azure 資料磁碟並設定共用資料夾。 Azure 資料磁碟使用直接寫入式快取，可確保在重新啟動 VM 時保持寫入磁碟。  
  
小型租用戶可以結合檔案伺服器與在租用戶環境中單一虛擬機器上執行 RD 連線代理人和 RD 授權角色的虛擬機器，進而降低成本。  
  
其他資訊  
[檔案和存放服務概觀](https://technet.microsoft.com/library/hh831487.aspx)  
[如何將資料磁碟連結至虛擬機器](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>使用者設定檔磁碟  
使用者設定檔磁碟可讓使用者在登入至集合中 RD 工作階段主機伺服器上的工作階段時，儲存個人設定和檔案，在登入至集合中不同的 RD 工作階段主機伺服器時，則可存取相同的設定和檔案。 使用者第一次登入時，使用者設定檔磁碟會在租用戶的檔案伺服器上建立，並將該磁碟掛接到使用者連線的 RD 工作階段主機伺服器。 之後每次登入時，使用者設定檔磁碟都會掛接到適當的 RD 工作階段主機伺服器，並在每次登出時取消掛接。 設定檔磁碟的內容只能由該使用者存取。  
  


