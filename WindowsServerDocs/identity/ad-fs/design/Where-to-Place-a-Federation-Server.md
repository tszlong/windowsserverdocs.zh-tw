---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: "放置聯盟伺服器的位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server"></a>放置聯盟伺服器的位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

基於安全性最佳練習、 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器前面防火牆然後將它們連接到您的企業網路，以避免遭受從網際網路。 因為聯盟伺服器有完整的授權以授與的安全性權杖，這很重要。 因此，它們應該會有為網域控制站的相同的保護。 受到聯盟伺服器，惡意使用者所有 Web 應用程式，並聯盟伺服器的 Active Directory 同盟服務 \(AD FS\) 資源合作夥伴每個組織都受發出權杖完整存取權的能力。  
  
> [!NOTE]  
> 安全性與最佳做法，請避免在網際網路上遇到聯盟伺服器直接存取。 請考慮實驗室測試或組織不具有周邊網路時，您的設定時，只提供您聯盟伺服器直接存取網際網路。  
  
一般的企業網路，intranet\ 面向防火牆建立公司網路和周邊網路，並在 Internet\ 面向防火牆通常會建立周邊網路與網際網路之間。 此時，聯盟伺服器位於中的企業網路，並不是用網際網路直接存取。  
  
> [!NOTE]  
> Client 電腦連接到企業網路，可以直接與透過 Windows 整合式驗證聯盟伺服器通訊。  
  
您在設定使用您防火牆伺服器 AD FS 進行之前，聯盟 proxy 伺服器應該會放在周邊網路。 如需詳細資訊，請查看[放置聯盟 Proxy 伺服器](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>設定防火牆伺服器聯盟伺服器  
使聯盟伺服器可以直接與聯盟的 proxy 伺服器通訊，必須聯盟伺服器安全超傳輸通訊協定 \(HTTPS\) 流量允許從聯盟 proxy 伺服器設定內部防火牆伺服器。 這是需求，因為內部防火牆伺服器必須發行使用連接埠 443 聯盟伺服器 proxy 周邊網路存取聯盟伺服器聯盟伺服器。  
  
此外，intranet\ 面向防火牆伺服器，例如執行的網際網路安全性與加速伺服器 \(ISA\) 伺服器，使用處理程序稱為伺服器發行散發網際網路 client 要求的適當公司聯盟伺服器。 這表示，您必須手動執行 Isa 發行叢集的聯盟伺服器的 URL，例如 http:///\/fs.fabrikam.com 內部伺服器上建立的伺服器發行規則。  
  
如需有關如何周邊網路中設定伺服器發行的詳細資訊，請查看[放置聯盟 Proxy 伺服器](Where-to-Place-a-Federation-Server-Proxy.md)。 了解如何設定 Isa 發行伺服器的資訊，請查看[建立安全的網頁發行規則](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
