---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: 放置同盟伺服器 Proxy 的位置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9cc40920d366c973ace06a0b6d438a1c2d84b03e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190506"
---
# <a name="where-to-place-a-federation-server-proxy"></a>放置同盟伺服器 Proxy 的位置

您可以將 Active Directory Federation Services \(AD FS\)同盟伺服器 proxy 在周邊網路中提供一個對抗可能來自網際網路的惡意使用者的保護層。 同盟伺服器 proxy 非常適合於周邊網路環境，因為它們沒有用來建立權杖之私用金鑰的存取權。 不過，同盟伺服器 proxy 可以有效率地路由傳送連入要求已獲授權可以產生這些權杖的同盟伺服器。  
  
您不需要將放在公司網路內的同盟伺服器 proxy 帳戶夥伴或資源夥伴，因為連線到公司網路的用戶端電腦可以直接與同盟伺服器通訊。 在此案例中，同盟伺服器也提供同盟伺服器 proxy 的功能即將從公司網路的用戶端電腦。  
  
現況與周邊網路，內部網路一般\-對向防火牆周邊網路與公司網路和網際網路之間建立\-對向防火牆之間通常會建立周邊網路，網際網路。 在此案例中，同盟伺服器 proxy 位於周邊網路上的這些防火牆這兩者間。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>針對同盟伺服器 proxy 設定您的防火牆伺服器  
同盟伺服器 proxy 重新導向處理序才會成功，所有防火牆伺服器必須都設定成允許安全超文字傳輸通訊協定\(HTTPS\)流量。 需要使用 HTTPS，因為防火牆伺服器必須發佈同盟伺服器 proxy，使用連接埠 443，如此一來，同盟伺服器 proxy 在周邊網路中的可以存取公司網路中的同盟伺服器。  
  
> [!NOTE]  
> 與用戶端電腦之間的所有往來通訊也透過 HTTPS 發生。  
  
此外，網際網路\-對向防火牆伺服器，例如電腦執行 Microsoft Internet Security and Acceleration \(ISA\)伺服器，將網際網路用戶端使用稱為伺服器發佈的程序適當的周邊和公司網路的伺服器，例如同盟伺服器 proxy 或同盟伺服器的要求。  
  
伺服器發佈規則會決定伺服器發佈的運作方式 — 基本上，篩選通過 ISA Server 電腦的所有連入和連出要求。 伺服器發佈規則將連入用戶端要求對應至適當的 ISA Server 電腦背後的伺服器。 如需有關如何設定 ISA Server 以發佈伺服器的資訊，請參閱 <<c0> [ 建立安全網頁發佈規則](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
在 AD FS 的同盟世界中，這些用戶端要求通常對特定 URL 提出，例如，同盟伺服器的識別項 URL 如 http://fs.fabrikam.com。 因為這些用戶端要求，會在來自網際網路的網際網路\-對向防火牆伺服器必須設定為發佈的同盟伺服器的識別項 URL 以供部署在周邊網路中每部同盟伺服器 proxy。  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>設定 ISA Server 以允許 SSL  
若要促進安全的 AD FS 通訊，您必須設定 ISA Server 以允許安全通訊端層\(SSL\)下列之間的通訊：  
  
-   **同盟伺服器和同盟伺服器 proxy。** 同盟伺服器和同盟伺服器 proxy 之間的所有通訊需要 SSL 通道。 因此，您必須設定 ISA Server 以允許公司網路與周邊網路之間的 SSL 連線。  
  
-   **用戶端電腦、 同盟伺服器和同盟伺服器 proxy。** 以便用戶端電腦與同盟伺服器之間，或用戶端電腦與同盟伺服器 proxy 之間進行通訊，您可以將執行 ISA Server 的同盟伺服器或同盟伺服器 proxy 的電腦。  
  
    如果您的組織同盟伺服器或同盟伺服器 proxy，對執行 SSL 用戶端驗證，當您將執行 ISA Server 的同盟伺服器或同盟伺服器 proxy 的電腦，必須將伺服器設定階段\-透過 SSL 連線因為 SSL 連線必須在同盟伺服器或同盟伺服器 proxy 會終止。  
  
    如果您的組織不會執行 SSL 用戶端驗證同盟伺服器 proxy 的同盟伺服器上，其他選項是終止 SSL 連線，在電腦上執行 ISA Server，然後重新\-建立 SSL 連線同盟伺服器或同盟伺服器 proxy。  
  
> [!NOTE]  
> 同盟伺服器或同盟伺服器 proxy 需要連線經過 ssl 保護安全性權杖的內容。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
