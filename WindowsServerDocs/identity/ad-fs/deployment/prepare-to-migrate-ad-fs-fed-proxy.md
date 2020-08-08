---
title: 準備移轉 AD FS 2.0 同盟伺服器 Proxy
description: 提供準備將 AD FS 伺服器 proxy 遷移至 Windows Server 2012 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: e0a040dd3ec717e1315c842d6e6b407025ceabfd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940659"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>準備移轉 AD FS 2.0 同盟伺服器 Proxy

若要準備將 AD FS 2.0 同盟伺服器 proxy 遷移至 Windows Server 2012，您必須從這個伺服器 proxy 匯出並備份 AD FS 設定資料。  本主題中的步驟適用於具備一個 Proxy 同盟伺服器或多個 Proxy 同盟伺服器的案例。

 若要匯出 AD FS 設定資料，請執行下列工作：

-   [步驟 1：匯出 Proxy 服務設定](#step-1-export-proxy-service-settings)

-   [步驟 2：備份網頁的自訂項目](#step-2-back-up-webpage-customizations)

##  <a name="step-1-export-proxy-service-settings"></a>步驟 1：匯出 Proxy 服務設定
 若要匯出同盟伺服器 Proxy 服務設定，請執行下列程序：

### <a name="to-export-proxy-service-settings"></a>匯出 Proxy 服務設定

1.  將安全通訊端層 (SSL) 憑證及其私密金鑰匯出至 .pfx 檔案。 如需詳細資訊，請參閱 [Export the Private Key Portion of a Server Authentication Certificate](export-the-private-key-portion-of-a-server-authentication-certificate.md)。

> [!NOTE]
>  這個步驟是選擇性的，因為作業系統升級期間會保留這個憑證。

2. 將 AD FS 2.0 同盟 Proxy 屬性匯出至檔案。 您可以使用 Windows PowerShell 來執行此動作。

開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，將同盟 Proxy 屬性匯出至檔案： `PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`。

3. 請確定您知道帳戶的認證，且此帳戶必須是 AD FS 同盟伺服器的系統管理員帳戶或 AD FS 同盟服務據以執行的服務帳戶。  此為 Proxy 信任安裝的必要資訊。

   完成此步驟就會開始收集設定 AD FS 同盟伺服器 Proxy 所需的下列資訊：

-   AD FS 同盟服務名稱

-   Proxy 信任安裝需要網域帳戶的名稱。

-   HTTP Proxy 的位址和連接埠 (如果 AD FS 同盟伺服器 Proxy 與 AD FS 同盟伺服器之間有 HTTP Proxy 的話)

##  <a name="step-2-back-up-webpage-customizations"></a>步驟 2：備份網頁的自訂項目
 若要備份網頁自訂項目，請複製 AD FS Proxy 網頁和 **web.config** 檔案，它位於與 IIS 中虛擬路徑 **“/adfs/ls”** 對應的目錄中。  根據預設，該檔案位於 **%systemdrive%\inetpub\adfs\ls** 目錄。

## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)[準備遷移 AD FS 2.0 同盟伺服器 proxy](prepare-to-migrate-ad-fs-fed-proxy.md) [遷移 AD FS 2.0 同盟](migrate-the-ad-fs-fed-server.md)伺服器遷移[AD FS 2.0 同盟伺服器 proxy](migrate-the-ad-fs-2-fed-server-proxy.md) [遷移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)程式