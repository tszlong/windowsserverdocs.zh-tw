---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: "聯盟伺服器 Proxy 角色設定電腦"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>聯盟伺服器 Proxy 角色設定電腦

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您將電腦設定的必要的憑證，並安裝同盟服務 Proxy 角色服務之後，您已經設定電腦，以成為聯盟 proxy 伺服器。 您可以使用下列程序，讓電腦的作用聯盟伺服器 proxy 角色。  
  
> [!IMPORTANT]  
> 使用此程序設定聯盟伺服器 proxy 電腦之前，請確定您有依照所有中的步驟執行[檢查清單︰ 設定好聯盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)所列的順序。 請確定該至少一個聯盟部署伺服器與所有所需的認證授權聯盟 proxy 伺服器設定實作。 您還必須設定安全通訊端層 \(SSL\) 繫結預設的網站，或這個精靈將不會開始。 所有工作必須先都完成此聯盟伺服器 proxy 可以運作。  
  
電腦設定完成後，請確認聯盟 proxy 伺服器的如預期般運作。 如需詳細資訊，請查看[確認聯盟伺服器 Proxy 是操作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>若要設定聯盟 proxy 角色電腦  
  
1.  有兩種方法可以開始 AD FS 聯盟伺服器設定精靈。 若要開始精靈中，執行下列其中一個動作：  
  
    -   在**[開始]**畫面中，輸入**AD FS 聯盟伺服器 Proxy 設定精靈**，然後按 ENTER 鍵。  
  
    -   依照本身需求加以安裝精靈完成，開放 Windows 檔案總管] 之後，瀏覽至**C:\\Windows\\ADFS**資料夾，然後 double\ 按**FspConfigWizard.exe**。  
  
2.  使用任一個方法，[開始] 精靈中，並在**歡迎**頁面上，按一下 [**下一步**。  
  
3.  在**指定同盟服務名稱**頁面上，在**同盟服務名稱**，輸入代表同盟服務的 proxy 角色做這台電腦的名稱。  
  
4.  根據您的特定網路需求，判斷是否您將需要使用 HTTP proxy 伺服器轉送要求同盟服務。 若是如此，請選取 [**傳送到此同盟服務要求時使用 HTTP proxy 伺服器**核取方塊，在**HTTP proxy 伺服器位址**輸入 proxy 伺服器的位址，請按一下**測試連接]**以確認連接，然後再按一下**下一步**。  
  
5.  當系統提示您指定所需之間這個聯盟伺服器 proxy 和同盟服務建立信任的憑證。  
  
    根據預設，只服務 account 使用同盟服務或 BUILTIN\\Administrators 本機群組成員可以授權聯盟 proxy 伺服器。  
  
6.  在**適用於設定準備**頁面上，檢視詳細資料。 若出現正確設定，請按一下**下一步**若要開始使用這些 proxy 設定設定此電腦。  
  
7.  在**設定結果**頁面上，檢視結果。 所有的設定步驟完成時，按**關閉**以結束精靈。  
  
    不還有任何 Microsoft Management Console \(MMC\) snap\-中管理聯盟伺服器 proxys 使用。 若要設定的每個聯盟伺服器 proxys 設定在組織中，使用 Windows PowerShell cmdlet。  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>設定其他 TCP\ 日 IP 連接埠 Proxy 作業  
根據預設，聯盟 proxy 伺服器設定使用 HTTPS 流量流量和連接埠 80 HTTP 與聯盟伺服器通訊的 TCP 連接埠 443。 若要設定的 HTTPS 444 的 TCP 連接埠和連接埠 81 HTTP，例如不同的連接埠必須完成以下工作。  
  
> [!NOTE]  
> 如果您想要一開始部署 AD FS 在替代 TCP\ 日 IP 連接埠運作，您應該第一次修改連接埠，在您 IIS 通訊協定繫結 HTTP 與 HTTPS 聯盟伺服器和聯盟 proxy 伺服器的電腦上。 這應該會執行 AD FS 設定精靈的初始設定之前先發生。 如果您是第一次設定 \(IIS\)，替代 TCP\ 日 IP 連接埠設定發現時 wizard\ 為基礎的設定，就會發生在 AD FS，不需要下列程序。 如果您想要變更的連接埠設定之後，更新 IIS 通訊協定繫結，然後使用下列程序更新連接埠設定正確。 如需有關如何編輯 IIS 繫結的詳細資訊，請[文章 149605](https://go.microsoft.com/fwlink/?LinkId=190275) Microsoft 知識庫中。  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>設定使用聯盟伺服器 proxy 替代 TCP\ 日 IP 連接埠  
  
1.  設定為使用非預設連接埠聯盟伺服器。  
  
    若要這樣做，請指定連接埠號碼非預設包含與*HttpsPort*和*HttpPort*選項的一部分**Set\-ADFSProperties** cmdlet。 例如，如果設定這些連接埠，您可以使用下列命令聯盟伺服器電腦上的 Windows PowerShell 工作階段中：  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  設定為使用非預設連接埠聯盟伺服器 proxy。  
  
    若要這樣做，請指定連接埠號碼非預設包含與*HttpsPort*和*HttpPort*選項的一部分**Set\-ADFSProxyProperties** cmdlet。 例如，如果設定這些連接埠，您可以使用下列命令聯盟伺服器電腦上的 Windows PowerShell 工作階段中：  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > 聯盟 proxy 伺服器的預設不會支援端點 Url。 如果您設定新聯盟伺服器安裝，您必須先讓聯盟伺服器 proxy 服務端點。 例如，我們假設，針對所有是指的範例此程序中的端點您有支援這些 proxy AD FS 管理 snap\ 中選取它們，然後選取**上 proxy 讓**。  
  
3.  更新聯盟 proxy 伺服器 IIS 安裝，如此安全性判斷提示標記語言 \(SAML\) 和 WS\ 信任端點都能反映更新連接埠號碼。 若要這樣做，您可以使用「記事本」修改下列 web.config，這是位於 systemdrive%\\inetpub\\adfs\\ls\\ 聯盟伺服器 proxy 電腦上。 例如假設您有一個名為 sts1.contoso.com 的聯盟伺服器，新的連接埠號碼」可以是 444 瀏覽和聯盟伺服器 proxy 電腦上，在「記事本」開放 web.config，找出的下一節，修改反白顯示，以下為連接埠號碼，然後儲存結束「記事本」。  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  新增聯盟伺服器 proxy 服務帳號存取控制清單 \(ACL\) 相關的端點 url。 例如，如果的連接埠號碼 1234 年，用來執行 AD FSfederation 伺服器 proxy 服務在帳號且 built\ 中網路服務帳號，在命令提示字元中輸入下列命令：  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    聯盟伺服器和聯盟伺服器 proxy 電腦必須執行前一個命令。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

