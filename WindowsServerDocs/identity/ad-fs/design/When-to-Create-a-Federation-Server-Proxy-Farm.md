---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: "建立聯盟 Proxy 伺服器陣列的時機"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8935760cad272d5b82edb675cda85caf0456565f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>建立聯盟 Proxy 伺服器陣列的時機

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

請考慮安裝其他聯盟伺服器 proxy 當您有大量的 Active Directory 同盟服務 \(AD FS\) 部署，而您想要提供您 proxy 部署容錯、 load\ 平衡和擴充性。 在相同的周邊網路 proxy 伺服器建立兩個或更多聯盟和設定的每個保護相同 AD FS 同盟服務的動作，會建立聯盟 proxy 伺服器陣列。  
  
您可以建立聯盟 proxy 伺服器陣列或使用 AD FS 聯盟伺服器 Proxy 設定精靈現有發電廠安裝其他聯盟的 proxy 伺服器。 如需詳細資訊，請查看[當建立聯盟 Proxy 伺服器](When-to-Create-a-Federation-Server-Proxy.md)。  
  
聯盟伺服器 proxy 一起為陣列後才能正確運作，您必須先叢集他們在一個 IP 位址和一個網域名稱系統 \(DNS\) 完整的網域名稱 \(FQDN\)。 您可以藉由部署周邊網路中的 Microsoft 網路負載平衡 \(NLB\) 叢集伺服器。 下表中的工作需要 NLB 叢集發電廠聯盟 proxy 伺服器設定正確。  
  
如需有關如何設定 FQDN 叢集使用 Microsoft NLB 技術的詳細資訊，請查看[指定叢集參數](https://go.microsoft.com/fwlink/?linkid=74651)。  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>設定聯盟伺服器 proxy 伺服器陣列  
下表描述，因此每個聯盟伺服器 proxy 可以參與發電廠必須完成的工作。  
  
|工作|描述|  
|--------|---------------|  
|指向陣列中的所有 proxy 同名 AD FS 同盟服務|當您建立聯盟 proxy 伺服器時，您必須輸入 AD FS 聯盟伺服器 Proxy 設定精靈同盟服務名稱相同的所有聯盟伺服器 proxy 發電廠參與。 聯盟 proxy 伺服器使用，以判斷哪一個 AD FS 同盟服務執行個體此 DNS 主機名稱連絡人的 URL。<br /><br />如需詳細資訊，請查看[設定電腦聯盟 Proxy 角色](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。|  
|取得並分享憑證|您可以取得伺服器驗證憑證的公用憑證授權單位 \ (CA\)，例如 VeriSign-然後設定憑證，讓所有聯盟伺服器 proxy 都分享的每個聯盟伺服器 proxy 的相同金鑰的私密部分預設的網站上相同的憑證。 若要分享的憑證，您必須安裝預設的網站上相同的伺服器驗證憑證的每個聯盟伺服器 proxy。 如需詳細資訊，請查看[匯入伺服器驗證憑證的預設網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。<br /><br />如需詳細資訊，請查看[聯盟的 Proxy 伺服器的憑證需求](Certificate-Requirements-for-Federation-Server-Proxies.md)。|  
  
如新增新的聯盟伺服器 proxy 建立聯盟 proxy 伺服器陣列的相關詳細資訊，請查看[檢查清單︰ 設定好聯盟伺服器 Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
