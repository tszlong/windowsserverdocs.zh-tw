---
title: "移轉 AD FS 2.0 聯盟伺服器 proxy"
description: "AD FS 聯盟伺服器 proxy 移轉到 Windows Server 2012 提供相關資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>移轉 AD FS 2.0 聯盟伺服器 proxy
本文件會提供 AD FS 2.0 聯盟 proxy 伺服器移轉到 Windows Server 2012 上的詳細的資訊。

## <a name="migrate-the-proxy"></a>移轉 proxy

若要將 AD FS 2.0 聯盟伺服器 proxy 移轉到 Windows Server 2012，執行下列程序：  
  
1.  每個聯盟伺服器 proxy 想要移轉到 Windows Server 2012、檢視與執行中的程序[準備移轉 AD FS 2.0 聯盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)。  
  
2.  移除負載平衡器聯盟 proxy 伺服器。  
  
3.  此伺服器從 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 上執行作業系統的就地升級。 如需詳細資訊，請查看[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS proxy 設定將會遺失，並且移除 AD FS 2.0 伺服器角色。 Windows Server 2012 AD FS 伺服器角色已安裝改為，但未設定。 您必須手動建立原始 AD FS proxy 設定，並還原完成聯盟伺服器 proxy 移轉的剩餘 AD FS proxy 設定。  
  
4.  使用原始 AD FS proxy 設定建立**AD FS 聯盟伺服器 Proxy 設定精靈**。 如需詳細資訊，請查看[設定電腦聯盟 Proxy 角色](configure-a-computer-for-the-federation-server-proxy-role.md)。 為您執行精靈中，使用您所收集的資訊中準備移轉 AD FS 2.0 聯盟伺服器 Proxy，如下所示：  
  
 
|**聯盟 Proxy 伺服器] 精靈輸入的選項**|**使用下列值**|
|-----|-----|  
|**聯盟服務名稱**|輸入 BaseHostName 值從 proxyproperties.txt 檔案|  
|**傳送到此聯盟要求時使用 HTTP proxy 伺服器**服務核取方塊|如果您的檔案 proxyproperties.txt 包含 ForwardProxyUrl 屬性的值，檢查此方塊|  
|**HTTP proxy 伺服器位址**|輸入 ForwardProxyUrl 值從 proxyproperties.txt 檔案|  
|認證提示|輸入認證帳號，AD FS 聯盟伺服器的系統管理員或服務帳號 AD FS 同盟服務執行。|  
  
5.  更新您的 AD FS 網頁此伺服器上。 如果您的移轉準備您聯盟伺服器 proxy 時備份您自訂 AD FS proxy 網頁，使用您的備份資料覆寫預設 AD FS 預設中建立網頁**%systemdrive%\inetpub\adfs\ls**根據 Windows Server 2012 中 AD FS proxy 設定目錄。  
  
6.  [新增此伺服器回負載平衡器。  
  
7.  如果您有移轉其他 AD FS 2.0 聯盟伺服器 proxy，重複步驟 2 到 6 其餘聯盟伺服器 proxy 電腦。  
  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)