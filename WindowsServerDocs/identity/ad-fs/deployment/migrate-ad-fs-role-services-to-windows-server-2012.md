---
title: "Active Directory 同盟服務角色服務移轉到 Windows Server 2012"
description: "AD FS 服務移轉到 Windows Server 2012 的指示。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Active Directory 同盟服務角色服務移轉到 Windows Server 2012

以下提供下列角色服務移轉到 Windows Server 2012 上的 Active Directory 同盟 Services (AD FS) 上的指示：  
  
-   AD FS 1.1 Windows 預付碼型代理程式和 AD FS 1.1 宣告感知代理程式安裝 Windows Server 2008 或 Windows Server 2008 R2  
  
-   AD FS 2.0 聯盟伺服器並安裝 Windows Server 2008 或 Windows Server 2008 R2 上 AD FS 2.0 聯盟伺服器 proxy    
  
## <a name="supported-migration-scenarios"></a>支援的移轉案例  
 移轉指示包含下列工作：  
  
-   從您正在執行 Windows Server 2008 的伺服器或 Windows Server 2008 R2 匯出 AD FS 2.0 設定的資料  
  
-   執行從 Windows Server 2008 或 Windows Server 2008 R2 的此伺服器作業系統的就地升級到 Windows Server 2012。
  
-   重新建立原始 AD FS 設定與還原剩餘 AD FS 服務此伺服器，現在就執行 AD FS 伺服器角色所安裝的 Windows Server 2012 上的設定。  
  
 本指南不包含移轉執行多個角色伺服器的指示操作。 如果您的伺服器執行多個角色，我們建議您設計特定自訂移轉處理程序伺服器環境，根據移轉的其他角色指南所提供的資訊。 適用於其他角色移轉指南可在[Windows 伺服器移轉入口網站](https://go.microsoft.com/fwlink/?LinkId=247608)。  
  
## <a name="supported-operating-systems"></a>支援的作業系統  
 **伺服器作業系統目的：**  
  

-  Windows Server 2012 或 Windows Server 2008 R2（Server Core 和完整安裝選項）  
  
 **目的地伺服器處理器：**  
  

-  x64 型  
  
|來源伺服器的處理器|來源伺服器作業系統|  
|-----|-----|  
|x86-或 x64-基礎|Windows Server 2003 含 Service Pack 2|  
|x86-或 x64-基礎|Windows Server 2003 R2|  
|x86-或 x64-基礎|Windows Server 2008、Server Core 安裝選項與完整|  
|x64 型|Windows Server 2008 R2|  
|x64 型|Windows Server 2008 R2 的 server Core 安裝選項|  
|x64 型|伺服器 Core 和 Windows Server 2012 的完整安裝選項|  
  
> [!NOTE]
>  -   除了上述表格中所列的版本是作業系統的組合舊的作業系統和支援的 service pack。  
> -   Windows Server 作業系統的基礎，Standard、企業版和 Datacenter 版本支援做為來源或目的伺服器。  
> -   實體作業系統與 virtual 作業系統之間移轉的支援。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>支援 AD FS 角色服務與功能  
 下表描述 AD FS 角色服務與他們所述本指南各個設定移轉案例。  
  
|從|若要安裝 Windows Server 2012 AD FS|  
|----------|-----|  
|AD FS 1.0 聯盟 server 與 Windows Server 2003 R2 一起安裝|不支援移轉|  
|AD FS 1.0 聯盟伺服器 proxy 安裝 Windows Server 2003 R2|不支援移轉|  
|AD FS 1.0 Windows 預付碼以安裝代理程式與 Windows Server 2003 R2|不支援移轉|  
|AD FS 1.0 宣告感知代理程式安裝 Windows Server 2003 R2）|不支援移轉|  
|安裝 Windows Server 2008 或 Windows Server 2008 R2 AD FS 1.1 聯盟伺服器|不支援移轉|  
|AD FS 1.1 聯盟伺服器 proxy 安裝 Windows Server 2008 或 Windows Server 2008 R2|不支援移轉|  
|AD FS 1.1 Windows 預付碼以安裝代理程式與 Windows Server 2008 或 Windows Server 2008 R2|支援移轉上相同的伺服器，但移轉的 AD FS Windows 預付碼型專員將只安裝 Windows Server 2008 或 Windows Server 2008 R2 AD FS 1.1 同盟服務與運作。 如需詳細資訊，請查看：<br /><br /> [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)<br /><br /> [AD FS 進行相互操作 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 宣告感知安裝代理程式與 Windows Server 2008 或 Windows Server 2008 R2）|支援移轉相同的伺服器上。 移轉的 AD FS 1.1 宣告感知 web 代理程式將會以下列功能：<br /><br /> 安裝 Windows Server 2008 或 Windows Server 2008 R2 AD FS 1.1 同盟服務<br /><br /> 安裝 Windows Server 2008 或 Windows Server 2008 R2 上 AD FS 2.0 同盟服務<br /><br /> 安裝 Windows Server 2012 AD FS 同盟服務<br /><br /> 如需詳細資訊，請查看：<br /><br /> [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)<br /><br /> [AD FS 進行相互操作 1.x](Interoperating-with-AD-FS-1.x.md)|  
|安裝 Windows Server 2008 或 Windows Server 2008 R2 上 AD FS 2.0 聯盟伺服器|支援移轉相同的伺服器上。 如需詳細資訊，請查看：<br /><br /> [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)|  
|安裝 Windows Server 2008 或 Windows Server 2008 R2 上 AD FS 2.0 聯盟伺服器 proxy|支援移轉相同的伺服器上。  如需詳細資訊查看：<br /><br /> [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>也了  
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)