---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: 建立同盟伺服器 Proxy 伺服器陣列的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 247daf1b9b49124188f6bb16bce7da381fe997ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402428"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>建立同盟伺服器 Proxy 伺服器陣列的時機

當您有大型 Active Directory 同盟服務 \(AD FS\) 部署，而且想要為 proxy 部署提供容錯、負載\-平衡和擴充性時，請考慮安裝額外的同盟伺服器 proxy。 在相同的周邊網路中建立兩個或多個同盟伺服器 proxy，並將其設定為保護相同的 AD FS 的動作同盟服務建立同盟伺服器 proxy 伺服器陣列。  
  
您可以使用 [AD FS 同盟伺服器 Proxy 設定] Wizard，建立同盟伺服器 proxy 伺服器陣列，或將其他的同盟伺服器 proxy 安裝到現有的伺服器陣列。 如需詳細資訊，請參閱 [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md)。  
  
您必須先在一個 IP 位址和一個網域名稱系統（\(DNS\) 完整功能變數名稱 \(FQDN\)，將所有同盟伺服器 proxy 當做伺服器陣列一起運作。 您可以藉由在周邊網路內 \(NLB\) 部署 Microsoft 網路負載平衡，來叢集伺服器。 下表中的工作需要適當地設定 NLB，以叢集伺服器陣列中的同盟伺服器 proxy。  
  
如需如何使用 Microsoft NLB 技術為叢集設定 FQDN 的詳細資訊，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?linkid=74651)。  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>設定伺服器陣列的同盟伺服器 Proxy  
下表描述必須完成的工作，讓每個同盟伺服器 proxy 都可以參與伺服器陣列。  
  
|工作|描述|  
|--------|---------------|  
|將伺服器陣列中的所有 proxy 指向相同的 AD FS 同盟服務名稱|當您建立同盟伺服器 Proxy 時，您必須在 [AD FS 同盟伺服器 Proxy 設定] Wizard 中，為要參與伺服器陣列的所有同盟伺服器 Proxy 輸入相同的同盟服務名稱。 同盟伺服器 proxy 會使用組成此 DNS 主機名稱的 URL 來判斷它所聯絡的 AD FS 同盟服務實例。<br /><br />如需詳細資訊，請參閱 [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。|  
|取得及共用憑證|您可以從公開證書\)\(頒發機構單位（例如 VeriSign）取得伺服器驗證憑證，然後設定憑證，讓所有同盟伺服器 proxy 在預設網站上共用相同憑證的相同私密金鑰部分，以供每個同盟伺服器 proxy 使用。 若要共用憑證，您必須在每個同盟伺服器 proxy 的預設網站上安裝相同的伺服器驗證憑證。 如需詳細資訊，請參閱將[伺服器驗證憑證匯入至預設的網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。<br /><br />如需詳細資訊，請參閱 [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md)。|  
  
如需新增同盟伺服器 proxy 以建立同盟伺服器 proxy 伺服器陣列的詳細資訊，請參閱[檢查清單：設定同盟伺服器 proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
