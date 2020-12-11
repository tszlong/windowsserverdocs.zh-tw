---
description: 深入瞭解：如何放置同盟伺服器 Proxy
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: 要在何處放置同盟伺服器 Proxy
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 734ac147176d7650c068e8920b4c704cdb936ab5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042046"
---
# <a name="where-to-place-a-federation-server-proxy"></a>要在何處放置同盟伺服器 Proxy

您可以將 Active Directory 同盟服務 \( AD FS \) 同盟伺服器 proxy 放在周邊網路，以針對可能來自網際網路的惡意使用者提供保護層。 同盟伺服器 proxy 非常適合於周邊網路環境，因為它們沒有用來建立權杖之私用金鑰的存取權。 但是，同盟伺服器 proxy 可以有效率地將連入要求路由傳送到授權產生這些權杖的同盟伺服器。

由於連線到公司網路的用戶端電腦可以直接與同盟伺服器通訊，因此不需要為帳戶夥伴或資源夥伴將同盟伺服器 proxy 放在公司網路內。 在此案例中，同盟伺服器也會為來自公司網路的用戶端電腦提供同盟伺服器 proxy 功能。

如同周邊網路一般， \- 在周邊網路與公司網路之間建立的內部網路防火牆， \- 通常是在周邊網路和網際網路之間建立的網際網路面向防火牆。 在此案例中，同盟伺服器 proxy 位於周邊網路上的這兩個防火牆之間。

## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>針對同盟伺服器 proxy 設定您的防火牆伺服器
若要讓同盟伺服器 proxy 重新導向程式成功，必須將所有防火牆伺服器設定為允許安全超文字傳輸通訊協定 \( HTTPS \) 流量。 使用 HTTPS 是必要的，因為防火牆伺服器必須使用埠443發佈同盟伺服器 proxy，讓周邊網路中的同盟伺服器 proxy 可以存取公司網路中的同盟伺服器。

> [!NOTE]
> 與用戶端電腦之間的所有往來通訊也透過 HTTPS 發生。

此外，網際網路對 \- 向防火牆伺服器（例如執行 Microsoft Internet Security And 加速 ISA server 的電腦 \( ）會 \) 使用稱為伺服器發佈的程式，將網際網路用戶端要求散發到適當的周邊和企業網路伺服器，例如同盟伺服器 proxy 或同盟伺服器。

伺服器發佈規則會決定伺服器發佈的運作方式 — 基本上，篩選通過 ISA Server 電腦的所有連入和連出要求。 伺服器發佈規則將連入用戶端要求對應至適當的 ISA Server 電腦背後的伺服器。 如需有關如何設定 ISA Server 以發佈伺服器的詳細資訊，請參閱 [建立安全的網頁發佈規則](https://go.microsoft.com/fwlink/?LinkId=75182)。

在 AD FS 的同盟世界中，這些用戶端要求通常是對特定 URL 提出，例如 HTTP：/fs.fabrikam.com 等同盟伺服器識別碼 URL \/ 。 因為這些用戶端要求來自網際網路，所以 \- 必須將網際網路面向的防火牆伺服器設定為針對周邊網路中部署的每個同盟伺服器 proxy 發佈同盟伺服器識別碼 URL。

### <a name="configuring-isa-server-to-allow-ssl"></a>設定 ISA Server 以允許 SSL
為了促進安全的 AD FS 通訊，您必須設定 ISA Server，以允許下列各項 \( 之間安全通訊端層 SSL \) 通訊：

-   **同盟伺服器和同盟伺服器 proxy。** 同盟伺服器與同盟伺服器 proxy 之間的所有通訊都需要 SSL 通道。 因此，您必須設定 ISA Server 以允許公司網路與周邊網路之間的 SSL 連線。

-   **用戶端電腦、同盟伺服器和同盟伺服器 proxy。** 如此一來，在用戶端電腦與同盟伺服器之間，或在用戶端電腦與同盟伺服器 proxy 之間進行通訊，您就可以將執行 ISA Server 的電腦放置在同盟伺服器或同盟伺服器 proxy 之前。

    如果您的組織在同盟伺服器或同盟伺服器 proxy 上執行 SSL 用戶端驗證，則當您將執行 ISA Server 的電腦放在同盟伺服器或同盟伺服器 proxy 之前，伺服器必須設定為 \- 通過 ssl 連線，因為 ssl 連線必須在同盟伺服器或同盟伺服器 proxy 上終止。

    如果您的組織不在同盟伺服器或同盟伺服器 proxy 上執行 SSL 用戶端驗證，還有一個額外的選項，就是在執行 ISA Server 的電腦上終止 SSL 連線，然後再 \- 對同盟伺服器或同盟伺服器 proxy 建立 ssl 連接。

> [!NOTE]
> 同盟伺服器或同盟伺服器 proxy 會要求使用 SSL 保護連線，以保護安全性權杖的內容。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
