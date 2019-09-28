---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: 放置同盟伺服器的位置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c64b28f0e62839ff771cd50c0f09b534861cf9e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358783"
---
# <a name="where-to-place-a-federation-server"></a>放置同盟伺服器的位置

基於安全性最佳作法，請將 Active Directory 同盟服務 \(AD FS @ no__t-1federation 伺服器放在防火牆前面，並將它們連線到您的公司網路，以避免暴露在網際網路上。 這很重要，因為同盟伺服器具有授與安全性權杖的完整授權。 因此，它們應該像網域控制站一樣受到保護。 如果同盟伺服器遭到入侵，惡意使用者就能夠將完整存取權杖發行至所有 Web 應用程式和受 Active Directory 同盟服務保護的同盟伺服器，@no__t 0AD FS @ no__t-1 在所有資源夥伴中各.  
  
> [!NOTE]  
> 基於安全性最佳作法，請避免在網際網路上直接存取您的同盟伺服器。 請考慮在您設定測試實驗室環境時，或當您的組織沒有周邊網路時，讓您的同盟伺服器直接存取網際網路。  
  
對於一般的公司網路，公司網路與周邊網路之間會建立內部網路 @ no__t-0facing 防火牆，而周邊網路與網際網路之間通常會建立網際網路 @ no__t-1facing 防火牆。 在此情況下，同盟伺服器位於公司網路內，且不能由網際網路用戶端直接存取。  
  
> [!NOTE]  
> 連線到公司網路的用戶端電腦可以透過 Windows 整合式驗證直接與同盟伺服器通訊。  
  
您必須先將同盟伺服器 proxy 放在周邊網路中，才能設定防火牆伺服器以與 AD FS 搭配使用。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>設定同盟伺服器的防火牆伺服器  
為了讓同盟伺服器能夠直接與同盟伺服器 proxy 通訊，內部網路防火牆伺服器必須設定為允許安全超文字傳輸通訊協定 \(HTTPS @ no__t-1 流量從同盟伺服器 proxy 到同盟伺服器。 這是一項需求，因為內部網路防火牆伺服器必須使用埠443發佈同盟伺服器，讓周邊網路中的同盟伺服器 proxy 可以存取同盟伺服器。  
  
此外，內部網路 @ no__t-0facing 防火牆伺服器（例如執行 Internet Security and 加速 \(ISA @ no__t-2 Server 的伺服器）會使用稱為伺服器發佈的程式，將網際網路用戶端要求散發到適當的公司同盟伺服器。 這表示您必須在執行 ISA Server 併發布叢集同盟伺服器 URL 的內部網路伺服器上，手動建立伺服器發佈規則，例如 HTTP： \/\/fs.fabrikam.com。  
  
如需有關如何在周邊網路中設定伺服器發佈的詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。 如需有關如何設定 ISA Server 以發佈伺服器的詳細資訊，請參閱[建立安全的 Web 發行規則](https://go.microsoft.com/fwlink/?LinkId=75182)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
