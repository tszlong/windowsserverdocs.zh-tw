---
title: 連接要求原則
description: 本主題提供的網路原則伺服器連接要求原則 Windows Server 2016 中的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496ba87597b0be6cad1109269d2f72f52de017f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-policies"></a>連接要求原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何使用 NPS 連接要求原則設定伺服器 NPS RADIUS 伺服器、RADIUS proxy，或兩。

>[!NOTE]
>本主題中，除了下列連接要求原則文件會提供。
> - [設定連接要求原則](nps-crp-configure.md)
> - [設定遠端 RADIUS 伺服器群組](nps-crp-rrsg-configure.md)

連接要求原則是設定的條件和設定，可讓網路系統管理員，若要指定哪些遠端驗證 Dial 使用者服務 (RADIUS) 伺服器執行驗證和執行的網路原則 Server (NPS) 伺服器接收從 RADIUS 連接要求的授權。 連接要求原則可以指定哪些 RADIUS 伺服器用於會計 RADIUS 設定。

您可以建立連接要求原則，以便在本機處理一些 RADIUS 要求寄從 RADIUS（NPS RADIUS 伺服器為使用）和其他類型的簡訊轉送到（NPS RADIUS proxy 為使用）的另一個 RADIUS 伺服器。

連接要求原則，您可以使用 NPS RADIUS 伺服器為或 RADIUS proxy，根據因素如下所示： 

- 的時間和星期幾
- 在連接要求領域名稱
- 連接所要求的類型
- RADIUS client 的 IP 位址

處理或才收到的訊息的設定值符合 NPS 伺服器上設定連接要求原則其中轉送 nps RADIUS 存取要求訊息。 

如果符合原則設定原則需要 NPS 伺服器處理訊息，NPS 做為 RADIUS 伺服器、驗證和授權連接要求。 如果符合原則設定原則需要 NPS 伺服器轉送訊息，NPS 做為 RADIUS proxy 與轉送到遠端的處理 RADIUS 伺服器連接要求。

如果傳入 RADIUS 存取要求訊息設定不符合連接要求原則其中存取-退回郵件已傳送到 RADIUS client 和使用者或電腦連接到網路就無法存取。

## <a name="configuration-examples"></a>設定範例

下列設定範例示範如何使用連接要求原則。

### <a name="nps-as-a-radius-server"></a>NPS RADIUS 伺服器

預設連接要求原則是唯一設定的原則。 在此範例中，設定 NPS RADIUS 伺服器為和本機伺服器 NPS 處理所有連接要求。 NPS 伺服器可以驗證和授權其帳號而網域中的 NPS 伺服器網域信任的網域中的使用者。


### <a name="nps-as-a-radius-proxy"></a>NPS RADIUS proxy 為

預設連接要求刪除原則，並要求轉送到不同的兩個網域建立兩個新連接要求原則。 在此範例中，設定 NPS RADIUS proxy 為。 NPS 不處理本機伺服器上的任何連接要求。 改為，它會連接要求轉送 NPS 或其他 RADIUS 伺服器設定為遠端 RADIUS 伺服器群組成員。


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS RADIUS 伺服器，並 RADIUS proxy 為

除了預設連接要求原則，會建立新的連接要求原則，將轉送 NPS 或受信任的網域中的其他 RADIUS 伺服器連接要求。 在此範例中，proxy 原則會出現的原則排序清單中的第一次。 如果連接要求符合 proxy 原則，連接要求轉送 RADIUS 伺服器遠端 RADIUS 伺服器群組中。 如果連接要求不符 proxy 原則，但符合預設連接要求原則、NPS 處理連接要求本機伺服器上。 如果連接要求不符任一原則，會捨棄它。

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS RADIUS 伺服器遠端計量伺服器為

在此範例中，執行計量未設定 NPS 本機伺服器以及預設連接要求原則修訂使 RADIUS 計量郵件轉寄 NPS 或遠端 RADIUS 伺服器群組中的其他 RADIUS 伺服器。 轉送計量簡訊，但不轉送驗證和授權訊息，本機伺服器 NPS 本機網域執行這些功能及所有受信任的網域。


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS 的 Windows 使用者對應遠端 RADIUS

在此範例中，NPS 的作用為 RADIUS 伺服器，並為每個個人連接要求 RADIUS proxy 轉送遠端 RADIUS 伺服器的驗證要求時使用本機的 Windows 使用者 account 授權。 此設定是由做為條件連接要求原則設定的 Windows 使用者對應屬性遠端 RADIUS 實作。 （另外，帳號必須會建立本機具有相同名稱遠端 RADIUS 伺服器針對執行驗證遠端使用者 account）。

## <a name="connection-request-policy-conditions"></a>連接要求原則條件

連接要求原則的條件是比較屬性傳入 RADIUS 存取要求訊息中的一或多個 RADIUS 屬性。 如果有多個條件，請在連接的條件的所有要求訊息，然後順序原則 nps 會執行原則必須符合連接要求中。

以下是您可以在連接要求原則設定可條件屬性。

### <a name="connection-properties-attribute-group"></a>連接屬性屬性群組

連接屬性屬性群組包含下列屬性。

- **框架通訊協定**。 用於指定類型框架輸入封包。 範例包括點對點通訊協定 (PPP)、序列行網際網路通訊協定（名單）、框架轉接和 X.25。
- **服務類型**。 用於指定所要求服務的類型。 範例（例如 PPP 連接）架構（例如，Telnet 連接）登入。 如需 RADIUS 服務類型，查看 RFC 2865，「遠端驗證 Dial 使用者服務 (RADIUS)。」
- **通道輸入**。 用於指定由要求 client 的通道的類型。 通道類型包括點對點通道通訊協定 (PPTP) 和層級兩種通道通訊協定 (L2TP)。

### <a name="day-and-time-restrictions-attribute-group"></a>日期和時間限制屬性群組 

包含 [日期和時間限制屬性屬性群組日期和時間限制。 利用此屬性，您可以指定星期幾和連接嘗試的一天的時間。 日期和時間是與相對的日期和時間 NPS 伺服器。

### <a name="gateway-attribute-group"></a>閘道屬性群組

閘道屬性群組包含下列屬性。

- **基座 ID 稱為「**。 用於指定的網路存取伺服器的電話號碼。 此屬性為字串。 您可以使用模式符合語法指定區域驗證碼。
- **NAS 識別碼**。 用於指定網路存取伺服器的名稱。 此屬性為字串。 您可以指定 NAS 識別碼使用模式符合語法。
- **NAS IPv4 位址**。 用於指定網際網路通訊協定第 4 (IPv4) (RADIUS client) 的網路存取伺服器的位址。 此屬性為字串。 若要指定 IP 網路，您可以使用模式符合語法。
- **NAS IPv6 位址**。 用於指定網際網路通訊協定第 6 (IPv6) (RADIUS client) 的網路存取伺服器的位址。 此屬性為字串。 若要指定 IP 網路，您可以使用模式符合語法。
- **NAS 連接埠輸入**。 用於指定存取 client 所使用的媒體類型。 範例類比手機行（也就是非同步）、整合數位網路的服務 (ISDN)、可愛或 virtual 私人網路 (Vpn、) IEEE 802.11 wireless，而乙太網路切換。

### <a name="machine-identity-attribute-group"></a>電腦的身分屬性群組

電腦的身分屬性群組包含電腦身分屬性。 使用此屬性，您可以指定原則的使用都會戶端的方法。

### <a name="radius-client-properties-attribute-group"></a>RADIUS Client 屬性屬性群組

RADIUS Client 屬性屬性群組包含下列屬性。

- **通話基座 ID**。 用於指定播報來電者 (client 存取) 來使用的電話號碼。 此屬性為字串。 您可以使用模式符合語法指定區域驗證碼。
- **Client 易記名稱**。 用於指定要求驗證 RADIUS client 電腦的名稱。 此屬性為字串。 您可以使用模式符合語法指定 client 的名稱。
- **Client IPv4 位址**。 若要指定 IPv4 位址的網路存取伺服器 (RADIUS client) 使用。 此屬性為字串。 若要指定 IP 網路，您可以使用模式符合語法。
- **Client IPv6 位址**。 若要指定 IPv6 位址的網路存取伺服器 (RADIUS client) 使用。 此屬性為字串。 若要指定 IP 網路，您可以使用模式符合語法。
- **Client 廠商**。 用於指定廠商要求驗證的網路存取伺服器。 執行路由並遠端存取服務的電腦是 Microsoft NAS 製造商。 您可以使用此屬性，來設定不同原則的不同 NAS 製造商。 此屬性為字串。 您可以使用模式符合語法。

### <a name="user-name-attribute-group"></a>使用者名稱屬性群組

使用者名稱屬性群組包含的使用者名稱屬性。 使用此屬性，您可以指定的使用者名稱或使用者名稱，必須符合存取 client RADIUS 訊息中所提供的使用者名稱的一部分。 此屬性是字元字串，通常會包含領域名稱與 account 使用者名稱。 您可以使用模式符合語法指定的使用者名稱。

## <a name="connection-request-policy-settings"></a>連接要求原則設定

連接要求原則設定的一組屬性，可套用至連入 RADIUS 訊息。 設定包含下列群組的屬性。

- 驗證
- 計量
- 屬性操作
- 轉送要求
- 進階

下列章節提供這些設定的相關的其他詳細資料。

### <a name="authentication"></a>驗證

使用此設定，您可以覆寫驗證設定中所有的網路原則設定，您可以指定的驗證方法和連接到您的網路所需的類型。

>[!IMPORTANT]
>如果您在您的網路原則設定的驗證方法比不安全的連接要求原則設定的驗證方法，就會覆寫更安全的驗證方法，您的網路原則設定。 例如，如果您有的網路原則，需要使用的受保護延伸驗證通訊協定 Microsoft 挑戰交換驗證通訊協定第 2 \ (PEAP MS-CHAP v2\)，這是安全的無線密碼架構的驗證方法，您也可以設定連接要求原則，以允許未授權的存取，結果很戶端不會需要使用了 v2 PEAP MS-CHAP 驗證。 在此範例中，會授與所有戶端連接到您的網路未經授權的存取。

### <a name="accounting"></a>計量

使用此設定，您可以設定轉寄 NPS 或遠端 RADIUS 伺服器群組中的其他 RADIUS 伺服器計量資訊，以便遠端 RADIUS 伺服器群組執行計量連接要求原則。

>[!NOTE]
>如果您有多個 RADIUS 伺服器和您想要計量資訊儲存在一個中央 RADIUS 計量資料庫中的所有伺服器，您可以使用連接要求原則計量設定中每個 RADIUS 伺服器上的原則向前計量資料所有一個 NPS 伺服器或其他程式指定為計量伺服器 RADIUS 伺服器。

連接要求原則計量設定功能的本機伺服器 NPS 計量組態的獨立。 亦即如果您設定來登入 RADIUS 計量資訊本機的檔案或的 Microsoft SQL Server 資料庫 NPS 本機伺服器，它就會如此無論您是否設定連接要求原則將計量郵件轉寄給遠端 RADIUS 伺服器群組。

如果您想要登入遠端而不是在本機計量資訊，您必須設定以不執行計量，同時也連接要求向前計量資料到遠端的 RADIUS 伺服器群組原則中設定計量 NPS 本機伺服器。

### <a name="attribute-manipulation"></a>屬性操作

您可以設定 [尋找取代規則操作文字字串屬性下列其中一組。

- 使用者名稱
- 稱為的基座 ID
- 通話基座 ID

尋找取代規則處理發生之前的屬性 RADIUS 訊息之前受驗證及計量設定。 屬性操作規則僅適用於單一屬性。 您無法設定為每個屬性屬性操作規則。 此外的屬性，您可以管理清單是靜態清單。您無法新增到清單中供操作屬性。

>[!NOTE]
>如果您使用 MS-CHAP v2 驗證通訊協定，您就不能如果向前 RADIUS 訊息來連接要求原則操作使用者名稱屬性。 僅限例外發生於使用斜線 (\) 的字元和管理只會影響左邊的資訊。 以指出（右邊的斜線字元資訊）的網域中的使用者 account 名稱，且網域名稱（資訊向左斜線字元）通常是斜線字元。 若是如此，允許只屬性操作規則修改或取代網域名稱。

範例如何管理領域名稱中的使用者名稱屬性，請參考主題中的區段「範例操作領域中的使用者名稱屬性名稱][使用規則運算式 NPS 在](nps-crp-reg-expressions.md)。

### <a name="forwarding-request"></a>轉送要求

您可以將下列轉接要求用於 RADIUS 存取要求訊息的選項：

- **驗證要求此伺服器上的**。 使用此設定，NPS 使用 Windows nt4.0 網域、Active Directory 或本機安全性帳號 Manager（坡）使用者帳號資料庫驗證連接要求。 此設定也會指定對應的網路原則設定 NPS，在撥號的屬性帳號，以及由授權連接要求 NPS。 此時，請 NPS 伺服器為 RADIUS 伺服器執行設定。

- **轉送要求下列遠端 RADIUS 伺服器群組到**。 使用此設定，NPS 轉送給指定遠端 RADIUS 伺服器群組連接要求。 NPS 伺服器接收有效的存取權接受郵件對應存取要求訊息時，如果連接嘗試視為驗證而且授權。 此時，請 NPS 伺服器扮演 RADIUS proxy。

- **接受使用者驗證憑證的而**。 使用此設定，來 NPS 不會驗證嘗試連上網路的使用者身分和 NPS 不會嘗試驗證使用者或電腦已連上網路的權限。 NPS 已允許未經授權的存取權時收到連接要求、NPS 會立即傳送存取接受 RADIUS client 的使用者或電腦的訊息會授與網路存取權。 某些類型的位置的存取 client 使用通道之前驗證使用者認證強制通道會使用此設定。

>[!NOTE]
>存取 client 的驗證通訊協定 MS-CHAP v2 或延伸驗證通訊協定傳輸層安全性 (EAP-TLS)，這兩種提供互加好友的驗證時無法使用此驗證選項。 在 [互加好友的驗證存取 client 證明它是有效的存取 client 驗證伺服器（伺服器 NPS），而且很存取 client 有效驗證伺服器的驗證伺服器證明。 使用此驗證選項時，會傳回存取接受訊息。 不過，驗證伺服器無法提供給存取 client、驗證和互加好友的驗證失敗。

如何使用規則運算式建立遠端 RADIUS 伺服器群組，轉送給指定的領域名稱 RADIUS 訊息的路徑規則的範例，請參考主題中的區段「範例 RADIUS 郵件轉寄 proxy 伺服器][在 NPS 使用規則運算式](nps-crp-reg-expressions.md)。

### <a name="advanced"></a>進階

您可以設定進階的屬性，指定的 RADIUS 屬性一系列：

- 當正伺服器 NPS RADIUS 驗證或計量伺服器加入 RADIUS 回應訊息。 屬性，指定的網路原則和連接要求原則上時，會傳送 RADIUS 回應訊息中的屬性的屬性兩個組的組合。
- 當正伺服器 NPS RADIUS 驗證或計量 proxy 加入 RADIUS 訊息。 如果屬性已經轉送訊息，這會取代連接要求原則中指定的屬性的值。 

此外，可供連接設定的某些屬性，要求原則**設定**索引標籤中**進階]**分類提供的特殊的功能。 例如，您可以設定**Windows 使用者對應遠端 RADIUS**當您想要分割驗證，以及連接要求之間兩個使用者的授權帳號資料庫屬性。

**Windows 使用者對應遠端 RADIUS**屬性，指定的 Windows 授權，就會發生的使用者透過遠端 RADIUS 伺服器的驗證。 亦即，遠端 RADIUS 伺服器遠端使用者帳號資料庫中執行驗證的帳號，但 NPS 本機伺服器會授與對使用者的本機帳號資料庫中帳號連接要求。 當您想要允許訪客存取您的網路，這非常有用。

例如訪客合作夥伴可以自己合作夥伴公司 RADIUS 伺服器的驗證，再使用 Windows 使用者帳號，您的組織來賓區域網路（區域網路）您網路上的存取。

其他屬性所提供的特殊的功能包括：

- **MS-隔離-IPFilter 與 MS 隔離-工作階段逾**。 您路由並遠端存取 VPN 部署網路存取隔離控制項 (NAQC) 時，會使用這些屬性。
- **Passport 型使用者對應-UPN-尾碼**。 此屬性可以讓您驗證使用 Windows Live™ ID 使用者 account 認證連接要求。
- **通道標籤**。 此屬性指定的連接應該部署區域網路 (Vlan) 時的 nas 指派 VLAN 編號。

## <a name="default-connection-request-policy"></a>預設連接要求原則

當您安裝 NPS 建立預設連接要求原則。 這項原則有下列設定。

- 未設定驗證。
- 計量未設定為往後遠端 RADIUS 伺服器群組計量資訊。
- 未設定屬性，轉送給連接要求遠端 RADIUS 伺服器群組操作規則的屬性。
- 轉送要求是設定為使連接要求驗證，而且僅授權 NPS 本機伺服器上。
- 未設定進階的屬性。

預設連接要求原則使用 NPS RADIUS 伺服器。 若要將執行 NPS 做為 RADIUS proxy 伺服器設定，您必須也設定遠端 RADIUS 伺服器群組。 使用新的連接要求原則精靈建立新連接要求原則時，您可以建立新的遠端 RADIUS 伺服器群組。 您可以 delete 預設連接要求原則或驗證預設連接要求原則是最後 nps 處理放置原則排序清單中的最後一次的原則。

>[!NOTE]
>如果已安裝 NPS 和遠端存取服務遠端存取相同的電腦上設定 Windows 驗證服務及計量，可能會遠端存取驗證及計量要求轉寄 RADIUS 伺服器。 遠端存取驗證及計量要求符合連接要求原則是設定為轉寄到遠端的 RADIUS 伺服器群組時，這可能會發生。