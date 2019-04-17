---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: "聯盟的 Proxy 伺服器的憑證需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7fb8e71afed1c0eb6b55857835d95f2dd0ec9d5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>聯盟的 Proxy 伺服器的憑證需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用安全通訊端層 \(SSL\) 伺服器驗證憑證需要聯盟伺服器 proxy 中的角色 \(AD FS\) Active Directory 同盟服務正在執行的伺服器。 聯盟伺服器 proxy 使用 SSL 伺服器驗證憑證安全網路戶端與 Web 伺服器流量的通訊。  
  
網際網路上的電腦未包含在您企業公用基礎結構通常公開聯盟伺服器 proxy \(PKI\)。 因此，使用伺服器驗證憑證的公用 \(third\-party\) 憑證授權單位發行 \(CA\)，例如 VeriSign。  
  
當您有 proxy 發電廠聯盟伺服器時，所有聯盟伺服器 proxy 電腦必須都使用相同的伺服器驗證憑證。 如需詳細資訊，請查看[當建立聯盟 Proxy 伺服器陣列](When-to-Create-a-Federation-Server-Proxy-Farm.md)。  
  
請務必驗證中伺服器驗證憑證的主體名稱符合 AD FS 管理 snap\ 中指定的同盟服務名稱值。 要尋找此值，請打開 snap\ 中 right\ 按一下**服務**，按一下 [**編輯同盟服務屬性**，然後尋找中的值**同盟服務名稱**文字方塊。  
  
一般使用 SSL 憑證的詳細資訊，會看到設定安全通訊端層 IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/ 嗎？LinkID\ = 108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) 和設定伺服器的憑證在 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/ 嗎？LinkID\ = 108545](https://go.microsoft.com/fwlink/?LinkID=108545)\)。  
  
> [!NOTE]  
> Client 驗證憑證並不需要 AD FS 聯盟伺服器 proxy。  
  
如果您使用的任何憑證有撤銷列出 \(CRLs\) 憑證，設定憑證伺服器必須連絡分散 Crl 伺服器。 CRL 類型會判斷使用何種連接埠。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
