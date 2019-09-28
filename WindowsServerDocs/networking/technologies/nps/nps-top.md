---
title: 網路原則伺服器 (NPS)
description: 本主題概要說明 Windows Server 2016 和 Windows Server 2019 中的網路原則伺服器，並包含 NPS 其他指引的連結。
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 5685d4dae742245c131ff2fee3114e905e94e9bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396012"
---
# <a name="network-policy-server-nps"></a>網路原則伺服器 (NPS)

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2019

您可以使用本主題，以瞭解 Windows Server 2016 和 Windows Server 2019 中的網路原則伺服器。 當您在 Windows Server 2016 和伺服器2019中安裝網路原則與存取服務（NPAS）功能時，會安裝 NPS。

> [!NOTE]
> 除了本主題之外，還提供下列 NPS 檔。
> - [網路原則伺服器的最佳作法](nps-best-practices.md)
> - [網路原則伺服器的消費者入門](nps-getstart-top.md)
> - [規劃網路原則伺服器](nps-plan-top.md)
> - [部署網路原則伺服器](nps-deploy.md)
> - [管理網路原則伺服器](nps-manage-top.md)
> - Windows PowerShell 中適用于 Windows Server 2016 和 Windows 10 的[網路原則伺服器（NPS） Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)
> - Windows PowerShell 中適用于 Windows Server 2012 R2 和 Windows 8.1 的[網路原則伺服器（NPS） Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)
> - Windows PowerShell 中適用于 Windows Server 2012 和 Windows 8 的[NPS Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)

網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

您也可以將 NPS 設定為遠端驗證撥入使用者服務（RADIUS） proxy，將連線要求轉送到遠端 NPS 或其他 RADIUS 伺服器，讓您可以對連線要求進行負載平衡，並將它們轉送到正確的網域進行驗證和授權。

NPS 可讓您使用下列功能，集中設定和管理網路存取驗證、授權和帳戶處理：

- **RADIUS 伺服器**。 NPS 會針對無線、驗證交換器、遠端存取撥號和虛擬私人網路（VPN）連線，執行集中式驗證、授權和帳戶處理。 當您使用 NPS 做為 RADIUS 伺服器時，可以設定網路存取伺服器 (例如無線存取點與 VPN 伺服器) 做為 NPS 中的 RADIUS 用戶端。 您也可以設定 NPS 用來授權連線要求的網路原則，並且可以設定 RADIUS 帳戶處理，讓 NPS 將計量資訊記錄到本機硬碟上或 Microsoft SQL Server 資料庫中的記錄檔。 如需詳細資訊，請參閱[RADIUS 伺服器](#radius-server)。
- **RADIUS proxy**。 當您使用 NPS 做為 RADIUS proxy 時，您可以設定連線要求原則，告訴 NPS 哪些連線要求轉送到其他 RADIUS 伺服器，以及要將連線要求轉送到哪些 RADIUS 伺服器。 您也可以設定 NPS 轉送要由遠端 RADIUS 伺服器群組中一或多部電腦記錄的計量資料。 若要將 NPS 設定為 RADIUS proxy 伺服器，請參閱下列主題。 如需詳細資訊，請參閱[RADIUS proxy](#radius-proxy)。
    - [設定連線要求原則](nps-crp-configure.md)
- **RADIUS 帳戶**處理。 您可以設定 NPS 將事件記錄到本機記錄檔或 Microsoft SQL Server 的本機或遠端實例。 如需詳細資訊，請參閱[NPS 記錄](#nps-logging)。

> [!IMPORTANT]
> 網路存取保護 \(NAP @ no__t-1、健康情況登錄授權單位 \(HRA @ no__t-3 和主機認證授權通訊協定 \(HCAP @ no__t-5 在 Windows Server 2012 R2 中已被取代，且在 Windows Server 2016 中無法使用。 如果您使用 Windows Server 2016 之前的作業系統進行 NAP 部署，就無法將 NAP 部署遷移至 Windows Server 2016。

您可以使用這些功能的任何組合來設定 NPS。 例如，您可以將一個 NPS 設定為 VPN 連線的 RADIUS 伺服器，同時做為 RADIUS proxy，將一些連線要求轉送到遠端 RADIUS 伺服器群組的成員，以便在另一個網域中進行驗證和授權。

## <a name="windows-server-editions-and-nps"></a>Windows Server 版本和 NPS

NPS 會根據您安裝的 Windows Server 版本提供不同的功能。

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 或 Windows Server 2019 Standard/Datacenter Edition

使用 Windows Server 2016 Standard 或 Datacenter 中的 NPS，您可以設定不限數量的 RADIUS 用戶端和遠端 RADIUS 伺服器群組。 此外，您可以藉由指定一個 IP 位址範圍來設定 RADIUS 用戶端。

> [!NOTE]
> 使用 Server Core 安裝選項安裝的系統上無法使用 [WIndows 網路原則與存取服務] 功能。

下列各節提供更多有關 NPS 作為 RADIUS 伺服器和 proxy 的詳細資訊。

## <a name="radius-server-and-proxy"></a>RADIUS 伺服器和 proxy

您可以使用 NPS 做為 RADIUS 伺服器、RADIUS proxy 或兩者。

### <a name="radius-server"></a>RADIUS 伺服器

NPS 是 Microsoft 在 Rfc 2865 和2866中，由網際網路工程任務推動小組 \(IETF @ no__t-1 所指定的 RADIUS 標準執行。 做為 RADIUS 伺服器，NPS 會針對許多網路存取類型執行集中式連線驗證、授權和帳戶管理，包括無線、驗證交換器、撥號和虛擬私人網路 \(VPN @ no__t-1 遠端存取，以及路由器對路由器連線。

> [!NOTE]
> 如需將 NPS 部署為 RADIUS 伺服器的相關資訊，請參閱[部署網路原則伺服器](nps-deploy.md)。

NPS 可讓您使用一組不同的無線、交換器、遠端存取或 VPN 設備。 您可以將 NPS 與遠端存取服務搭配使用，這可在 Windows Server 2016 中取得。

NPS 會使用 Active Directory Domain Services @no__t 0AD DS @ no__t-1 網域或本機安全性帳戶管理員（SAM）使用者帳戶資料庫來驗證連線嘗試的使用者認證。 當執行 NPS 的伺服器是 AD DS 網域的成員時，NPS 會使用目錄服務做為其使用者帳戶資料庫，而且是單一登入解決方案的一部分。 網路存取控制會使用同一組認證 @no__t 0authenticating 和授權存取網路 @ no__t-1，以及登入 AD DS 網域。

> [!NOTE]
> NPS 會使用使用者帳戶的撥入內容和網路原則來授權連接。

網際網路服務提供者 \(ISPs @ no__t-1 和維護網路存取的組織，無論使用哪一種網路存取設備，都能從單一管理點管理所有網路存取類型的挑戰。 RADIUS 標準在同質和異類環境中都支援這項功能。 RADIUS 是一種用戶端伺服器通訊協定，可讓網路存取設備（做為 RADIUS 用戶端）提交驗證和帳戶處理要求給 RADIUS 伺服器。

RADIUS 伺服器具有使用者帳戶資訊的存取權，而且可以檢查網路存取驗證認證。 如果驗證使用者認證並授權連線嘗試，RADIUS 伺服器會根據指定的條件來授權使用者存取，然後在帳戶處理記錄中記錄網路存取連線。 使用 RADIUS 可讓網路存取使用者驗證、授權和帳戶處理資料在集中位置收集及維護，而不是在每部存取伺服器上。

#### <a name="using-nps-as-a-radius-server"></a>使用 NPS 做為 RADIUS 伺服器

在下列情況中，您可以使用 NPS 作為 RADIUS 伺服器：

- 您使用 AD DS 網域或本機 SAM 使用者帳戶資料庫做為存取用戶端的使用者帳戶資料庫。 
- 您在多部撥號伺服器、VPN 伺服器或指定撥號路由器上使用遠端存取，而您想要集中設定網路原則和連線記錄和帳戶處理。 
- 您會將撥號、VPN 或無線存取外包給服務提供者。 存取伺服器使用 RADIUS 來驗證和授權您組織成員所進行的連線。
- 您想要集中管理一組不同存取伺服器的驗證、授權和帳戶處理。

下圖顯示 NPS 作為各種存取用戶端的 RADIUS 伺服器。 

![NPS 做為 RADIUS 伺服器](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>RADIUS Proxy

做為 RADIUS proxy，NPS 會將驗證和帳戶處理訊息轉送到 NPS 和其他 RADIUS 伺服器。 您可以使用 NPS 做為 RADIUS proxy，在 RADIUS 用戶端之間提供 RADIUS 訊息的路由 @no__t 0also 稱為網路存取伺服器 @ no__t-1 和 RADIUS 伺服器，以執行連線嘗試的使用者驗證、授權和帳戶處理。 

做為 RADIUS proxy 使用時，NPS 是 RADIUS 存取和帳戶處理訊息流向的中央切換或路由點。 NPS 會在帳戶處理記錄中記錄轉送訊息的資訊。

#### <a name="using-nps-as-a-radius-proxy"></a>使用 NPS 做為 RADIUS proxy

在下列情況中，您可以使用 NPS 做為 RADIUS proxy：

- 您是為多個客戶提供外包撥號、VPN 或無線網路存取服務的服務提供者。 您的 Nas 會將連接要求傳送到 NPS RADIUS proxy。 根據連線要求中的使用者名稱領域部分，NPS RADIUS proxy 會將連線要求轉送到客戶維護的 RADIUS 伺服器，並可驗證和授權連線嘗試。
- 您想要為使用者帳戶提供驗證和授權，而不是 NPS 所屬網域的成員，或是另一個與 NPS 所屬網域具有雙向信任關係的網域。 這包括不受信任網域中的帳戶、單向信任網域，以及其他樹系。 您可以設定讓存取伺服器將其連線要求傳送到 nps radius 伺服器，而不是將其連線要求傳送到 NPS radius 伺服器。 NPS RADIUS proxy 會使用使用者名稱的領域名稱部分，並將要求轉送至正確網域或樹系中的 NPS。 對一個網域或樹系中的使用者帳戶進行連線嘗試，可以針對另一個網域或樹系中的 Nas 進行驗證。
- 您想要使用非 Windows 帳戶資料庫的資料庫來執行驗證和授權。 在此情況下，符合指定領域名稱的連線要求會轉送到 RADIUS 伺服器，其可存取不同的使用者帳戶和授權資料資料庫。 其他使用者資料庫的範例包括 Novell 目錄服務（NDS）和結構化查詢語言 (SQL) （SQL）資料庫。
- 您想要處理大量的連接要求。 在此情況下，您可以將其設定為將連線和帳戶處理要求傳送到 NPS RADIUS proxy，而不是將 RADIUS 用戶端設為嘗試在多個 RADIUS 伺服器之間平衡其連線和帳戶處理要求。 NPS RADIUS proxy 會動態地平衡多部 RADIUS 伺服器之間的連線和帳戶管理要求負載，並增加每秒處理大量 RADIUS 用戶端和驗證的次數。
- 您想要為外包服務提供者提供 RADIUS 驗證和授權，並將內部網路防火牆設定降至最低。 內部網路防火牆介於您的周邊網路（內部網路與網際網路之間的網路）與內部網路之間。 藉由將 NPS 放在您的周邊網路上，您周邊網路與內部網路之間的防火牆必須允許 NPS 與多個網域控制站之間的流量流動。 藉由使用 NPS proxy 來取代 NPS，防火牆必須僅允許在 NPS proxy 與內部網路內的一或多個 Nps 之間流動的 RADIUS 流量。

下圖顯示 NPS 做為 RADIUS 用戶端與 RADIUS 伺服器之間的 RADIUS proxy。

![NPS 做為 RADIUS Proxy](../../media/Nps-Proxy/Nps-Proxy.jpg)

透過 NPS，組織也可以將遠端存取基礎結構外包給服務提供者，同時保有使用者驗證、授權和帳戶處理的控制權。

在下列案例中，可以建立 NPS 設定：

- 無線存取
- 組織撥號或虛擬私人網路（VPN）遠端存取
- 外包撥號或無線存取
- 網路與網際網路存取
- 商務夥伴的已驗證存取外部網路資源

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>RADIUS 伺服器和 RADIUS proxy 設定範例

下列設定範例示範如何將 NPS 設定為 RADIUS 伺服器和 RADIUS proxy。

**NPS 做為 RADIUS 伺服器**。 在此範例中，NPS 設定為 RADIUS 伺服器，預設連線要求原則是唯一設定的原則，而所有連線要求都是由本機 NPS 處理。 NPS 可以驗證和授權其帳戶位於 NPS 網域和受信任網域中的使用者。

**NPS 做為 RADIUS proxy**。 在此範例中，NPS 設定為 RADIUS proxy，將連線要求轉送至兩個不受信任網域中的遠端 RADIUS 伺服器群組。 系統會刪除預設連線要求原則，並建立兩個新的連線要求原則，以將要求轉送到這兩個不受信任網域中的每一個。 在此範例中，NPS 不會處理本機伺服器上的任何連接要求。 

**NPS 同時做為 radius 伺服器和 radius proxy**。 除了預設的連線要求原則，這會指定在本機處理連線要求，會建立新的連線要求原則，將連線要求轉送到不受信任網域中的 NPS 或其他 RADIUS 伺服器。 第二個原則的名稱是 Proxy 原則。 在此範例中，Proxy 原則會先出現在原則的已排序清單中。 如果連線要求符合 Proxy 原則，則會將連線要求轉送到遠端 RADIUS 伺服器群組中的 RADIUS 伺服器。 如果連線要求不符合 Proxy 原則，但符合預設的連線要求原則，NPS 會在本機伺服器上處理連接要求。 如果連線要求不符合任一個原則，則會將它捨棄。

**NPS 作為具有遠端帳戶處理伺服器的 RADIUS 伺服器**。 在此範例中，本機 NPS 未設定為執行帳戶處理，而且會修訂預設連線要求原則，以便將 RADIUS 帳戶處理訊息轉送到遠端 RADIUS 伺服器群組中的 NPS 或其他 RADIUS 伺服器。 雖然會轉送帳戶處理訊息，但不會轉送驗證和授權訊息，而且本機 NPS 會針對本機網域和所有信任的網域執行這些功能。

**具有遠端 RADIUS 到 Windows 使用者對應的 NPS**。 在此範例中，NPS 會將驗證要求轉送到遠端 RADIUS 伺服器，並使用本機 Windows 使用者帳戶進行授權，以作為每個個別連線要求的 radius 伺服器和 RADIUS proxy。 此設定的執行方式是將遠端 RADIUS 設定為 Windows 使用者對應屬性，做為連線要求原則的條件。 @no__t 0In 加入時，必須在與遠端使用者帳戶相同名稱的 RADIUS 伺服器本機上建立使用者帳戶，以供遠端 RADIUS 伺服器執行驗證。 \)

## <a name="configuration"></a>組態

若要將 NPS 設定為 RADIUS 伺服器，您可以在 NPS 主控台或伺服器管理員中，使用 [標準設定] 或 [advanced configuration]。 若要將 NPS 設定為 RADIUS proxy，您必須使用 [advanced configuration]。

### <a name="standard-configuration"></a>標準設定

使用標準設定時，會提供可協助您在下列案例中設定 NPS 的嚮導：

- 撥號或 VPN 連線的 RADIUS 伺服器
- 802.1X 無線或有線連線的 RADIUS 伺服器

若要使用 wizard 設定 NPS，請開啟 NPS 主控台，並選取上述其中一個案例，然後按一下開啟嚮導的連結。

### <a name="advanced-configuration"></a>Advanced configuration

當您使用 [advanced configuration] 時，您會以手動方式將 NPS 設定為 RADIUS 伺服器或 RADIUS proxy。 

若要使用 [advanced configuration] 設定 NPS，請開啟 NPS 主控台，然後按一下 [ **Advanced configuration** ] 旁的箭號以展開此區段。

提供下列的先進設定專案。

#### <a name="configure-radius-server"></a>設定 RADIUS 伺服器

若要將 NPS 設定為 RADIUS 伺服器，您必須設定 RADIUS 用戶端、網路原則和 RADIUS 帳戶處理。

如需有關進行這些設定的指示，請參閱下列主題。

- [設定 RADIUS 用戶端](nps-radius-clients-configure.md)
- [設定網路原則](nps-np-configure.md)
- [設定網路原則伺服器帳戶處理](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>設定 RADIUS proxy

若要將 NPS 設定為 RADIUS proxy，您必須設定 RADIUS 用戶端、遠端 RADIUS 伺服器群組和連線要求原則。

如需有關進行這些設定的指示，請參閱下列主題。

- [設定 RADIUS 用戶端](nps-radius-clients-configure.md)
- [設定遠端 RADIUS 伺服器群組](nps-crp-rrsg-configure.md)
- [設定連線要求原則](nps-crp-configure.md)

## <a name="nps-logging"></a>NPS 記錄

NPS 記錄也稱為 RADIUS 帳戶處理。 設定 NPS 記錄以符合您的需求，不論 NPS 是用來做為 RADIUS 伺服器、proxy 或這些設定的任何組合。

若要設定 NPS 記錄，您必須設定您要記錄並使用事件檢視器查看哪些事件，然後決定您想要記錄哪些其他資訊。 此外，您必須決定是否要將使用者驗證和帳戶處理資訊記錄到儲存在本機電腦或遠端電腦上 SQL Server 資料庫的文字記錄檔。

如需詳細資訊，請參閱[設定網路原則伺服器帳戶](nps-accounting-configure.md)處理。
