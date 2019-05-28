---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: 放置同盟伺服器的位置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b883126f60950c0015b3a21e2ca5abc251b25b84
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190464"
---
# <a name="where-to-place-a-federation-server"></a>放置同盟伺服器的位置

安全性最佳做法是，Active Directory Federation Services 的地方\(AD FS\)同盟伺服器的防火牆前面，並連接至您公司的網路，以避免曝露在網際網路。 這很重要，因為同盟伺服器擁有完整權限可授與安全性權杖。 因此，它們應該像網域控制站一樣受到保護。 如果同盟伺服器遭到入侵，惡意使用者能夠為所有 Web 應用程式，Active Directory 同盟服務所保護的同盟伺服器發出完整存取權杖\(AD FS\)中所有的資源夥伴組織。  
  
> [!NOTE]  
> 基於安全性最佳做法，避免您直接存取的同盟伺服器在網際網路上。 為您的同盟伺服器直接存取網際網路，您要設定測試實驗室環境，或您的組織沒有周邊網路時，才應考慮。  
  
對於一般公司網路，內部\-對向防火牆公司網路與周邊網路和網際網路之間建立\-對向防火牆之間通常會建立周邊網路，網際網路。 在此情況下，同盟伺服器位於內部公司網路，並不是由網際網路用戶端直接存取。  
  
> [!NOTE]  
> 連線到公司網路的用戶端電腦可以直接與透過 Windows 整合式驗證的同盟伺服器通訊。  
  
同盟伺服器 proxy 與 AD FS 設定防火牆伺服器供使用之前應該置於周邊網路。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>設定同盟伺服器的防火牆伺服器  
內部網路防火牆伺服器，讓同盟伺服器可以直接與同盟伺服器 proxy 通訊，必須設定為允許安全超文字傳輸通訊協定\(HTTPS\)來自同盟伺服器 proxy，將流量同盟伺服器。 這是需求，因為內部網路防火牆伺服器必須發佈，讓同盟伺服器 proxy 在周邊網路中的可以存取的同盟伺服器使用連接埠 443 的同盟伺服器。  
  
此外，內部網路\-對向防火牆伺服器，例如執行 Internet Security and Acceleration server \(ISA\)伺服器，會使用稱為伺服器發佈的程序來發佈到網際網路的用戶端要求適當的公司同盟伺服器。 這表示，您必須手動執行 ISA Server 來發行叢集的同盟伺服器 URL，例如，http 的內部網路伺服器上建立的伺服器發佈規則：\/\/fs.fabrikam.com。  
  
如需有關如何在周邊網路中設定伺服器發佈的詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。 如需有關如何設定 ISA Server 以發佈伺服器的資訊，請參閱 <<c0> [ 建立安全的網頁發行規則](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
