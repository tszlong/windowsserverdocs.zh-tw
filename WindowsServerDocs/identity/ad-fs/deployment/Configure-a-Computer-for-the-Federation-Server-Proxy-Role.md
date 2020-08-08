---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: 為電腦設定同盟伺服器 Proxy 角色
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 06bccb135963d5b5bc754e895843a4d75153c120
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963119"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>為電腦設定同盟伺服器 Proxy 角色

設定具有所需憑證的電腦，並安裝同盟服務 Proxy 角色服務之後，您就可以將電腦設定為同盟伺服器 Proxy。 您可以使用下列程序，讓電腦以同盟伺服器 Proxy 角色運作。

> [!IMPORTANT]
> 在您使用此程式設定同盟伺服器 proxy 電腦之前，請確定您已遵循[檢查清單：設定同盟伺服器 proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)的順序中的所有步驟。 請確認至少已部署一部同盟伺服器，而且已實作授權同盟伺服器 Proxy 設定所需的所有認證。 您也必須 \( \) 在 [預設的網站] 上設定安全通訊端層 SSL 系結，否則此 wizard 不會啟動。 在此同盟伺服器 Proxy 可以運作之前，必須完成所有這些工作。

完成電腦設定之後，請確認同盟伺服器 Proxy 如預期般運作。 如需詳細資訊，請參閱[驗證同盟伺服器 Proxy 是否正確運作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)。

若要完成此程序，至少需要本機電腦之 **Administrators** 群組的成員資格或同等權限。  請參閱在[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中使用適當帳戶和群組成員資格的詳細資料。

### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>為電腦設定同盟伺服器 Proxy 角色

1.  有兩種方式可以啟動 AD FS 同盟伺服器設定精靈。 若要啟動該精靈，請執行下列其中一個動作：

    -   在 [**開始**] 畫面上，輸入**AD FS 同盟伺服器 Proxy 設定向導]**，然後按 enter。

    -   在安裝程式完成後，請開啟 [Windows Explorer]，流覽至**C： \\ windows \\ ADFS**資料夾，然後按兩下 [ \- **FspConfigWizard.exe**]。

2.  使用其中一種方法啟動此精靈，然後在 **[歡迎使用]** 頁面上按 [下一步]****。

3.  在 [指定 Federation Service 名稱]**** 頁面上的 [Federation Service 名稱]**** 下，輸入代表將以此電腦作為 Proxy 角色之 Federation Service 的名稱。

4.  根據您的特定網路需求而定，判斷您是否需要使用 HTTP Proxy 伺服器來將要求轉送到 Federation Service。 如果需要，請選取 [在傳送要求至此 Federation Service 時使用 HTTP Proxy 伺服器]**** 核取方塊、在 [HTTP Proxy 伺服器位址]**** 下輸入 Proxy 伺服器的位址、按一下 [測試連線]**** 以驗證連線，然後按 [下一步]****。

5.  當您收到提示時，請指定在此同盟伺服器 Proxy 與 Federation Service 之間建立信任關係時所需的憑證。

    根據預設，只有同盟服務所使用的服務帳戶或本機內建系統 \\ 管理員群組的成員可以授權同盟伺服器 proxy。

6.  在 [已可套用設定]**** 頁面上，檢閱詳細資料。 如果顯示的設定正確無誤，請按 [下一步]****，開始以這些 Proxy 設定進行此電腦的設定。

7.  在 [設定結果]**** 頁面上檢閱結果。 完成所有設定步驟之後，請按一下 [**關閉**] 結束嚮導。

    沒有 Microsoft Management Console mmc 嵌入式 \( \) 管理單元 \- 可用來管理同盟伺服器 proxys。 若要設定組織中每個同盟伺服器 proxys 的設定，請使用 Windows PowerShell Cmdlet。

## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>設定 \/ Proxy 作業的替代 TCP IP 埠
根據預設，同盟伺服器 proxy 服務會設定為針對 HTTPS 流量使用 TCP 埠443，並將埠80用於與同盟伺服器通訊的 HTTP 流量。 若要設定其他連接埠 (例如使用 TCP 連接埠 444 來處理 HTTPS 並使用連接埠 81 來處理 HTTP)，請執行下列工作。

> [!NOTE]
> 如果您打算一開始部署 AD FS 以在替代的 TCP \/ IP 埠下運作，您應該先在同盟伺服器和同盟伺服器 proxy 電腦上，修改 HTTP 和 HTTPS 的 IIS 通訊協定系結中的埠。 執行 AD FS 設定向導進行初始設定之前，應該會發生這種情況。 如果您先設定 Internet Information Services \( IIS，則在 \) \/ AD FS 內進行以嚮導為基礎的設定時，會探索到您的替代 TCP IP 通訊埠設定， \- 而且不需要執行下列程式。 若稍後想要變更連接埠設定，請先更新 IIS 通訊協定繫結，然後使用下列程序適當地更新連接埠設定。 如需編輯 IIS 系結的詳細資訊，請參閱 Microsoft 知識庫中的[文章 149605](https://go.microsoft.com/fwlink/?LinkId=190275) 。

#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>為 \/ 同盟伺服器 proxy 設定要使用的替代 TCP IP 埠

1.  將同盟伺服器設定為使用非預設連接埠。

    若要這麼做，請指定非預設的埠號碼，方法是將它包含在*HttpsPort*和*HttpPort*選項中，做為**Set \- set-adfsproperties** Cmdlet 的一部分。 例如，若要設定這些埠，請在同盟伺服器電腦上的 Windows PowerShell 會話中使用下列命令：

    ```
    Set-ADFSProperties -HttpsPort 444
    Set-ADFSProperties -HttpPort 81
    ```

2.  將同盟伺服器 proxy 設定為使用非預設埠。

    若要這麼做，請指定非預設的埠號碼，方法是將它包含在*HttpsPort*和*HttpPort*選項中，做為**Set \- ADFSProxyProperties** Cmdlet 的一部分。 例如，若要設定這些埠，請在同盟伺服器電腦上的 Windows PowerShell 會話中使用下列命令：

    ```
    Set-ADFSProxyProperties -HttpsPort 444
    Set-ADFSProxyProperties -HttpPort 81
    ```

    > [!NOTE]
    > 同盟伺服器 proxy 服務預設不會啟用端點 Url。 如果您要設定新的同盟伺服器安裝，您必須先啟用同盟伺服器 proxy 服務端點。 例如，假設針對此程式中的範例所參考的所有端點，您都已在 [AD FS 管理] 嵌入式管理單元 \- 中選取，然後選取 [**在 Proxy 上啟用**]，以啟用 proxy。

3.  更新同盟伺服器 proxy 上的 IIS 安裝，以便將安全性聲明標記語言 \( SAML \) 和 ws-trust \- 端點設定為反映更新的埠號碼。 若要這樣做，您可以使用 [記事本] 修改 Web.config 檔案中的下列各項，此檔案位於 \\ \\ \\ \\ 同盟伺服器 proxy 電腦上的 systemdrive% inetpub adfs ls。 例如，假設您有名為 sts1.contoso.com 的同盟伺服器，且新的埠號碼為444，請在同盟伺服器 proxy 電腦上流覽並開啟 [記事本] 中的 Web.config 檔案，找出下列區段，如下所示修改埠號碼，然後儲存並結束 [記事本]。

    ```
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />
    ```

4.  將同盟伺服器 proxy 服務使用者帳戶新增至 \( 相關端點 url 的存取控制清單 ACL \) 。 例如，如果埠號碼為1234，而用來執行 AD FSfederation 伺服器 proxy 服務的使用者帳戶是 \- 內建網路服務帳戶，請在命令提示字元中輸入下列命令：

    ```
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"

    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"
    ```

    先前的命令必須在同盟伺服器和同盟伺服器 proxy 電腦上執行。

## <a name="additional-references"></a>其他參考資料
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)


