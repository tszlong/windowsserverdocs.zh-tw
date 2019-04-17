---
title: 網路原則伺服器 (NPS)
description: 本主題提供概觀在 Windows Server 2016 的網路原則伺服器，並且包含 NPS 設計的額外指導連結。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9c7a67e0-0953-479c-8736-ccb356230bde
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f48cbdaec82e9e60f734a4a8949082187b13166
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-nps"></a>網路原則伺服器 (NPS)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題適用於 Windows Server 2016 中的網路原則伺服器的概觀。

>[!NOTE]
>本主題中，除了下列 NPS 文件會提供。
> - [網路原則伺服器最佳做法](nps-best-practices.md)
> - [開始使用的網路原則伺服器](nps-getstart-top.md)
> - [規劃網路原則伺服器](nps-plan-top.md)
> - [部署原則的網路伺服器](nps-deploy.md)
> - [管理網路原則伺服器](nps-manage-top.md)
> - [網路原則 (NPS) 伺服器 Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)適用於 Windows Server 2016 和 Windows 10
> - [網路 Windows PowerShell 中的原則 (NPS) 伺服器 Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)適用於 Windows Server 2012 R2 和 Windows 8.1
> - [Windows PowerShell 中的 NPS Cmdlet](https://technet.microsoft.com/library/jj872739.aspx)適用於 Windows Server 2012 和 Windows 8

網路原則 Server (NPS) 可讓您建立並執行適用於連接要求驗證與授權全組織網路存取原則。

您也可以設定 NPS 遠端驗證 Dial 使用者服務 (RADIUS) proxy 要求轉送給連接遠端 NPS 或其他 RADIUS 伺服器，讓您可以負載平衡連接要求和轉寄驗證與授權正確的網域。

NPS 可讓您集中設定及管理網路存取驗證、授權及下列功能計量︰

- **RADIUS 伺服器**。 NPS 執行集中式的驗證、授權及計量無線、驗證切換、遠端存取撥號以及連接私人網路 virtual (VPN)。 當您使用 NPS RADIUS 伺服器時，您可以為中 NPS RADIUS 戶端設定網路存取伺服器，例如 wireless 存取點和 VPN 伺服器。 您也需要 NPS 授權連接要求，使用的網路原則設定，以便 NPS 登計量資訊來登入本機硬碟或的 Microsoft SQL Server 資料庫中的檔案，您可以設定 RADIUS 計量。 如需詳細資訊，請查看[RADIUS 伺服器](#bkmk_server)。
- **RADIUS proxy**。 當您使用 NPS RADIUS proxy 時，您可以設定告訴哪些連接要求送給其他 RADIUS 伺服器和您想要轉寄連接要求哪些 RADIUS 伺服器 NPS 伺服器連接要求原則。 您也可以設定 NPS 向前計量資料來登入遠端 RADIUS 伺服器群組中的一或多個電腦。 若要設定 NPS RADIUS proxy 伺服器，查看下列主題。 如需詳細資訊，請查看[RADIUS proxy](#bkmk_proxy)。
    - [設定連接要求原則](nps-crp-configure.md)
- **RADIUS 計量**。 您可以設定 NPS 登事件本機登入檔案，或是的 Microsoft SQL Server 本機或遠端執行個體。 如需詳細資訊，請查看[NPS 登入](#bkmk_logging)。

>[!IMPORTANT]
>網路存取保護 \(NAP\)、健康登記授權單位 \(HRA\)，主機 Credential 授權通訊協定 \(HCAP\) 而被取代在 Windows Server 2012 R2，並不是在 Windows Server 2016 中可使用。 如果您使用 Windows Server 2016 以前作業系統 NAP 部署，您將無法 NAP 部署移轉到 Windows Server 2016。

您可以使用這些功能的任何組合來設定 NPS。 例如，您可以設定一個 NPS 伺服器 VPN 連接 RADIUS 伺服器以及也 RADIUS proxy 轉送一些連接要求驗證及其他網域中的授權遠端 RADIUS 伺服器群組成員。

## <a name="windows-server-editions-and-nps"></a>Windows Server 版本和 NPS

NPS 提供根據您所安裝的 Windows Server 的版本不同的功能。

### <a name="windows-server-2016-datacenter-edition"></a>Windows Server 2016 Datacenter Edition

具有 NPS 在 Windows Server 2016 Datacenter，您可以設定無限 RADIUS 戶端和遠端 RADIUS 伺服器群組。 此外，您可以設定 RADIUS 戶端指定 IP 位址。

### <a name="windows-server-2016-standard-edition"></a>Windows Server 2016 Standard Edition

具有 NPS 在 Windows Server 2016 標準，您可以設定最多的 50 RADIUS 用並最多 2 遠端 RADIUS 伺服器群組。 您可以定義 RADIUS client 使用的完整的網域名稱或 IP 位址，但您不能指定 IP 位址範圍定義的 RADIUS 用群組。 如果 RADIUS client 的完整的網域名稱解析為多個 IP 位址，NPS 伺服器使用傳回查詢網域名稱系統」(DNS) 中的第一個 IP 位址。

下列章節提供 NPS RADIUS 伺服器以及 proxy 更多詳細的資訊。

## <a name="radius-server-and-proxy"></a>RADIUS 伺服器，並 proxy

您可以使用 NPS RADIUS 伺服器、RADIUS proxy，或兩者。

### <a name="bkmk_server"></a>RADIUS 伺服器

NPS 是 Microsoft 的標準 RADIUS 指定 Rfc 2865 和 2866 年網際網路工程設計工作人員 \(IETF\)。 為 RADIUS 伺服器、NPS 執行集中的連接驗證、授權及計量許多類型的網路存取權，包括無線，驗證切換，撥號和 \(VPN\) 遠端存取 virtual 私人網路，以及連接至路由器。

>[!NOTE]
>如部署 NPS RADIUS 伺服器的資訊，請查看[部署的網路原則伺服器](nps-deploy.md)。

NPS 可讓您使用的無線異質性設定、切換、遠端存取或 VPN 設備。 您可以使用遠端存取服務，可在 Windows Server 2016 NPS。

NPS 會使用 Active Directory Domain Services \(AD DS\) 網域，或嘗試本機安全性帳號 Manager（坡）使用者帳號資料庫驗證使用者認證連接。 執行 NPS 伺服器時 AD DS 網域的成員，NPS 為其使用者 account 資料庫中使用 directory 服務，而且部分單一登入方案。 相同的認證組使用的網路存取控制 \（驗證與授權的存取權 network\）並登入 AD DS 網域。

>[!NOTE]
>NPS 授權連接使用使用者 account 和網路原則撥號的屬性。

網際網路服務提供者 \(ISPs\) 和維護網路存取權的組織有增加的挑戰從單一位置管理，不論使用的網路存取設備類型的管理所有類型的網路存取權。 RADIUS 標準同和異質性環境中支援這項功能。 RADIUS 是讓網路存取設備（作為 RADIUS 戶端）來提交驗證及計量要求 RADIUS 伺服器 client 伺服器通訊協定。

RADIUS 伺服器存取使用者 account 資訊，以及可以查看的網路存取驗證憑證。 如果驗證使用者認證，授權連接嘗試 RADIUS 伺服器授權的使用者存取根據指定的條件，並再登入計量登入的 [網路存取權。 使用 RADIUS 允許網路存取權的使用者驗證、授權及計量資料收集及維護中央位置，而不是每個存取伺服器上。

#### <a name="using-nps-as-a-radius-server"></a>使用 NPS RADIUS 伺服器

您可以使用 NPS RADIUS 伺服器為時：

- 您用於 AD DS 網域或本機坡使用者帳號資料庫與您的使用者 account 資料庫存取戶端。 
- 您使用遠端存取伺服器撥號多個、VPN 伺服器上或撥號路由器，您想要的網路原則和連接登入及計量設定的集中管理。 
- 您會將您為撥號、VPN、或 wireless 存取服務提供者。 存取伺服器驗證，以及授權成員您的組織所進行的連接到使用 RADIUS。
- 您想要驗證、授權及計量一組異質性存取伺服器的集中管理。

下圖 NPS RADIUS 伺服器的各種不同的存取用。 

![NPS RADIUS 伺服器](../../media/Nps-Server/Nps-Server.jpg)

### <a name="bkmk_proxy"></a>RADIUS proxy

為 RADIUS proxy、NPS 轉送給驗證和計量訊息 NPS 及其他 RADIUS 伺服器。 您可以使用 NPS RADIUS proxy 提供的 RADIUS 路由郵件之間 RADIUS 戶端 \（也稱為網路存取 servers\）和連接嘗試執行驗證使用者、授權及計量 RADIUS 伺服器。 

作為 RADIUS proxy，NPS 是中央切換或透過哪些 RADIUS 存取及計量訊息流程路由點。 NPS 此資訊在計量登入轉寄簡訊。

#### <a name="using-nps-as-a-radius-proxy"></a>使用 NPS RADIUS proxy

您可以使用 NPS RADIUS proxy 為時：

- 您有多個針對外部的撥號、VPN 或 wireless 網路存取服務提供者的服務提供者。 您 Nas 會傳送給 NPS RADIUS proxy 連接要求。 根據領域部分連接要求的使用者名稱、NPS RADIUS proxy 連接要求維護客戶 RADIUS 伺服器轉送給可以驗證及授權連接嘗試。
- 您想要提供驗證和授權，但不是 NPS 伺服器的網域或其他雙向具有 NPS 伺服器的網域信任的網域成員帳號。 這包括帳號受信任的網域，其他的樹系單向受信任的網域。 而不設定 NPS RADIUS 伺服器來傳送連接要求存取伺服器，您可以將它們傳送 NPS RADIUS proxy 連接要求設定。 NPS RADIUS proxy 使用領域名稱部分的使用者名稱，然後轉寄 NPS 伺服器正確網域或森林中的邀請。 連接嘗試另一個網域或森林中的 Nas 驗證帳號一個網域或森林中的使用者。
- 您想要使用的不是 Windows account 資料庫資料庫驗證且授權執行。 此時，請連接要求符合領域指定的名稱被轉送 RADIUS 伺服器、到不同的資料庫帳號及授權資料的存取。 其他使用者資料庫的範例包括 Novell Directory NDS 和結構化查詢的語言 (SQL) 資料庫。
- 您想要處理大量連接要求。 在這種情形下，而不是用來嘗試在多部 RADIUS 伺服器平衡他們連接及計量要求 RADIUS 戶端設定時，您可以設定他們連接及計量要求傳送給 NPS RADIUS proxy 它們。 NPS RADIUS proxy 動態餘額連接載入及計量要求跨多個 RADIUS 伺服器增加的 RADIUS 用大量處理和每秒鐘。
- 您想要提供 RADIUS 驗證和外部的服務提供者的授權最小化內部防火牆設定。 內部防火牆是之間周邊網路（您的企業網路與網際網路之間網路）與內部網路。 NPS 伺服器置於周邊網路、周邊網路之間內部防火牆必須允許多網域控制站伺服器 NPS 之間的流量。 具有 NPS proxy 取代 NPS 伺服器，防火牆必須允許只有在您的企業網路的一或多個 NPS 伺服器 NPS proxy 之間傳送 RADIUS 流量。

下圖顯示 NPS RADIUS proxy RADIUS 戶端與 RADIUS 伺服器之間為。

![NPS RADIUS Proxy 為](../../media/Nps-Proxy/Nps-Proxy.jpg)


具有 NPS、組織也外包遠端存取的基礎結構服務提供者同時保留驗證使用者、授權及計量的控制。

您可以建立 NPS 設定如下：

- Wireless 存取
- 遠端存取的組織撥號或 virtual 私人網路 (VPN)
- 外部的撥號或 wireless 存取
- 網際網路存取權
- 協力廠商的已驗證的存取外部網路資源

## <a name="radius-server-and-radius-proxy-configuration-examples"></a>RADIUS 伺服器，並 RADIUS proxy 設定範例

下列設定範例示範如何設定 NPS RADIUS 伺服器以及 RADIUS proxy。

**NPS RADIUS 伺服器為**。 在此範例中，設定 NPS RADIUS 伺服器為、預設連接要求原則是唯一設定的原則，然後所有連接要求都處理 NPS 本機伺服器。 NPS 伺服器可以驗證和授權其帳號的網域 NPS 伺服器與受信任的網域中的使用者。

**NPS RADIUS proxy 為**。 在此範例中，NPS 伺服器為 RADIUS proxy 連接要求送給遠端 RADIUS 伺服器群組兩個信任的網域中的設定。 預設連接要求刪除原則，並要求轉送到每個兩個受信任的網域建立兩個新連接要求原則。 在此範例中，NPS 不處理本機伺服器上的任何連接要求。 

**NPS RADIUS 伺服器，並 RADIUS proxy 為**。 除了預設連接要求原則，將指定在本機處理連接要求時，會建立新的連接要求原則，轉送給連接要求 NPS 或受信任的網域中的其他 RADIUS 伺服器。 此第二個原則被名 Proxy 原則。 在此範例中，Proxy 原則會出現的原則排序清單中的第一次。 如果連接要求符合 Proxy 原則，連接要求轉送 RADIUS 伺服器遠端 RADIUS 伺服器群組中。 如果連接要求不符 Proxy 原則，但符合預設連接要求原則、NPS 處理連接要求本機伺服器上。 如果連接要求不符任一原則，會捨棄它。

**NPS RADIUS 伺服器遠端計量伺服器為**。 在此範例中，執行計量未設定 NPS 本機伺服器，預設連接要求原則修改，因此 RADIUS 計量訊息的轉送 NPS 伺服器或其他 RADIUS 伺服器遠端 RADIUS 伺服器群組中。 轉送計量簡訊，但不轉送驗證和授權訊息，本機伺服器 NPS 本機網域執行這些功能及所有受信任的網域。

**NPS 的 Windows 使用者對應遠端 RADIUS**。 在此範例中，NPS 的作用為 RADIUS 伺服器，並為每個個人連接要求 RADIUS proxy 轉送遠端 RADIUS 伺服器的驗證要求時使用本機的 Windows 使用者 account 授權。 此設定是由做為條件連接要求原則設定的 Windows 使用者對應屬性遠端 RADIUS 實作。 \ (另外，帳號必須建立本機具有相同名稱遠端 RADIUS 伺服器針對執行驗證遠端使用者 account RADIUS 伺服器上。\)

## <a name="configuration"></a>設定

若要設定 NPS RADIUS 伺服器，您可以使用標準設定或 [進階的設定 NPS 主控台中，或在伺服器管理員中。 若要設定 NPS RADIUS proxy 為，您必須使用進階的設定。

### <a name="standard-configuration"></a>一般設定

使用標準設定時，會提供精靈可協助您針對下列案例設定 NPS:

- 撥號或 VPN 連接 RADIUS 伺服器
- 適用於 802.1 X wireless 或有線連接 RADIUS 伺服器

若要設定使用精靈 NPS、開放 NPS 主機、選取前一個案例，其中一個，然後按一下開啟精靈的連結。

### <a name="advanced-configuration"></a>[進階的設定

當您使用 [進階的設定時，您手動設定 NPS RADIUS 伺服器或 RADIUS proxy。 

若要設定 NPS 使用 [進階的設定，開放 NPS 主控台中，，然後按一下旁邊的箭頭**進階設定**以展開區段此。

提供下列進階的設定項目。

#### <a name="configure-radius-server"></a>設定 RADIUS 伺服器

若要設定 NPS RADIUS 伺服器，您必須設定 RADIUS 戶端、網路原則和 RADIUS 計量。

指示進行這些設定，請查看下列主題。

- [設定 RADIUS 戶端](nps-radius-clients-configure.md)
- [設定原則的網路](nps-np-configure.md)
- [設定的網路原則伺服器計量](nps-accounting-configure.md)

#### <a name="configure-radius-proxy"></a>設定 RADIUS proxy

若要設定 NPS RADIUS proxy 為，您必須設定 RADIUS 戶端、遠端 RADIUS 伺服器群組和連接要求原則。

指示進行這些設定，請查看下列主題。

- [設定 RADIUS 戶端](nps-radius-clients-configure.md)
- [設定遠端 RADIUS 伺服器群組](nps-crp-rrsg-configure.md)
- [設定連接要求原則](nps-crp-configure.md)

## <a name="bkmk_logging"></a>NPS 登入

NPS 登入也稱為 RADIUS 計量。 設定是否作為 NPS RADIUS 伺服器、proxy，或是這些設定的任何組合 NPS 登入您的需求。

若要設定 NPS 登入，您必須設定您想要登入的事件您要登入並檢視的事件檢視器]，然後判斷哪些其他資訊。 此外，您必須認為您是要登入驗證使用者以及計量資訊儲存在本機電腦上的文字登入檔案，或 edition 在本機電腦或遠端電腦上。

如需詳細資訊，請查看[設定的網路原則伺服器計量](nps-accounting-configure.md)。
