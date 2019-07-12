---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: 建立同盟伺服器 Proxy 伺服器陣列的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c33475d7420383448439e2b769562e55127c7b0e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190635"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>建立同盟伺服器 Proxy 伺服器陣列的時機

請考慮安裝其他的同盟伺服器 proxy，當您有大型的 Active Directory Federation Services \(AD FS\)部署，而您想要提供容錯、 負載\-平衡和延展性您的 proxy 部署。 在相同的周邊網路中建立兩個或多個同盟伺服器 proxy 及設定每個要保護相同的 AD FS Federation Service 會建立同盟伺服器 proxy 伺服器陣列。  
  
您可以建立同盟伺服器 proxy 伺服器陣列，或使用 AD FS 同盟伺服器 Proxy 設定精靈，將其他同盟伺服器 proxy 安裝到現有的伺服器陣列。 如需詳細資訊，請參閱 [建立同盟伺服器 Proxy 伺服器陣列的時機](When-to-Create-a-Federation-Server-Proxy.md)。  
  
所有同盟伺服器 proxy 可以一起都運作為伺服器陣列之前，您必須先將它們叢集在一個 IP 位址和一個網域名稱系統\(DNS\)完整的網域名稱\(FQDN\)。 您可以藉由部署 Microsoft 網路負載平衡叢集伺服器\(NLB\)在周邊網路內。 下表中的工作需要適當地設定為叢集伺服器陣列中的同盟伺服器 proxy 的 NLB。  
  
如需如何設定使用 Microsoft NLB 技術在叢集的 FQDN 的詳細資訊，請參閱[指定叢集參數](https://go.microsoft.com/fwlink/?linkid=74651)。  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>設定伺服器陣列的同盟伺服器 Proxy  
下表描述必須已完成，如此每個同盟伺服器 proxy 可以參與伺服器陣列中的工作。  
  
|工作|描述|  
|--------|---------------|  
|伺服器陣列中的所有 proxy 都指向相同的 AD FS 同盟服務名稱|當您建立同盟伺服器 proxy 時，您必須在 AD FS 同盟伺服器 Proxy 設定精靈中輸入相同的 Federation Service 名稱，將會參與伺服器陣列的所有同盟伺服器 proxy 的項目。 同盟伺服器 proxy 使用的 URL，可讓此 DNS 主機名稱，來判斷哪一個 AD FS 同盟服務執行個體的連絡人。<br /><br />如需詳細資訊，請參閱 <<c0> [ 同盟伺服器 Proxy 角色設定的電腦](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。|  
|取得及共用憑證|可以從公開憑證授權單位取得伺服器驗證憑證\(CA\)— 比方說，VeriSign，然後設定憑證，如此一來所有同盟伺服器 proxy 都共用相同私密金鑰部分在每部同盟伺服器 proxy 預設網站上的相同憑證。 若要共用憑證，您必須安裝在預設網站上相同的伺服器驗證憑證的每個同盟伺服器 proxy。 如需詳細資訊，請參閱 <<c0> [ 伺服器驗證憑證匯入至預設網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。<br /><br />如需詳細資訊，請參閱 <<c0> [ 同盟伺服器 Proxy 的憑證需求](Certificate-Requirements-for-Federation-Server-Proxies.md)。|  
  
如需有關加入新的同盟伺服器 proxy，若要建立同盟伺服器 proxy 伺服器陣列的詳細資訊，請參閱[檢查清單：設定同盟伺服器 Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
