---
title: 核心網路元件
description: 本指南提供有關如何在 Windows Server 2016 的新樹系中規劃和部署全功能網路所需的核心元件，以及新的 Active Directory 網域的指示。
manager: brianlic
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f746422ee05afc4f000693138c1f2efb3b7f70a9
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866117"
---
# <a name="core-network-components"></a>核心網路元件

>適用於：Windows Server (半年度管道)、Windows Server 2016

本指南提供的指示說明如何在新樹系中規劃和部署全功能網路所需的核心元件，以及新的 Active Directory 網域。

> [!NOTE]
> 您可以從 TechNet 元件庫下載 Microsoft Word 格式的指南。 如需詳細資訊，請參閱 [Windows Server 2016 的核心網路指南](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

本指南涵蓋下列各節。

- [關於本指南](#BKMK_about)

- [核心網路概觀](#BKMK_overview)

- [核心網路規劃](#BKMK_planning)

- [核心網路部署](#BKMK_deployment)

- [其他技術資源](#BKMK_resources)

- [附錄 A 到 E](#BKMK_appendix)

## <a name="about-this-guide"></a><a name="BKMK_about"></a>關於本指南
本指南是針對網路與系統管理員所設計，他們安裝新網路或建立網域型網路，取代含有工作群組的網路。 如果您預期未來有新增更多服務與功能至網路的需求，則本指南提供的部署案例特別有用。

建議您檢閱此部署案例中所用每個技術的設計和部署指南，以協助您判斷此指南是否提供您需要的服務與設定。

核心網路是網路硬體、裝置以及軟體的集合，提供組織資訊技術 (IT) 需求的基礎服務。

Windows Server 核心網路提供許多好處，包括下列各項。

- 電腦與其他傳輸控制通訊協定/網際網路通訊協定 (TCP/IP) 相容裝置之間網路連線能力的核心通訊協定。 TCP/IP 是一組標準的通訊協定，用於連線電腦與建置網路。 TCP/IP 是 Microsoft Windows 作業系統所提供的網路通訊協定軟體，可實行並支援 TCP/IP 通訊協定套件。

- 動態主機設定通訊協定 (DHCP) 會自動將 IP 位址指派至已設定為 DHCP 用戶端的電腦和其他裝置。 手動設定網路上所有電腦的 IP 位址，比使用 DHCP 伺服器動態提供電腦及其他裝置使用的 IP 位址設定，更為耗時且缺乏彈性。

- 網域名稱系統 (DNS) 名稱解析服務。 DNS 允許使用者、電腦、應用程式及服務，使用電腦或裝置的完整網域名稱，尋找網路上電腦和裝置的 IP 位址。

- 樹系是一或多個 Active Directory 網域，它們共用相同類別與屬性定義 (架構)、站台與複寫資訊 (設定) 以及全樹系搜尋功能 (通用類別目錄)。

- 樹系根網域是在新樹系中建立的第一個網域。 Enterprise Admins 與 Schema Admins 群組是全樹系系統管理群組，位於樹系根網域中。 此外，樹系根網域如同其他網域一樣，都是系統管理員在 Active Directory 網域服務 (AD DS) 中定義之電腦、使用者及群組物件的集合。 這些物件共用一般目錄資料庫與安全性原則。 如果您隨著組織擴充而新增網域，它們也可以與其他網域共用安全性關係。 目錄服務也會儲存目錄資料，並允許授權的電腦、應用程式及使用者存取資料。

- 使用者與電腦帳戶資料庫。 目錄服務提供集中的使用者帳戶資料庫，可讓您為有權連線到您網路及存取網路資源 (例如應用程式、資料庫、共用檔案與資料夾及印表機) 的人及電腦建立使用者與電腦帳戶。

核心網路也可以讓您隨著組織擴充及 IT 需求變更來調整網路大小。 例如，透過核心網路，您可以新增網域、IP 子網、遠端存取服務、無線服務，以及 Windows Server 2016 所提供的其他功能和伺服器角色。

### <a name="network-hardware-requirements"></a>網路硬體需求
若要成功部署核心網路，就必須部署網路硬體，包括下列各項：

- 乙太網路、快速乙太網路或 GB 乙太網路配線

- 集線器、第 2 層交換器或第 3 層交換器、路由器或在電腦與裝置之間執行網路流量傳遞功能的其他裝置。

- 符合個別用戶端與伺服器作業系統最低硬體需求的電腦。

## <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容
本指南未提供部署下列內容的說明：

- 網路硬體，例如配線、路由器、交換器以及集線器

- 其他網路資源，例如印表機及檔案伺服器

- 網際網路連線

- 遠端存取

- 無線存取

- 用戶端電腦部署

> [!NOTE]
> 預設會將執行 Windows 用戶端作業系統的電腦設定為接收來自 DHCP 伺服器的 IP 位址租用。 因此，不需要用戶端電腦的其他 DHCP 或網際網路通訊協定第 4 版 (IPv4) 設定。

## <a name="technology-overviews"></a>技術概觀
下列幾節提供為建立核心網路所部署之必要技術的簡短概觀。

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
目錄是階層結構，可以儲存網路上物件的相關資訊，例如，使用者和電腦。 目錄服務 (例如 AD DS) 提供儲存目錄資料的方法，並使這些資料可供網路使用者與系統管理員使用。 例如，AD DS 會儲存使用者帳戶的相關資訊，包含名稱、電子郵件地址、密碼及電話號碼，然後讓相同網路上其他授權的使用者存取此資訊。

### <a name="dns"></a>DNS
DNS 是 TCP/IP 網路 (例如網際網路或組織網路) 的名稱解析通訊協定。 DNS 伺服器所裝載的資訊可讓用戶端電腦和服務將容易辨識的英數 DNS 名稱解析為電腦之間互相通訊所使用的 IP 位址。

### <a name="dhcp"></a>DHCP
DHCP 是簡化主機 IP 設定管理的 IP 標準。 DHCP 標準提供使用 DHCP 伺服器做為管理動態 IP 位址配置的方法，以及網路上啟用 DHCP 的用戶端的其他相關設定詳細資料。

DHCP 可讓您使用 DHCP 伺服器，以動態方式將 IP 位址指派給區域網路上的電腦或其他裝置 (例如印表機)。 TCP/IP 網路上的每部電腦都必須擁有唯一的 IP 位址，因為 IP 位址及其相關的子網路遮罩會識別主機電腦和電腦所連線的子網路。 藉由使用 DHCP，您可以確定已設定為 DHCP 用戶端的所有電腦都會收到適合其網路位置和子網路的 IP 位址，而且藉由使用 DHCP 選項 (例如，預設閘道和 DNS 伺服器)，您可以自動為 DHCP 用戶端提供它們在您網路上正常運作所需的資訊。

對於 TCP/IP 網路，DHCP 可減少重新設定電腦所牽涉的複雜性及系統管理工作量。

### <a name="tcpip"></a>TCP/IP
Windows Server 2016 中的 TCP/IP 如下：

- 以工業標準網路通訊協定為基礎的網路軟體。

- 支援 Windows 電腦連線到區域網路 (LAN) 與廣域網路 (WAN) 環境的可路由、企業網路通訊協定。

- 將 Windows 電腦連線到相異系統以共用資訊的核心技術與公用程式。

- 取得全域網際網路服務存取權的基礎，例如全球資訊網與檔案傳輸通訊協定 (FTP) 伺服器。

- 穩固、可擴充及跨平台的用戶端/伺服器架構。

TCP/IP 提供基本的 TCP/IP 公用程式，可讓 Windows 電腦與其他 Microsoft 及非 Microsoft 系統連線和共用資訊，其中包括：

-  Windows Server 2016

- Windows 10

-  Windows Server 2012 R2

- Windows 8.1

-  Windows Server 2012

- Windows 8

-  Windows Server 2008 R2

-  Windows 7

-  Windows Server 2008

- Windows Vista

- 網際網路主機

- Apple Macintosh 系統

- IBM 主機

- UNIX 和 Linux 系統

- Open VMS 系統

- 網路就緒印表機

- 啟用有線乙太網路或無線802.11 技術的平板電腦和行動電話電話

## <a name="core-network-overview"></a><a name="BKMK_overview"></a>核心網路概觀
下圖顯示 Windows Server 核心網路拓撲。

![Windows Server 核心網路拓撲](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> 本指南也包含將選用的網路原則伺服器 (NPS) 和網頁伺服器 (IIS) 等伺服器新增到網路拓撲的指示，以提供安全網路存取解決方案的基礎，例如，您可以使用核心網路附屬指南來實作的 802.1X 有線與無線部署。 如需詳細資訊，請參閱[部署網路存取授權和 Web 服務的選用功能](#BKMK_optionalfeatures)。

### <a name="core-network-components"></a>核心網路元件
下面是核心網路的元件。

##### <a name="router"></a>路由器
本部署指南提供部署核心網路的說明，此基礎網路使用已啟用 DHCP 轉寄的路由器來分隔兩個子網路。 不過，根據您的需求與資源而定，您可以部署第 2 層交換器、第 3 層交換器或集線器。 如果您部署交換器，則此交換器必須能夠執行 DHCP 轉寄，或您必須在每個子網路上設置一個 DHCP 伺服器。 如果您部署集線器，就是在部署單一子網路，則不需要 DHCP 轉寄或 DHCP 伺服器上的第二個領域。

##### <a name="static-tcpip-configurations"></a>靜態 TCP/IP 設定
此部署中的伺服器都設定為靜態 IPv4 位址。 根據預設值，用戶端電腦設定為接收從 DHCP 伺服器租用的 IP 位址。

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Active Directory 網域服務通用類別目錄和 DNS 伺服器 DC1
Active Directory 網域服務 (AD DS) 與網域名稱系統 (DNS) 都安裝在此伺服器 (名為 DC1) 上，提供網路上所有電腦與裝置的目錄與名稱解析服務。

##### <a name="dhcp-server-dhcp1"></a>DHCP 伺服器 DHCP1
DHCP 伺服器 (名為 DHCP1) 已設定為有一個領域，此領域提供網際網路通訊協定 (IP) 位址供本機子網路的電腦租用。 DHCP 伺服器也可以設定額外的領域，使其在路由器上設定 DHCP 轉寄的狀況下，可提供 IP 位址供其他子網路上的電腦租用。

##### <a name="client-computers"></a>用戶端電腦
預設會將執行 Windows 用戶端作業系統的電腦設定為 DHCP 用戶端，以自動從 DHCP 伺服器取得 IP 位址和 DHCP 選項。

## <a name="core-network-planning"></a><a name="BKMK_planning"></a>核心網路規劃
在部署核心網路之前，您必須規劃下列項目。

- [規劃子網路](#bkmk_NetFndtn_Pln_Subnt)

- [規劃所有伺服器的基本設定](#bkmk_NetFndtn_Pln_AllSrvrs)

- [規劃 DC1 的部署](#bkmk_NetFndtn_Pln_AD-DNS-01)

- [規劃網域存取](#bkmk_NetFndtn_Pln_DomAccess)

- [規劃 DHCP1 的部署](#bkmk_NetFndtn_Pln_DHCP-01)

下列幾節提供這些項目的詳細資料。

> [!NOTE]
> 如需規劃部署的協助，另請參閱 [附錄 E-核心網路規劃準備表](#BKMK_E)。

### <a name="planning-subnets"></a><a name="bkmk_NetFndtn_Pln_Subnt"></a>規劃子網路
在傳輸控制通訊協定/網際網路通訊協定 (TCP/IP) 網路中，路由器是用來相互連接用於不同實體網路區段的硬體和軟體，這些網路區段稱為子網路。 路由器也可以用來在每個子網路之間轉寄 IP 封包。 在繼續本指南的說明之前，先確定網路的實體配置，包括您需要的路由器與子網路數目。

此外，若要使用靜態 IP 位址來設定網路的伺服器，必須確定您要用於核心網路伺服器所在之子網路的 IP 位址範圍。 在本指南中，會使用私人 IP 位址範圍 10.0.0.1-10.0.0.254 和 10.0.1.1-10.0.1.254 作為範例，但您可以使用任何您偏好的私人 IP 位址範圍。

> [!IMPORTANT]
> 當您選取要用於每個子網路的 IP 位址範圍之後，請確定您為路由器所設定的 IP 位址，是來自與安裝該路由器的子網路上所使用的相同 IP 位址範圍。 例如，如果您的路由器預設設定為 192.168.1.1 的 IP 位址，但是您正在 IP 位址範圍為 10.0.0.0/24 的子網路上安裝路由器，則必須重新設定路由器，以使用來自 10.0.0.0/24 IP 位址範圍的 IP 位址。

下列已辨識的私人 IP 位址範圍由網際網路要求建議 (RFC) 1918 所指定：

- 10.0.0.0-10.255.255.255

- 172.16.0.0-172.31.255.255

- 192.168.0.0-192.168.255.255

使用 RFC 1918 中指定的私人 IP 位址範圍時，您不能使用私人 IP 位址直接連線到網際網路，因為網際網路服務提供者 (ISP) 路由器會自動捨棄在這些位址來回的要求。 如果稍後要新增網際網路連線能力至核心網路，您必須連絡 ISP 以取得公用 IP 位址。

> [!IMPORTANT]
> 使用私人 IP 位址時，您必須使用某種 Proxy 或網路位址轉譯 (NAT) 伺服器，將區域網路上的私人 IP 位址範圍轉換成可在網際網路上路由的公用 IP 位址。 大部分的路由器都提供 NAT 服務，因此，選取支援 NAT 的路由器應該非常簡單。

如需詳細資訊，請參閱[規劃 DHCP1 的部署](#bkmk_NetFndtn_Pln_DHCP-01)。

### <a name="planning-basic-configuration-of-all-servers"></a><a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>規劃所有伺服器的基本設定
針對核心網路中的每部伺服器，您必須將電腦重新命名，並為該電腦指派和設定靜態 IPv4 位址及其他 TCP/IP 內容。

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>規劃電腦與裝置的命名慣例
如需跨網路的一致性，建議伺服器、印表機及其他裝置使用一致的名稱。 您可以使用電腦名稱協助使用者與系統管理員輕鬆識別伺服器、印表機或其他裝置的目的與位置。 例如，如果您有三部 DNS 伺服器，一個在三藩市，一個在洛杉磯，一個在芝加哥，則您可以使用命名慣例伺服器函式 *server function* - *位置* - *編號*：

- DNS-DEN-01。 這個名稱代表位於科羅拉多州 Denver 的 DNS 伺服器。 如果在 Denver 新增其他 DNS 伺服器，則名稱中的數值會增加，例如 DNS-DEN-02 與 DNS-DEN-03。

- DNS-SPAS-01。 這個名稱代表位於加利福尼亞州 South Pasadena 的 DNS 伺服器。

- DNS-ORL-01。 這個名稱代表位於佛羅里達州 Orlando 的 DNS 伺服器。

在本指南中，伺服器命名慣例非常簡單，由主要伺服器功能和編號組成。 例如，將網域控制站命名為 DC1，將 DHCP 伺服器命名為 DHCP1。

建議您在使用本指南安裝核心網路之前，先選擇命名慣例。

#### <a name="planning-static-ip-addresses"></a>規劃靜態 IP 位址
使用靜態 IP 位址設定每一部電腦之前，您必須規劃子網路與 IP 位址範圍。 此外，您必須決定 DNS 伺服器的 IP 位址。 如果您計畫為靜態 IP 位址設定安裝路由器以存取其他網路，例如其他子網路或網際網路，則您必須知道路由器的 IP 位址，也稱為預設閘道。

下表提供靜態 IP 位址設定的範例值。

|設定項目|範例值|
|-----------------------|------------------|
|IP 位址|10.0.0.2|
|子網路遮罩|255.255.255.0|
|預設閘道 (路由器 IP 位址)|10.0.0.1|
|慣用 DNS 伺服器|10.0.0.2|

> [!NOTE]
> 如果您規劃部署一部以上的 DNS 伺服器，也可以規劃其他 DNS 伺服器 IP 位址。

### <a name="planning-the-deployment-of-dc1"></a><a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>規劃 DC1 的部署
下面是在 DC1 上安裝 Active Directory 網域服務 (AD DS) 與 DNS 之前的主要規劃步驟。

#### <a name="planning-the-name-of-the-forest-root-domain"></a>規劃樹系根網域的名稱
AD DS 設計處理作業中的第一個步驟，是決定您的組織需要多少樹系。 樹系是最上層 AD DS 容器，其中含有一或多個網域共用通用架構及通用類別目錄。 組織可以擁有多個樹系，但單一樹系設計是大多數組織慣用的模型，而且最容易管理。

在組織中建立第一個網域控制站時，就是建立第一個網域 (也稱為樹系根網域) 與第一個樹系。 不過，使用本指南採取這個動作之前，您必須為組織決定最佳的網域名稱。 在多數狀況下，可以使用組織名稱做為網域名稱，而在許多狀況下，會註冊這個網域名稱。 如果您正在規劃部署對外的網際網路型網頁伺服器，以便為客戶或合作夥伴提供資訊和服務，請選擇尚未使用的網域名稱，然後登錄該網域名稱，如此一來，您的組織便能擁有該名稱。

#### <a name="planning-the-forest-functional-level"></a>規劃樹系功能等級
在安裝 AD DS 時，您必須選擇要使用的樹系功能等級。 Windows Server 2003 Active Directory 中引進的網域與樹系功能，提供在網路環境中啟用全網域或全樹系 Active Directory 功能的方法。 視您的環境而定，提供了不同等級的網域功能和樹系功能。

樹系功能會啟用樹系中跨所有網域的功能。 樹系功能等級有下列幾種：

-  Windows Server 2008。 此樹系功能等級只支援執行 Windows Server 2008 和更新版本之 Windows Server 作業系統的網域控制站。

-  Windows Server 2008 R2。 此樹系功能等級支援 Windows Server 2008 R2 網域控制站，以及執行較新版本之 Windows Server 作業系統的網域控制站。

-  Windows Server 2012。 此樹系功能等級支援 Windows Server 2012 網域控制站，以及執行較新版本之 Windows Server 作業系統的網域控制站。

-  Windows Server 2012 R2。 此樹系功能等級支援 Windows Server 2012 R2 網域控制站，以及執行較新版本之 Windows Server 作業系統的網域控制站。

-  Windows Server 2016。 此樹系功能等級只支援 Windows Server 2016 網域控制站，以及執行較新版本之 Windows Server 作業系統的網域控制站。

如果您要在新的樹系中部署新網域，而且所有的網域控制站都執行 Windows Server 2016，建議您在 AD DS 安裝期間，使用 Windows Server 2016 樹系功能等級來設定 AD DS。

> [!IMPORTANT]
> 在提高樹系功能等級之後，便無法將執行舊版作業系統的網域控制站引入樹系。 例如，如果您將樹系功能等級提高至 Windows Server 2016，則無法將執行 Windows Server 2012 R2 或 Windows Server 2008 的網域控制站新增至樹系。

下表提供 AD DS 的設定項目範例。

|設定項目：|範例值：|
|------------------------|-------------------|
|完整 DNS 名稱|範例：<p>-corp.contoso.com<br />-example.com|
|樹系功能等級|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Active Directory 網域服務資料庫資料夾位置|E:\Configuration\\<p>或接受預設位置。|
|Active Directory 網域服務記錄檔資料夾位置|E:\Configuration\\<p>或接受預設位置。|
|Active Directory 網域服務 SYSVOL 資料夾位置|E:\Configuration\\<p>或接受預設位置|
|目錄還原模式系統管理員密碼|**J \* p2leO4 $ F**|
|回應檔案名稱 (選擇性)|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>規劃 DNS 區域
在與 Active Directory 整合的主要 DNS 伺服器上，預設會在安裝 DNS 伺服器角色期間建立正向對應區域。 正向對應區域可讓電腦與裝置根據其 DNS 名稱來查詢另一部電腦或裝置的 IP 位址。 除了正向對應區域之外，建議您建立 DNS 反向對應區域。 利用 DNS 反向對應查詢，電腦或裝置可以使用其 IP 位址來探索另一部電腦或裝置的名稱。 部署反向對應區域通常會改善 DNS 效能，並大為提升 DNS 查詢的成功。

在建立反向對應區域時，會在 DNS 中設定 in-addr.arpa 網域，此網域定義於 DNS 標準中，且保留在網際網路 DNS 命名空間中，以提供一個實際且可靠的反向查詢執行方法。 為了建立反向命名空間，會在 in-addr.arpa 網域中組成子網域，而這些子網域是使用反向數字順序的 IP 位址十進位小數點表示法來表示。

in-addr.arpa 網域可套用到根據第 4 版網際網路通訊協定 (IPv4) 定址的所有 TCP/IP 網路。 當您建立新的反向對應區域時，新增區域精靈會自動假設您要使用這個網域。

當您執行 [新增區域精靈] 時，建議使用下列選項：

|設定項目|範例值|
|-----------------------|------------------|
|區域類型|選取 [主要區域] 及 [將區域存放在 Active Directory 上]|
|Active Directory 區域複寫領域|**到這個網域中的所有 DNS 伺服器**|
|第一個反向對應區域名稱精靈頁面|**IPv4 反向對應區域**|
|第二個反向對應區域名稱精靈頁面|網路識別碼 = 10.0.0。|
|動態更新|**只允許安全的動態更新**|

### <a name="planning-domain-access"></a><a name="bkmk_NetFndtn_Pln_DomAccess"></a>規劃網域存取
若要登入網域，電腦必須是網域成員電腦，而且必須在嘗試登入之前在 AD DS 中建立使用者帳戶。

> [!NOTE]
> 執行 Windows 的個別電腦都會有一個本機使用者和群組使用者帳戶資料庫，這個資料庫稱為安全性帳戶管理員 (SAM) 使用者帳戶資料庫。 當您在本機電腦的 SAM 資料庫中建立使用者帳戶時，可以登入本機電腦，但無法登入網域。 網域使用者帳戶是在網域控制站上使用 [Active Directory 使用者和電腦] Microsoft Management Console (MMC) 所建立，而不是使用本機電腦上的本機使用者和群組建立的。

首次使用網域登入認證成功登入之後，會保存其登入設定，除非從網域移除電腦或手動變更登入設定。

登入網域之前：

- 在 Active Directory 使用者和電腦中建立使用者帳戶。 每一個使用者在 [Active Directory 使用者和電腦] 中都必須有一個 Active Directory 網域服務使用者帳戶。 如需詳細資訊，請參閱[在 Active Directory 使用者和電腦中建立使用者帳戶](#BKMK_createUA)。

- 確定 IP 位址設定正確。 若要將電腦加入網域，電腦必須有 IP 位址。 在本指南中，伺服器設定靜態 IP 位址，用戶端電腦接收從 DHCP 伺服器租用的 IP 位址。 基於這個原因，必須在將用戶端加入網域之前部署 DHCP 伺服器。 如需詳細資訊，請參閱[部署 DHCP1](#BKMK_deployDHCP01)。

- 將電腦加入網域。 提供或存取網路資源的任何電腦都必須加入網域。 如需詳細資訊，請參閱[將伺服器電腦加入網域並登入](#BKMK_joinlogserver)和[將用戶端電腦加入網域並登入](#BKMK_joinlogclients)。

### <a name="planning-the-deployment-of-dhcp1"></a><a name="bkmk_NetFndtn_Pln_DHCP-01"></a>規劃 DHCP1 的部署
下面是在 DHCP1 上安裝 DHCP 伺服器角色之前的主要規劃步驟。

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>規劃 DHCP 伺服器與 DHCP 轉寄
因為 DHCP 訊息是廣播訊息，所以路由器不會在子網路之間轉寄它們。 如果您有多個子網路，而且想要為每個子網路提供 DHCP 服務，就必須執行下列其中一項：

- 在每個子網路安裝 DHCP 伺服器

- 設定路由器跨越子網路轉寄 DHCP 廣播訊息，在 DHCP 伺服器上設定多個領域，每個子網路一個領域。

在多數狀況下，設定路由器轉寄 DHCP 廣播訊息，比在網路的每個實體區段部署 DHCP 伺服器更符合成本效益。

#### <a name="planning-ip-address-ranges"></a>規劃 IP 位址範圍
每個子網路都必須有它自己的唯一 IP 位址範圍。 這些範圍在 DHCP 伺服器上是以領域表示。

領域是一組電腦的 IP 位址，具有管理性質，位於使用 DHCP 服務的子網路上。 系統管理員首先為每個實體子網路建立一個領域，然後使用領域來定義用戶端所使用的參數。

領域具有下列內容：

- IP 位址範圍，從此範圍包括或排除用於 DHCP 服務租用供應項目的位址。

- 子網路遮罩，決定指定 IP 位址的子網路首碼。

- 建立領域時指派的領域名稱。

- 租用期間值，指派給 DHCP 用戶端，接收動態配置的 IP 位址。

- 為指派給 DHCP 用戶端所設定的任何 DHCP 領域選項，例如，DNS 伺服器 IP 位址以及路由器/預設閘道 IP 位址。

- 保留項目可選擇性地用來確保 DHCP 用戶端永遠接收相同的 IP 位址。

在部署伺服器之前，請列出您的子網路以及用於每個子網路的 IP 位址範圍。

#### <a name="planning-subnet-masks"></a>規劃子網路遮罩
使用子網路遮罩來區別 IP 位址內的網路識別碼與主機識別碼。 每個子網路遮罩都是 32 位元數字，使用全部都是一 (1) 的連續位元群組來識別網路識別碼，使用全部都是零 (0) 的連續位元群組來識別 IP 位址的主機識別碼部分。

例如，通常搭配 IP 位址 131.107.16.200 使用的子網路遮罩，是下列 32 位元二進位數：

```
11111111 11111111 00000000 00000000
```

這個子網路遮罩數字是 16 個 1 位元加上 16 個 0 位元，表示 IP 位址的網路識別碼與主機識別碼區段的長度都是 16 位元。 一般來說，這個子網路遮罩會以小數點十進位表示法顯示為 255.255.0.0。

下表顯示網際網路位址類別的子網路遮罩。

|位址類別|子網路遮罩的位元|子網路遮罩|
|-----------------|------------------------|---------------|
|類別 A|11111111 00000000 00000000 00000000|255.0.0.0|
|類別 B|11111111 11111111 00000000 00000000|255.255.0.0|
|類別 C|11111111 11111111 11111111 00000000|255.255.255.0|

在 DHCP 中建立領域並輸入領域的 IP 位址範圍時，DHCP 會提供這些預設子網路遮罩值。 通常，大部分沒有特殊需求的網路都能接受預設的子網路遮罩值，而這其中的每個 IP 網路區段都會對應至單一實體網路。

在某些狀況下，您可以使用自訂的子網路遮罩來實作 IP 子網路。 利用 IP 子網路，您可以細分 IP 位址的預設主機識別碼部分以指定子網路，這是原始類別型網路識別碼的細分。

透過自訂子網路遮罩長度，您可以減少用於實際主機識別碼的位元數。

若要防止定址及路由問題，您應該確定網路區段上的所有 TCP/IP 電腦都使用相同的子網路遮罩，且每一部電腦或裝置都有唯一的 IP 位址。

#### <a name="planning-exclusion-ranges"></a>規劃排除範圍
當您在 DHCP 伺服器上建立領域時，可以指定一個 IP 位址範圍，其中包含 DHCP 伺服器可租用給 DHCP 用戶端 (例如電腦和其他裝置) 的所有 IP 位址。 如果您接著執行並利用來自 DHCP 伺服器使用的相同 IP 位址範圍的靜態 IP 位址來設定部分伺服器和其他裝置，則您會意外造成 IP 位址衝突，您和 DHCP 伺服器會將相同的 IP 位址指派給不同的裝置。

若要解決此問題，您可以為 DHCP 領域建立排除範圍。 排除範圍是範圍 IP 位址範圍內，不允許 DHCP 伺服器使用的連續 IP 位址範圍。 如果您建立排除範圍，DHCP 伺服器就不會指派該範圍中的位址，讓您能夠手動指派這些位址，而不會產生 IP 位址衝突。

您可以透過 DHCP 伺服器建立每個領域的排除範圍，排除分配的 IP 位址。 使用靜態 IP 位址設定的所有裝置，都應該予以排除。 排除的位址應該包括手動設定給其他伺服器、非 DHCP 用戶端、無磁碟工作站或路由及遠端存取以及 PPP 用戶端的所有 IP 位址。

建議您使用額外的位址來設定排除範圍，以因應未來的網路成長。 下表針對 IP 位址範圍為 10.0.0.1 - 10.0.0.254 且子網路遮罩為 255.255.255.0 的領域，提供排除範圍範例。

|設定項目|範例值|
|-----------------------|------------------|
|排除範圍起始 IP 位址|10.0.0.1|
|排除範圍結束 IP 位址|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>規劃 TCP/IP 靜態設定
如路由器、DHCP 伺服器以及 DNS 伺服器的特定裝置都必須設定為使用靜態 IP 位址。 此外，您可能想要確定其他裝置永遠使用相同的 IP 位址，例如印表機。 列出您要為每個子網路靜態設定的裝置，然後規劃用於 DHCP 伺服器的排除範圍，以確保 DHCP 伺服器不會租用靜態設定裝置的 IP 位址。 排除範圍是領域中要從 DHCP 服務供應項目中排除的有限 IP 位址順序。 排除範圍假設伺服器不會提供這些範圍中的任何位址給網路上的 DHCP 用戶端。

例如，如果子網路的 IP 位址範圍是 192.168.0.1 至 192.168.0.254，而且您有十個裝置要設定為使用靜態 IP 位址，則可以為 192.168.0.*x* 領域建立排除範圍，使其包含十個或更多 IP 位址：192.168.0.1 至 192.168.0.15。

在這個範例中，您使用十個已排除的 IP 位址來設定伺服器與其他裝置使用靜態 IP 位址，五個額外的 IP 位址保留供您未來可能新增之裝置的靜態設定使用。 利用此排除範圍，為 DHCP 伺服器保留的位址集區為 192.168.0.16 至 192.168.0.254。

下表提供 AD DS 與 DNS 設定項目的其他範例。

|設定項目|範例值|
|-----------------------|------------------|
|網路連線繫結|乙太網路|
|DNS 伺服器設定|DC1.corp.contoso.com|
|慣用 DNS 伺服器 IP 位址|10.0.0.2|
|[新增領域] 對話方塊值<p>1. 範圍名稱<br />2. 起始 IP 位址<br />3. 結束 IP 位址<br />4. 子網路遮罩<br />5. 預設閘道 (選用) <br />6. 租用持續時間|1. 主要子網<br />2. 10.0.0。1<br />3. 10.0.0.254<br />4. 255.255.255。0<br />5. 10.0.0。1<br />6. 8 天|
|IPv6 DHCP 伺服器操作模式|未啟用|

## <a name="core-network-deployment"></a><a name="BKMK_deployment"></a>核心網路部署
若要部署核心網路，其基本步驟如下：

1.  [設定所有伺服器](#BKMK_configuringAll)

2.  [部署 DC1](#BKMK_deployADDNS01)

3.  [將伺服器電腦加入網域並登入](#BKMK_joinlogserver)

4.  [部署 DHCP1](#BKMK_deployDHCP01)

5.  [將用戶端電腦加入網域並登入](#BKMK_joinlogclients)

6.  [部署網路存取授權和 Web 服務的選用功能](#BKMK_optionalfeatures)

> [!NOTE]
> - 本指南中的大部分程序都會有同等的 Windows PowerShell 命令。 在 Windows PowerShell 中執行這些 Cmdlet 之前，請以您網路部署適用的值取代範例值。 此外，您必須在 Windows PowerShell 中以單行方式輸入每個 Cmdlet。 在本指南中，個別的 Cmdlet 可能會因為格式化的限制和透過瀏覽器或其他應用程式顯示文件的關係，而出現在數行中。
> - 本指南的程序不包含開啟 [使用者帳戶控制] 對話方塊來要求權限以繼續執行的說明。 如果執行本指南的程序時此對話方塊開啟，或此對話方塊因回應您的動作而開啟，請按一下 [繼續]。

### <a name="configuring-all-servers"></a><a name="BKMK_configuringAll"></a>設定所有伺服器
在安裝其他技術 (例如 Active Directory 網域服務或 DHCP) 之前，重點是要設定下列項目。

- [重新命名電腦](#BKMK_rename)

- [設定靜態 IP 位址](#BKMK_ip)

您可以使用下列幾節為每部伺服器執行這些動作。

若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

#### <a name="rename-the-computer"></a><a name="BKMK_rename"></a>重新命名電腦
您可以使用本節中的程式來變更電腦名稱稱。 當作業系統已自動建立您不想使用的電腦名稱時，將電腦重新命名就很有用。

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell，將下列 Cmdlet 輸入不同行，然後按 ENTER。 您也必須以您要使用的名稱取代 *ComputerName*。
>
> `Rename-Computer`*名稱*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>重新命名執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的電腦

1.  在 [伺服器管理員] 中，按一下 [本機伺服器]。 電腦 [內容] 會顯示在詳細資料窗格中。

2.  在 [內容] 的 [電腦名稱] 中，按一下現有的電腦名稱。 [系統內容] 對話方塊隨即開啟。 按一下 [變更]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

3.  在 [電腦名稱/網域變更] 對話方塊的 [電腦名稱] 中，輸入電腦的新名稱。 例如，如果想要將電腦命名為 DC1，請輸入 **DC1**。

4.  按兩次 [確定]，然後按一下 [關閉]。 如果想要立即重新啟動電腦以完成名稱變更，請按一下 [立即重新啟動]。 否則，請按一下 [稍後重新啟動]。

> [!NOTE]
> 如需如何將執行其他 Microsoft 作業系統的電腦重新命名的相關資訊，請參閱 [附錄 A-重新命名電腦](#BKMK_A)。

#### <a name="configure-a-static-ip-address"></a><a name="BKMK_ip"></a>設定靜態 IP 位址
您可以使用本主題中的程式，針對執行 Windows Server 2016 的電腦，使用靜態 IP 位址來設定網路連線的第四版網際網路協定 (IPv4) 屬性。

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell，將下列 Cmdlet 輸入不同行，然後按 ENTER。 您也必須使用您要用來設定電腦的值，取代這個範例中的介面名稱和 IP 位址。
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>若要在執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的電腦上設定靜態 IP 位址

1.  在工作列上，以滑鼠右鍵按一下 [網路] 圖示，然後按一下 [開啟網路和共用中心]。

2.  在  **網路和共用中心** 中，按一下 [ **變更介面卡設定**]。 [網路連線] 資料夾隨即開啟，並顯示可用的網路連線。

3.  在 [網路連線] 中，以滑鼠右鍵按一下您要設定的連線，然後按一下 [內容]。 網路連線 [內容] 對話方塊隨即開啟。

4.  在網路連線 [內容] 對話方塊的 [這個連線使用下列項目] 中，選取 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。 [網際網路通訊協定第 4 版 (TCP/IPv4) 內容] 對話方塊隨即開啟。

5.  在 [網際網路通訊協定第 4 版 (TCP/IPv4) 內容] 的 [一般] 索引標籤上，按一下 [使用下列的 IP 位址]。 在 [IP 位址] 中，輸入您要使用的 IP 位址。

6.  按下索引標籤，以便將游標放在 [子網路遮罩] 中。 系統會自動輸入子網路遮罩的預設值。 接受預設的子網路遮罩，或輸入您要使用的子網路遮罩。

7.  在 [預設閘道] 中，輸入預設閘道的 IP 位址。

    > [!NOTE]
    > 您必須以您在路由器的區域網路 (LAN) 介面上使用的相同 IP 位址，設定 [預設閘道]。 例如，如果您的路由器連線至廣域網路 (WAN) (例如，網際網路) 和 LAN，請使用您將指定為 [預設閘道] 的相同 IP 位址來設定 LAN 介面。 在另一個範例中，如果您的路由器連線至兩個 LAN，其中 LAN A 使用位址範圍 10.0.0.0/24，而 LAN B 使用位址範圍 192.168.0.0/24，請使用該位址範圍的位址 (例如 10.0.0.1) 來設定 LAN A 路由器 IP 位址。 此外，在這個位址範圍的 DHCP 領域中，使用 IP 位址 10.0.0.1 來設定 [預設閘道]。 對於 LAN B，請使用來自該位址範圍的位址 (例如 192.168.0.1) 設定 LAN B 路由器介面，然後使用 [預設閘道] 的值 192.168.0.1 設定 LAN B 領域 192.168.0.0/24。

8.  在 [慣用 DNS 伺服器] 中，輸入 DNS 伺服器的 IP 位址。 如果您計畫使用本機電腦當做慣用 DNS 伺服器，請輸入本機電腦的 IP 位址。

9. 在 [其他 DNS 伺服器] 中，輸入其他 DNS 伺服器的 IP 位址 (如果有的話)。 如果您計畫使用本機電腦當做其他 DNS 伺服器，請輸入本機電腦的 IP 位址。

10. 按一下 **[確定]** ，然後按一下 **[關閉]** 。

> [!NOTE]
> 如需有關如何在執行其他 Microsoft 作業系統的電腦上設定靜態 IP 位址的詳細資訊，請參閱 [附錄 B-設定靜態 ip 位址](#BKMK_B)。

#### <a name="deploying-dc1"></a><a name="BKMK_deployADDNS01"></a>部署 DC1
若要部署執行 Active Directory 網域服務 (AD DS) 與 DNS 的電腦 DC1，您必須依照下列順序來完成這些步驟：

- 執行[設定所有伺服器](#BKMK_configuringAll)一節中的步驟。

- [為新樹系安裝 AD DS 與 DNS](#BKMK_installAD-DNS)

- [在 Active Directory 使用者和電腦中建立使用者帳戶](#BKMK_createUA)

- [指派群組成員資格](#BKMK_assigngroup)

- [設定 DNS 反向對應區域](#BKMK_reverse)

**系統管理許可權**

如果您正在安裝小型網路，且您是該網路唯一的系統管理員，建議您自行建立使用者帳戶，然後將該使用者帳戶加入 Enterprise Admins 與 Domain Admins 群組成為其成員。 如此可讓您更容易扮演所有網路資源的系統管理員。 建議當您需要執行系統管理工作時，才使用此帳戶登入，而且您要建立另一個使用者帳戶來執行非 IT 相關工作。

如果您擁有具備多位系統管理員的大型組織，請參閱 AD DS 文件以決定組織員工的最佳群組成員資格。

**網域使用者帳戶與本機電腦上使用者帳戶之間的差異**

網域型基礎結構的優點之一，是您不需要在網域的每一部電腦上建立使用者帳戶。 不論電腦是用戶端電腦或伺服器均如此。

因此，您不應該在網域的每一部電腦上建立使用者帳戶。 在 [Active Directory 使用者和電腦] 中建立所有使用者帳戶，然後使用先前的程序來指派群組成員資格。 依照預設，所有的使用者帳戶都是 Domain Users 群組的成員。

Domain Users 群組的所有成員都可以登入任何已加入網域的用戶端電腦。

您可以設定使用者帳戶來指定允許使用者登入電腦的日期及時間。 您也可以指定允許每個使用者使用的電腦。 若要設定這些設定，請開啟 [Active Directory 使用者和電腦]，找出您要設定的使用者帳戶，然後按兩下這個帳戶。 在使用者帳戶的 [內容] 中，按一下 [帳戶] 索引標籤，然後按 [登入時數] 或 [登入]。

#### <a name="install-ad-ds-and-dns-for-a-new-forest"></a><a name="BKMK_installAD-DNS"></a>為新樹系安裝 AD DS 與 DNS

您可以使用下列其中一個程式來安裝 Active Directory Domain Services (AD DS) 和 DNS，以及在新樹系中建立新網域。

第一個程式會提供使用 Windows PowerShell 執行這些動作的指示，而第二個程式則說明如何使用伺服器管理員來安裝 AD DS 和 DNS。

>[!IMPORTANT]
>當您完成執行此程式中的步驟之後，電腦會自動重新開機。

**使用 Windows PowerShell 安裝 AD DS 和 DNS**

您可以使用下列命令來安裝和設定 AD DS 和 DNS。 您必須以您要用於網域的值取代此範例中的功能變數名稱。

>[!NOTE]
>如需這些 Windows PowerShell 命令的詳細資訊，請參閱下列參考主題。
>- [Install-](/powershell/module/servermanager/install-windowsfeature)
>- [安裝-Install-addsforest](/powershell/module/addsdeployment/install-addsforest)

若要執行此程序，至少需要 **Administrators** 的成員資格。

- 以系統管理員身分執行 Windows PowerShell，輸入下列命令，然後按 ENTER：

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`
```

順利完成安裝時，會在 Windows PowerShell 中顯示下列訊息。

| Success | 需要重新開機 | 結束碼 |  功能結果 |
|--|--|--|--|
| True | 否 | Success | {Active Directory Domain Services，群組 P .。。 |

- 在 Windows PowerShell 中，輸入下列命令，將文字 **corp.contoso.com** 取代為您的功能變數名稱，然後按 enter：

````powershell
Install-ADDSForest -DomainName "corp.contoso.com"
````

- 在安裝和設定程式期間（顯示于 Windows PowerShell 視窗頂端），會出現下列提示。 出現之後，輸入密碼，然後按 ENTER。

    **SafeModeAdministratorPassword:**

- 輸入密碼並按下 ENTER 後，會出現下列確認提示。 輸入相同的密碼，然後按 ENTER。

    **確認 SafeModeAdministratorPassword：**

- 出現下列提示時，請輸入字母 **Y** ，然後按 enter 鍵。

    ```
    The target server will be configured as a domain controller and restarted when this operation is complete.
    Do you want to continue with this operation?
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    ```

- 如果您想要的話，可以閱讀在 AD DS 和 DNS 的正常安裝期間所顯示的警告訊息。 這些訊息是正常的，不是安裝失敗的指示。

- 安裝成功之後，會出現一則訊息，指出您即將登出電腦，讓電腦可以重新開機。 如果您按一下 [ **關閉**]，則會立即登出電腦，並重新啟動電腦。 如果您沒有按一下 [ **關閉**]，電腦會在一段預設時間後重新開機。

- 重新開機伺服器之後，您可以確認 Active Directory Domain Services 和 DNS 的安裝是否成功。 開啟 Windows PowerShell，輸入下列命令，然後按 ENTER。

````powershell
Get-WindowsFeature
````

此命令的結果會顯示在 Windows PowerShell 中，而且應該類似下圖中的結果。 針對已安裝的技術，技術名稱左邊的括弧包含字元 **X**，並 **安裝****安裝狀態** 的值。

![Get-WindowsFeature 命令的結果](../media/Core-Network-Guide/server-roles-installed.jpg)

**使用伺服器管理員安裝 AD DS 和 DNS**

1.  在 DC1 的 [伺服器管理員] 中，按一下 [管理]，然後按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即開啟。

2.  在 [在您開始前] 中，按 [下一步]。

    > [!NOTE]
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。

3.  在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。

4.  在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步] 。

5.  在 [選取伺服器角色] 的 [角色] 中，按一下 [Active Directory 網域服務]。 在 [新增 Active Directory 網域服務所需的功能] 中，按一下 [新增功能]。 按一下 [下一步] 。

6.  在 [選取功能] 中，按 [下一步]，然後在 [Active Directory 網域服務]中，檢閱提供的資訊，接著按 [下一步]。

7.  在 [確認安裝選項] 中，按一下 [安裝]。 [安裝進度] 頁面會在安裝程序期間顯示狀態。 當程序完成時，在訊息詳細資料中，按一下 [將此伺服器升級為網域控制站]。 [Active Directory 網域服務設定精靈] 隨即開啟。

8.  在 [部署設定] 中，選取 [新增樹系]。 在 [根網域名稱] 中，輸入網域的完整網域名稱 (FQDN)。 例如，如果您的 FQDN 是 corp.contoso.com，請輸入 **corp.contoso.com**。 按一下 [下一步] 。

9. 在 [網域控制站選項] 的 [選取新樹系和根網域的功能等級] 中，選取您要使用的樹系功能等級和網域功能等級。 在 [指定網域控制站功能] 中，確定已選取 [網域名稱系統 (DNS) 伺服器] 和 [通用類別目錄 (GC)]。 在 [密碼] 和 [確認密碼] 中，輸入您要使用的目錄服務還原模式 (DSRM) 密碼。 按一下 [下一步] 。

10. 按一下 [DNS 選項] 中的 [下一步]。

11. 在 [其他選項] 中，確認指派給網域的 NetBIOS 名稱，並且只在需要時變更。 按一下 [下一步] 。

12. 在 [路徑] 的 [指定 AD DS 資料庫、記錄檔及 SYSVOL 的位置] 中，執行下列其中一項：

    - 接受預設值。

    - 輸入 [資料庫資料夾]、[記錄檔資料夾] 以及 [SYSVOL 資料夾] 的資料夾位置。

13. 按一下 [下一步] 。

14. 在 [檢閱選項] 中檢閱您的選項。

15. 如果想要將設定匯出至 Windows PowerShell 指令碼，請按一下 [檢視指令碼]。 隨即會在 [記事本] 開啟指令碼，您可以將它儲存到所需的資料夾位置。 按一下 [下一步] 。 在 [先決條件檢查] 中，驗證您的選取項目。 檢查完成時，按一下 [安裝]。 當 Windows 提示時，按一下 [關閉]。 伺服器會重新開機，以完成 AD DS 和 DNS 的安裝。

16. 若要確認安裝是否成功，請在伺服器重新開機之後，查看伺服器管理員主控台。 AD DS 和 DNS 應該會出現在左窗格中，例如下圖中反白顯示的專案。

![伺服器管理員中的 AD DS 和 DNS](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="create-a-user-account-in-active-directory-users-and-computers"></a><a name="BKMK_createUA"></a>在 Active Directory 使用者和電腦中建立使用者帳戶
您可以使用此程序在 [Active Directory 使用者和電腦] Microsoft Management Console (MMC) 中建立新網域使用者帳戶。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell，將下列 Cmdlet 輸入一行，然後按 ENTER。 您也必須以您要使用的值取代這個範例中的使用者帳戶名稱。
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> 按 ENTER 之後，輸入使用者帳戶的密碼。 帳戶隨即建立，而且預設會授與 Domain Users 群組的成員資格。
>
> 使用下列 Cmdlet，您可以為新的使用者帳戶指派其他群組成員資格。 下列範例會將 User1 新增至 Domain Admins 和 Enterprise Admins 群組。 請確定在執行這個命令之前，先變更使用者帳戶名稱、網域名稱及群組，以符合您的需求。
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>建立使用者帳戶

1.  在 DC1 的 [伺服器管理員] 中，依序按一下 [工具] 和 [Active Directory 使用者和電腦]。 這樣會開啟 [Active Directory 使用者和電腦] MMC。 如果尚未選取，請按一下您的網域節點。 例如，如果您的網域是 corp.contoso.com，請按一下 **corp.contoso.com**。

2.  在詳細資料窗格中，使用滑鼠右鍵按一下您想新增使用者帳戶的資料夾。

    **：?**

    - Active Directory 消費者和電腦/*網域節點* / *資料夾*

3.  指向 [新增]，然後按一下 [使用者]。 [ **新增物件-使用者** ] 對話方塊隨即開啟。

4.  在 [名字] 中輸入使用者的名字。

5.  在 [英文縮寫] 中輸入使用者的縮寫。

6.  在 [姓氏] 中輸入使用者的姓氏。

7.  修改 [全名] 以新增縮寫或讓名字與姓氏的順序互換。

8.  在 [使用者登入名稱] 中輸入使用者的登入名稱。 按一下 [下一步] 。

9. 在 [新增物件 - 使用者]、[密碼] 及 [確認密碼] 中，輸入使用者的密碼，然後選取適當的密碼選項。

10. 按 [下一步]，檢閱新的使用者帳戶設定，然後按一下 [完成]。

##### <a name="assign-group-membership"></a><a name="BKMK_assigngroup"></a>指派群組成員資格
您可以使用此程序在 [Active Directory 使用者和電腦] Microsoft Management Console (MMC) 中新增使用者、電腦或群組至群組中。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

###### <a name="to-assign-group-membership"></a>指派群組成員資格

1.  在 DC1 的 [伺服器管理員] 中，依序按一下 [工具] 和 [Active Directory 使用者和電腦]。 這樣會開啟 [Active Directory 使用者和電腦] MMC。 如果尚未選取，請按一下您的網域節點。 例如，如果您的網域是 corp.contoso.com，請按一下 **corp.contoso.com**。

2.  在詳細資料窗格中，按兩下包含您要新增成員之群組的資料夾。

    何地？

    - **Active Directory 消費者和電腦** /*網域節點* /*包含群組的資料夾*

3.  在詳細資料窗格中，以滑鼠右鍵按一下您要新增至群組的物件 (例如使用者或電腦)，然後按一下 [內容]。 物件的 [ **屬性** ] 對話方塊隨即開啟。 按一下 [成員隸屬] 索引標籤。

4.  在 [成員隸屬] 索引標籤上，按一下 [新增]。

5.  在 [輸入物件名稱來選取] 中，輸入想要新增物件的群組名稱，然後按一下 [確定]。

6.  若要指派群組成員資格給其他使用者、群組或電腦，請重複此程序的步驟 4 與 5。

##### <a name="configure-a-dns-reverse-lookup-zone"></a><a name="BKMK_reverse"></a>設定 DNS 反向對應區域
您可以使用此程序來設定網域名稱系統 (DNS) 中的反向對應區域。

若要執行此程序，至少需要 **Domain Admins** 的成員資格。

> [!NOTE]
> - 針對中型和大型組織，建議您在 Active Directory 消費者和電腦中設定和使用 DNSAdmins 群組。 如需詳細資訊，請參閱[其他技術資源](#BKMK_resources)。
> - 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell，將下列 Cmdlet 輸入一行，然後按 ENTER。 您也必須以您要使用的值取代這個範例中的 DNS 反向對應區域和區域檔案名稱。 請確定您會反轉反向對應區域名稱的網路識別碼。 例如，如果網路識別碼為192.168.0，請建立反向對應區功能變數名稱稱 **0.168.192.in-addr. arpa**。
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>設定 DNS 反向對應區域

1.  在 DC1 的 [伺服器管理員] 中，按一下 [工具]，然後按一下 [DNS]。 此時會開啟 DNS MMC。

2.  在 DNS 中，如果尚未將它展開，請按兩下伺服器名稱，即可展開樹狀目錄。 例如，如果 DNS 伺服器名稱為 DC1，請按兩下 [DC1]。

3.  選取 [反向對應區域]，在 [反向對應區域] 按一下滑鼠右鍵，然後按一下 [新增區域]。 [新增區域精靈] 隨即開啟。

4.  在 [歡迎使用新增區域精靈] 中，按 [下一步]。

5.  在 [區域類型] 中，選取 [主要區域]。

6.  如果 DNS 伺服器是可寫入的網域控制站，請確定已選取 [將區域存放在 Active Directory 上]。 按一下 [下一步] 。

7.  在 [Active Directory 區域複寫領域] 中，除非您有特殊理由要選擇其他選項，否則請選取 [到這個網域中網域控制站執行的所有 DNS 伺服器]。 按一下 [下一步] 。

8.  在第一個 [反向對應區域名稱] 頁面中，選取 [IPv4 反向對應區域]。 按一下 [下一步] 。

9. 在第二個 [反向對應區域名稱] 頁面中，執行下列其中一項：

    - 在 [網路識別碼] 中，輸入您的 IP 位址範圍的網路識別碼。 例如，如果您的 IP 位址範圍是 10.0.0.1 到 10.0.0.254，請輸入 **10.0.0**。

    - 在 [反向對應區域名稱] 中，會自動新增您的 IPv4 反向對應區域名稱。 按一下 [下一步] 。

10. 在 [動態更新] 中，選取您所允許的動態更新類型。 按一下 [下一步] 。

11. 在 [完成新增區域精靈] 中，檢閱您所做的選擇，然後按 [完成]。

#### <a name="joining-server-computers-to-the-domain-and-logging-on"></a><a name="BKMK_joinlogserver"></a>將伺服器電腦加入網域並登入
在安裝 Active Directory 網域服務 (AD DS) 並建立一或多個有權將電腦加入網域的使用者帳戶之後，您可以將核心網路伺服器加入網域並登入伺服器，以便安裝其他技術，例如動態主機設定通訊協定 (DHCP)。

在您所部署的所有伺服器上，除了執行 AD DS 的伺服器之外，請執行下列各項：

1.  完成[設定所有伺服器](#BKMK_configuringAll)中提供的程序。

2.  使用後續兩個程序中的說明，將您的伺服器加入網域，並登入伺服器以執行其他部署工作：

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell 並輸入下列 Cmdlet，然後按 ENTER 鍵。 您也必須以您要使用的名稱取代網域名稱。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 提示您這樣做時，請為有權將電腦加入網域的帳戶輸入使用者名稱和密碼。 若要重新啟動電腦，請輸入下列命令，並按 ENTER 鍵：
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>將執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的電腦加入網域

1.  在 [伺服器管理員] 中，按一下 [本機伺服器]。 在詳細資料窗格中，按一下 [工作群組]。 [系統內容] 對話方塊隨即開啟。

2.  按一下 [系統內容] 對話方塊中的 [變更]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

3.  在 [電腦名稱] 的 [成員隸屬] 中按一下 [網域]，然後輸入您要加入的網域名稱。 例如，如果網域名稱是 corp.contoso.com，請輸入 **corp.contoso.com**。

4.  按一下 [確定]。 [Windows 安全性] 對話方塊隨即開啟。

5.  在 [電腦名稱/網域變更] 的 [使用者名稱] 中，輸入使用者名稱，在 [密碼] 中輸入密碼，然後按一下 **[確定]**。 此時會開啟 [電腦名稱/網域變更] 對話方塊，歡迎您使用此網域。 按一下 [確定]。

6.  [電腦名稱/網域變更] 對話方塊顯示的訊息指出您必須重新啟動電腦才能套用變更。 按一下 [確定]。

7.  在 [系統內容] 對話方塊的 [電腦名稱] 索引標籤上，按一下 [關閉]。 此時會開啟 [Microsoft Windows] 對話方塊，再度顯示指出您必須重新啟動電腦才能套用變更的訊息。 按一下 [立即重新啟動]。

> [!NOTE]
> 如需如何將執行其他 Microsoft 作業系統的電腦加入網域的相關資訊，請參閱 [附錄 C-將電腦加入網域](#BKMK_C)。

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>使用執行 Windows Server 2016 的電腦登入網域

1.  登出電腦，或重新啟動電腦。

2.  按 CTRL + ALT + DELETE。 此時會顯示登入畫面。

3.  在左下角按一下 [ **其他使用者**]。

4.  在 [ **使用者名稱**] 中，輸入您的使用者名稱。

5.  在 [密碼] 中，輸入網域密碼，然後按方向鍵或按 ENTER。

> [!NOTE]
> 如需如何使用執行其他 Microsoft 作業系統的電腦登入網域的相關資訊，請參閱 [附錄 D-登入網域](#BKMK_D)。

#### <a name="deploying-dhcp1"></a><a name="BKMK_deployDHCP01"></a>部署 DHCP1
在部署核心網路的這個元件時，您必須執行下列各項：

- 執行[設定所有伺服器](#BKMK_configuringAll)一節中的步驟。

- 執行[將伺服器電腦加入網域並登入](#BKMK_joinlogserver)一節中的步驟。

若要部署執行動態主機設定通訊協定 (DHCP) 伺服器角色的電腦 DHCP1，您必須依照下列順序完成這些步驟：

- [安裝動態主機設定通訊協定 (DHCP)](#BKMK_installDHCP)

- [建立和啟用新的 DHCP 領域](#BKMK_newscopeDHCP)

> [!NOTE]
> 若要使用 Windows PowerShell 執行這些程序，請開啟 PowerShell，將下列 Cmdlet 輸入不同行，然後按 ENTER 鍵。 您也必須以您要使用的值，取代這個範例中的領域名稱、IP 位址起始與結束範圍、子網路遮罩及其他值。
>
> `Install-WindowsFeature DHCP -IncludeManagementTools`
>
> `Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2`
>
> `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com`

##### <a name="install-dynamic-host-configuration-protocol-dhcp"></a><a name="BKMK_installDHCP"></a>安裝動態主機設定通訊協定 (DHCP)
您可以使用此程序，使用 [新增角色及功能精靈] 來安裝和設定 DHCP 伺服器角色。

若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

###### <a name="to-install-dhcp"></a>安裝 DHCP

1.  在 DHCP1 的 [伺服器管理員] 中，按一下 [管理]，然後按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即開啟。

2.  在 [在您開始前] 中，按 [下一步]。

    > [!NOTE]
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。

3.  在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。

4.  在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步] 。

5.  在 [ **選取伺服器角色**] 的 [ **角色**] 中，選取 [ **DHCP 伺服器**]。 在 [新增 DHCP 伺服器所需的功能] 中，按一下 [新增功能]。 按一下 [下一步] 。

6.  在 [選取功能] 中，按 [下一步]，然後在 [DHCP 伺服器]中，檢閱提供的資訊，接著按 [下一步]。

7.  在 [確認安裝選項] 中，按一下 [必要時自動重新啟動目的地伺服器]。 提示您確認這個選取項目時，按一下 [是]，然後按一下 [安裝]。 [ **安裝進度** ] 頁面會在安裝過程中顯示狀態。 當程式完成時，會顯示「需要設定」訊息。 *Computername* 上的 [安裝成功] 會顯示，其中 *ComputerName* 是您安裝 DHCP 伺服器的電腦名稱稱。 在訊息視窗中，按一下 [完成 DHCP 設定]。 [DHCP 後續安裝設定精靈] 隨即開啟。 按一下 [下一步] 。

8.  在 [授權] 中，指定您要在 Active Directory 網域服務中用來授權 DHCP 伺服器的認證，然後按一下 [認可]。 授權完成後，請按一下 [關閉]。

##### <a name="create-and-activate-a-new-dhcp-scope"></a><a name="BKMK_newscopeDHCP"></a>建立和啟用新的 DHCP 領域
您可以使用這個程序，使用 DHCP Microsoft Management Console (MMC) 來建立新的 DHCP 領域。 完成程序時，即會啟用該領域，而您所建立的排除範圍會防止 DHCP 伺服器租用您用來靜態設定需要靜態 IP 位址的伺服器和其他裝置的 IP 位址。

若要執行此程序，至少需要 DHCP 系統管理員的成員資格或同等權限。

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>建立和啟用新的 DHCP 領域

1.  在 DHCP1 的 [伺服器管理員] 中，按一下 [工具]，然後按一下 [DHCP]。 此時會開啟 DHCP MMC。

2.  在 **DHCP** 中，展開伺服器名稱。 例如，如果 DHCP 伺服器名稱是 DHCP1.corp.contoso.com，請按一下 [ **DHCP1.corp.contoso.com**] 旁的向下箭號。

3.  在 [伺服器名稱] 下方，以滑鼠右鍵按一下 [ **IPv4**]，然後按一下 [ **新增領域**]。 此時會開啟 [新增領域精靈]。

4.  在 [歡迎使用新增領域精靈] 中，按 [下一步]。

5.  在 [領域名稱] 的 [名稱] 中輸入領域的名稱。 例如，輸入「子網路 1」。

6.  在 [描述] 中輸入新領域的說明，然後按 [下一步]。

7.  在 [IP 位址範圍]，執行下列各項：

    1.  在 [起始 IP 位址] 中，輸入範圍中第一個 IP 位址的 IP 位址。 例如，輸入 **10.0.0.1**。

    2.  在 [結束 IP 位址] 中，輸入範圍中最後一個 IP 位址的 IP 位址。 例如，輸入 **10.0.0.254**。 根據您在 [起始 IP 位址] 輸入的 IP 位址，會自動輸入 [長度] 及 [子網路遮罩] 的值。

    3.  請視需要來修改 [長度] 或 [子網路遮罩] 的值，使其適於您的定址配置。

    4.  按一下 [下一步] 。

8.  在 [新增排除範圍]，執行下列各項：

    1.  在 [起始 IP 位址] 中，輸入排除範圍中第一個 IP 位址的 IP 位址。 例如，輸入 **10.0.0.1**。

    2.  在 [結束 IP 位址] 中，輸入排除範圍中最後一個 IP 位址的 IP 位址，例如輸入 **10.0.0.15**。

9. 按一下 [新增]，再按 [下一步]。

10. 在 [租用期間] 中，修改 [天]、[時] 以及 [分] 的預設值，使其適於您的網路，然後按 [下一步]。

11. 在 [設定 DHCP 選項] 中，選取 [是，我要現在設定這些選項]，然後按 [下一步]。

12. 在 [路由器 (預設閘道)] 中，執行下列其中一項：

    - 如果您的網路上沒有路由器，請按 [下一步]。

    - 在 [IP 位址] 中，輸入路由器或預設閘道的 IP 位址。 例如，輸入 **10.0.0.1**。 按一下 [新增]，再按 [下一步]。

13. 在 [網域名稱和 DNS 伺服器] 中，執行下列各項：

    1.  在 [父系網域] 中，輸入用戶端用於名稱解析的 DNS 網域的名稱。 例如，輸入 **corp.contoso.com**。

    2.  在 [伺服器名稱] 中，輸入用戶端用於名稱解析的 DNS 電腦的名稱。 例如，輸入 **DC1**。

    3.  按一下 [解析]。 DNS 伺服器的 IP 位址會加入 [IP 位址] 中。 按一下 [新增]，等待 DNS 伺服器 IP 位址驗證完成，然後按 [下一步]。

14. 因為您的網路上沒有 WINS 伺服器，所以請在 [WINS 伺服器] 中按 [下一步]。

15. 在 [啟用領域] 中，選取 [是，我要立即啟動這個領域]。

16. 按 [下一步]  ，然後按一下 [完成]  。

> [!IMPORTANT]
> 若要建立其他子網路的新領域，請重複執行這個程序。 針對您計畫部署的每個子網路使用不同的 IP 位址範圍，並確定已在引導至其他子網路的所有路由器上啟用 DHCP 訊息轉送。

### <a name="joining-client-computers-to-the-domain-and-logging-on"></a><a name="BKMK_joinlogclients"></a>將用戶端電腦加入網域並登入

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell 並輸入下列 Cmdlet，然後按 ENTER 鍵。 您也必須以您要使用的名稱取代網域名稱。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 提示您這樣做時，請為有權將電腦加入網域的帳戶輸入使用者名稱和密碼。 若要重新啟動電腦，請輸入下列命令，並按 ENTER 鍵：
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>將執行 Windows 10 的電腦加入網域

1.  使用本機 Administrator 帳戶登入電腦。

2.  在 **[搜尋 web 和 Windows**] 中輸入 **System**。 在 [搜尋結果] 中，按一下 [ **系統 (控制台])**。 [系統] 對話方塊隨即開啟。

3.  在 [ **系統**] 中，按一下 [ **Advanced system settings**]。 [系統內容] 對話方塊隨即開啟。 按一下 [ **電腦名稱稱** ] 索引標籤。

4.  在 [ **電腦名稱稱**] 中，按一下 [ **變更**]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

5.  在 [ **電腦名稱稱/網域變更** ] 的 [ **成員隸屬**] 中，按一下 [ **網域**]，然後輸入您要加入之網域的名稱。 例如，如果網域名稱是 corp.contoso.com，請輸入 **corp.contoso.com**。

6.  按一下 [確定]。 [Windows 安全性] 對話方塊隨即開啟。

7.  在 [電腦名稱/網域變更] 的 [使用者名稱] 中，輸入使用者名稱，在 [密碼] 中輸入密碼，然後按一下 **[確定]**。 此時會開啟 [電腦名稱/網域變更] 對話方塊，歡迎您使用此網域。 按一下 [確定]。

8.  [電腦名稱/網域變更] 對話方塊顯示的訊息指出您必須重新啟動電腦才能套用變更。 按一下 [確定]。

9. 在 [系統內容] 對話方塊的 [電腦名稱] 索引標籤上，按一下 [關閉]。 此時會開啟 [Microsoft Windows] 對話方塊，再度顯示指出您必須重新啟動電腦才能套用變更的訊息。 按一下 [立即重新啟動]。

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>將執行 Windows 8.1 的電腦加入網域

1.  使用本機 Administrator 帳戶登入電腦。

2.  以滑鼠右鍵按一下 [ **開始**]，然後按一下 [ **系統**]。 [系統] 對話方塊隨即開啟。

3.  在 [ **系統**] 中，按一下 [ **Advanced system settings**]。 [系統內容] 對話方塊隨即開啟。 按一下 [ **電腦名稱稱** ] 索引標籤。

4.  在 [ **電腦名稱稱**] 中，按一下 [ **變更**]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

5.  在 [ **電腦名稱稱/網域變更** ] 的 [ **成員隸屬**] 中，按一下 [ **網域**]，然後輸入您要加入之網域的名稱。 例如，如果網域名稱是 corp.contoso.com，請輸入 **corp.contoso.com**。

6.  按一下 [確定]。 [Windows 安全性] 對話方塊隨即開啟。

7.  在 [電腦名稱/網域變更] 的 [使用者名稱] 中，輸入使用者名稱，在 [密碼] 中輸入密碼，然後按一下 **[確定]**。 此時會開啟 [電腦名稱/網域變更] 對話方塊，歡迎您使用此網域。 按一下 [確定]。

8.  [電腦名稱/網域變更] 對話方塊顯示的訊息指出您必須重新啟動電腦才能套用變更。 按一下 [確定]。

9. 在 [系統內容] 對話方塊的 [電腦名稱] 索引標籤上，按一下 [關閉]。 此時會開啟 [Microsoft Windows] 對話方塊，再度顯示指出您必須重新啟動電腦才能套用變更的訊息。 按一下 [立即重新啟動]。

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>若要使用執行 Windows 10 的電腦登入網域

1.  登出電腦，或重新啟動電腦。

2.  按 CTRL + ALT + DELETE。 此時會顯示登入畫面。

3.  在左下方按一下 [ **其他使用者**]。

4.  在 [使用者名稱] 中，以 *網域\使用者* 格式輸入網域及使用者名稱。 例如，若要使用帳戶 **User-01** 登入網域 corp.contoso.com，請輸入 **CORP\User-01**。

5.  在 [密碼] 中，輸入網域密碼，然後按方向鍵或按 ENTER。

### <a name="deploying-optional-features-for-network-access-authentication-and-web-services"></a><a name="BKMK_optionalfeatures"></a>部署網路存取授權和 Web 服務的選用功能
如果您想要部署網路存取伺服器（例如無線存取點或 VPN 伺服器），在安裝核心網路之後，建議您同時部署 NPS 和 Web 服務器。 對於網路存取部署，建議使用安全憑證式驗證方法。 您可以使用 NPS 管理網路存取原則以及部署安全驗證方法。 您可以使用網頁伺服器發佈憑證授權單位 (CA) 的憑證撤銷清單 (CRL)，為安全驗證提供憑證。

> [!NOTE]
> 您可以使用核心網路附屬指南來部署伺服器憑證和其他額外功能。 如需詳細資訊，請參閱[其他技術資源](#BKMK_resources)。

下圖顯示已新增 NPS 和 Web 服務器的 Windows Server Core 網路拓撲。

![已新增 NPS 和 Web 服務器的 Windows Server 核心網路拓撲](../media/Core-Network-Guide/cng16_overview_2.jpg)

下列各節提供將 NPS 和網頁伺服器新增至網路的相關資訊。

- [部署 NPS1](#BKMK_deployNPS1)

- [部署 WEB1](#BKMK_IIS)

#### <a name="deploying-nps1"></a><a name="BKMK_deployNPS1"></a>部署 NPS1
安裝網路原則伺服器 (NPS) 伺服器是部署其他網路存取技術的準備步驟，例如虛擬私人網路 (VPN) 伺服器、無線存取點以及 802.1X 驗證交換器。

網路原則伺服器 (NPS) 可讓您使用下列功能集中設定和管理網路原則：遠端驗證撥入消費者服務 (RADIUS) 伺服器和 RADIUS proxy。

NPS 是核心網路的選用元件，但是如果下列任一項為真，您應該安裝 NPS：

- 您正在規劃擴充您的網路，使其包含與 RADIUS 通訊協定相容的遠端存取服務器，例如執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 以及路由及遠端存取服務、終端機服務閘道或遠端桌面閘道的電腦。


- 您打算針對有線或無線存取部署 802.1 X 驗證。

部署此角色服務之前，您必須在要設定為 NPS 的電腦上執行下列步驟。

- 執行[設定所有伺服器](#BKMK_configuringAll)一節中的步驟。

- 執行[將伺服器電腦加入網域並登入](#BKMK_joinlogserver)一節中的步驟

若要部署 NPS1 (執行 [網路原則與存取服務] 伺服器角色之 [網路原則伺服器 (NPS)] 角色服務的電腦)，您必須完成此步驟：

- [計畫 NPS1 的部署](#bkmk_NetFndtn_Pln_NPS-01)

- [安裝網路原則伺服器 (NPS)](#BKMK_installNPS)

- [在預設網域中註冊 NPS](#BKMK_registerNPS)

> [!NOTE]
> 本指南提供在獨立伺服器或名為 NPS1 的 VM 上部署 NPS 的指示。  另一個建議的部署模型是在網域控制站上安裝 NPS。 如果您想要在網域控制站上安裝 NPS，而不是在獨立伺服器上安裝 NPS，請在 DC1 上安裝 NPS。

##### <a name="planning-the-deployment-of-nps1"></a><a name="bkmk_NetFndtn_Pln_NPS-01"></a>計畫 NPS1 的部署
若您要部署網路存取伺服器，例如無線存取點或 VPN 伺服器，則在部署核心網路之後，建議您部署 NPS。

當您使用 NPS 當作遠端驗證撥入使用者服務 (RADIUS) 伺服器時，NPS 會透過網路存取伺服器來執行連線要求的驗證及授權作業。 NPS 也可以讓您集中設定和管理網路原則，以決定誰可以存取網路、它們如何存取網路、以及它們何時存取網路。

下面是安裝 NPS 之前的主要規劃步驟。

- 規劃使用者帳戶資料庫。 根據預設值，如果您將執行 NPS 的伺服器加入 Active Directory 網域，NPS 會使用 AD DS 使用者帳戶資料庫來執行驗證及授權作業。 在某些狀況下，例如使用 NPS 當作 RADIUS Proxy 將連線要求轉送到其他 RADIUS 伺服器的大型網路，您可能要在非網域成員電腦上安裝 NPS。

- 規劃 RADIUS 帳戶處理。 您可以使用 NPS 將帳戶資料記錄到本機電腦上的 SQL Server 資料庫或文字檔。 如果您要使用 SQL Server 記錄，請規劃執行 SQL Server 的伺服器的安裝與設定作業。

##### <a name="install-network-policy-server-nps"></a><a name="BKMK_installNPS"></a>安裝網路原則伺服器 (NPS)
您可以使用此程式，使用 [新增角色及功能] 嚮導，將網路原則伺服器安裝 (NPS) 。 NPS 是網路原則與存取服務伺服器角色的角色服務。

> [!NOTE]
> 根據預設值，NPS 會為所有已安裝的網路介面卡接聽連接埠 1812、1813、1645 和 1646 上的 RADIUS 流量。 如果您在安裝 NPS 時啟用 [具有 Advanced Security 的 Windows 防火牆]，則會在安裝程式期間，針對網際網路通訊協定第6版 \( IPv6 \) 和 IPv4 流量自動建立這些埠的防火牆例外。 如果您的網路存取伺服器設定為透過非預設的埠來傳送 RADIUS 流量，請移除 NPS 安裝期間在 [Windows 防火牆] 中建立的例外狀況，並為您要用於 RADIUS 流量的埠建立例外。

**系統管理認證**

您必須是 **Domain Admins** 群組的成員，才可以完成這個程序。

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell 並輸入下列內容，然後按 ENTER 鍵。
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>安裝 NPS

1.  在 NPS1 的 [伺服器管理員] 中，按一下 [管理]，然後按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即開啟。

2.  在 [在您開始前] 中，按 [下一步]。

    > [!NOTE]
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。

3.  在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。

4.  在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步] 。

5.  在 [ **選取伺服器角色**] 的 [ **角色**] 中，選取 [ **網路原則與存取服務**]。 對話方塊隨即開啟，詢問是否應新增網路原則和存取服務所需的功能。 按一下 [新增功能]  ，然後按 [下一步]  。

6.  在 [選取功能] 中，按 [下一步]，然後在 [網路原則與存取服務]中，檢閱提供的資訊，接著按 [下一步]。

7.  在 [選取角色服務] 中，按一下 [網路原則伺服器]。  在 [新增網路原則伺服器所需的功能] 中，按一下 [新增功能]。 按一下 [下一步] 。

8.  在 [確認安裝選項] 中，按一下 [必要時自動重新啟動目的地伺服器]。 提示您確認這個選取項目時，按一下 [是]，然後按一下 [安裝]。 [安裝進度] 頁面會在安裝程序期間顯示狀態。 當程式完成時，會顯示「 *computername* 上安裝成功」的訊息，其中 *ComputerName* 是您安裝網路原則伺服器的電腦名稱稱。 按一下 [關閉]  。

##### <a name="register-the-nps-in-the-default-domain"></a><a name="BKMK_registerNPS"></a>在預設網域中註冊 NPS
您可以使用此程式在伺服器是網域成員的網域中註冊 NPS。

NPSs 必須在 Active Directory 中註冊，才能在授權程式期間有權讀取使用者帳戶的撥入屬性。 註冊 NPS 會將伺服器新增至 Active Directory 中的 [ **RAS 和 資訊存取伺服器** ] 群組。

**系統管理認證**

您必須是 **Domain Admins** 群組的成員，才可以完成這個程序。

> [!NOTE]
> 若要使用網路介面 (Netsh) Windows PowerShell 內的命令來執行此程式，請開啟 PowerShell 並輸入下列命令，然後按 ENTER。
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-in-its-default-domain"></a>在其預設網域中註冊 NPS

1.  在 NPS1 的 [伺服器管理員] 中，按一下 [工具]，然後按一下 [網路原則伺服器]。 此時會開啟 [網路原則伺服器] MMC。

2.  在 [NPS (本機)] 上按一下滑鼠右鍵，然後按一下 [在 Active Directory 中登錄伺服器]。 此時會開啟 [網路原則伺服器] 對話方塊。

3.  在 [網路原則伺服器] 中，按一下 [確定]，再按一次 [確定]。

如需網路原則伺服器的詳細資訊，請參閱 [網路原則伺服器 (NPS) ](../technologies/nps/nps-top.md)。

#### <a name="deploying-web1"></a><a name="BKMK_IIS"></a>部署 WEB1

Windows Server 2016 中的 Web 服務器 (IIS) 角色，提供安全、容易管理、模組化且可擴充的平臺，可哥靠地裝載網站、服務和應用程式。 使用 Internet Information Services (IIS) ，您可以與網際網路、內部網路或外部網路上的使用者共用資訊。 IIS 是整合的 web 平臺，可將 IIS、ASP.NET、FTP 服務、PHP 和 Windows Communication Foundation (WCF) 。

除了允許您發佈 CRL 以供網域成員電腦存取之外，網頁伺服器 (IIS) 伺服器角色可讓您設定和管理多個網站、web 應用程式和 FTP 網站。 IIS 也提供下列優點：

- 透過減少伺服器使用量和自動化應用程式隔離來最大化網站安全性。

- 在相同的伺服器上輕鬆部署和執行 ASP.NET、傳統 ASP 和 PHP Web 應用程式。

- 藉由提供工作者處理序一個唯一識別碼以及預設的沙箱設定來達到應用程式隔離，進一步降低安全性風險。

- 使用符合客戶需求的自訂模組，輕鬆新增、移除或甚至取代內建的 IIS 元件。

- 透過內建的動態快取和增強型壓縮，提升您的網站速度。

若要部署 WEB1 (此為正在執行網頁伺服器 (IIS) 伺服器角色的電腦)，您必須執行下列各項：

- 執行[設定所有伺服器](#BKMK_configuringAll)一節中的步驟。

- 執行[將伺服器電腦加入網域並登入](#BKMK_joinlogserver)一節中的步驟

- [安裝 Web 伺服器 (IIS) 伺服器角色](#BKMK_install_IIS)

##### <a name="install-the-web-server-iis-server-role"></a><a name="BKMK_install_IIS"></a>安裝 Web 伺服器 (IIS) 伺服器角色
若要完成此程序，您必須是 **Administrators** 群組的成員。

> [!NOTE]
> 若要使用 Windows PowerShell 執行這個程序，請開啟 PowerShell 並輸入下列內容，然後按 ENTER 鍵。
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  在 [伺服器管理員] 中，按一下 [管理]，然後按一下 [新增角色及功能]。 [新增角色及功能精靈] 隨即開啟。

2.  在 [在您開始前] 中，按 [下一步]。

    > [!NOTE]
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。

3.  在 [ **選取安裝類型** ] 頁面上，按 **[下一步]**。

4.  在 [ **選取目的地伺服器** ] 頁面上，確定已選取 [本機電腦]，然後按 **[下一步]**。

5.  在 [ **選取伺服器角色** ] 頁面上，選取 [ **WEB 伺服器 (IIS)**。 [ **新增 Web 服務器 (IIS) 所需的功能** ] 對話方塊隨即開啟。 按一下 [新增功能]  ，然後按 [下一步]  。

6.  在您接受所有的預設網頁伺服器設定之後，按 [下一步]，然後按一下 [安裝]。

7.  確認所有安裝成功，然後按一下 [關閉]。

## <a name="additional-technical-resources"></a><a name="BKMK_resources"></a>其他技術資源
如需本指南中之技術的詳細資訊，請參閱下列資源：

 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 技術文件庫資源

- [Windows Server 2016 中 Active Directory Domain Services (AD DS) 的新功能](../../identity/whats-new-active-directory-domain-services.md)

- [Active Directory Domain Services 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831484(v=ws.11)) https://technet.microsoft.com/library/hh831484.aspx 。

- [網域名稱系統 (DNS) 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831667(v=ws.11)) https://technet.microsoft.com/library/hh831667.aspx 。

- [執行 DNS 管理員角色](/previous-versions/windows/it-pro/windows-server-2003/cc756152(v=ws.10))

- [ (DHCP) 總覽的動態主機設定通訊協定](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831825(v=ws.11)) https://technet.microsoft.com/library/hh831825.aspx 。

- [網路原則與存取服務](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831683(v=ws.11)) 的總覽 https://technet.microsoft.com/library/hh831683.aspx 。

- [Web 服務器 (IIS) 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831725(v=ws.11)) https://technet.microsoft.com/library/hh831725.aspx 。

## <a name="appendices-a-through-e"></a><a name="BKMK_appendix"></a>附錄 A 到 E
下列各節包含執行 Windows Server 2016、Windows 10、Windows Server 2012 和 Windows 8 以外作業系統之電腦的其他設定資訊。 此外，還提供網路準備工作表來協助您進行部署。

1.  [附錄 A - 將電腦重新命名](#BKMK_A)

2.  [附錄 B - 設定靜態 IP 位址](#BKMK_B)

3.  [附錄 C-將電腦加入網域](#BKMK_C)

4.  [附錄 D-登入網域](#BKMK_D)

5.  [附錄 E - 核心網路規劃準備表](#BKMK_E)

## <a name="appendix-a---renaming-computers"></a><a name="BKMK_A"></a>附錄 A - 將電腦重新命名
您可以使用本節中的程式，為執行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的電腦提供不同的電腦名稱稱。

- [Windows Server 2008 R2 與 Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

- [Windows Server 2008 和 Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="windows-server-2008-r2-and-windows-7"></a><a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 與 Windows 7
若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>重新命名執行 Windows Server 2008 R2 和 Windows 7 的電腦

1.  按一下 [開始]，在 [電腦] 上按一下滑鼠右鍵，然後按一下 [內容]。 [系統] 對話方塊隨即開啟。

2.  在 [電腦名稱、網域及工作群組設定] 中，按一下 [變更設定]。 [系統內容] 對話方塊隨即開啟。

    > [!NOTE]
    > 在執行 Windows 7 的電腦上，在 [ **系統** 內容] 對話方塊開啟之前，[ **使用者帳戶控制** ] 對話方塊隨即開啟，要求許可權才能繼續。 按一下 [繼續] 繼續。

3.  按一下 [變更]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

4.  在 [電腦名稱] 中，輸入電腦的名稱。 例如，如果想要將電腦命名為 DC1，請輸入 **DC1**。

5.  按兩次 [確定]，按一下 [關閉]，然後按一下 [立即重新啟動] 來重新啟動電腦。

### <a name="windows-server-2008-and-windows-vista"></a><a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 和 Windows Vista
若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>重新命名執行 Windows Server 2008 與 Windows Vista 的電腦

1.  按一下 [開始]，在 [電腦] 上按一下滑鼠右鍵，然後按一下 [內容]。 [系統] 對話方塊隨即開啟。

2.  在 [電腦名稱、網域及工作群組設定] 中，按一下 [變更設定]。 [系統內容] 對話方塊隨即開啟。

    > [!NOTE]
    > 在執行 Windows Vista 的電腦上，在 [ **系統** 內容] 對話方塊開啟之前，[ **使用者帳戶控制** ] 對話方塊隨即開啟，要求許可權才能繼續。 按一下 [繼續] 繼續。

3.  按一下 [變更]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

4.  在 [電腦名稱] 中，輸入電腦的名稱。 例如，如果想要將電腦命名為 DC1，請輸入 **DC1**。

5.  按兩次 [確定]，按一下 [關閉]，然後按一下 [立即重新啟動] 來重新啟動電腦。

## <a name="appendix-b---configuring-static-ip-addresses"></a><a name="BKMK_B"></a>附錄 B - 設定靜態 IP 位址
這個主題提供在執行下列作業系統的電腦上設定靜態 IP 位址的程序：

- [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

- [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="windows-server-2008-r2"></a><a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
若要執行此程序，至少需要 **Administrators** 的成員資格或同等權限。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>在執行 Windows Server 2008 R2 的電腦上設定靜態 IP 位址

1.  按一下 [開始]，然後按一下 [控制台]。

2.  在 [控制台] 中，按一下 [網路和網際網路]。 [網路和網際網路] 隨即開啟。

    在 [網路和網際網路] 中，按一下 [網路和共用中心]。 [網路和共用中心] 隨即開啟。

3.  在 [網路和共用中心] 中，按一下 [變更介面卡設定]。 [網路連線] 隨即開啟。

4.  在 [網路連線] 中，以滑鼠右鍵按一下您要設定的網路連線，然後按一下 [內容]。

5.  在 [本機區域連線內容] 的 [這個連線使用下列項目] 中，選取 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。 [網際網路通訊協定第 4 版 (TCP/IPv4) 內容] 對話方塊隨即開啟。

6.  在 [網際網路通訊協定第 4 版 (TCP/IPv4) 內容] 的 [一般] 索引標籤上，按一下 [使用下列的 IP 位址]。 在 [IP 位址] 中，輸入您要使用的 IP 位址。

7.  按下索引標籤，以便將游標放在 [子網路遮罩] 中。 系統會自動輸入子網路遮罩的預設值。 接受預設的子網路遮罩，或輸入您要使用的子網路遮罩。

8.  在 [預設閘道] 中，輸入預設閘道的 IP 位址。

9. 在 [慣用 DNS 伺服器] 中，輸入 DNS 伺服器的 IP 位址。 如果您計畫使用本機電腦當做慣用 DNS 伺服器，請輸入本機電腦的 IP 位址。

10. 在 [其他 DNS 伺服器] 中，輸入其他 DNS 伺服器的 IP 位址 (如果有的話)。 如果您計畫使用本機電腦當做其他 DNS 伺服器，請輸入本機電腦的 IP 位址。

11. 按一下 **[確定]** ，然後按一下 **[關閉]** 。

### <a name="windows-server-2008"></a><a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
若要執行這些程序，至少需要 **Administrators** 的成員資格或同等權限。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>在執行 Windows Server 2008 的電腦上設定靜態 IP 位址

1.  按一下 [開始]，然後按一下 [控制台]。

2.  在 [控制台] 中，確認選取 [傳統檢視]，然後按兩下 [網路和共用中心]。

3.  在 [網路和共用中心] 的 [工作] 中，按一下 [管理網路連線]。

4.  在 [網路連線] 中，以滑鼠右鍵按一下您要設定的網路連線，然後按一下 [內容]。

5.  在 [本機區域連線內容] 的 [這個連線使用下列項目] 中，選取 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。 [網際網路通訊協定第 4 版 (TCP/IPv4) 內容] 對話方塊隨即開啟。

6.  在 [網際網路通訊協定第 4 版 (TCP/IPv4) 內容] 的 [一般] 索引標籤上，按一下 [使用下列的 IP 位址]。 在 [IP 位址] 中，輸入您要使用的 IP 位址。

7.  按下索引標籤，以便將游標放在 [子網路遮罩] 中。 系統會自動輸入子網路遮罩的預設值。 接受預設的子網路遮罩，或輸入您要使用的子網路遮罩。

8.  在 [預設閘道] 中，輸入預設閘道的 IP 位址。

9. 在 [慣用 DNS 伺服器] 中，輸入 DNS 伺服器的 IP 位址。 如果您計畫使用本機電腦當做慣用 DNS 伺服器，請輸入本機電腦的 IP 位址。

10. 在 [其他 DNS 伺服器] 中，輸入其他 DNS 伺服器的 IP 位址 (如果有的話)。 如果您計畫使用本機電腦當做其他 DNS 伺服器，請輸入本機電腦的 IP 位址。

11. 按一下 **[確定]** ，然後按一下 **[關閉]** 。

## <a name="appendix-c---joining-computers-to-the-domain"></a><a name="BKMK_C"></a>附錄 C-將電腦加入網域
您可以使用這些程式，將執行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的電腦加入網域。

- [Windows Server 2008 R2 與 Windows 7](#BKMK_c1)

- [Windows Server 2008 和 Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> 若要將電腦加入網域，您必須使用本機 Administrator 帳戶登入電腦；或者如果您使用沒有本機電腦系統管理認證的使用者帳戶登入電腦，就必須在將電腦加入網域的處理作業期間，提供本機 Administrator 帳戶的認證。 此外，您必須在電腦要加入的網域中擁有使用者帳戶。 在將電腦加入網域的處理作業期間，將提示您輸入網域帳戶認證 (使用者名稱與密碼)。

### <a name="windows-server-2008-r2-and-windows-7"></a><a name="BKMK_c1"></a>Windows Server 2008 R2 與 Windows 7
若要執行此程序，至少需要 **Domain Users** 的成員資格或同等權限。

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>將執行 Windows Server 2008 R2 與 Windows 7 的電腦加入網域

1.  使用本機 Administrator 帳戶登入電腦。

2.  按一下 [開始]，在 [電腦] 上按一下滑鼠右鍵，然後按一下 [內容]。 [系統] 對話方塊隨即開啟。

3.  在 [電腦名稱、網域及工作群組設定] 中，按一下 [變更設定]。 [系統內容] 對話方塊隨即開啟。

    > [!NOTE]
    > 在執行 Windows 7 的電腦上，在 [ **系統** 內容] 對話方塊開啟之前，[ **使用者帳戶控制** ] 對話方塊隨即開啟，要求許可權才能繼續。 按一下 [繼續] 繼續。

4.  按一下 [變更]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

5.  在 [電腦名稱] 的 [成員隸屬] 中選取 [網域]，然後輸入您要加入的網域名稱。 例如，如果網域名稱是 corp.contoso.com，請輸入 **corp.contoso.com**。

6.  按一下 [確定]。 [Windows 安全性] 對話方塊隨即開啟。

7.  在 [電腦名稱/網域變更] 的 [使用者名稱] 中，輸入使用者名稱，在 [密碼] 中輸入密碼，然後按一下 **[確定]**。 此時會開啟 [電腦名稱/網域變更] 對話方塊，歡迎您使用此網域。 按一下 [確定]。

8.  [電腦名稱/網域變更] 對話方塊顯示的訊息指出您必須重新啟動電腦才能套用變更。 按一下 [確定]。

9. 在 [系統內容] 對話方塊的 [電腦名稱] 索引標籤上，按一下 [關閉]。 此時會開啟 [Microsoft Windows] 對話方塊，再度顯示指出您必須重新啟動電腦才能套用變更的訊息。 按一下 [立即重新啟動]。

### <a name="windows-server-2008-and-windows-vista"></a><a name="BKMK_c2"></a>Windows Server 2008 和 Windows Vista
若要執行此程序，至少需要 **Domain Users** 的成員資格或同等權限。

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>將執行 Windows Server 2008 與 Windows Vista 的電腦加入網域

1.  使用本機 Administrator 帳戶登入電腦。

2.  按一下 [開始]，在 [電腦] 上按一下滑鼠右鍵，然後按一下 [內容]。 [系統] 對話方塊隨即開啟。

3.  在 [電腦名稱、網域及工作群組設定] 中，按一下 [變更設定]。 [系統內容] 對話方塊隨即開啟。

4.  按一下 [變更]。 [電腦名稱/網域變更] 對話方塊隨即開啟。

5.  在 [電腦名稱] 的 [成員隸屬] 中選取 [網域]，然後輸入您要加入的網域名稱。 例如，如果網域名稱是 corp.contoso.com，請輸入 **corp.contoso.com**。

6.  按一下 [確定]。 [Windows 安全性] 對話方塊隨即開啟。

7.  在 [電腦名稱/網域變更] 的 [使用者名稱] 中，輸入使用者名稱，在 [密碼] 中輸入密碼，然後按一下 **[確定]**。 此時會開啟 [電腦名稱/網域變更] 對話方塊，歡迎您使用此網域。 按一下 [確定]。

8.  [電腦名稱/網域變更] 對話方塊顯示的訊息指出您必須重新啟動電腦才能套用變更。 按一下 [確定]。

9. 在 [系統內容] 對話方塊的 [電腦名稱] 索引標籤上，按一下 [關閉]。 此時會開啟 [Microsoft Windows] 對話方塊，再度顯示指出您必須重新啟動電腦才能套用變更的訊息。 按一下 [立即重新啟動]。

## <a name="appendix-d---log-on-to-the-domain"></a><a name="BKMK_D"></a>附錄 D-登入網域
您可以使用這些程式，使用執行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的電腦登入網域。

- [Windows Server 2008 R2 與 Windows 7](#BKMK_d1)

- [Windows Server 2008 和 Windows Vista](#BKMK_d2)

### <a name="windows-server-2008-r2-and-windows-7"></a><a name="BKMK_d1"></a>Windows Server 2008 R2 與 Windows 7
若要執行此程序，至少需要 **Domain Users** 的成員資格或同等權限。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>使用執行 Windows Server 2008 R2 及 Windows 7 的電腦登入網域

1.  登出電腦，或重新啟動電腦。

2.  按 CTRL + ALT + DELETE。 此時會顯示登入畫面。

3.  按一下 [切換使用者]，然後按 [其他使用者]。

4.  在 [使用者名稱] 中，以 *網域\使用者* 格式輸入網域及使用者名稱。 例如，若要使用帳戶 **User-01** 登入網域 corp.contoso.com，請輸入 **CORP\User-01**。

5.  在 [密碼] 中，輸入網域密碼，然後按方向鍵或按 ENTER。

### <a name="windows-server-2008-and-windows-vista"></a><a name="BKMK_d2"></a>Windows Server 2008 和 Windows Vista
若要執行此程序，至少需要 **Domain Users** 的成員資格或同等權限。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>使用執行 Windows Server 2008 與 Windows Vista 的電腦登入網域

1.  登出電腦，或重新啟動電腦。

2.  按 CTRL + ALT + DELETE。 此時會顯示登入畫面。

3.  按一下 [切換使用者]，然後按 [其他使用者]。

4.  在 [使用者名稱] 中，以 *網域\使用者* 格式輸入網域及使用者名稱。 例如，若要使用帳戶 **User-01** 登入網域 corp.contoso.com，請輸入 **CORP\User-01**。

5.  在 [密碼] 中，輸入網域密碼，然後按方向鍵或按 ENTER。

## <a name="appendix-e---core-network-planning-preparation-sheet"></a><a name="BKMK_E"></a>附錄 E - 核心網路規劃準備表
您可以使用此「網路規劃準備表」來收集安裝核心網路所需的資訊。 這個主題提供的表格包含安裝或設定程序期間，您必須提供資訊或特定值之每部伺服器電腦的個別設定項目。 每個設定項目都有提供範例值。

基於規劃和追蹤目的，每個表格中都為您提供空格，讓您輸入部署所用的值。 若您在這些表格中記錄安全性相關的值，就應該將此資訊存放在安全的位置。

下列連結將引導至這個主題的章節，以提供本指南所述部署程序相關的設定項目及範例值。

1.  [安裝 Active Directory 網域服務與 DNS](#BKMK_FndtnPrep_InstallAD)

    - [設定 DNS 反向對應區域](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [安裝 DHCP](#BKMK_FndtnPrep_InstallDHCP)

    - [在 DHCP 中建立排除範圍](#BKMK_FndtnPrep_DHCP_Exclusn)

    - [建立新的 DHCP 領域](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [安裝網路原則伺服器 (選擇性)](#BKMK_FndtnPrep_InstallNPS)

### <a name="installing-active-directory-domain-services-and-dns"></a><a name="BKMK_FndtnPrep_InstallAD"></a>安裝 Active Directory 網域服務與 DNS
本節的表格列出預先安裝與安裝 Active Directory 網域服務 (AD DS) 及 DNS 時的設定項目。

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>AD DS 與 DNS 的預先安裝設定項目
以下表格列出預先安裝的設定項目，如[設定所有伺服器](#BKMK_configuringAll)所述：

- [設定靜態 IP 位址](#BKMK_ip)

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|IP 位址|10.0.0.2||
|子網路遮罩|255.255.255.0||
|預設閘道|10.0.0.1||
|慣用 DNS 伺服器|127.0.0.1||
|其他 DNS 伺服器|10.0.0.15||

- [重新命名電腦](#BKMK_rename)

|設定項目|範例值|值|
|----------------------|-----------------|---------|
|電腦名稱|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>AD DS 與 DNS 安裝設定項目
Windows Server 核心網路部署程序[安裝新樹系的 AD DS 與 DNS](#BKMK_installAD-DNS) 的設定項目：

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|完整 DNS 名稱|corp.contoso.com||
|樹系功能等級|Windows Server 2003||
|Active Directory 網域服務資料庫資料夾位置|E:\Configuration\\<p>或接受預設位置。||
|Active Directory 網域服務記錄檔資料夾位置|E:\Configuration\\<p>或接受預設位置。||
|Active Directory 網域服務 SYSVOL 資料夾位置|E:\Configuration\\<p>或接受預設位置||
|目錄還原模式系統管理員密碼|J*p2leO4$F||
|回應檔案名稱 (選擇性)|AD DS_AnswerFile||

#### <a name="configuring-a-dns-reverse-lookup-zone"></a><a name="BKMK_FndtnPrep_DNSRevrsLook"></a>設定 DNS 反向對應區域

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|區域類型：|-主要區域<br />-次要區域<br />-存根區域||
|區域類型<p>**將區域存放在 Active Directory 上**|-已選取<br />-未選取||
|Active Directory 區域複寫領域|-至此樹系中的所有 DNS 伺服器<br />-到此網域中的所有 DNS 伺服器<br />-到此網域中的所有網域控制站<br />-到此目錄磁碟分割範圍中指定的所有網域控制站||
|反向對應區域名稱<p>(IP 類型)|-IPv4 反向對應區域<br />-IPv6 反向對應區域||
|反向對應區域名稱<p>(網路識別碼)|10.0.0||

### <a name="installing-dhcp"></a><a name="BKMK_FndtnPrep_InstallDHCP"></a>安裝 DHCP
本節中的表格列出預先安裝及安裝 DHCP 的設定項目。

##### <a name="pre-installation-configuration-items-for-dhcp"></a>DHCP 的預先安裝設定項目
以下表格列出預先安裝的設定項目，如[設定所有伺服器](#BKMK_configuringAll)所述：

- [設定靜態 IP 位址](#BKMK_ip)

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|IP 位址|10.0.0.3||
|子網路遮罩|255.255.255.0||
|預設閘道|10.0.0.1||
|慣用 DNS 伺服器|10.0.0.2||
|其他 DNS 伺服器|10.0.0.15||

- [重新命名電腦](#BKMK_rename)

|設定項目|範例值|值|
|----------------------|-----------------|---------|
|電腦名稱|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>DHCP 安裝設定項目
Windows Server 核心網路部署程序[安裝動態主機設定通訊協定 (DHCP)](#BKMK_installDHCP) 的設定項目：

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|網路連線繫結|乙太網路||
|DNS 伺服器設定|DC1||
|慣用 DNS 伺服器 IP 位址|10.0.0.2||
|其他 DNS 伺服器 IP 位址|10.0.0.15||
|範圍名稱|Corp1||
|起始 IP 位址|10.0.0.1||
|結束 IP 位址|10.0.0.254||
|子網路遮罩|255.255.255.0||
|預設閘道 (選擇性)|10.0.0.1||
|租用期間|8 天||
|IPv6 DHCP 伺服器操作模式|未啟用||

#### <a name="creating-an-exclusion-range-in-dhcp"></a><a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>在 DHCP 中建立排除範圍
在 DHCP 中建立領域時，建立排除範圍的設定項目。

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|範圍名稱|Corp1||
|領域說明|總公司子網路 1||
|排除範圍起始 IP 位址|10.0.0.1||
|排除範圍結束 IP 位址|10.0.0.15||

#### <a name="creating-a-new-dhcp-scope"></a><a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>建立新的 DHCP 領域
Windows Server 核心網路部署程序[建立和啟用新的 DHCP 領域](#BKMK_newscopeDHCP)的設定項目：

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|新領域名稱|Corp2||
|領域說明|總公司子網2||
|(IP 位址範圍)<p>起始 IP 位址|10.0.1.1||
|(IP 位址範圍)<p>結束 IP 位址|10.0.1.254||
|長度|8||
|子網路遮罩|255.255.255.0||
|(排除範圍) 起始 IP 位址|10.0.1.1||
|排除範圍結束 IP 位址|10.0.1.15||
|租用期間<p>天<p>小時<p>分鐘|-8<br />-0<br />-0||
|路由器 (預設閘道)<p>IP 位址|10.0.1.1||
|DNS 父系網域|corp.contoso.com||
|DNS 伺服器<p>IP 位址|10.0.0.2||

### <a name="installing-network-policy-server-optional"></a><a name="BKMK_FndtnPrep_InstallNPS"></a>安裝網路原則伺服器 (選擇性)
本節中的表格列出預先安裝及安裝 NPS 的設定項目。

##### <a name="pre-installation-configuration-items"></a>預先安裝設定項目
以下三個表格列出預先安裝的設定項目，如[設定所有伺服器](#BKMK_configuringAll)所述：

- [設定靜態 IP 位址](#BKMK_ip)

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|IP 位址|10.0.0.4||
|子網路遮罩|255.255.255.0||
|預設閘道|10.0.0.1||
|慣用 DNS 伺服器|10.0.0.2||
|其他 DNS 伺服器|10.0.0.15||

- [重新命名電腦](#BKMK_rename)

|設定項目|範例值|值|
|----------------------|-----------------|---------|
|電腦名稱|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>網路原則伺服器安裝設定項目
Windows Server 核心網路 NPS 部署程式的設定專案會將 [網路原則伺服器安裝 (NPS) ](#BKMK_installNPS) 並 [在預設網域中註冊 nps](#BKMK_registerNPS)。

- 不需要其他設定項目即可安裝及登錄 NPS。
