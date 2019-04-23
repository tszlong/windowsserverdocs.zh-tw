---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: 為電腦設定同盟伺服器 Proxy 角色
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861799"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>為電腦設定同盟伺服器 Proxy 角色

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

電腦設定所需的憑證，並已安裝 Federation Service Proxy 角色服務之後，您就能夠設定電腦成為同盟伺服器 proxy。 您可以使用下列程序，讓電腦以同盟伺服器 Proxy 角色運作。  
  
> [!IMPORTANT]  
> 設定同盟伺服器 proxy 電腦的情況下，您在使用此程序之前，請確定您已遵循中的所有步驟[檢查清單：設定註冊 a Federation Server Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)所列出的順序。 請確認至少已部署一部同盟伺服器，而且已實作授權同盟伺服器 Proxy 設定所需的所有認證。 您也必須設定安全通訊端層\(SSL\)繫結預設的網站或此精靈將不會啟動。 在此同盟伺服器 Proxy 可以運作之前，必須完成所有這些工作。  
  
完成電腦設定之後，請確認同盟伺服器 Proxy 如預期般運作。 如需詳細資訊，請參閱[驗證同盟伺服器 Proxy 是否正確運作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>為電腦設定同盟伺服器 Proxy 角色  
  
1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   在 **開始**畫面上，輸入**AD FS 同盟伺服器 Proxy 設定精靈**，然後按 ENTER 鍵。  
  
    -   隨時安裝精靈完成，請開啟 Windows 檔案總管後，瀏覽至**c:\\Windows\\ADFS**資料夾，，然後按兩下\-按一下**FspConfigWizard.exe**.  
  
2.  另一種方式是啟動該精靈，然後按一下 [歡迎] 頁面上的 [下一步]。  
  
3.  在 [指定 Federation Service 名稱] 頁面上，在 [Federation Service 名稱] 下輸入代表此電腦將以 Proxy 角色運作之 Federation Service 的名稱。  
  
4.  根據您的特定網路需求而定，判斷您是否需要使用 HTTP Proxy 伺服器來將要求轉送到 Federation Service。 若需要，請選取 [傳送要求給此 Federation Service 時使用 HTTP Proxy 伺服器] 核取方塊，在 [HTTP Proxy 伺服器位址] 下輸入 Proxy 伺服器的位址，按一下 [測試連線] 以檢查連線功能，然後按一下 [下一步]。  
  
5.  當您收到提示時，請指定在此同盟伺服器 Proxy 與 Federation Service 之間建立信任關係時所需的憑證。  
  
    根據預設，只有同盟服務或本機內建的成員所使用的服務帳戶\\Administrators 群組可以授權同盟伺服器 proxy。  
  
6.  在 [準備套用設定] 頁面上，檢閱詳細資料。 若設定正確，請按一下 [下一步] 開始使用這些  Proxy 設定來設定此電腦。  
  
7.  在 [設定結果] 頁面上，檢閱結果。 當所有的組態步驟都完成後時，按一下**關閉**結束精靈。  
  
    沒有任何 Microsoft Management Console \(MMC\)貼齊\-可用來管理同盟伺服器 proxys 中。 若要設定您組織中的每個同盟伺服器 proxys 的設定，請使用 Windows PowerShell cmdlet。  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>設定替代的 TCP\/IP 連接埠，針對 Proxy 操作  
根據預設，同盟伺服器 proxy 服務被設定為使用 TCP 連接埠 443 用於 HTTPS 流量，連接埠 80 用於 HTTP 資料傳輸，與同盟伺服器進行通訊。 若要設定其他連接埠 (例如使用 TCP 連接埠 444 來處理 HTTPS 並使用連接埠 81 來處理 HTTP)，請執行下列工作。  
  
> [!NOTE]  
> 如果您想要一開始部署 AD FS 操作替代 TCP\/IP 連接埠，您應該先修改您 IIS 通訊協定繫結中 HTTP 與 HTTPS 在同盟伺服器與同盟伺服器 proxy 電腦上的連接埠。 這應該會在執行 AD FS 設定精靈進行初始設定之前發生。 如果您設定 Internet Information Services \(IIS\)第一，替代的 TCP\/IP 連接埠設定時會探索到，當精靈\-根據的組態，就會發生在 AD FS 和下列程序不是必要。 若稍後想要變更連接埠設定，請先更新 IIS 通訊協定繫結，然後使用下列程序適當地更新連接埠設定。 如需有關如何編輯 IIS 繫結的詳細資訊，請參閱 <<c0> [ 文章 149605](https://go.microsoft.com/fwlink/?LinkId=190275) Microsoft 知識庫中。  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>若要設定替代的 TCP\/同盟伺服器 proxy 使用的 IP 連接埠  
  
1.  將同盟伺服器設定為使用非預設連接埠。  
  
    若要這樣做，請指定非預設連接埠號碼包含在與*HttpsPort*並*HttpPort*選項做為一部分**設定\-ADFSProperties** cmdlet。 例如，若要設定這些連接埠，請在同盟伺服器電腦上的 Windows PowerShell 工作階段中使用下列命令：  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  設定同盟伺服器 proxy，若要使用非預設連接埠。  
  
    若要這樣做，請指定非預設連接埠號碼包含在與*HttpsPort*並*HttpPort*選項做為一部分**設定\-ADFSProxyProperties**cmdlet。 例如，若要設定這些連接埠，請在同盟伺服器電腦上的 Windows PowerShell 工作階段中使用下列命令：  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > 預設值為同盟伺服器 proxy 服務不會啟用端點 Url。 如果您要設定新的同盟伺服器安裝，您必須先啟用同盟伺服器 proxy 服務端點。 例如，假設的所有端點，此程序中的範例是指您已啟用這些 proxy 的方法是在 AD FS 管理嵌入式管理單元中選取它們\-中，然後選取**proxy 上啟用**。  
  
3.  更新 IIS 安裝在同盟伺服器 proxy，因此該安全性聲明標記語言\(SAML\)和 WS\-信任端點設定為反映已更新的連接埠號碼。 若要這樣做，您可以使用 [記事本] 來修改在 Web.config 檔案中，位於 %systemdrive\\inetpub\\adfs\\ls\\同盟伺服器 proxy 電腦上。 比方說，假設您有一個名為 sts1.contoso.com 的同盟伺服器，且新的連接埠號碼是 444，瀏覽至與同盟伺服器 proxy 電腦上，在記事本中開啟 Web.config 檔案，找出下一節，修改為連接埠號碼反白顯示，然後儲存並結束 [記事本]。  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  將同盟伺服器 proxy 服務的使用者帳戶新增至存取控制清單\(ACL\)相關的端點 url。 範例中，如果連接埠號碼是 1234年和用以執行 AD FSfederation 伺服器 proxy 的使用者帳戶下的服務是內建\-在 Network Service 帳戶，請在命令提示字元中輸入下列命令：  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    上述命令必須在同盟伺服器和同盟伺服器 proxy 電腦上執行。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

