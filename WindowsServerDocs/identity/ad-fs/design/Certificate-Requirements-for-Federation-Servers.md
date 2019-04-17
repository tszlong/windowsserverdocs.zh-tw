---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: "聯盟伺服器的憑證需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-servers"></a>聯盟伺服器的憑證需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在任何 Active Directory 同盟服務 \(AD FS\) 設計，必須使用各種不同的憑證以安全通訊，並促進使用者之間網際網路戶端與聯盟伺服器的驗證。 每個聯盟伺服器憑證服務通訊並 token\ 簽署的憑證前必須參與 AD FS 通訊。 下表描述聯盟伺服器相關聯的憑證類型。  
  
|憑證類型|描述|  
|--------------------|---------------|  
|Token\ 簽署的憑證|Token\ 簽署的憑證會 X509 憑證。 聯盟伺服器以數位方式簽署的它們產生所有安全性權杖使用相關聯的 public\ 日私人配對。 這包括發行的聯盟中繼資料和成品解析度要求的登入。<br /><br />您可以有多個 token\ 簽署憑證 snap\ 在一個憑證時接近過期的憑證變換允許 AD FS 管理設定。 根據預設，所有的憑證在清單中的發行，但只主要 token\ 簽署的憑證使用 AD FS 確實登入權杖。 您的所有憑證都必須私密金鑰相對應。<br /><br />如需詳細資訊，請查看[權杖簽署的憑證](Token-Signing-Certificates.md)和[中加入權杖簽署的憑證](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md)。|  
|服務通訊的憑證|聯盟伺服器使用伺服器驗證憑證，也就是服務通訊的 Windows 通訊基本知識 \(WCF\) 訊息安全性。 根據預設，這是安全通訊端層 \(SSL\) 中的憑證網際網路資訊服務 \(IIS\) 聯盟伺服器使用相同憑證。 **注意：** AD FS 管理 snap\ 中指的是伺服器驗證憑證的同盟服務通訊的憑證以的伺服器。<br /><br />如需詳細資訊，請查看[服務通訊的憑證](Service-Communications-Certificates.md)和[設定服務通訊憑證](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md)。<br /><br />因為服務通訊憑證必須受 client 電腦使用，我們建議您使用的受信任的憑證授權單位已簽署的憑證 \(CA\)。 您的所有憑證都必須私密金鑰相對應。|  
|安全通訊端層 \(SSL\) 憑證|聯盟伺服器保護 Web 服務的資料傳輸 Web 戶端與聯盟伺服器 proxy SSL 通訊使用 SSL 憑證。<br /><br />由於 client 的電腦必須信任 SSL 憑證，我們建議您使用的由信賴 CA 簽署的憑證。 您的所有憑證都必須私密金鑰相對應。|  
|Token\-解密憑證|這個憑證用來解密權杖所此聯盟伺服器接收。<br /><br />您可以有多個解密憑證。 這可讓您可能可以解密發行新的憑證已設為主要解密憑證後使用較舊的憑證發出的資源聯盟伺服器。 所有憑證可以都用來解密，但僅限主要 token\ 解密憑證實際上發行聯盟中繼資料中。 您的所有憑證都必須私密金鑰相對應。<br /><br />如需詳細資訊，請查看[加入權杖解密憑證](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md)。|  
  
您可以要求和所要求服務通訊憑證透過 Microsoft Management Console \(MMC\) snap\ 安裝 SSL 憑證或服務通訊憑證-中 iis。 有關更一般使用 SSL 憑證，請查看[IIS 7.0：設定安全通訊端層 IIS 7.0 在](https://go.microsoft.com/fwlink/?LinkID=108544)和[IIS 7.0: IIS 7.0 中設定伺服器憑證](https://go.microsoft.com/fwlink/?LinkID=108545)。  
  
> [!NOTE]  
> AD FS 中，您可以變更適用於數位簽章 SHA\-1 或 SHA\-256 \(more secure\) 安全 Hash 演算法 \(SHA\) 層級。 AD FSdoes 不支援使用憑證的其他 hash 方法，例如 MD5 \（預設 hash 的演算法所使用的 Makecert.exe command\ 列 tool\）。 最好的安全性，以我們建議您使用 SHA\-256 \（這由 default\ 設定）的所有特徵標記。 建議 SHA\-1 只在您必須交互不支援使用 SHA\-256，例如 non\ Microsoft product 或 AD FS 1 通訊 product 的案例。 *x *.  
  
## <a name="determining-your-ca-strategy"></a>判斷您 CA 策略  
AD FS 不需要 CA，發行憑證。 不過，SSL 憑證 \（也會使用預設值為服務通訊 certificate\ 憑證）必須信任 AD FS 用。 我們建議您無法使用 self\ 簽署的憑證這些憑證類型。  
  
> [!IMPORTANT]  
> 使用 self\ 簽章，production 環境中 SSL 憑證可讓使用者惡意 account 合作夥伴公司資源合作夥伴組織掌控聯盟伺服器中。 這種安全性風險存在因為 self\ 簽署的憑證根憑證。 它們必須新增受信任的根網上商店的另一部聯盟伺服器 \ (例如，資源聯盟 server\)，這可以讓該伺服器容易受到攻擊。  
  
您收到 CA 憑證之後，確認所有憑證的匯都入至本機電腦的個人憑證存放區。 您可以個人憑證 MMC snap\ 在使用網上商店匯入的憑證。  
  
或者，使用 snap\ 中的憑證，您可以也匯入您的預設網站指派 SSL 憑證 IIS 管理員 snap\ 中時使用 SSL 憑證。 如需詳細資訊，請查看[匯入伺服器驗證憑證的預設網站](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)。  
  
> [!NOTE]  
> 您的電腦將會變成聯盟伺服器上安裝 AD FS 軟體之前，請確定的本機電腦個人憑證存放區中的兩個憑證和的 SSL 憑證已指派給預設網站。 有關更多的聯盟伺服器設定所需的工作順序，請查看[檢查清單︰ 設定好聯盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
根據您的安全性和預算需求，仔細考量，您憑證將會取得公用，CA 或公司 CA。 下圖顯示建議的 CA 發行者指定的憑證類型。 這個建議反映 best\-方法安全性及成本。  
  
![憑證需求](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>憑證撤銷清單  
如果您使用的任何憑證有 Crl，必須連絡伺服器分配 Crl 伺服器的憑證設定。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
