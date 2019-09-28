---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: 同盟伺服器 Proxy 的憑證需求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: dab77c3e3226e89eb3ac9b74e7db9b6df8f181bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408143"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>同盟伺服器 Proxy 的憑證需求

在 Active Directory 同盟服務 \(AD FS @ no__t-1 中執行同盟伺服器 proxy 角色的伺服器，必須使用安全通訊端層 \(SSL @ no__t-3 伺服器驗證憑證。 同盟伺服器 proxy 會使用 SSL 伺服器驗證憑證，保護網頁伺服器與 Web 用戶端之間通訊流量的安全。  
  
同盟伺服器 proxy 通常會公開至網際網路上未包含在企業公開金鑰基礎結構中的電腦 \(PKI @ no__t-1。 因此，請使用由 public \(third @ no__t-1party @ no__t-2 憑證授權單位單位所發行的伺服器驗證憑證 \(CA @ no__t-4，例如 VeriSign。  
  
當您擁有同盟伺服器 proxy 伺服器陣列時，所有的同盟伺服器 proxy 電腦都必須使用相同的伺服器驗證憑證。 如需詳細資訊，請參閱 [建立同盟伺服器 Proxy 伺服器陣列的時機](When-to-Create-a-Federation-Server-Proxy-Farm.md)。  
  
請務必確認伺服器驗證憑證中的主體名稱符合 AD FS Management snap @ no__t-0in 中指定的同盟服務名稱值。 若要找出這個值，請開啟 no__t-0in、right @ no__t-1click**服務**，然後按一下 **編輯 同盟服務屬性**，然後在 **同盟服務名稱** 文字方塊中尋找值。  
  
如需使用 SSL 憑證的一般資訊，請參閱在 IIS 7.0 中設定安全通訊端層 \([HTTP： \/\/go.microsoft.com @ no__t-4fwlink @ no__t-5？ LinkID @ no__t-6108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) 和在 IIS 7.0 中設定伺服器憑證 \([HTTP： 01go.microsoft.com @ no__t-12fwlink @ no__t-13？ LinkID @ no__t-14108545](https://go.microsoft.com/fwlink/?LinkID=108545)5。  
  
> [!NOTE]  
> AD FS 同盟伺服器 proxy 不需要用戶端驗證憑證。  
  
如果您使用的任何憑證具有 \(CRLs @ no__t-1 的憑證撤銷清單，則具有所設定憑證的伺服器必須能夠與散發 Crl 的伺服器聯繫。 CRL 的類型決定了要使用的連接埠。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
