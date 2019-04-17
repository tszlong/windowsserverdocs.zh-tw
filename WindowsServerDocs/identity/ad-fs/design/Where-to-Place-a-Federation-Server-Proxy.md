---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: "放置聯盟 Proxy 伺服器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server-proxy"></a>放置聯盟 Proxy 伺服器

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以將 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器 proxy 周邊網路提供對抗惡意的使用者可能會推出從網際網路的保護層級。 因為他們無法存取的私密金鑰可以用來建立權杖，聯盟伺服器 proxy 非常適合周邊網路環境中。 不過，聯盟伺服器 proxy 有效率可以傳送輸入要求授權製作權杖聯盟伺服器。  
  
您不需要將 account 協力廠商或資源合作夥伴聯盟伺服器中的企業網路 proxy，因為 client 電腦連接到企業網路，可以直接與聯盟伺服器通訊。 在本案例中，聯盟伺服器也提供聯盟 proxy 伺服器的功能來自公司網路 client 的電腦。  
  
Intranet\ 面向防火牆周邊網路通常是建立周邊網路之間的企業網路，並在 Internet\ 面向防火牆通常會建立周邊網路與網際網路之間。 在本案例中，聯盟 proxy 伺服器位於之間這兩個這些防火牆周邊網路上。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>適用於聯盟 proxy 伺服器設定防火牆伺服器  
聯盟伺服器 proxy 重新導向處理程序成功，必須允許安全超傳輸通訊協定 \(HTTPS\) 流量設定所有防火牆伺服器。 使用 HTTPS 是因為防火牆伺服器必須發行聯盟伺服器 proxy 使用連接埠 443，以便聯盟伺服器周邊網路 proxy 可以存取伺服器聯盟公司網路中。  
  
> [!NOTE]  
> 所有通訊 client 電腦的也可能會都發生在 HTTPS 上。  
  
此外，Internet\ 面向防火牆伺服器，例如電腦執行 Microsoft 網際網路安全性和加速 \(ISA\) 伺服器，使用處理程序稱為伺服器發行散發網際網路 client 要求適當周邊與公司網路伺服器，例如聯盟的 proxy 伺服器或聯盟伺服器。  
  
伺服器發行規則判斷伺服器發行的運作方式，基本上、 篩選透過 Isa 電腦的所有傳入的和傳出要求。 伺服器發行規則對應傳入 client 要求 Isa 電腦背後的適當的伺服器。 了解如何設定 Isa 發行伺服器的資訊，請查看[建立安全網路發行規則](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
在聯盟 AD FS 的世界中，特定的 URL，例如聯盟伺服器識別碼 URL 例如 http://fs.fabrikam.com 通常會做這些 client 要求。因為這些 client 要求會在從網際網路、Internet\ 面向防火牆伺服器必須設定為聯盟伺服器識別碼 URL 發行的每個聯盟伺服器 proxy 部署周邊網路中。  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>設定允許 SSL Isa  
若要加速安全 AD FS 通訊，您必須設定 Isa 允許之間下列安全通訊端層 \(SSL\) 通訊：  
  
-   **聯盟伺服器，並聯盟的 proxy 伺服器。** 需要的所有通訊聯盟伺服器 proxy 伺服器聯盟之間 SSL 的通道。 因此，您必須設定 Isa 允許 SSL 連接周邊網路之間公司網路。  
  
-   **Client 電腦、 聯盟伺服器及聯盟的 proxy 伺服器。** 使 client 電腦和聯盟伺服器或之間 client 電腦及聯盟伺服器 proxy 發生通訊，您可以將電腦執行 Isa 聯盟伺服器或聯盟伺服器 proxy 前面。  
  
    如果您的組織執行 SSL client 驗證聯盟伺服器或聯盟伺服器 proxy，當您將電腦執行 Isa 聯盟伺服器或聯盟伺服器 proxy 前面，必須設定伺服器的 pass\-透過 SSL 連接的因為 SSL 連接必須聯盟伺服器 proxy 伺服器聯盟或終止。  
  
    如果您的組織不執行聯盟伺服器或聯盟伺服器 proxy SSL client 驗證，其他選項是結束 SSL 連接的電腦執行 Isa 然後 re\-建立 SSL 聯盟伺服器或聯盟伺服器 proxy 連接。  
  
> [!NOTE]  
> 聯盟伺服器或聯盟伺服器 proxy 需要連接受保護的安全性權杖 SSL。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
