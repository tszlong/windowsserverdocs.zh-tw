---
title: 網路原則伺服器 (NPS)
description: 本主題概要說明 Windows Server 2016 和 Windows Server 2019 中的網路原則伺服器，並包含 NPS 其他指引的連結。
manager: dougkim
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: lizross
author: eross-msft
ms.date: 06/20/2018
ms.openlocfilehash: 04498a73c04df8f9bce091c8ba583c323f28bbf1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990411"
---
# <a name="network-policy-server-nps"></a>網路原則伺服器 (NPS)

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2019

您可以使用本主題，以瞭解 Windows Server 2016 和 Windows Server 2019 中的網路原則伺服器。 當您在 Windows Server 2016 和伺服器2019中安裝網路原則與存取服務 (NPAS) 功能時，會安裝 NPS。

> [!NOTE]
> 除了本主題之外，還提供下列 NPS 檔。
> - [網路原則伺服器最佳做法](nps-best-practices.md)
> - [開始使用網路原則伺服器](nps-getstart-top.md)
> - [規劃網路原則伺服器](nps-plan-top.md)
> - [部署網路原則伺服器](nps-deploy.md)
> - [管理網路原則伺服器](nps-manage-top.md)
> - Windows PowerShell 中適用于 Windows Server 2016 和 Windows 10 的[網路原則伺服器 (NPS) Cmdlet](/powershell/module/nps/?view=win10-ps)
> - Windows PowerShell 中適用于 Windows Server 2012 R2 和 Windows 8.1 的[網路原則伺服器 (NPS) Cmdlet](/powershell/module/nps/?view=win10-ps)
> - Windows PowerShell 中適用于 Windows Server 2012 和 Windows 8 的[NPS Cmdlet](/powershell/module/nps/?view=win10-ps)

網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

您也可以將 NPS 設定為遠端驗證撥入使用者服務 (RADIUS) proxy，將連線要求轉送到遠端 NPS 或其他 RADIUS 伺服器，讓您可以對連線要求進行負載平衡，並將它們轉送到正確的網域以進行驗證和授權。

NPS 可讓您使用下列功能，集中設定和管理網路存取驗證、授權和帳戶處理：

- **RADIUS 伺服器**。 NPS 會針對無線、驗證交換器、遠端存取撥號和虛擬私人網路 (VPN) 連線，執行集中式驗證、授權和帳戶處理。 當您使用 NPS 做為 RADIUS 伺服器時，可以設定網路存取伺服器 (例如無線存取點與 VPN 伺服器) 做為 NPS 中的 RADIUS 用戶端。 您也可以設定 NPS 用來授權連線要求的網路原則，並且可以設定 RADIUS 帳戶處理，讓 NPS 將計量資訊記錄到本機硬碟上或 Microsoft SQL Server 資料庫中的記錄檔。 如需詳細資訊，請參閱[RADIUS 伺服器](#radius-server)。
- **RADIUS proxy**。 當您使用 NPS 做為 RADIUS proxy 時，您可以設定連線要求原則，告訴 NPS 哪些連線要求轉送到其他 RADIUS 伺服器，以及要將連線要求轉送到哪些 RADIUS 伺服器。 您也可以設定 NPS 轉送要由遠端 RADIUS 伺服器群組中一或多部電腦記錄的計量資料。 若要將 NPS 設定為 RADIUS proxy 伺服器，請參閱下列主題。 如需詳細資訊，請參閱[RADIUS proxy](#radius-proxy)。
    - [設定連線要求原則](nps-crp-configure.md)
- **RADIUS 帳戶**處理。 您可以設定 NPS 將事件記錄到本機記錄檔或 Microsoft SQL Server 的本機或遠端實例。 如需詳細資訊，請參閱[NPS 記錄](#nps-logging)。

> [!IMPORTANT]
> 網路存取保護 \( NAP \) 、健康情況登錄 \( 授權 \) 單位 HRA 和主機認證授權通訊協定 \( HCAP \) 在 windows server 2012 R2 中已被取代，且在 windows server 2016 中無法使用。 如果您使用 Windows Server 2016 之前的作業系統進行 NAP 部署，就無法將 NAP 部署遷移至 Windows Server 2016。

您可以使用這些功能的任何組合來設定 NPS。 例如，您可以將一個 NPS 設定為 VPN 連線的 RADIUS 伺服器，同時做為 RADIUS proxy，將一些連線要求轉送到遠端 RADIUS 伺服器群組的成員，以便在另一個網域中進行驗證和授權。

## <a name="windows-server-editions-and-nps"></a>Windows Server 版本和 NPS

NPS 會根據您安裝的 Windows Server 版本提供不同的功能。

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 或 Windows Server 2019 Standard/Datacenter Edition

使用 Windows Server 2016 Standard 或 Datacenter 中的 NPS，您可以設定不限數量的 RADIUS 用戶端和遠端 RADIUS 伺服器群組。 還可以藉由指定 IP 位址範圍設定 RADIUS 用戶端。

> [!NOTE]
> 使用 Server Core 安裝選項安裝的系統上無法使用 [WIndows 網路原則與存取服務] 功能。

下列各節提供更多有關 NPS 作為 RADIUS 伺服器和 proxy 的詳細資訊。

## <a name="radius-server-and-proxy"></a>RADIUS 伺服器及 Proxy

您可以使用 NPS 做為 RADIUS 伺服器、RADIUS proxy 或兩者。

### <a name="radius-server"></a>RADIUS 伺服器

NPS 是 Microsoft 在 \( \) rfc 2865 和2866中，由網際網路工程任務強制 IETF 所指定之 RADIUS 標準的實施。 做為 RADIUS 伺服器，NPS 會針對許多網路存取類型執行集中式連線驗證、授權和帳戶管理，包括無線、驗證交換器、撥號和虛擬私人網路 \( VPN \) 遠端存取，以及路由器對路由器連線。

> [!NOTE]
> 如需將 NPS 部署為 RADIUS 伺服器的相關資訊，請參閱[部署網路原則伺服器](nps-deploy.md)。

NPS 啟用無線、交換器、遠端存取或 VPN 設備的異質集使用。 您可以將 NPS 與遠端存取服務搭配使用，這可在 Windows Server 2016 中取得。

NPS 會使用 Active Directory Domain Services \( AD DS \) 網域或本機安全性帳戶管理員 (SAM) 使用者帳戶資料庫來驗證使用者認證以進行連線嘗試。 當執行 NPS 的伺服器是 AD DS 網域的成員時，NPS 會使用目錄服務做為其使用者帳戶資料庫，而且是單一登入解決方案的一部分。 相同的認證集用於網路存取控制 \( 驗證和授權存取網路 \) ，以及登入 AD DS 網域。

> [!NOTE]
> NPS 使用使用者帳戶的撥入內容以及網路原則來授權連線。

\( \) 無論所使用的網路存取裝置類型為何，網際網路服務提供者的 isp 和維護網路存取的組織，都能從單一管理點管理所有網路存取類型的挑戰。 RADIUS 標準在同質和異質環境中都支援此功能。 RADIUS 是一種從屬通訊協定，可讓網路存取設備 (當作 RADIUS 用戶端使用) 將驗證和帳戶處理要求提交到 RADIUS 伺服器。

RADIUS 伺服器有權存取使用者帳戶資訊，且可檢查網路存取驗證認證。 如果驗證使用者認證並授權連線嘗試，RADIUS 伺服器會根據指定的條件來授權使用者存取，然後在帳戶處理記錄中記錄網路存取連線。 使用 RADIUS 可讓您在集中位置收集和管理網路存取使用者驗證、授權以及帳戶處理資料，不需在每個存取伺服器上執行這些工作。

#### <a name="using-nps-as-a-radius-server"></a>使用 NPS 做為 RADIUS 伺服器

您可以在下列情況使用 NPS 做為 RADIUS 伺服器：

- 您使用 AD DS 網域或本機 SAM 使用者帳戶資料庫做為存取用戶端的使用者帳戶資料庫。
- 您在多部撥號伺服器、VPN 伺服器或指定撥號路由器上使用遠端存取，而您想要集中設定網路原則和連線記錄和帳戶處理。
- 您將撥號、VPN 或無線存取外包給服務提供者。 存取伺服器使用 RADIUS 來驗證和授權您組織成員所建立的連線。
- 您想要集中管理一組異質存取伺服器的驗證、授權以及帳戶處理。

下圖顯示 NPS 作為各種存取用戶端的 RADIUS 伺服器。

![NPS 做為 RADIUS 伺服器](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>RADIUS Proxy

做為 RADIUS proxy，NPS 會將驗證和帳戶處理訊息轉送到 NPS 和其他 RADIUS 伺服器。 您可以使用 NPS 做為 RADIUS proxy，以提供 radius 用戶端（ \( 也稱為網路存取伺服器）和 radius 伺服器之間的 radius 訊息路由，以 \) 執行連線嘗試的使用者驗證、授權和帳戶處理。

做為 RADIUS Proxy 時，NPS 是 RADIUS 存取和處理帳戶訊息流的中央交換點或路由點。 NPS 將轉寄訊息的相關資訊記錄在帳戶處理記錄中。

#### <a name="using-nps-as-a-radius-proxy"></a>使用 NPS 做為 RADIUS proxy

當下列條件成立時，您可以使用 NPS 做為 RADIUS Proxy：

- 您是為多個客戶提供外包撥號、VPN 或無線網路存取服務的服務提供者。 您的 Nas 會將連接要求傳送到 NPS RADIUS proxy。 NPS RADIUS Proxy 會根據連線要求中的使用者名稱領域部分，將連線要求轉寄到由客戶維護的 RADIUS 伺服器，而且可驗證和授權連線嘗試。
- 您想要為使用者帳戶提供驗證和授權，而不是 NPS 所屬網域的成員，或是另一個與 NPS 所屬網域具有雙向信任關係的網域。 這些帳戶包括不受信任的網域、單向受信任的網域和其他樹系中的帳戶。 您可以不要設定存取伺服器將它們的連線要求傳送到 NPS RADIUS 伺服器，而是傳送到 NPS RADIUS Proxy。 NPS RADIUS proxy 會使用使用者名稱的領域名稱部分，並將要求轉送至正確網域或樹系中的 NPS。 對一個網域或樹系中的使用者帳戶進行連線嘗試，可以針對另一個網域或樹系中的 Nas 進行驗證。
- 您要使用不是 Windows 帳戶資料庫的資料庫執行驗證與授權。 在這種情況下，會將符合指定領域名稱的連線要求轉寄到 RADIUS 伺服器，此伺服器可以存取不同使用者帳戶的資料庫與授權資料。 其他使用者資料庫的範例包括 Novell 目錄服務 (NDS) 與結構化查詢語言 (SQL) 資料庫。
- 您要處理大量的連線要求。 在這種情況下，您可以不要設定 RADIUS 用戶端嘗試在多個 RADIUS 伺服器之間平衡它們的連線與帳戶處理要求，而是將連線與帳戶處理要求傳送到 NPS RADIUS Proxy。 NPS RADIUS proxy 會動態地平衡多部 RADIUS 伺服器之間的連線和帳戶管理要求負載，並增加每秒處理大量 RADIUS 用戶端和驗證的次數。
- 您要提供 RADIUS 驗證與授權給委外服務提供者，並將內部網路防火牆設定減至最低。 內部網路防火牆介於周邊網路 (內部網路與網際網路之間的網路) 與內部網路之間。 藉由將 NPS 放在您的周邊網路上，您周邊網路與內部網路之間的防火牆必須允許 NPS 與多個網域控制站之間的流量流動。 藉由使用 NPS proxy 來取代 NPS，防火牆必須僅允許在 NPS proxy 與內部網路內的一或多個 Nps 之間流動的 RADIUS 流量。

下圖顯示 NPS 做為 RADIUS 用戶端與 RADIUS 伺服器之間的 RADIUS proxy。

![NPS 做為 RADIUS Proxy](../../media/Nps-Proxy/Nps-Proxy.jpg)

利用 NPS，組織也可以將遠端存取基礎結構外包給服務提供者，同時保留使用者驗證、授權以及帳戶處理的控制。

在下列案例中，可以建立 NPS 設定：

- 無線存取
- 組織撥號或虛擬私人網路 (VPN) 遠端存取
- 委外撥號或無線存取
- 網際網路存取
- 企業夥伴驗證存取外部網路資源

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>RADIUS 伺服器與 RADIUS Proxy 設定範例

下列設定範例示範如何將 NPS 設定為 RADIUS 伺服器與 RADIUS Proxy。

**NPS 做為 RADIUS 伺服器**。 在此範例中，NPS 設定為 RADIUS 伺服器，預設連線要求原則是唯一設定的原則，而所有連線要求都是由本機 NPS 處理。 NPS 可以驗證和授權其帳戶位於 NPS 網域和受信任網域中的使用者。

**NPS 做為 RADIUS proxy**。 在此範例中，NPS 設定為 RADIUS proxy，將連線要求轉送至兩個不受信任網域中的遠端 RADIUS 伺服器群組。 系統會刪除預設連線要求原則，並建立兩個新的連線要求原則，以將要求轉送到這兩個不受信任網域中的每一個。 在此範例中，NPS 不會處理本機伺服器上的任何連接要求。

**NPS 同時做為 radius 伺服器和 radius proxy**。 除了指定在本機處理連線要求的預設連線要求原則之外，還會建立新的連線要求原則，將連線要求轉送到不受信任網域中的 NPS 或其他 RADIUS 伺服器。 第二個原則稱為 Proxy 原則。 在此範例中，Proxy 原則會顯示在原則排序清單中的第一個。 如果連線要求符合 Proxy 原則，會將連線要求轉送到遠端 RADIUS 伺服器群組中的 RADIUS 伺服器。 如果連線要求不符合 Proxy 原則，但符合預設的連線要求原則，NPS 會在本機伺服器上處理連線要求。 如果連線要求不符合任一原則，就會被捨棄。

**NPS 作為具有遠端帳戶處理伺服器的 RADIUS 伺服器**。 在此範例中，本機 NPS 未設定為執行帳戶處理，而且會修訂預設連線要求原則，以便將 RADIUS 帳戶處理訊息轉送到遠端 RADIUS 伺服器群組中的 NPS 或其他 RADIUS 伺服器。 雖然會轉送帳戶處理訊息，但不會轉送驗證和授權訊息，而且本機 NPS 會針對本機網域和所有信任的網域執行這些功能。

**具有遠端 RADIUS 到 Windows 使用者對應的 NPS**。 在此範例中，NPS 扮演每個連線要求的 RADIUS 伺服器與 RADIUS Proxy，將驗證要求轉送到遠端 RADIUS 伺服器，而使用本機 Windows 使用者帳戶進行授權。 藉由設定 [Windows 使用者對應的遠端 RADIUS] 屬性做為連線要求原則的條件，可以實作此設定。 \(此外，您必須在與遠端使用者帳戶相同名稱的 RADIUS 伺服器本機上建立使用者帳戶，以供遠端 RADIUS 伺服器執行驗證。\)

## <a name="configuration"></a>組態

若要將 NPS 設定為 RADIUS 伺服器，您可以在 NPS 主控台或伺服器管理員中，使用 [標準設定] 或 [advanced configuration]。 若要設定 NPS 做為 RADIUS Proxy，則必須使用進階設定。

### <a name="standard-configuration"></a>標準設定

使用標準設定時，會提供精靈協助您針對下列情況設定 NPS：

- 撥號或 VPN 連線的 RADIUS 伺服器
- 802.1X 無線或有線連線的 RADIUS 伺服器

若要使用精靈設定 NPS，請開啟 NPS 主控台，選取前述其中一種情況，然後按一下連結來開啟精靈。

### <a name="advanced-configuration"></a>進階組態

當您使用 [advanced configuration] 時，您會以手動方式將 NPS 設定為 RADIUS 伺服器或 RADIUS proxy。

若要使用 [advanced configuration] 設定 NPS，請開啟 NPS 主控台，然後按一下 [ **Advanced configuration** ] 旁的箭號以展開此區段。

會提供下列進階設定項目。

#### <a name="configure-radius-server"></a>設定 RADIUS 伺服器

若要設定 NPS 做為 RADIUS 伺服器，您必須設定 RADIUS 用戶端、網路原則以及 RADIUS 帳戶處理。

如需有關進行這些設定的指示，請參閱下列主題。

- [設定 RADIUS 用戶端](nps-radius-clients-configure.md)
- [設定網路原則](nps-np-configure.md)
- [設定網路原則伺服器計量](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>設定 RADIUS Proxy

若要設定 NPS 做為 RADIUS Proxy，您必須設定 RADIUS 用戶端、遠端 RADIUS 伺服器群組以及連線要求原則。

如需有關進行這些設定的指示，請參閱下列主題。

- [設定 RADIUS 用戶端](nps-radius-clients-configure.md)
- [設定遠端 RADIUS 伺服器群組](nps-crp-rrsg-configure.md)
- [設定連線要求原則](nps-crp-configure.md)

## <a name="nps-logging"></a>NPS 記錄

NPS 記錄也稱為 RADIUS 帳戶處理。 設定 NPS 記錄以符合您的需求，不論 NPS 是用來做為 RADIUS 伺服器、proxy 或這些設定的任何組合。

若要設定 NPS 記錄，您必須設定您要記錄並使用事件檢視器查看哪些事件，然後決定您想要記錄哪些其他資訊。 此外，您還必須決定要將使用者驗證與帳戶處理資訊記錄到儲存在本機電腦的記錄檔中，或是記錄到本機電腦或遠端電腦的 SQL Server 資料庫中。

如需詳細資訊，請參閱[設定網路原則伺服器帳戶](nps-accounting-configure.md)處理。