---
title: 將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012
description: 提供將 AD FS 服務遷移至 Windows Server 2012 的指示。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cdb5523ade5c3c7572656d62d1b4f744683ec96e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408272"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012

以下提供將下列角色服務遷移至 Windows Server 2012 上 Active Directory 同盟服務（AD FS）的指示：  
  
-   AD FS 1.1 Windows 權杖型代理程式和 AD FS 1.1 宣告感知代理程式，隨 Windows Server 2008 或 Windows Server 2008 R2 安裝  
  
-   在 Windows Server 2008 或 Windows Server 2008 R2 上安裝 AD FS 2.0 同盟伺服器和 AD FS 2.0 同盟伺服器 proxy    
  
## <a name="supported-migration-scenarios"></a>支援的移轉案例  
 遷移指示包含下列工作：  
  
- 從執行 Windows Server 2008 或 Windows Server 2008 R2 的伺服器匯出 AD FS 2.0 設定資料  
  
- 將此伺服器的作業系統從 Windows Server 2008 或 Windows Server 2008 R2 就地升級到 Windows Server 2012。
  
- 重新建立原始的 AD FS 設定，並還原此伺服器上剩餘的 AD FS 服務設定，這現在正在執行與 Windows Server 2012 一起安裝的 AD FS 伺服器角色。  
  
  本指南不包含移轉正在執行多個角色之伺服器的指示。 如果您的伺服器正在執行多個角色，則建議您根據其他角色移轉指南中提供的資訊，設計伺服器環境特有的自訂移轉程序。 您可以在 [Windows Server 移轉入口網站](https://go.microsoft.com/fwlink/?LinkId=247608)上取得其他角色的移轉指南。  
  
## <a name="supported-operating-systems"></a>支援的作業系統  
 **目的地伺服器作業系統：**  
  

- Windows Server 2012 或 Windows Server 2008 R2 （Server Core 和完整安裝選項）  
  
  **目的地伺服器處理器：**  
  

- x64 型  
  
|來源伺服器處理器|來源伺服器作業系統|  
|-----|-----|  
|x86 或 x64 型|Windows Server 2003 含 Service Pack 2|  
|x86 或 x64 型|Windows Server 2003 R2|  
|x86 或 x64 型|Windows Server 2008，包括完整和 Server Core 安裝選項|  
|x64 型|Windows Server 2008 R2|  
|x64 型|Windows Server 2008 R2 的伺服器核心安裝選項|  
|x64 型|Windows Server 2012 的 Server Core 和完整安裝選項|  
  
> [!NOTE]
> - 上表中所列的作業系統版本是支援的最舊作業系統與 Service Pack 組合。  
>   -   Windows Server 作業系統的 Foundation、Standard、Enterprise 和 Datacenter 版本可支援做為來源或目的地伺服器。  
>   -   支援在實體作業系統和虛擬作業系統之間移轉。  
  
### <a name="supported-ad-fs-role-services-and-features"></a>支援的 AD FS 角色服務和功能  
 下表描述 AD FS 角色服務的遷移案例，以及本指南中所述的個別設定。  
  
|從|若要使用 Windows Server 2012 安裝 AD FS|  
|----------|-----|  
|隨 Windows Server 2003 R2 安裝的 AD FS 1.0 同盟伺服器|不支援移轉|  
|與 Windows Server 2003 R2 一起安裝的 AD FS 1.0 同盟伺服器 proxy|不支援移轉|  
|AD FS 1.0 以 Windows Server 2003 R2 安裝的 Windows 權杖型代理程式|不支援移轉|  
|AD FS 1.0 與 Windows Server 2003 R2 一起安裝的宣告感知代理程式）|不支援移轉|  
|隨 Windows Server 2008 或 Windows Server 2008 R2 安裝的 AD FS 1.1 同盟伺服器|不支援移轉|  
|與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的 AD FS 1.1 同盟伺服器 proxy|不支援移轉|  
|AD FS 1.1 以 Windows Server 2008 或 Windows Server 2008 R2 安裝的 Windows 權杖型代理程式|支援在同一部伺服器上進行遷移，但遷移的 AD FS Windows 權杖型代理程式只能與隨 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的 AD FS 1.1 federation service 搭配運作。 如需詳細資訊，請參閱：<br /><br /> [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)<br /><br /> [與 AD FS 1.x 互通](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 與 Windows Server 2008 或 Windows Server 2008 R2 一起安裝的宣告感知代理程式）|支援相同伺服器的移轉。 已遷移的 AD FS 1.1 宣告感知 web 代理程式將與下列內容搭配運作：<br /><br /> 隨 Windows Server 2008 或 Windows Server 2008 R2 安裝的 AD FS 1.1 federation service<br /><br /> 安裝在 Windows Server 2008 或 Windows Server 2008 R2 上的 AD FS 2.0 federation service<br /><br /> 與 Windows Server 2012 一起安裝 AD FS federation service<br /><br /> 如需詳細資訊，請參閱：<br /><br /> [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)<br /><br /> [與 AD FS 1.x 互通](Interoperating-with-AD-FS-1.x.md)|  
|安裝在 Windows Server 2008 或 Windows Server 2008 R2 上的 AD FS 2.0 同盟伺服器|支援相同伺服器的移轉。 如需詳細資訊，請參閱：<br /><br /> [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)|  
|在 Windows Server 2008 或 Windows Server 2008 R2 上安裝 AD FS 2.0 同盟伺服器 proxy|支援相同伺服器的移轉。  如需詳細資訊，請參閱：<br /><br /> [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>另請參閱  
 [準備將 AD FS 2.0 同盟伺服器遷移](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備將 AD FS 2.0 同盟伺服器 Proxy 遷移](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [將 AD FS 2.0 同盟伺服器遷移](migrate-the-ad-fs-fed-server.md)   
 [遷移 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)