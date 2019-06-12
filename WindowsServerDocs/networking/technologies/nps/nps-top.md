---
title: 網路原則伺服器 (NPS)
description: 本主題提供在 Windows Server 2016 和 Windows Server 2019，網路原則伺服器的概觀，並包含 NPS 的其他指導連結。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.date: 06/20/2018
ms.openlocfilehash: 515012a21ba6e90abe2c4db2150fd1feaad06677
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812364"
---
# <a name="network-policy-server-nps"></a>網路原則伺服器 (NPS)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2019

如需在 Windows Server 2016 和 Windows Server 2019 的網路原則伺服器的概觀，您可以使用本主題。 當您在 Windows Server 2016 和 Server 2019 安裝網路原則與存取服務 (NPAS) 功能時，會安裝 NPS。

> [!NOTE]
> 本主題中，除了下列 NPS 文件使用。
> - [網路原則伺服器的最佳作法](nps-best-practices.md)
> - [開始使用網路原則伺服器](nps-getstart-top.md)
> - [規劃網路原則伺服器](nps-plan-top.md)
> - [部署網路原則伺服器](nps-deploy.md)
> - [管理網路原則伺服器](nps-manage-top.md)
> - [網路原則伺服器 (NPS) Cmdlet，在 Windows PowerShell 中的](https://technet.microsoft.com/library/jj872739.aspx)適用於 Windows Server 2016 和 Windows 10
> - [網路原則伺服器 (NPS) Cmdlet，在 Windows PowerShell 中的](https://technet.microsoft.com/library/jj872739.aspx)for Windows Server 2012 R2 和 Windows 8.1
> - [Windows PowerShell 中的 NPS Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)適用於 Windows Server 2012 和 Windows 8

網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

您也可以設定 NPS 做為遠端驗證撥號使用者服務 (RADIUS) proxy，將連線要求轉送到遠端 NPS 或是其他 RADIUS 伺服器，以便您可以連接的要求負載平衡，並將它們轉送至正確的網域進行驗證和授權。

NPS 可讓您集中設定和管理網路存取驗證、 授權和帳戶處理下列功能：

- **RADIUS 伺服器**。 NPS 會執行集中化的驗證、 授權和帳戶處理為無線、 驗證交換器、 遠端存取撥號和虛擬私人網路 (VPN) 連線。 當您使用 NPS 做為 RADIUS 伺服器時，可以設定網路存取伺服器 (例如無線存取點與 VPN 伺服器) 做為 NPS 中的 RADIUS 用戶端。 您也可以設定 NPS 用來授權連線要求的網路原則，並且可以設定 RADIUS 帳戶處理，讓 NPS 將計量資訊記錄到本機硬碟上或 Microsoft SQL Server 資料庫中的記錄檔。 如需詳細資訊，請參閱 < [RADIUS 伺服器](#radius-server)。
- **RADIUS proxy**。 當您使用 NPS 做為 RADIUS proxy 時，您會設定連線要求原則，告訴哪些連線要求轉送到其他 RADIUS 伺服器，以及哪些 RADIUS 伺服器，您想要將連線要求轉送到 NPS。 您也可以設定 NPS 轉送要由遠端 RADIUS 伺服器群組中一或多部電腦記錄的計量資料。 若要設定 NPS 做為 RADIUS proxy 伺服器，請參閱下列主題。 如需詳細資訊，請參閱 < [RADIUS proxy](#radius-proxy)。
    - [設定連線要求原則](nps-crp-configure.md)
- **RADIUS 帳戶處理**。 您可以設定 NPS 事件記錄到本機記錄檔或 Microsoft SQL Server 的本機或遠端執行個體。 如需詳細資訊，請參閱 < [NPS 記錄](#nps-logging)。

> [!IMPORTANT]
> 網路存取保護\(NAP\)，健康情況登錄授權單位\(HRA\)，以及主機認證授權通訊協定\(HCAP\) Windows Server 2012 R2 中已被取代和 Windows Server 2016 中無法使用。 如果您有稍早於 Windows Server 2016 使用作業系統的 NAP 部署，您無法移轉至 Windows Server 2016 的 NAP 部署。

您可以使用這些功能的任何組合來設定 NPS。 例如，您可以設定一個 NPS 作為 RADIUS 伺服器進行 VPN 連線，也為 RADIUS proxy 來轉送一些連線要求進行驗證和授權，另一個網域中的遠端 RADIUS 伺服器群組的成員。

## <a name="windows-server-editions-and-nps"></a>Windows Server 版本與 NPS

NPS 提供不同的功能，視您安裝的 Windows Server 版本而定。

### <a name="windows-server-2016-or-windows-server-2019-standarddatacenter-edition"></a>Windows Server 2016 或 Windows Server 2019 Standard/Datacenter Edition

在 Windows Server 2016 Standard 或 Datacenter 中的 nps，您可以設定無限的數量的 RADIUS 用戶端和遠端 RADIUS 伺服器群組。 此外，您可以藉由指定一個 IP 位址範圍來設定 RADIUS 用戶端。

> [!NOTE]
> 無法使用 Server Core 安裝選項安裝的系統上使用 WIndows 網路原則與存取服務的功能。

下列各節提供有關 NPS 為 RADIUS 伺服器和 proxy 的詳細的資訊。

## <a name="radius-server-and-proxy"></a>RADIUS 伺服器和 proxy

您可以使用 NPS 做為 RADIUS 伺服器、 RADIUS proxy，或兩者。

### <a name="radius-server"></a>RADIUS 伺服器

NPS 是 Microsoft 實作的 RADIUS 標準指定網際網路工程任務推動小組所\(IETF\) Rfc 2865 與 2866年中。 為 RADIUS 伺服器，NPS 會執行集中化的連線驗證、 授權和帳戶處理的許多類型的網路存取權，包括無線、 驗證交換器、 撥號和虛擬私人網路\(VPN\)遠端存取，以及路由器對路由器連線。

> [!NOTE]
> 如需部署 NPS 做為 RADIUS 伺服器的資訊，請參閱[部署的網路原則伺服器](nps-deploy.md)。

NPS 可讓您使用異質組的無線、 交換器、 遠端存取或 VPN 設備。 您可以使用 NPS 與遠端存取服務，也就是 Windows Server 2016 中，您可以使用。

NPS 使用 Active Directory 網域服務\(AD DS\)網域或本機安全性帳戶管理員 (SAM) 使用者帳戶資料庫來驗證使用者認證，連接嘗試。 AD DS 網域的成員執行 NPS 的伺服器時，NPS 目錄服務做為其使用者帳戶資料庫，而且是單一登入解決方案的一部分。 同一組認證用於網路存取控制\(驗證和授權存取網路\)並登入 AD DS 網域。

> [!NOTE]
> NPS 使用的撥入內容的使用者帳戶和網路原則來授權連線。

網際網路服務提供者\(Isp\)和維護網路存取權的組織有增加的挑戰，可以從單一的管理點，無論何種類型的網路存取權管理所有類型的網路存取使用的設備。 RADIUS 標準在同質和異質環境中支援這項功能。 RADIUS 是用戶端-伺服器通訊協定，可讓送出驗證和帳戶處理要求至 RADIUS 伺服器的網路存取設備 （當作 RADIUS 用戶端）。

RADIUS 伺服器可以存取使用者帳戶資訊，而且可以檢查網路存取驗證認證。 如果會驗證使用者認證，而且在授權連線嘗試，RADIUS 伺服器會授權使用者存取，根據指定的條件，並再記錄帳戶處理記錄檔中的 網路存取連線。 使用 RADIUS 可讓網路存取的使用者驗證、 授權和計量資料收集與保留在中央位置，而不是在每個存取伺服器上。

#### <a name="using-nps-as-a-radius-server"></a>使用 NPS 做為 RADIUS 伺服器

您可以使用 NPS 做為 RADIUS 伺服器時：

- 您使用 AD DS 網域或本機 SAM 使用者帳戶的資料庫做為您的使用者帳戶資料庫存取用戶端。 
- 您會使用 「 遠端存取多個撥號伺服器、 VPN 伺服器上，或指定撥號路由器，而您想要集中設定網路原則，並連接記錄和計量。 
- 外包您撥號、 VPN 或無線存取的服務提供者。 存取伺服器會使用 RADIUS 來驗證和授權您組織的成員所建立的連線。
- 您想要集中管理驗證、 授權和帳戶處理的一組異質存取伺服器。

下圖會顯示各種不同的存取用戶端做為 RADIUS 伺服器的 NPS。 

![NPS 做為 RADIUS 伺服器](../../media/Nps-Server/Nps-Server.jpg)

### <a name="radius-proxy"></a>RADIUS Proxy

為 RADIUS proxy，NPS 會轉送驗證和帳戶處理訊息到 NPS 與其他 RADIUS 伺服器。 因為 RADIUS 用戶端之間的 RADIUS proxy，以提供路由的 RADIUS 訊息，您可以使用 NPS\(也稱為網路存取伺服器\)和執行使用者驗證、 授權和帳戶處理的 RADIUS 伺服器嘗試連線。 

使用做為 RADIUS proxy，NPS 會是中央交換或路由點 RADIUS 存取和帳戶管理訊息流程。 NPS 記錄帳戶處理記錄檔中的相關資訊會轉送的訊息。

#### <a name="using-nps-as-a-radius-proxy"></a>使用 NPS 做為 RADIUS proxy

您可以使用 NPS 做為 RADIUS proxy 時：

- 您是提供委外的撥號、 VPN 或無線網路存取服務給多個客戶的服務提供者。 您的 Nas 會將連接要求傳送到 NPS RADIUS proxy。 根據連線要求中的使用者名稱的領域部分，NPS RADIUS proxy 會轉送連線要求給 RADIUS 伺服器是由客戶維護和可驗證及授權連線嘗試。
- 您想要提供驗證和授權的使用者帳戶不是 NPS 所屬的網域或具有 NPS 所屬的網域具有雙向信任的另一個網域的成員。 這包括不受信任的網域、 單向受信任的網域和其他樹系中的帳戶。 而不需要設定您的存取伺服器，以將其連接要求傳送到 NPS RADIUS 伺服器，您可以將其設定為將其連接要求傳送到 NPS RADIUS proxy。 NPS RADIUS proxy 使用的使用者名稱的領域名稱部分，並將要求轉送至正確的網域或樹系中的 NPS。 一個網域或樹系中的帳戶可以驗證另一個網域或樹系中的 Nas 的使用者嘗試連線。
- 您想要使用的資料庫，不是 Windows 帳戶資料庫來執行驗證與授權。 在此情況下，符合指定的領域名稱的連接要求會轉送給 RADIUS 伺服器，其具有不同的資料庫的使用者帳戶和授權資料的存取權。 其他使用者資料庫的範例包括 Novell 目錄服務 (NDS) 和結構化查詢語言 (SQL) 資料庫。
- 您想要處理大量的連接要求。 在此情況下，您不需要設定您的 RADIUS 用戶端嘗試在多部 RADIUS 伺服器之間平衡它們的連線與帳戶處理要求，您可以設定它們傳送到 NPS RADIUS proxy 的連線與帳戶處理要求。 NPS RADIUS proxy 會動態地平衡連線的負載和帳戶處理要求到多部 RADIUS 伺服器，並會增加處理的大量 RADIUS 用戶端和每秒的驗證。
- 您想要提供 RADIUS 驗證與授權給委外的服務提供者和內部網路防火牆設定降到最低。 內部網路防火牆是您的周邊網路 （您的內部網路與網際網路之間的網路） 與內部網路之間。 藉由在周邊網路上放置 NPS，您的周邊網路與內部網路之間的防火牆必須允許 NPS 與多個網域控制站之間的流量。 如果以 NPS proxy 取代 NPS，防火牆必須允許只有 RADIUS NPS proxy 與內部網路中的一或多個 NPSs 之間流動的流量。

下圖顯示 NPS 做為 RADIUS 用戶端與 RADIUS 伺服器之間的 RADIUS proxy。

![NPS 做為 RADIUS Proxy](../../media/Nps-Proxy/Nps-Proxy.jpg)

利用 NPS，組織也外包給服務提供者的遠端存取基礎結構同時保有使用者驗證、 授權和帳戶處理的控制。

在下列情況下，可以建立 NPS 設定：

- 無線存取
- 組織撥號或虛擬私人網路 (VPN) 遠端存取
- 委外的撥號或無線存取
- 網路與網際網路存取
- 已驗證的存取協力廠商的外部網路資源

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>RADIUS 伺服器與 RADIUS proxy 設定範例

下列組態範例示範如何設定 NPS 做為 RADIUS 伺服器與 RADIUS proxy。

**NPS 做為 RADIUS 伺服器**。 在此範例中，NPS 被設定為 RADIUS 伺服器、 預設連線要求原則是唯一的設定的原則，，然後由本機 NPS 處理所有連線要求。 NPS 可以驗證和授權其帳戶是網域中的 NPS 和受信任網域中的使用者。

**NPS 做為 RADIUS proxy**。 在此範例中，NPS 被設定為 RADIUS proxy，將連線要求轉送到兩個不受信任網域中的遠端 RADIUS 伺服器群組。 刪除預設連線要求原則，並將要求轉送至每個兩個不受信任的網域建立兩個新的連線要求原則。 在此範例中，NPS 不會處理在本機伺服器上的任何連線要求。 

**NPS 做為 RADIUS 伺服器與 RADIUS proxy**。 除了預設連線要求原則，指定在本機處理連線要求，被建立新的連線要求原則，將連線要求轉送到 NPS 或其他 RADIUS 伺服器不信任的網域中。 此第二個原則稱為 Proxy 原則。 在此範例中，Proxy 原則會顯示原則的已排序清單中的第一個。 如果連線要求符合 Proxy 原則，連接要求被轉送到遠端 RADIUS 伺服器群組中的 RADIUS 伺服器。 如果連線要求不符合 Proxy 原則，但符合預設連線要求原則，NPS 會處理連線要求，在本機伺服器上。 如果連線要求不符合任一原則，它會將它捨棄。

**NPS 作為 RADIUS 伺服器的遠端帳戶處理伺服器**。 在此範例中，本機 NPS 未執行帳戶處理設定，預設連線要求原則會修訂案例使 RADIUS 帳戶處理訊息轉送到 NPS 或其他 RADIUS 伺服器的遠端 RADIUS 伺服器群組中。 雖然會轉送帳戶處理訊息，但不會轉送驗證和授權的訊息，本機 NPS 的本機網域中執行這些函式和所有信任的網域。

**NPS 以搭配 Windows 使用者對應的遠端 RADIUS**。 在此範例中，NPS 會扮演做為 RADIUS 伺服器，並為每個個別的連線要求的 RADIUS proxy 藉由將驗證要求轉送到遠端 RADIUS 伺服器時使用本機 Windows 使用者帳戶進行授權。 此設定被藉由設定 Windows 使用者對應屬性的遠端 RADIUS 連線要求原則的條件。 \(此外，必須在本機建立的使用者帳戶上的 RADIUS 伺服器的遠端 RADIUS 伺服器對其執行驗證的遠端使用者帳戶名稱相同。\)

## <a name="configuration"></a>組態

若要設定 NPS 做為 RADIUS 伺服器，您可以使用標準的設定] 或 [進階的設定在 NPS 主控台中或在 [伺服器管理員] 中。 若要設定 NPS 做為 RADIUS proxy，您必須使用進階的組態。

### <a name="standard-configuration"></a>標準組態

使用標準設定時，會提供精靈協助您設定 NPS 在下列案例：

- 撥號或 VPN 連線的 RADIUS 伺服器
- 802.1X 無線或有線連線的 RADIUS 伺服器

若要設定 NPS 使用精靈，請開啟 NPS 主控台，選取其中一個先前案例中，，然後按一下連結來開啟精靈。

### <a name="advanced-configuration"></a>進階的組態

當您使用進階的組態時，您以手動方式設定 NPS 做為 RADIUS 伺服器或 RADIUS proxy。 

要設定 NPS 使用進階的組態，請開啟 NPS 主控台中，，然後按一下箭號旁**進階組態**以展開此區段。

提供下列進階的組態項目。

#### <a name="configure-radius-server"></a>設定 RADIUS 伺服器

若要設定 NPS 做為 RADIUS 伺服器，您必須設定 RADIUS 用戶端、 網路原則以及 RADIUS 帳戶處理。

如需進行這些設定的指示，請參閱下列主題。

- [設定 RADIUS 用戶端](nps-radius-clients-configure.md)
- [設定網路原則](nps-np-configure.md)
- [設定網路原則伺服器計量](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>設定 RADIUS proxy

若要設定 NPS 做為 RADIUS proxy，您必須設定 RADIUS 用戶端、 遠端 RADIUS 伺服器群組和連線要求原則。

如需進行這些設定的指示，請參閱下列主題。

- [設定 RADIUS 用戶端](nps-radius-clients-configure.md)
- [設定遠端 RADIUS 伺服器群組](nps-crp-rrsg-configure.md)
- [設定連線要求原則](nps-crp-configure.md)

## <a name="nps-logging"></a>NPS 記錄

NPS 記錄也稱為 RADIUS 帳戶處理。 設定 NPS 記錄您的需求，是否使用 NPS 做為 RADIUS 伺服器、 proxy 或任何組合，這些設定。

若要設定 NPS 記錄，您必須設定您想要記錄哪些事件，您想要記錄和使用事件檢視器 檢視，，然後判斷哪些其他資訊。 此外，您必須決定是否要記錄使用者驗證與帳戶處理資訊儲存在本機電腦上的文字記錄檔或在本機電腦或遠端電腦上的 SQL Server 資料庫。

如需詳細資訊，請參閱 <<c0> [ 設定網路原則伺服器 Accounting](nps-accounting-configure.md)。
