---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: 為電腦設定同盟伺服器 Proxy 角色
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d47f7d3985aa779276f0712347eb9030857cefdb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359795"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>為電腦設定同盟伺服器 Proxy 角色

設定具有所需憑證的電腦，並安裝同盟服務 Proxy 角色服務之後，您就可以將電腦設定為同盟伺服器 Proxy。 您可以使用下列程序，讓電腦以同盟伺服器 Proxy 角色運作。  
  
> [!IMPORTANT]  
> 在您使用此程式設定同盟伺服器 proxy 電腦之前，請確定您已遵循 [Checklist 中的所有步驟：依照列出的順序設定同盟伺服器 Proxy @ no__t-0。 請確認至少已部署一部同盟伺服器，而且已實作授權同盟伺服器 Proxy 設定所需的所有認證。 您也必須在預設網站上設定安全通訊端層 \(SSL @ no__t-1 系結，否則此 wizard 不會啟動。 在此同盟伺服器 Proxy 可以運作之前，必須完成所有這些工作。  
  
完成電腦設定之後，請確認同盟伺服器 Proxy 如預期般運作。 如需詳細資訊，請參閱[驗證同盟伺服器 Proxy 是否正確運作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>為電腦設定同盟伺服器 Proxy 角色  
  
1.  有兩種方式可以啟動 [AD FS 同盟伺服器設定向導]。 若要啟動該精靈，請執行下列其中一個動作：  
  
    -   在 **開始** 畫面上，輸入**AD FS 同盟伺服器 Proxy 設定向導**，然後按 enter。  
  
    -   在安裝程式完成之後，請開啟 [Windows Explorer]，流覽至**C： \\Windows @ no__t-2ADFS**資料夾，然後按兩下 @ no__t-3click **fspconfigwizard.exe .exe**。  
  
2.  另一種方式是啟動該精靈，然後按一下 [歡迎] 頁面上的 [下一步]。  
  
3.  在 [指定 Federation Service 名稱] 頁面上，在 [Federation Service 名稱] 下輸入代表此電腦將以 Proxy 角色運作之 Federation Service 的名稱。  
  
4.  根據您的特定網路需求而定，判斷您是否需要使用 HTTP Proxy 伺服器來將要求轉送到 Federation Service。 若需要，請選取 [傳送要求給此 Federation Service 時使用 HTTP Proxy 伺服器] 核取方塊，在 [HTTP Proxy 伺服器位址] 下輸入 Proxy 伺服器的位址，按一下 [測試連線] 以檢查連線功能，然後按一下 [下一步]。  
  
5.  當您收到提示時，請指定在此同盟伺服器 Proxy 與 Federation Service 之間建立信任關係時所需的憑證。  
  
    根據預設，只有同盟服務所使用的服務帳戶或本機內建 @ no__t-0Administrators 群組的成員可以授權同盟伺服器 proxy。  
  
6.  在 [準備套用設定] 頁面上，檢閱詳細資料。 若設定正確，請按一下 [下一步] 開始使用這些  Proxy 設定來設定此電腦。  
  
7.  在 [設定結果] 頁面上，檢閱結果。 完成所有設定步驟之後，請按一下 [**關閉**] 結束嚮導。  
  
    沒有 Microsoft 管理主控台 \(MMC @ no__t-1 snap @ no__t-2in，用於管理同盟伺服器 proxys。 若要設定組織中每個同盟伺服器 proxys 的設定，請使用 Windows PowerShell Cmdlet。  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>為 Proxy 作業設定替代的 TCP @ no__t-0IP 埠  
根據預設，同盟伺服器 proxy 服務會設定為針對 HTTPS 流量使用 TCP 埠443，並將埠80用於與同盟伺服器通訊的 HTTP 流量。 若要設定其他連接埠 (例如使用 TCP 連接埠 444 來處理 HTTPS 並使用連接埠 81 來處理 HTTP)，請執行下列工作。  
  
> [!NOTE]  
> 如果您想要一開始部署 AD FS 以在替代的 TCP @ no__t-0IP 埠下運作，您應該先在同盟伺服器和同盟伺服器 proxy 電腦上，修改 HTTP 和 HTTPS 的 IIS 通訊協定系結中的埠。 執行 AD FS 設定向導進行初始設定之前，應該會發生這種情況。 如果您先設定 Internet Information Services \(IIS @ no__t-1，當 wizard @ no__t-3based 設定發生在 AD FS 內時，就會發現您的替代 TCP @ no__t-2IP 埠設定，而且不需要執行下列程式。 若稍後想要變更連接埠設定，請先更新 IIS 通訊協定繫結，然後使用下列程序適當地更新連接埠設定。 如需編輯 IIS 系結的詳細資訊，請參閱 Microsoft 知識庫中的[文章 149605](https://go.microsoft.com/fwlink/?LinkId=190275) 。  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>設定替代的 TCP @ no__t-0IP 埠，讓同盟伺服器 proxy 使用  
  
1.  將同盟伺服器設定為使用非預設連接埠。  
  
    若要這麼做，請指定非預設的埠號碼，方法是將它包含在*HttpsPort*和*HttpPort*選項中，做為**Set @ no__t-3ADFSProperties** Cmdlet 的一部分。 例如，若要設定這些埠，請在同盟伺服器電腦上的 Windows PowerShell 會話中使用下列命令：  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  將同盟伺服器 proxy 設定為使用非預設埠。  
  
    若要這麼做，請指定非預設的埠號碼，方法是將它包含在*HttpsPort*和*HttpPort*選項中，做為**Set @ no__t-3ADFSProxyProperties** Cmdlet 的一部分。 例如，若要設定這些埠，請在同盟伺服器電腦上的 Windows PowerShell 會話中使用下列命令：  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > 同盟伺服器 proxy 服務預設不會啟用端點 Url。 如果您要設定新的同盟伺服器安裝，您必須先啟用同盟伺服器 proxy 服務端點。 例如，假設針對此程式中的範例所參考的所有端點，您已在 AD FS 管理 snap @ no__t-0in 中選取它們，然後選取 **在 proxy 上啟用**，以啟用 proxy。  
  
3.  更新同盟伺服器 proxy 上的 IIS 安裝，讓安全性聲明標記語言 \(SAML @ no__t-1 和 WS @ no__t-2Trust 端點設定為反映更新的埠號碼。 若要這樣做，您可以使用 [記事本] 修改 Web.config 檔案中的下列資訊，此檔案位於同盟伺服器 proxy 電腦上的 systemdrive% \\inetpub @ no__t-1adfs @ no__t-2ls @ no__t-3。 例如，假設您有一個名為 sts1.contoso.com 的同盟伺服器，而新的埠號碼是444，請在同盟伺服器 proxy 電腦上的 [記事本] 中流覽並開啟 web.config 檔案，找出下列區段，將 [埠號碼] 修改為在下方反白顯示，然後儲存並結束 [記事本]。  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  將同盟伺服器 proxy 服務使用者帳戶新增至存取控制清單 \(ACL @ no__t-1，適用于相關的端點 Url。 例如，如果埠號碼為1234，而用來執行 AD FSfederation 伺服器 proxy 服務的使用者帳戶是建立的 @ no__t-0in Network Service 帳戶，請在命令提示字元中輸入下列命令：  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    先前的命令必須在同盟伺服器和同盟伺服器 proxy 電腦上執行。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

