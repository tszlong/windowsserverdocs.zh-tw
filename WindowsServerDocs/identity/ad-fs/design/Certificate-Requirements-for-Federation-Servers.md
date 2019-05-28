---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: 同盟伺服器的憑證需求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ce301f6320ed3347b1ee802f57c2b2ebd4394970
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191636"
---
# <a name="certificate-requirements-for-federation-servers"></a>同盟伺服器的憑證需求

在任何 Active Directory Federation Services \(AD FS\)設計，必須使用各種憑證來保護通訊，並協助執行網際網路用戶端與同盟伺服器之間的使用者驗證。 每一部同盟伺服器必須擁有服務通訊憑證和權杖\-簽署憑證，才能參與 AD FS 通訊之前。 下表描述與同盟伺服器相關聯的憑證類型。  
  
|憑證類型|描述|  
|--------------------|---------------|  
|語彙基元\-簽署憑證|語彙基元\-簽署憑證是 X509 憑證。 同盟伺服器會使用相關聯的公用\/私用金鑰組，以數位方式簽署它們產生的所有安全性權杖。 這包括簽署已發佈的同盟中繼資料及成品解析要求。<br /><br />您可以有多個語彙基元\-簽章憑證設定 AD FS 管理嵌入式管理單元中\-在一個憑證即將到期時，允許進行憑證變換。 根據預設，清單中的所有憑證都會都發佈，但只有主要權杖\-可到由 AD FS 來實際簽署權杖簽署憑證。 您選取的所有憑證都必須具備相對應的私密金鑰。<br /><br />如需詳細資訊，請參閱 [Token-Signing Certificates](Token-Signing-Certificates.md) 和 [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md)。|  
|服務通訊憑證|同盟伺服器會使用伺服器驗證憑證，也就是 Windows Communication Foundation 服務通訊\(WCF\)訊息安全性。 根據預設，這是相同的憑證，同盟伺服器做為 Secure Sockets Layer \(SSL\) Internet Information Services 中的憑證\(IIS\)。 **注意：** AD FS 管理嵌入式管理單元\-在參考同盟伺服器，做為服務通訊憑證的伺服器驗證憑證。<br /><br />如需詳細資訊，請參閱 <<c0> [ 服務通訊憑證](Service-Communications-Certificates.md)並[設定服務通訊憑證](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md)。<br /><br />由於服務通訊憑證必須受到用戶端電腦的信任，我們建議您使用由受信任的憑證授權單位所簽署的憑證\(CA\)。 您選取的所有憑證都必須具備相對應的私密金鑰。|  
|安全通訊端層\(SSL\)憑證|同盟伺服器會使用 SSL 憑證，針對與 Web 用戶端及同盟伺服器 Proxy 的 SSL 通訊來保護 Web 服務流量的安全。<br /><br />由於 SSL 憑證必須受到用戶端電腦信任，因此，建議您使用由信任的 CA 所簽署的憑證。 您選取的所有憑證都必須具備相對應的私密金鑰。|  
|語彙基元\-解密憑證|此憑證用來解密此同盟伺服器收到的權杖。<br /><br />您可以有多個解密憑證。 這可讓資源同盟伺服器能夠設定新的憑證做為主要解密憑證之後，使用較舊的憑證所簽發的權杖解密。 所有的憑證可用來解密，但只有主要權杖\-解密憑證實際發佈同盟中繼資料。 您選取的所有憑證都必須具備相對應的私密金鑰。<br /><br />如需詳細資訊，請參閱 <<c0> [ 新增權杖解密憑證](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md)。|  
  
您可以要求並安裝 SSL 憑證或服務通訊憑證要求服務通訊憑證，透過 Microsoft Management Console \(MMC\)貼齊\-中適用於 IIS。 如需更多使用 SSL 憑證的一般資訊，請參閱 [IIS 7.0：設定安全通訊端層，在 IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544)和[IIS 7.0:在 IIS 7.0 中設定伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108545)。  
  
> [!NOTE]  
> 在 AD FS 中，您可以變更安全雜湊演算法\(SHA\)使用於數位簽章來任一 SHA 的層級\-1 或 SHA\-256\(更安全\)。 AD FSdoes 不支援使用憑證與其他雜湊方法，例如 MD5 \(Makecert.exe 命令搭配使用的預設雜湊演算法\-線條 工具\)。 最佳安全性做法，我們建議您改用 SHA\-256\(根據預設，這會設定\)針對所有簽章。 SHA\-1 建議只有在案例中您必須與交互操作的產品，不支援使用 SHA 通訊\-256，例如非\-Microsoft 產品或 AD FS 1。 *x*。  
  
## <a name="determining-your-ca-strategy"></a>決定您的 CA 策略  
AD FS 不需要 CA 所簽發的憑證。 但是，SSL 憑證\(也會使用預設值做為服務通訊憑證的憑證\)必須受到 AD FS 用戶端的信任。 我們建議您不要使用自我\-這些憑證類型簽署憑證。  
  
> [!IMPORTANT]  
> 使用自我\-簽署，在生產環境中的 SSL 憑證會讓惡意使用者接管資源夥伴組織中的同盟伺服器的帳戶夥伴組織中。 此安全性風險存在是因為自我\-簽署的憑證是根憑證。 必須加入另一部同盟伺服器的受信任的根存放區\(比方說，資源同盟伺服器\)，這可以讓該伺服器很容易遭受攻擊。  
  
在您收到來自 CA 的憑證之後，請確定會將所有憑證匯入本機電腦的個人憑證存放區。 您可以將憑證匯入到個人存放區，使用憑證 MMC 嵌入式管理單元\-中。  
  
除了使用憑證嵌入式管理單元\-中，您也可以匯入 SSL 憑證與 IIS 管理員 嵌入式管理單元\-中時，您指派 SSL 憑證至預設的網站。 如需詳細資訊，請參閱 <<c0> [ 伺服器驗證憑證匯入至預設網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
> [!NOTE]  
> 在即將成為同盟伺服器電腦上安裝 AD FS 軟體之前，請確定這兩種憑證都在本機電腦個人憑證存放區中，而且 SSL 憑證指派到預設的網站。 如需有關設定同盟伺服器所需的工作順序的詳細資訊，請參閱[檢查清單：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
根據您的安全性和預算需求而定，請仔細考慮您的哪些憑證將透過公用 CA 或企業 CA 來取得。 下圖顯示特定憑證類型之建議的 CA 簽發者。 這項建議反映最佳\-練習關於安全性與成本的方法。  
  
![憑證需求](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>憑證撤銷清單  
如果您使用的任何憑證具有 CRL，含有已設定憑證的伺服器就必須能夠連絡發佈 CRL 的伺服器。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
