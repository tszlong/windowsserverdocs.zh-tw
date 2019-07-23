---
title: 移轉 AD FS 2.0 同盟伺服器 proxy
description: 提供移轉到 Windows Server 2012 的 AD FS 同盟伺服器 proxy 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eddb0d3432c69ecff4542ff1b8f2204b96ce0820
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445659"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>移轉 AD FS 2.0 同盟伺服器 proxy
本文件提供有關移轉到 Windows Server 2012 的 AD FS 2.0 同盟 proxy 伺服器的詳細的資訊。

## <a name="migrate-the-proxy"></a>移轉 proxy

若要將 AD FS 2.0 同盟伺服器 proxy 移轉到 Windows Server 2012 中，執行下列程序：  
  
1.  針對您打算移轉到 Windows Server 2012 的每個同盟伺服器 proxy，請檢閱，並執行中的程序[準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)。  
  
2.  從負載平衡器中移除同盟伺服器 Proxy。  
  
3.  在此伺服器的 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 上執行作業系統就地的升級。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級會造成此伺服器上的 AD FS Proxy 設定遺失，且 AD FS 2.0 伺服器角色會被移除。 相反地，安裝 Windows Server 2012 的 AD FS 伺服器角色，但尚未進行設定。 您必須手動建立原始 AD FS Proxy 設定並還原剩餘的 AD FS Proxy 設定，來完成同盟伺服器 Proxy 移轉。  
  
4. 請使用 **AD FS 同盟伺服器 Proxy 設定精靈**建立原始 AD FS Proxy 設定。 如需詳細資訊，請參閱 [為電腦設定同盟伺服器 Proxy 角色](configure-a-computer-for-the-federation-server-proxy-role.md)。 當您執行精靈時，請使用您在「準備移轉 AD FS 2.0 同盟伺服器 Proxy」中收集的資訊，如下所示：  
  
 
|**同盟伺服器 Proxy 精靈輸入選項**|**使用下列值**|
|-----|-----|  
|**Federation Service 名稱**|輸入 proxyproperties.txt 檔案中的 BaseHostName 值|  
|**在將要求傳送至此同盟服務時使用 HTTP Proxy 伺服器**核取方塊|如果您的 proxyproperties.txt 檔案包含 ForwardProxyUrl 屬性的值，請核取此方塊|  
|**HTTP proxy 伺服器位址**|輸入 proxyproperties.txt 檔案中的 ForwardProxyUrl 值|  
|認證提示|輸入帳戶的認證，且此帳戶必須是 AD FS 同盟伺服器的系統管理員帳戶或 AD FS 同盟服務據以執行的服務帳戶。|  
  
5. 更新這個伺服器上您的 AD FS 網頁。 如果您的同盟伺服器 proxy 準備移轉時備份您自訂的 AD FS proxy 網頁，請使用備份資料來覆寫預設會在所建立的 AD FS 網頁 **%systemdrive%\inetpub\adfs\ls** Windows Server 2012 中 AD FS proxy 設定目錄。  
  
6. 將此伺服器重新新增至負載平衡器。  
  
7. 如果您有其他 AD FS 2.0 同盟伺服器 Proxy 要移轉，請針對其餘的同盟伺服器 Proxy 電腦重複執行步驟 2 到 6。  
  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)