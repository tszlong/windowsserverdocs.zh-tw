---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: 同盟伺服器的憑證需求
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7eeff82ef3311f18c8252c44c96310fcf3c18217
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359179"
---
# <a name="certificate-requirements-for-federation-servers"></a>同盟伺服器的憑證需求

在任何 Active Directory 同盟服務 \(AD FS\) 設計中，必須使用各種憑證來保護通訊，並協助網際網路用戶端與同盟伺服器之間的使用者驗證。 每部同盟伺服器都必須具有服務通訊憑證和權杖\-簽署憑證，才能參與 AD FS 通訊。 下表描述與同盟伺服器相關聯的憑證類型。  
  
|憑證類型|描述|  
|--------------------|---------------|  
|權杖\-簽署憑證|權杖\-簽署憑證是 X509 憑證。 同盟伺服器會使用相關聯的公用\/私密金鑰組，以數位方式簽署其產生的所有安全性權杖。 這包括簽署已發佈的同盟中繼資料及成品解析要求。<br /><br />您可以在的 [AD FS 管理] 嵌入式管理\-中設定多個權杖\-簽署憑證，以便在其中一個憑證即將到期時，允許進行憑證變換。 根據預設，清單中的所有憑證都會發佈，但 AD FS 只會使用主要權杖\-簽署憑證來實際簽署權杖。 您選取的所有憑證都必須具備相對應的私密金鑰。<br /><br />如需詳細資訊，請參閱 [Token-Signing Certificates](Token-Signing-Certificates.md) 和 [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md)。|  
|服務通訊憑證|同盟伺服器會使用伺服器驗證憑證，也稱為 Windows Communication Foundation \(WCF\) 訊息安全性的服務通訊。 根據預設，這是同盟伺服器用來作為安全通訊端層 \(SSL\) 憑證的憑證，Internet Information Services \(IIS\)中。 **注意：** 中的 [AD FS 管理] 嵌入式\-管理單元是指做為服務通訊憑證之同盟伺服器的伺服器驗證憑證。<br /><br />如需詳細資訊，請參閱[服務通訊憑證](Service-Communications-Certificates.md)和[設定服務通訊憑證](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md)。<br /><br />由於服務通訊憑證必須受到用戶端電腦信任，因此建議您使用由信任的憑證授權單位單位所簽署的憑證 \(CA\)。 您選取的所有憑證都必須具備相對應的私密金鑰。|  
|安全通訊端層 \(SSL\) 憑證|同盟伺服器會使用 SSL 憑證，針對與 Web 用戶端及同盟伺服器 Proxy 的 SSL 通訊來保護 Web 服務流量的安全。<br /><br />由於 SSL 憑證必須受到用戶端電腦信任，因此，建議您使用由信任的 CA 所簽署的憑證。 您選取的所有憑證都必須具備相對應的私密金鑰。|  
|權杖\-解密憑證|此憑證是用來解密此同盟伺服器所接收的權杖。<br /><br />您可以有多個解密憑證。 這讓資源同盟伺服器能夠在新憑證設定為主要解密憑證之後，將使用較舊憑證所簽發的權杖解密。 所有憑證都可以用來進行解密，但只有主要權杖\-解密憑證才會實際發佈在同盟中繼資料中。 您選取的所有憑證都必須具備相對應的私密金鑰。<br /><br />如需詳細資訊，請參閱[新增權杖解密憑證](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md)。|  
  
您可以要求並安裝 SSL 憑證或服務通訊憑證，方法是透過 Microsoft Management Console \(MMC\) 的嵌入式管理\-。 如需使用 SSL 憑證的一般資訊，請參閱[iis 7.0：在 iis 7.0](https://go.microsoft.com/fwlink/?LinkID=108544)和 iis 7.0 中設定安全通訊端層[：在 iis 7.0 中設定伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108545)。  
  
> [!NOTE]  
> 在 AD FS 您可以將用於數位簽章的安全雜湊演算法 \(SHA\) 層級變更為 SHA\-1 或 SHA\-256 \(更安全的\)。 AD FSdoes 不支援將憑證與其他雜湊方法搭配使用，例如 MD5 \(搭配 Makecert 命令使用的預設雜湊演算法\-line 工具\)。 基於安全性最佳作法的考慮，我們建議您使用 SHA\-256 \(預設會針對所有簽章設定\)。 只有在必須與不支援使用 SHA\-256 進行通訊的產品相交互操作的情況下（例如，非\-的 Microsoft 產品或 AD FS 1），才建議使用 SHA\-1。 *x*。  
  
## <a name="determining-your-ca-strategy"></a>決定您的 CA 策略  
AD FS 不需要 CA 所簽發的憑證。 不過，SSL 憑證 \(預設也使用為服務通訊憑證的憑證\) 必須受到 AD FS 用戶端的信任。 我們建議您不要針對這些憑證類型使用自我\-簽署的憑證。  
  
> [!IMPORTANT]  
> 在實際執行環境中使用自我\-簽署的 SSL 憑證，可以讓帳戶夥伴組織中的惡意使用者控制資源夥伴組織中的同盟伺服器。 此安全性風險存在，因為自我\-簽署的憑證是根憑證。 它們必須新增至另一個同盟伺服器的受根信任存放區 \(例如，資源同盟伺服器\)，這可能會讓該伺服器容易遭受攻擊。  
  
在您收到來自 CA 的憑證之後，請確定會將所有憑證匯入本機電腦的個人憑證存放區。 您可以使用中的憑證 MMC 嵌入式管理\-，將憑證匯入到個人存放區。  
  
除了使用中的 [憑證] 嵌入式管理\-單元以外，您也可以在將 SSL 憑證指派給 [預設的網站] 時，使用中的 [IIS 管理員] 嵌入式管理單元\-匯入 SSL 憑證。 如需詳細資訊，請參閱將[伺服器驗證憑證匯入至預設的網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
> [!NOTE]  
> 在即將成為同盟伺服器的電腦上安裝 AD FS 軟體之前，請確定這兩個憑證都位於 [本機電腦] 個人憑證存儲中，而且 SSL 憑證已指派給 [預設的網站]。 如需設定同盟伺服器所需之工作順序的詳細資訊，請參閱[檢查清單：設定同盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
根據您的安全性和預算需求而定，請仔細考慮您的哪些憑證將透過公用 CA 或企業 CA 來取得。 下圖顯示特定憑證類型之建議的 CA 簽發者。 這項建議反映了關於安全性和成本的最佳\-實務做法。  
  
![憑證需求](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>憑證撤銷清單  
如果您使用的任何憑證具有 CRL，含有已設定憑證的伺服器就必須能夠連絡發佈 CRL 的伺服器。  
  
## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
