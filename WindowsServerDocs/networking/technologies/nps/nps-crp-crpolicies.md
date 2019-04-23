---
title: 連線要求原則
description: 本主題提供 Windows Server 2016 中的網路原則伺服器連線要求原則的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21330b3838064c7b31e78ef70bdc04f0db0f50db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872939"
---
# <a name="connection-request-policies"></a>連線要求原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何使用 NPS 連線要求原則將 NPS 設定為 RADIUS 伺服器、 RADIUS proxy，或兩者。

>[!NOTE]
>本主題中，除了下列的連線要求原則文件使用。
> - [設定連線要求原則](nps-crp-configure.md)
> - [設定遠端 RADIUS 伺服器群組](nps-crp-rrsg-configure.md)

連線要求原則是條件的集合，並讓網路系統管理員指定哪些遠端驗證撥號使用者服務 (RADIUS) 伺服器的設定，請執行驗證和授權連線要求執行網路原則伺服器 (NPS) 伺服器接收自 RADIUS 用戶端。 若要指定哪些 RADIUS 伺服器用於 RADIUS 帳戶處理，就可以設定連線要求原則。

您可以建立連線要求原則，以便在本機處理從 RADIUS 用戶端傳送一些 RADIUS 要求訊息 （NPS 當作 RADIUS 伺服器） 和其他類型的訊息轉送到另一部 RADIUS 伺服器 （NPS 作為 RADIUS proxy）。

連線要求原則，您可以使用 NPS 做為 RADIUS 伺服器，或做為 RADIUS proxy，如下所示的因素： 

- 當日時間與星期幾
- 連線要求中的領域名稱
- 所要求的連線類型
- RADIUS 用戶端 IP 位址

處理或是只有當內送訊息的設定符合至少其中一個連線要求原則，NPS 上設定 nps 轉送 RADIUS Access-request 訊息。 

如果符合的原則設定的原則要求必須由 NPS 處理訊息，NPS 會扮演 RADIUS 伺服器，驗證和授權連線要求。 如果符合的原則設定的原則要求必須由 NPS 轉送訊息，NPS 做為 RADIUS proxy，並轉送連線要求給遠端 RADIUS 伺服器進行處理。

如果連入 RADIUS Access-request 訊息的設定不符合至少其中一個連線要求原則，將 Access-reject 訊息傳送的 RADIUS 用戶端，並嘗試連線到網路的電腦或使用者被拒絕存取。

## <a name="configuration-examples"></a>設定範例

下列組態範例示範如何使用連線要求原則。

### <a name="nps-as-a-radius-server"></a>做為 RADIUS 伺服器的 NPS

預設連線要求原則是唯一的設定的原則。 在此範例中，NPS 被設定為 RADIUS 伺服器，並由本機 NPS 處理所有的連線要求。 NPS 可驗證及授權其帳戶在 NPS 網域的網域和信任的網域使用者。


### <a name="nps-as-a-radius-proxy"></a>做為 RADIUS Proxy 的 NPS

刪除預設連線要求原則，並將要求轉送至兩個不同的網域建立兩個新的連線要求原則。 在此範例中，NPS 被設定為 RADIUS proxy。 NPS 不會處理在本機伺服器上的任何連線要求。 相反地，它將連接要求轉送到 NPS 或其他 RADIUS 伺服器設定為遠端 RADIUS 伺服器群組的成員。


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS 做為 RADIUS 伺服器與 RADIUS proxy

除了預設連線要求原則，被建立新的連線要求原則，將連線要求轉送到 NPS 或其他 RADIUS 伺服器不信任的網域中。 在此範例中，proxy 原則會顯示原則的已排序清單中的第一個。 如果連線要求符合 proxy 原則，連接要求被轉送到遠端 RADIUS 伺服器群組中的 RADIUS 伺服器。 如果連線要求不符合 proxy 原則，但符合預設連線要求原則，NPS 會處理連線要求，在本機伺服器上。 如果連線要求不符合任一原則，它會將它捨棄。

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS 做為 RADIUS 伺服器的遠端帳戶處理伺服器

在此範例中，本機 NPS 未執行帳戶處理設定，預設連線要求原則會修訂案例使 RADIUS 帳戶處理訊息轉送到 NPS 或其他 RADIUS 伺服器的遠端 RADIUS 伺服器群組中。 雖然會轉送帳戶處理訊息，但不會轉送驗證和授權的訊息，本機 NPS 的本機網域中執行這些函式和所有信任的網域。


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS 以搭配 Windows 使用者對應的遠端 RADIUS

在此範例中，NPS 會扮演做為 RADIUS 伺服器，並為每個個別的連線要求的 RADIUS proxy 藉由將驗證要求轉送到遠端 RADIUS 伺服器時使用本機 Windows 使用者帳戶進行授權。 此設定被藉由設定 Windows 使用者對應屬性的遠端 RADIUS 連線要求原則的條件。 （此外，使用者帳戶必須在本機建立具有相同名稱做為遠端 RADIUS 伺服器對其執行驗證的遠端使用者帳戶。）

## <a name="connection-request-policy-conditions"></a>連線要求原則條件

連線要求原則條件是一或多個 RADIUS 屬性相較於連入 RADIUS Access-request 訊息的屬性。 如果有多個條件，然後在連接中條件的所有要求訊息，並在連線要求原則必須符合由 NPS 強制執行原則。


以下是您可以設定連線要求原則中的可用條件屬性。

### <a name="connection-properties-attribute-group"></a>連接屬性的屬性群組

連線內容 屬性群組包含下列屬性。

- **框架處理通訊協定**。 用來指定的連入封包的框架類型。 範例包括點對點通訊協定 (PPP)、 序列線路網際網路通訊協定 (SLIP)、 框架轉接以及 X.25。
- **服務類型**。 用來指定服務所要求的型別。 範例包括已框架處理 （例如 PPP 連線） 並登入 （例如 Telnet 連線）。 如需 RADIUS 服務類型的詳細資訊，請參閱 RFC 2865 「 遠端驗證撥入使用者服務 (RADIUS)。 」
- **通道類型**。 用來指定所要求的用戶端正在建立的通道類型。 通道類型包括 Point-to-Point Tunneling Protocol (PPTP) 和 Layer Two Tunneling Protocol (L2TP)。

### <a name="day-and-time-restrictions-attribute-group"></a>日期和時間限制的屬性群組 

日期和時間限制的屬性群組包含的日期和時間限制的屬性。 利用此屬性，您可以指定一週的星期幾和連線嘗試的當日時間。 日期和時間是相對於的日期和時間的 NPS。

### <a name="gateway-attribute-group"></a>閘道屬性群組

閘道屬性群組包含下列屬性。

- **呼叫的工作站識別碼**。 用來指定網路存取伺服器的電話號碼。 這個屬性是字元字串。 您可以使用模式比對語法指定區碼。
- **NAS 識別碼**。 用來指定網路存取伺服器的名稱。 這個屬性是字元字串。 您可以使用模式比對語法指定 NAS 識別碼。
- **NAS IPv4 位址**。 用來指定 Internet Protocol version 4 (IPv4) 位址的網路存取伺服器 （RADIUS 用戶端）。 這個屬性是字元字串。 您可以使用模式比對語法指定 IP 網路。
- **NAS IPv6 位址**。 用來指定網際網路通訊協定第 6 版 (IPv6) 位址的網路存取伺服器 （RADIUS 用戶端）。 這個屬性是字元字串。 您可以使用模式比對語法指定 IP 網路。
- **NAS 連接埠類型**。 用來指定存取用戶端所使用的媒體類型。 範例包括類比電話線 （稱為非同步）、 整合式服務數位網路 (ISDN)、 通道或虛擬私人網路 (Vpn)、 IEEE 802.11 無線以及乙太網路交換器。

### <a name="machine-identity-attribute-group"></a>機器身分識別屬性群組

機器身分識別屬性群組包含電腦身分識別屬性。 藉由使用這個屬性，您可以指定用戶端與識別的方法在原則中。

### <a name="radius-client-properties-attribute-group"></a>RADIUS 用戶端內容屬性群組

RADIUS 用戶端內容屬性群組包含下列屬性。

- **呼叫的工作站識別碼**。 用來指定呼叫端 （存取用戶端） 所使用的電話號碼。 這個屬性是字元字串。 您可以使用模式比對語法指定區碼。  802.1x 驗證在 MAC 位址通常會填入，並可以從用戶端中比對。  此欄位通常會用 Mac 位址略過的情況下，當連線要求原則設定為 [接受使用者而不驗證認證]。  
- **用戶端的好記名稱**。 用來指定要求驗證的 RADIUS 用戶端電腦的名稱。 這個屬性是字元字串。 您可以使用模式比對語法指定用戶端名稱。
- **用戶端 IPv4 位址**。 用來指定網路存取伺服器 （RADIUS 用戶端） 的 IPv4 位址。 這個屬性是字元字串。 您可以使用模式比對語法指定 IP 網路。
- **用戶端 IPv6 位址**。 用來指定網路存取伺服器 （RADIUS 用戶端） 的 IPv6 位址。 這個屬性是字元字串。 您可以使用模式比對語法指定 IP 網路。
- **用戶端廠商**。 用來指定要求驗證的網路存取伺服器廠商。 執行路由及遠端存取服務的電腦是 Microsoft NAS 製造商。 您可以使用這個屬性來設定不同 NAS 製造商的個別原則。 這個屬性是字元字串。 您可以使用模式比對語法。

### <a name="user-name-attribute-group"></a>使用者名稱屬性群組

使用者名稱屬性群組包含使用者名稱屬性。 藉由使用這個屬性，您可以指定使用者名稱或使用者名稱，必須符合 RADIUS 訊息中存取用戶端所提供的使用者名稱部分。 這個屬性是字元字串，其中通常包含領域名稱與使用者帳戶名稱。 您可以使用模式比對語法指定使用者名稱。

## <a name="connection-request-policy-settings"></a>連線要求原則設定

連線要求原則設定是一組套用至傳入的 RADIUS 訊息的屬性。 設定包含下列屬性群組。

- 驗證
- 帳戶處理
- 屬性操作
- 轉寄要求
- 進階

下列各節提供有關這些設定的其他詳細資料。

### <a name="authentication"></a>驗證

藉由使用這項設定，您可以覆寫所有的網路原則中設定的驗證設定，您可以指定驗證方法及連線到您的網路所需的類型。

>[!IMPORTANT]
>如果您在比您在網路原則中設定的驗證方法更不安全的連線要求原則設定的驗證方法時，會覆寫您在網路原則中設定其他安全性驗證方法。 例如，如果您有一個網路原則，需要使用受保護的可延伸驗證通訊協定 Microsoft Challenge Handshake 驗證通訊協定第 2 版\(PEAP MS-CHAP v2\)，這是以密碼為基礎為安全的無線、 驗證方法，而且您也會設定連線要求原則以允許未驗證的存取，結果就是沒有用戶端所使用 PEAP MS-CHAP v2 進行驗證。 在此範例中，會授與連線到網路的所有用戶端未經驗證的存取。

### <a name="accounting"></a>帳戶處理

藉由使用這項設定，您可以設定連線要求原則，將帳戶處理資訊轉送到 NPS 或其他 RADIUS 伺服器的遠端 RADIUS 伺服器群組中，以便遠端 RADIUS 伺服器群組執行帳戶處理。

>[!NOTE]
>如果您有多個 RADIUS 伺服器，而且您想要儲存在一個集中的 RADIUS 帳戶處理資料庫中的所有伺服器的帳戶處理資訊，您可以使用原則，以在每個 RADIUS 伺服器上的連線要求原則帳戶處理設定中的帳戶處理資料轉送所有的一個 nps 伺服器或其他 RADIUS 伺服器指定為帳戶處理伺服器。

連線要求原則帳戶處理設定功能無關的本機 NPS 帳戶處理設定。 換句話說，如果您設定本機 NPS 記錄到本機檔案或 Microsoft SQL Server 資料庫的 RADIUS 帳戶處理資訊時，它就會如此不論您是否設定連線要求原則，將帳戶處理訊息轉送到遠端 RADIUS伺服器群組。

如果您想從遠端，但不是在本機記錄帳戶處理資訊時，您必須設定本機 NPS 未執行帳戶處理，同時也設定連線要求原則帳戶處理資料轉送到遠端 RADIUS 伺服器群組中的 帳戶處理。

### <a name="attribute-manipulation"></a>屬性操作

您可以設定一組操作的下列屬性的其中一個文字字串的尋找和取代規則。

- 使用者名稱
- 被呼叫的工作站識別碼
- 呼叫的工作站識別碼

RADIUS 訊息受限於驗證和帳戶處理設定之前，尋找和取代規則處理就會發生的其中一個前述屬性。 屬性操作規則只適用於單一屬性。 您無法設定為每個屬性的屬性操作規則。 此外，您可以操作的屬性清單是靜態清單;您無法加入屬性可用於操作的清單。

>[!NOTE]
>如果您使用 MS-CHAP v2 驗證通訊協定，您不能操作的使用者名稱屬性，如果連線要求原則用來轉送 RADIUS 訊息。 唯一的例外狀況發生於反斜線 (\)用字元和操作只會影響它左邊的資訊。 反斜線字元通常用來指出網域名稱 （資訊左邊反斜線字元） 和網域 （反斜線字元右邊的資訊） 內的使用者帳戶名稱。 在此情況下，允許只屬性操作規則修改或取代網域名稱。

如需如何管理中 使用者名稱屬性，領域名稱的範例，請參閱主題中的 < 操作的使用者名稱屬性中的領域名稱的範例 > 一節[NPS 中的使用規則運算式](nps-crp-reg-expressions.md)。

### <a name="forwarding-request"></a>轉寄要求

您可以設定下列用於 RADIUS Access-request 訊息的轉寄要求選項：

- **驗證此伺服器上的要求**。 藉由使用這項設定，NPS 會使用 Windows NT 4.0 網域、 Active Directory 或本機安全性帳戶管理員 (SAM) 使用者帳戶資料庫來驗證連線要求。 這項設定也會指定，NPS 會使用在 NPS 中設定的使用者帳戶撥入內容以及相符網路原則來授權連線要求。 在此情況下，將 NPS 設定為執行做為 RADIUS 伺服器。

- **將要求轉送到下列遠端 RADIUS 伺服器群組**。 藉由使用這項設定，NPS 會將連線要求轉送到您指定的遠端 RADIUS 伺服器群組。 如果 NPS 收到有效的 Access-accept 訊息中對應到 Access-request 訊息，連線嘗試會被視為已驗證和授權。 在此情況下，NPS 會扮演 RADIUS proxy。

- **不確認認證即接受使用者**。 藉由使用這項設定，NPS 不會驗證嘗試連線到網路的使用者的身分識別，以及 NPS 不會嘗試驗證的使用者或電腦已連線到網路的權限。 當 NPS 被設定為允許未經驗證的存取，以及它收到連接要求時，NPS 便會立即傳送 RADIUS 用戶端的使用者或電腦的 Access-accept 訊息獲得網路存取權。 此設定用於某些類型的強制通道，存取用戶端使用通道之前會驗證使用者認證。

>[!NOTE]
>當存取用戶端的驗證通訊協定為 MS-CHAP v2 或可延伸驗證通訊協定-傳輸層安全性 (EAP-TLS)，兩者皆提供相互驗證，則無法使用此驗證選項。 在相互驗證中，存取用戶端證明它是有效的存取用戶端驗證伺服器 (NPS)，而且驗證伺服器證明它是存取用戶端的有效驗證伺服器。 使用此驗證選項時，會傳回 Access-accept 訊息。 不過，驗證伺服器不會提供存取用戶端驗證，因此相互驗證失敗。

如需範例，示範如何使用規則運算式來建立路由規則，將具有指定的領域名稱的 RADIUS 訊息轉送到遠端 RADIUS 伺服器群組，請參閱主題中的 < proxy 伺服器轉寄 RADIUS 訊息的範例 > 一節[使用在 NPS 中的規則運算式](nps-crp-reg-expressions.md)。

### <a name="advanced"></a>進階

您可以設定進階的屬性，以指定此系列 RADIUS 屬性：

- 當 NPS 做為 RADIUS 驗證或帳戶處理伺服器時，請新增至 RADIUS 回應訊息。 網路原則與連線要求原則中指定的屬性時，會傳送 RADIUS 回應訊息中的屬性會是兩組屬性的組合。
- 當 NPS 做為 RADIUS 驗證或帳戶處理 proxy 時，請新增至 RADIUS 訊息。 如果屬性已經存在於所轉送的訊息，它會取代在連線要求原則中指定之屬性的值。 

此外，一些屬性會提供連接上設定要求原則**設定**索引標籤中**進階**類別提供特殊的功能。 例如，您可以設定**Windows 使用者對應的遠端 RADIUS**屬性時，您要分割的驗證和授權連線要求之間兩個使用者的帳戶資料庫。

**Windows 使用者對應的遠端 RADIUS**屬性會指定 Windows 授權的使用者所驗證的遠端 RADIUS 伺服器就會發生。 換句話說，遠端 RADIUS 伺服器驗證的使用者帳戶遠端使用者帳戶在資料庫中執行，但本機 NPS 授權連線要求，針對 本機使用者帳戶資料庫中的使用者帳戶。 當您想要允許訪客存取網路時，這非常有用。

比方說，從夥伴組織的訪客可由其自己的夥伴組織的 RADIUS 伺服器，進行驗證，，然後在您的組織使用的 Windows 使用者帳戶來存取來賓區域網路 (LAN) 網路上。

提供特殊的功能的其他屬性包括：

- **MS Ms-quarantine-ipfilter 與 MS 隔離-工作階段逾**。 當您部署網路存取隔離控制 (NAQC) 與路由及遠端存取 VPN 部署時，會使用這些屬性。
- **Passport-使用者-對應-UPN 尾碼**。 這個屬性可讓您驗證連接要求使用 Windows Live™ ID 使用者帳戶認證。
- **Tunnel-tag**。 此屬性會指定要連接應該部署虛擬區域網路 (Vlan) 時，NAS 所指派的 VLAN ID 編號。

## <a name="default-connection-request-policy"></a>預設連線要求原則

當您安裝 NPS 時，會建立預設連線要求原則。 此原則具有下列組態。

- 未設定驗證。
- 將帳戶處理資訊轉送到遠端 RADIUS 伺服器群組未設定帳戶處理。
- 屬性未設定連線要求轉送到遠端 RADIUS 伺服器群組的屬性操作規則。
- 轉送要求，讓連線要求驗證和授權本機 NPS 上設定。
- 未設定進階的屬性。

預設連線要求原則使用 NPS 做為 RADIUS 伺服器。 若要設定執行 NPS 做為 RADIUS proxy 的伺服器，您也必須設定遠端 RADIUS 伺服器群組。 使用新的連線要求原則精靈建立新的連線要求原則時，您可以建立新的遠端 RADIUS 伺服器群組。 您可以刪除預設連線要求原則，或確認預設連線要求原則是放置在原則排序清單中的上一次，NPS 來處理的最後一個原則。

>[!NOTE]
>如果 NPS 和遠端存取服務安裝在同一部電腦，以及遠端存取服務設定為使用 Windows 驗證和帳戶處理，就可以遠端存取授權和帳戶處理要求轉送到 RADIUS 伺服器. 遠端存取驗證與帳戶處理要求符合連線要求原則設定將其轉送到遠端 RADIUS 伺服器群組時，可能發生這項目。
