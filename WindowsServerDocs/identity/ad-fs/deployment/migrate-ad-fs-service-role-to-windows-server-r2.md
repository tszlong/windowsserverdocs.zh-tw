---
title: "Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2"
description: "AD FS 服務移轉到 Windows Server 2012 R2 的指示。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2
 本文件會提供下列角色服務移轉到 Active Directory 同盟 Services (AD FS) 與 Windows Server 2012 R2 安裝指示：  
  
-   安裝 Windows Server 2008 或 Windows Server 2008 R2 上 AD FS 2.0 聯盟伺服器  
  
-   安裝 Windows Server 2012 上 AD FS 聯盟伺服器  
  
## <a name="supported-migration-scenarios"></a>支援的移轉案例  
 本指南移轉指示包含下列工作：  
  
-   從您正在執行 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 的伺服器匯出 AD FS 2.0 設定的資料  
  
-   從 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 執行此伺服器作業系統的就地升級，以 Windows Server 2012 R2。 
  
-   重新建立原始 AD FS 設定與還原剩餘 AD FS 服務此伺服器，現在就執行 AD FS 伺服器角色所安裝的 Windows Server 2012 R2 上的設定。  
  
 本指南不包含移轉執行多個角色伺服器的指示操作。 如果您的伺服器執行多個角色，我們建議您設計特定自訂移轉處理程序伺服器環境，根據移轉的其他角色指南所提供的資訊。 適用於其他角色移轉指南可在[Windows 伺服器移轉入口網站](https://go.microsoft.com/fwlink/?LinkId=247608)。  
  
### <a name="supported-operating-systems"></a>支援的作業系統  
 伺服器作業系統目的：  
  
 Windows Server 2012 R2（Server Core 和完整安裝選項）  
  
 目的地伺服器處理器：  
  
 x64 型  
  
|來源伺服器的處理器|來源伺服器作業系統|  
|-----------------------------|------------------------------------|  
|x86-或 x64-基礎| Windows Server 2008、Server Core 安裝選項與完整|  
|x64 型|Windows Server 2008 R2|  
|x64 型|Windows Server 2008 R2 的 server Core 安裝選項|  
|x64 型|伺服器 Core 和 Windows Server 2012 的完整安裝選項|  
  
> [!NOTE]
>  -   除了上述表格中所列的版本是作業系統的組合舊的作業系統和支援的 service pack。  
> -   Windows Server 作業系統的基礎，Standard、企業版和 Datacenter 版本支援做為來源或目的伺服器。  
> -   實體作業系統與 virtual 作業系統之間移轉的支援。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>支援 AD FS 角色服務與功能  
 下表描述 AD FS 角色服務與他們所述本指南各個設定移轉案例。  
  
|從|若要安裝 Windows Server 2012 R2 AD FS|  
|----------|----------------------------------------------------------------------------------------------|  
|安裝 Windows Server 2008 或 Windows Server 2008 R2 上 AD FS 2.0 聯盟伺服器|支援移轉相同的伺服器上。 如需詳細資訊，請查看：<br /><br /> [正在準備移轉 AD FS 聯盟伺服器](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)|  
|安裝 Windows Server 2012 上 AD FS 聯盟伺服器|支援移轉相同的伺服器上。  如需詳細資訊查看：<br /><br /> [正在準備移轉 AD FS 聯盟伺服器](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>後續步驟
 [正在準備移轉 AD FS 聯盟伺服器](prepare-migrate-ad-fs-server-r2.md)   
 [移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)   
 [移轉 AD FS 聯盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
 [檢查 AD FS 移轉到 Windows Server 2012 R2](verify-ad-fs-migration.md)