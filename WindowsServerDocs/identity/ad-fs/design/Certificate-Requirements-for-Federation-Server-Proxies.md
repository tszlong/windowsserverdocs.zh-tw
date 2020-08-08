---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: 同盟伺服器 Proxy 的憑證需求
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 51eb0e72f52fafdb2f1b8bbb3f9a2850602c3f75
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954334"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>同盟伺服器 Proxy 的憑證需求

在 Active Directory 同盟服務 AD FS 中以同盟伺服器 proxy 角色執行的伺服器 \( ， \) 必須使用安全通訊端層 \( SSL \) 伺服器驗證憑證。 同盟伺服器 proxy 會使用 SSL 伺服器驗證憑證，保護網頁伺服器與 Web 用戶端之間通訊流量的安全。

同盟伺服器 proxy 通常會公開給網際網路上不包含在您的企業公開金鑰基礎結構 PKI 中的 \( 電腦 \) 。 因此，請使用由公用 \( 第三 \- 方 \) 憑證授權單位單位 CA 所發行的伺服器驗證憑證 \( \) ，例如 VeriSign。

當您擁有同盟伺服器 proxy 伺服器陣列時，所有的同盟伺服器 proxy 電腦都必須使用相同的伺服器驗證憑證。 如需詳細資訊，請參閱 [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md)。

請務必確認伺服器驗證憑證中的主體名稱符合 AD FS 管理嵌入式管理單元中指定的同盟服務名稱值 \- 。 若要找出這個值，請開啟嵌入式管理單元 \- ，以滑鼠右鍵 \- 按一下 [**服務**]，按一下 [**編輯] 同盟服務屬性**]，然後在 [**同盟服務名稱**] 文字方塊中尋找值。

如需使用 SSL 憑證的一般資訊，請參閱在 IIS 7.0 中設定安全通訊端層 \( [HTTP： \/ \/ go.microsoft.com \/ fwlink \/ ？LinkID \= 108544](https://go.microsoft.com/fwlink/?LinkID=108544) \) 和在 IIS 中設定伺服器憑證 7.0 \( [HTTP： \/ \/ go.microsoft.com \/ fwlink \/ ？LinkID \= 108545](https://go.microsoft.com/fwlink/?LinkID=108545) \) 。

> [!NOTE]
> AD FS 同盟伺服器 proxy 不需要用戶端驗證憑證。

如果您使用的任何憑證具有 crl 的憑證 \( \) ，則具有所設定憑證的伺服器必須能夠與散發 crl 的伺服器聯繫。 CRL 的類型決定了要使用的連接埠。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
