---
title: 開始使用使用者存取記錄
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15906e8cc1e5e85a471f1b8725435eb60852f6f5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382873"
---
# <a name="get-started-with-user-access-logging"></a>開始使用使用者存取記錄

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用者存取記錄（UAL）是 Windows Server 中的一項功能，可依本機伺服器上的角色和產品匯總用戶端使用資料。 它可協助 Windows server 系統管理員針對本機伺服器上的角色和服務，量化用戶端電腦的要求。  
  
預設會安裝並啟用 UAL，並以近乎即時的方式收集資料。 不需要系統管理員設定，雖然可以停用或啟用 UAL。 如需詳細資訊，請參閱 [Manage User Access Logging](Manage-User-Access-Logging.md)。 使用者存取記錄服務會將角色和產品的用戶端使用量資料匯總至本機資料庫檔案。  IT 系統管理員稍後可以使用 Windows Management Instrumentation (WMI) 或 Windows PowerShell Cmdlet 來擷取依伺服器角色 (或軟體產品)、依使用者、依裝置、依本機伺服器以及依日期的數量和執行個體。  
  
> [!NOTE]  
> UAL 支援 [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000)。  
  
## <a name="BKMK_APP"></a>實際應用  
UAL 會匯總登入本機資料庫的唯一用戶端裝置和使用者要求事件。 然後，這些記錄可供使用 (透過伺服器管理員的查詢)，依伺服器角色、使用者、裝置、本機伺服器以及日期來抓取數量和執行個體。  此外，UAL 也已擴充，可讓非 Microsoft 軟體發展人員檢測其 UAL 事件，以由 Windows Server 進行匯總。  
  
UAL 可以執行下列工作：  
  
-   量化本機實體或虛擬伺服器的用戶端使用者要求。  
  
-   量化本機實體或虛擬伺服器上已安裝軟體產品的用戶端使用者要求。  
  
-   抓取執行 Hyper-V 之本機伺服器上的資料，以識別 Hyper-V 虛擬機器上的高需求和低需求期間。  
  
-   從多個遠端伺服器抓取 UAL 資料。  
  
此外，軟體發展人員可以檢測 UAL 事件，然後使用 WMI 和 Windows PowerShell 介面來加以匯總和抓取。  
  
UAL 可以支援下列伺服器角色和服務：  
  
-   Active Directory 憑證服務 (AD CS)  
  
-   Active Directory Rights Management Services (AD RMS)  
  
-   BranchCache  
  
-   網域名稱系統 (DNS)  
  
    > [!NOTE]  
    > UAL 每 24 小時會收集 DNS 資料，此案例有一個單獨的 UAL Cmdlet。  
  
-   動態主機設定通訊協定 (DHCP)  
  
-   傳真伺服器  
  
-   檔案服務  
  
-   檔案傳輸通訊協定 (FTP) 伺服器  
  
-   Hyper-V  
  
    > [!NOTE]  
    > UAL 每 24 小時會收集 Hyper-V 資料，此案例有一個單獨的 UAL Cmdlet。  
  
-   Web 伺服器 (IIS)  
  
    > [!WARNING]  
    > 若要搭配使用 UAL 與 IIS，您必須使用 iisual.exe。 如需詳細資訊，請參閱 [使用 IIS 使用者存取記錄分析用戶端使用資料](http://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging)。  
  
-   Microsoft Message 佇列 (MSMQ) 服務  
  
-   網路原則與存取服務  
  
-   列印和文件服務  
  
-   路由及遠端存取服務 (RRAS)  
  
-   Windows 部署服務 (WDS)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> 不建議在直接連線到網際網路的伺服器上 (例如可存取網際網路位址空間的網頁伺服器)，或當伺服器的主要功能是提供極高效能的情況下 (例如在 HPC 工作負載環境) 使用 UAL。 UAL 主要是用於需要高容量的小型、中型和企業內部網路案例，但不像是定期提供網際網路面向流量的部署一樣高。  
  
## <a name="BKMK_NEW"></a>重要功能  
下表描述 UAL 的主要功能和其潛在價值。  
  
|功能|值|  
|-----------------|---------|  
|以接近即時的方式收集並彙總用戶端要求事件資料。|可以儲存最多三年的資料。 **重要事項：** 系統管理員必須利用組織的隱私權原則和當地法規，強制執行所收集資料和資料保留週期的合規性。|  
|使用 WMI 或 Windows PowerShell 介面來查詢 UAL，以擷取本機或遠端伺服器上的用戶端要求資料。|UAL 可以啟用進行中使用資料的單一檢視。 伺服器和企業系統管理員可以擷取這項資料，並且與商務管理員協調以最佳化其大量軟體授權的使用。|  
|預設為啟用。|伺服器系統管理員不需要設定這項功能，就可以使用所有核心功能。|  
  
## <a name="data-logged-with-ual"></a>使用 UAL 記錄的資料  
下列使用者相關資料會使用 UAL 加以記錄。  
  
|Data|描述|  
|--------|---------------|  
|**使用者名稱**|伴隨已安裝角色及產品之 UAL 項目的用戶端上的使用者名稱 (如果有的話)。|  
|**ActivityCount**|特定使用者存取角色或服務的次數。|  
|**FirstSeen**|使用者第一次存取角色或服務的日期和時間。|  
|**LastSeen**|使用者最後一次存取角色或服務的日期和時間。|  
|**ProductName**|提供 UAL 資料之軟體父產品 (如 Windows) 的名稱。|  
|**RoleGUID**|UAL 指派或登錄的 GUID，代表伺服器角色或已安裝產品。|  
|**RoleName**|提供 UAL 資料之角色、元件或子產品的名稱。 它也與 ProductName 和 RoleGUID 關聯。|  
|**TenantIdentifier**|已安裝角色之租用戶用戶端及伴隨 UAL 資料之產品的唯一 GUID (如果有的話)。|  
  
下列裝置相關資料會使用 UAL 加以記錄。  
  
|Data|描述|  
|--------|---------------|  
|**Ip**|用來存取角色或服務之用戶端裝置的 IP 位址。|  
|**ActivityCount**|特定裝置存取角色或服務的次數。|  
|**FirstSeen**|IP 位址第一次用來存取角色或服務的日期和時間。|  
|**LastSeen**|IP 位址最後一次用來存取角色或服務的日期和時間。|  
|**ProductName**|提供 UAL 資料之軟體父產品 (如 Windows) 的名稱。|  
|**RoleGUID**|UAL 指派或登錄的 GUID，代表伺服器角色或已安裝產品。|  
|**RoleName**|提供 UAL 資料之角色、元件或子產品的名稱。 它也與 ProductName 和 RoleGUID 關聯。|  
|**TenantIdentifier**|已安裝角色之租用戶用戶端及伴隨 UAL 資料之產品的唯一 GUID (如果有的話)。|  
  
## <a name="BKMK_SOFT"></a>軟體需求  
在 Windows Server 2012 之後執行 Windows Server 版本的任何電腦上，都可以使用 UAL。  
  
## <a name="see-also"></a>另請參閱  
MSDN 上的[使用者存取記錄](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) 。  
[管理使用者存取記錄](Manage-User-Access-Logging.md)  
  

