---
title: "移轉 AD FS 2.0 聯盟伺服器 Proxy 準備"
description: "在準備好 AD FS proxy 伺服器移轉到 Windows Server 2012 中提供的資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f207993580e6fd06c9ff185e58e5b7e81af60252
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>移轉 AD FS 2.0 聯盟伺服器 Proxy 準備

準備 AD FS 2.0 聯盟伺服器 proxy 移轉到 Windows Server 2012 時，您必須匯出與從這個的 proxy 伺服器備份 AD FS 設定資料。  本主題中的步驟操作套用到的一個 proxy 聯盟伺服器的案例或多個 proxy 聯盟伺服器。  
  
 若要匯出 AD FS 設定資料，請執行下列工作：  
  
-   [步驟 1：匯出 proxy 服務設定](#step-1-export-proxy-service-settings)  
  
-   [步驟 2：備份網頁的自訂項目](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>步驟 1：匯出 proxy 服務設定  
 若要匯出聯盟伺服器 proxy 服務設定，請執行下列程序：  
  
### <a name="to-export-proxy-service-settings"></a>若要匯出 proxy 服務設定  
  
1.  安全通訊端層 (SSL) 憑證和匯出其私密金鑰.pfx 檔案。 如需詳細資訊，請查看[匯出私人鍵部分伺服器驗證憑證的](export-the-private-key-portion-of-a-server-authentication-certificate.md)。  
  
> [!NOTE]
>  這個步驟是選擇性的因為此憑證會在作業系統升級過程中保留。  
  
2.  檔案匯出 AD FS 2.0 聯盟 proxy 屬性。 您可以使用 Windows PowerShell 來執行的動作。  
  
打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以聯盟 proxy 屬性匯出至檔案：`PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`。  
  
3.  請確定您知道帳號，AD FS 聯盟伺服器的系統管理員或服務帳號 AD FS 同盟服務執行的認證。  Proxy 信任設定為需要這項資訊。  
  
 完成此步驟會導致收集所需設定您 AD FS 聯盟伺服器 proxy 下列資訊：  
  
-   AD FS 同盟服務名稱  
  
-   所需的安裝程式 proxy 信任的網域帳號名稱  
  
-   地址和（如果之間 AD FS 聯盟伺服器 proxy 伺服器 AD FS 聯盟 HTTP proxy）HTTP proxy 連接埠  
  
##  <a name="step-2-back-up-webpage-customizations"></a>步驟 2：備份網頁的自訂項目  
 若要備份網頁的自訂項目，複製 AD FS proxy 網頁和**web.config**檔案從 [對應至 virtual 路徑 directory **」日 adfs 日 ls]**在。  根據預設，這是在**%systemdrive%\inetpub\adfs\ls** directory。  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)