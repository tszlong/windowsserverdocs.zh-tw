---
title: 將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012
description: 提供移轉到 Windows Server 2012 的 AD FS 服務的指示。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850299"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012

下面提供的指示上將下列角色服務移轉到 Windows Server 2012 上的 Active Directory Federation Services (AD FS):  
  
-   AD FS 1.1 Windows 權杖型代理程式和 AD FS 1.1 宣告感知代理程式與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝  
  
-   AD FS 2.0 同盟伺服器和 Windows Server 2008 或 Windows Server 2008 R2 上安裝 AD FS 2.0 同盟伺服器 proxy    
  
## <a name="supported-migration-scenarios"></a>支援的移轉案例  
 移轉指示包含下列工作：  
  
-   匯出從您的伺服器執行 Windows Server 2008 或 Windows Server 2008 R2 的 AD FS 2.0 設定資料  
  
-   從 Windows Server 2008 或 Windows Server 2008 R2 中執行此伺服器的作業系統就地升級到 Windows Server 2012 中。
  
-   重新建立原始 AD FS 設定以及還原其餘的 AD FS 服務在此伺服器正在執行已安裝 Windows Server 2012 的 AD FS 伺服器角色上的設定。  
  
 本指南不包含移轉正在執行多個角色之伺服器的指示。 如果您的伺服器正在執行多個角色，則建議您根據其他角色移轉指南中提供的資訊，設計伺服器環境特有的自訂移轉程序。 您可以在 [Windows Server 移轉入口網站](https://go.microsoft.com/fwlink/?LinkId=247608)上取得其他角色的移轉指南。  
  
## <a name="supported-operating-systems"></a>支援的作業系統  
 **目的地伺服器作業系統：**  
  

-  Windows Server 2012 或 Windows Server 2008 R2 （Server Core 和完整安裝選項）  
  
 **目的地伺服器處理器：**  
  

-  x64 型  
  
|來源伺服器處理器|來源伺服器作業系統|  
|-----|-----|  
|x86 或 x64 型|Windows Server 2003 含 Service Pack 2|  
|x86 或 x64 型|Windows Server 2003 R2|  
|x86 或 x64 型|Windows Server 2008，完整和 Server Core 安裝選項兩者|  
|x64 型|Windows Server 2008 R2|  
|x64 型|Windows Server 2008 r2 的 server Core 安裝選項|  
|x64 型|Server Core 和 Windows Server 2012 的完整安裝選項|  
  
> [!NOTE]
>  -   上表中所列的作業系統版本是支援的最舊作業系統與 Service Pack 組合。  
> -   Windows Server 作業系統的 Foundation、 Standard、 Enterprise 和 Datacenter edition 支援作為來源或目的地伺服器。  
> -   支援在實體作業系統和虛擬作業系統之間移轉。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>支援 AD FS 角色服務和功能  
 下表說明 AD FS 角色服務和其各自的設定本指南中所述的移轉案例。  
  
|從|安裝 Windows Server 2012 的 AD FS|  
|----------|-----|  
|與 Windows Server 2003 R2 一起安裝的 AD FS 1.0 同盟伺服器|不支援移轉|  
|與 Windows Server 2003 R2 一起安裝的 AD FS 1.0 同盟伺服器 proxy|不支援移轉|  
|AD FS 1.0 Windows 權杖型代理程式與 Windows Server 2003 R2 一起安裝|不支援移轉|  
|AD FS 1.0 宣告感知代理程式與 Windows Server 2003 R2 一起安裝）|不支援移轉|  
|與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的 AD FS 1.1 同盟伺服器|不支援移轉|  
|與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的 AD FS 1.1 同盟伺服器 proxy|不支援移轉|  
|AD FS 1.1 Windows 權杖型代理程式與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝|支援同一部伺服器上的移轉，但移轉的 AD FS Windows 權杖型代理程式將只能與隨 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的 AD FS 1.1 同盟服務搭配運作。 如需詳細資訊，請參閱：<br /><br /> [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 宣告感知代理程式與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝）|支援相同伺服器的移轉。 移轉的 AD FS 1.1 宣告感知網路代理程式會使用下列函式：<br /><br /> 與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的 AD FS 1.1 同盟服務<br /><br /> Windows Server 2008 或 Windows Server 2008 R2 上安裝的 AD FS 2.0 同盟服務<br /><br /> 與 Windows Server 2012 安裝的 AD FS 同盟服務<br /><br /> 如需詳細資訊，請參閱：<br /><br /> [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Windows Server 2008 或 Windows Server 2008 R2 上安裝 AD FS 2.0 同盟伺服器|支援相同伺服器的移轉。 如需詳細資訊，請參閱：<br /><br /> [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)|  
|Windows Server 2008 或 Windows Server 2008 R2 上安裝的 AD FS 2.0 同盟伺服器 proxy|支援相同伺服器的移轉。  如需詳細資訊，請參閱：<br /><br /> [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>另請參閱  
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)