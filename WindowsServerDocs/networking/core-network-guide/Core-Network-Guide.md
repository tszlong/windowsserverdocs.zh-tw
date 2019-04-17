---
title: 核心網路指南
description: 本指南計劃及部署所需的正常運作的網路和新中新的 Windows Server 2016 的樹系的 Active Directory 網域核心元件的方式提供指示
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90390a7b975bc358fb96715d23fe542bc261220e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide"></a>核心網路指南

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本指南計劃和部署所需的正常運作的網路，並在新的樹系新 Active Directory domain 核心元件的方式指示。

> [!NOTE]
> 本指南已可供下載 TechNet 主題館從 Microsoft Word 格式。 如需詳細資訊，請查看[核心網路指南適用於 Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

本指南包含下列各節。

- [有關本指南](#BKMK_about)

- [核心網路概觀](#BKMK_overview)

- [核心網路計劃](#BKMK_planning)

- [核心網路部署](#BKMK_deployment)

- [其他技術資源](#BKMK_resources)

- [透過電子附錄 A](#BKMK_appendix)

## <a name="BKMK_about"></a>有關本指南
本指南適用於安裝新的網路，或想要建立網域型更換網路所組成工作群組網路網路和系統管理員。 本指南提供部署是非常有用，如果您認為需要未來將更多服務及功能新增至您的網路。

您檢視的設計，以及部署引導在本案例中部署用來協助您判斷這個指南提供服務和設定，您需要技術的建議。

核心網路網路硬體、裝置的收藏，軟體提供的基本服務，您組織的資訊技術 (IT)，必須。

Windows Server core 網路為您提供許多好處，其中包括下列。

-   適用於電腦和其他傳輸控制項通訊協定日網際網路通訊協定 (TCP/IP) 的相容裝置之間網路連接核心通訊協定。 TCP/IP 是一套連接電腦以及建立網路標準通訊協定。 TCP/IP 是網路通訊協定的軟體提供的 Microsoft Windows 作業系統，實作支援 TCP/IP 通訊協定。

-   動態主機設定通訊協定」(DHCP) 自動 IP 位址指派的電腦和其他裝置設定為 DHCP 戶端。 手動所有網路上的電腦上的 IP 位址設定，而且費時彈性不如動態提供電腦與其他裝置使用 DHCP 伺服器的 IP 位址設定。

-   網域名稱系統」(DNS) 的名稱解析服務。 DNS 可讓使用者、電腦、應用程式與服務使用網域全名的電腦或裝置的網路上尋找電腦和裝置的 IP 位址。

-   森林，這是一或多個 Active Directory 網域分享相同課程和屬性定義（結構描述）、網站及複寫資訊（設定）和樹系的搜尋功能（通用）。

-   建立新的樹系森林根網域，也就是第一個的網域。 企業系統管理員和架構管理員群組的樹系管理群組，這位於森林根網域中。 此外，森林根網域，與其他網域，是電腦、使用者和群組物件 Active Directory Domain Services (AD DS) 中的系統管理員所定義的收藏。 這些物件共用常見 directory 資料庫和安全性原則。 它們也可以共用與其他網域安全關係，如果您為您的組織成長加入網域。 Directory 服務也會儲存 directory 資料，並允許授權的電腦、應用程式和使用者資料的存取。

-   使用者和電腦 account 資料庫。 Directory 服務提供可讓您的連絡人和電腦連接到您的網路及存取網路資源，例如應用程式、資料庫，共用的檔案和資料夾和印表機授權建立使用者和電腦帳號中央的使用者帳號資料庫。

核心網路也可讓您以調整您的網路，您組織的成長，以及變更 IT 需求。 例如，核心網路的問題，您可以新增網域、的 IP 子網路、遠端存取服務、wireless 服務，及其他功能及提供 Windows Server 2016 伺服器角色。

### <a name="network-hardware-requirements"></a>網路的硬體需求
若要成功部署核心網路，您必須部署網路的硬體，包括：

-   乙太網路、（fast ring）乙太網路，或纜 Gb 乙太網路

-   中心、層級 2 或 3 切換、路由器或其他裝置執行的轉送電腦與裝置間網路流量的功能。

-   符合最低的硬體需求各自 client 和 server 作業系統的電腦。

## <a name="what-this-guide-does-not-provide"></a>未提供哪些本指南
本指南不提供下列部署的指示：

-   網路的硬體，例如纜、路由器、參數和 hub

-   其他網路資源，例如印表機和檔案伺服器

-   連接網際網路

-   遠端存取

-   Wireless 存取

-   Client 電腦部署

> [!NOTE]
> 電腦執行的 Windows client 作業系統接收 DHCP 伺服器的 IP 位址租用預設設定。 因此，不其他 DHCP」或「網際網路通訊協定第 4 版 (IPv4) 就需要 client 電腦的設定。

## <a name="technology-overviews"></a>技術概觀
下列章節提供所需的技術部署建立核心網路簡短的概觀。

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
Directory 是階層結構網路，例如使用者和電腦上儲存物件的相關資訊。 Directory 服務，例如 AD DS，提供適用於儲存 directory 資料，並讓網路使用者和系統管理員可以使用此資料的方法。 例如，AD DS 儲存帳號，包括名稱、電子郵件地址、密碼，以及電話數字的相關資訊，並讓其他授權的使用者在相同網路存取此資訊。

### <a name="dns"></a>DNS
DNS 是 TCP/IP 網路，例如網際網路或組織網路的名稱解析通訊協定。 DNS 伺服器主控讓電腦 client 的資訊和服務解析容易辨識，英數 DNS 名稱彼此使用電腦的 IP 位址。

### <a name="dhcp"></a>DHCP
DHCP 是標準簡化主機 IP 設定的管理的 IP。 標準 DHCP 提供使用 DHCP 伺服器管理 DHCP 式戶端，您網路上的 IP 位址動態配置及其他設定的相關詳細資料的方式。

DHCP 可讓您使用 DHCP 伺服器動態指派的電腦或其他裝置，例如印表機、您區域網路上的 IP 位址。 每個 TCP/IP 網路上的電腦必須唯一的 IP 位址，因為的 IP 位址，其相關子網路遮罩找出主機電腦和電腦連接的子網路。 使用 DHCP，您可以確保所有電腦設定為 DHCP 戶端都獲得適當的網路位置的子網路的 IP 位址，使用 DHCP 選項，例如預設閘道和 DNS 伺服器，您可以自動提供 DHCP 戶端正確運作，您網路上所需的資訊。

TCP 型網路，它可以減少參與重新設定電腦的系統管理工作量與複雜。

### <a name="tcpip"></a>TCP/IP
以下是在 Windows Server 2016 TCP/IP:

-   網路根據業界標準網路通訊協定的軟體。

-   路由企業網路通訊協定支援 windows 電腦的區域網路（區域網路）和寬區域 (WAN) 環境連接。

-   核心技術與公共事業適用於 windows 的電腦連接的不同系統，以分享的資訊。

-   適用於通用網際網路服務，例如全球檔案傳輸通訊協定（檔案）伺服器存取基本知識。

-   穩定，延展性跨平台，client 日伺服器架構。

TCP/IP 提供基本 TCP/IP 公用程式，可讓 Windows 電腦連接並分享的資訊和其他 Microsoft、非 Microsoft 系統，包括：

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   網際網路主機

-   蘋果 Macintosh 系統

-   大型 IBM 主機

-   UNIX 和 Linux 系統

-   開放 VM 系統

-   網路準備印表機

-   平板電腦和行動電話有線乙太網路或 wireless 802.11 的技術支援

## <a name="BKMK_overview"></a>核心網路概觀
下圖顯示 Windows Server Core 網路拓撲。

![Windows Server Core 網路拓撲](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> 本指南也包含針對安全的網路存取方案，例如 802.1 X 有線和 wireless 部署，可以使用核心網路小幫手指南實作提供基本知識選用的網路原則 Server (NPS) 與 Web 伺服器 (IIS) 伺服器加入您的網路拓撲指示。 如需詳細資訊，請查看[部署網路存取驗證和 Web 服務為選擇性功能](#BKMK_optionalfeatures)。

### <a name="core-network-components"></a>核心網路元件
以下是核心網路的元件。

##### <a name="router"></a>路由器
本部署指南核心網路部署具有兩個子網路分隔路由器 DHCP 轉送功能的指示操作。 您可以但是，部署層級 2 切換、層級 3 切換控制或中樞」，根據您的需求和資源。 如果您要部署的參數，必須能 DHCP 轉接開關切換至或您必須在每個子網路放置 DHCP 伺服器。 如果您要部署的中心，您要部署的單一子網路，不需要 DHCP 轉接或第二個範圍 DHCP 伺服器上。

##### <a name="static-tcpip-configurations"></a>靜態 TCP/IP 設定
在這個部署伺服器靜態 IPv4 位址設定。 Client 電腦接收 DHCP 伺服器的 IP 位址租用預設設定。

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Active Directory Domain Services 通用和 DNS 伺服器 DC1
同時 Active Directory Domain Services (AD DS) 網域名稱系統」(DNS) 名 DC1，提供 directory，此伺服器上安裝並名稱解析服務到所有電腦及網路上的裝置。

##### <a name="dhcp-server-dhcp1"></a>DHCP 伺服器 DHCP1
DHCP 伺服器，名 DHCP1，提供網際網路通訊協定」(IP) 位址租用電腦網路的範圍的設定。 DHCP 伺服器也可以設定的其他領域提供其他子網路到電腦的 IP 位址租用如果 DHCP 轉接在路由器設定。

##### <a name="client-computers"></a>Client 電腦
執行 Windows client 作業系統的電腦設定預設為 DHCP 戶端，從伺服器 DHCP 自動取得 IP 位址和 DHCP 選項。

## <a name="BKMK_planning"></a>核心網路計劃
部署核心網路之前，您必須計劃下列項目。

-   [規劃子網路](#bkmk_NetFndtn_Pln_Subnt)

-   [規劃基本所有伺服器設定](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [規劃 DC1 部署](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [規劃網域存取](#bkmk_NetFndtn_Pln_DomAccess)

-   [規劃 DHCP1 部署](#bkmk_NetFndtn_Pln_DHCP-01)

下列章節這些項目提供更多詳細資料。

> [!NOTE]
> 如需協助規劃部署，也會看到[附錄 E-核心網路規劃準備表](#BKMK_E)。

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>規劃子網路
網路傳輸控制項通訊協定日網際網路通訊協定 (TCP/IP)，在路由器用來連接硬體和使用不同的實體網路區段稱為子網路上的軟體。 路由器也會用來轉送 IP 子網路中的每個之間的封包。 判斷您的網路，包括數目路由器，您需要本文中的指示進行之前，先子網路中的實體配置。

此外，您必須設定伺服器您網路上的靜態 IP 位址，以判斷您想要核心網路伺服器的所在位置的子網路使用的 IP 位址範圍。 本指南，私人 IP 位址範圍 10.0.0.1-10.0.0.254 和 10.0.1.1-10.0.1.254 做為範例，但您可以使用您想要任何私人 IP 位址。

> [!IMPORTANT]
> 選取您想要使用的每個子網路的 IP 位址範圍之後，請確定您設定您的路由器，作為子網路上使用路由器安裝所在的相同的 IP 位址範圍的 IP 位址。 例如是否使用之 IP 位址 192.168.1.1，預設設定您的路由器，但您要安裝路由器上的 IP 位址各種不同的 10.0.0.0 月 24 子網路，必須重新設定要使用 IP 位址 10.0.0.0 24 IP 位址範圍從路由器。

下列辨識範圍所指定的網際網路要求建議 (RFC) 1918 年私人 IP 位址：

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

當您使用的私人 IP 位址範圍 RFC 1918 中所指定時，您無法直接連接到網際網路，因為要求移或這些位址，會自動捨棄網際網路服務提供者 (ISP) 路由器使用私人 IP 位址。 若要新增網際網路連接到您的網路核心之後，您必須合約 isp 取得公用 IP 位址。

> [!IMPORTANT]
> 使用私人 IP 位址，您必須使用某些類型的 proxy 伺服器或網路位址轉譯 (NAT) 伺服器来轉換的私人 IP 位址範圍公用 IP 位址，可以在網際網路上傳送到您區域網路上。 大部分路由器提供 NAT 服務，因此選取路由器 NAT 能力的應該非常簡單。

如需詳細資訊，請查看[規劃 DHCP1 部署](#bkmk_NetFndtn_Pln_DHCP-01)。

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>規劃基本所有伺服器設定
每個伺服器的核心網路，您必須重新命名電腦和指派和設定靜態 IPv4 位址和其他 TCP/IP 屬性的電腦。

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>規劃命名規格適用於電腦和裝置
在您的網路上的一致性，最好使用一致伺服器、印表機及其他裝置的名稱。 電腦名稱可用於協助使用者和系統管理員輕鬆地找出用途和伺服器、印表機或其他裝置的位置。 例如，如果您三個 DNS 伺服器、一個金山、洛杉磯，其中一個月在芝加哥，您可能會使用命名規格*伺服器功能*-*位置*-*號碼*:

-   DNS 房間 01。 此名稱代表丹佛，科羅拉多州的 DNS 伺服器。 如果其他 DNS 伺服器] 會新增丹佛，在名稱數值可以增加，如下所示 DNS-房間 02 和 DNS-房間 03。

-   DNS SPAS 01。 此名稱代表南方 Pasadena，加州 DNS 伺服器。

-   DNS ORL 01。 此名稱代表日，其中的 DNS 伺服器。

本指南伺服器命名規格非常簡單，以及包含主要伺服器功能和數字。 例如網域控制站為 DC1]，DHCP 伺服器命名 DHCP1。

建議您選擇命名規範，才能安裝核心網路使用本指南。

#### <a name="planning-static-ip-addresses"></a>規劃靜態 IP 位址
您必須先靜態 IP 位址設定每一部電腦，計劃您子網路和 IP 位址範圍。 此外，您必須判斷您的 DNS 伺服器的 IP 位址。 如果您要安裝提供其他網路，例如其他子網路或網際網路存取權的路由器，您必須知道路由器，也靜態 IP 位址設定中稱為 [預設閘道 IP 的位址。

下表範例值提供靜態 IP 位址設定。

|設定項目|範例值|
|-----------------------|------------------|
|IP 位址|10.0.0.2|
|子網路遮罩|255.255.255.0|
|預設閘道器（路由器 IP 位址）|10.0.0.1|
|慣用的 DNS 伺服器|10.0.0.2|

> [!NOTE]
> 如果您打算部署一部以上的 DNS 伺服器，您也可以計劃的其他 DNS 伺服器的 IP 位址。

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>規劃 DC1 部署
以下是金鑰規劃步驟之前，請先安裝 Active Directory Domain Services (AD DS) 和 DNS DC1 上的。

#### <a name="planning-the-name-of-the-forest-root-domain"></a>規劃樹系根網域的名稱
判斷您的組織需要幾個樹系為 AD DS 設計程序的第一個步驟。 樹系的最上層 AD DS 容器，並一或多個網域分享的常見架構和通用所組成。 組織可以有多個，但最組織的單一森林設計慣用的型號和管理最簡單。

當您在組織中建立網域控制站第一次時，您所建立的第一個網域（也稱為森林根網域）和第一次樹系。 您需要此動作，使用此快速入門之前，但是，您必須判斷您的組織的最佳網域名稱。 在大部分案例中，組織名稱是網域名稱，且通常係這個網域名稱。 如果您打算部署外部面向網際網路網頁伺服器提供資訊和服務您針對或協力廠商、選擇網域名稱已無法使用，並使您的組織擁有它，然後登記的網域名稱。

#### <a name="planning-the-forest-functional-level"></a>規劃網域功能等級
您必須安裝 AD DS，來選擇您想要使用的樹系功能層級。 網域與樹系的功能，在 Windows Server 2003 Active Directory，提供一種方式可讓網域-或全樹系的 Active Directory 功能，您網路的環境中。 網域功能與樹系的不同層級可用，根據您的環境。

樹系功能讓您森林中的所有網域的功能。 使用下列的樹系功能層級︰

-    Windows Server 2008。 此功能樹系的層級支援執行 Windows Server 2008 和 Windows Server 作業系統的版本中的網域控制站。

-    Windows Server 2008 R2。 此功能樹系的層級支援 Windows Server 2008 R2 網域控制站和執行較新版本的 Windows Server 作業系統的網域控制站。

-    Windows Server 2012。 此功能樹系的層級支援 Windows Server 2012 網域控制站和執行較新版本的 Windows Server 作業系統的網域控制站。

-    Windows Server 2012 R2。 此功能樹系的層級支援 Windows Server 2012 R2 網域控制站和執行較新版本的 Windows Server 作業系統的網域控制站。

-    Windows Server 2016。 此功能樹系的層級支援只有 Windows Server 2016 網域控制站和執行較新版本的 Windows Server 作業系統的網域控制站。

如果您要部署新的網域中新的網域控制站所有將會執行 Windows Server 2016，建議和 Windows Server 2016 森林功能層級設定 AD DS，AD DS 安裝期間。

> [!IMPORTANT]
> 引發的樹系功能層級之後，網域控制站執行更早版本作業系統，無法引入樹系。 例如，如果您提高 Windows Server 2016 的樹系功能層級，執行的是 Windows Server 2012 R2 或 Windows Server 2008 的網域控制站無法新增樹系。

下表中提供的 AD DS 範例設定項目。

|設定項目：|範例值：|
|------------------------|-------------------|
|完整的 DNS 名稱|範例：<br /><br />-corp.contoso.com<br />-example.com|
|森林功能層級|Windows Server 2008 <br />Windows Server 2008 R2 <br />Windows Server 2012 <br />Windows Server 2012 R2 <br />Windows Server 2016|
|Active Directory Domain Services 資料庫資料夾位置|E:\Configuration\\<br /><br />或接受預設的位置。|
|Active Directory Domain Services 登入檔案的資料夾位置|E:\Configuration\\<br /><br />或接受預設的位置。|
|Active Directory Domain Services SYSVOL 資料夾位置|E:\Configuration\\<br /><br />或接受預設的位置|
|Directory 還原模式系統管理員密碼|**J\ * p2leO4$ F**|
|回應檔案名稱（選擇性）|**廣告 DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>規劃區域 DNS
主要、Active Directory 整合 DNS 伺服器，預設會建立向前對應區域的 DNS 伺服器角色安裝期間。 往後對應區域讓電腦與另一部電腦或裝置的 IP 位址，其 DNS 名稱所依據的查詢裝置。 除了區域正向對應，建議您建立 DNS 反向對應區域。 使用 DNS 反向對應查詢、電腦或裝置探索另一部電腦或裝置使用的 IP 位址的名稱。 部署反向對應區域通常改善 DNS 效能和大幅增加 DNS 查詢的成功。

當您建立反向對應區域時，所定義 DNS 標準中，保留在網際網路 DNS 提供反向查詢執行實際且可靠的方式來命名空間 in-addr.arpa 網域中 DNS 設定。 若要建立反向命名空間的正確 in-addr.arpa 網域中，使用的數字帶小數點的 IP 位址反向排序。

In-addr.arpa 網域適用於所有 TCP/IP 網路網際網路通訊協定第 4 (IPv4) 為基礎的位址。 新的時區精靈會自動假設您使用這個網域當您建立新反向對應的區域。

當您執行的新的時區精靈時，建議使用下列選項：

|設定項目|範例值|
|-----------------------|------------------|
|輸入區|**主要區域**，並**市集區域 Active Directory 中**選取|
|Active Directory 區域複寫領域|**在這個網域中的所有 DNS 伺服器**|
|第一反向對應區域名稱精靈頁面|**IPv4 反向對應區域**|
|第二個反向對應區域名稱精靈頁面|網路 ID = 10.0.0。|
|動態更新|**允許安全的動態更新**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>規劃網域存取
若要登入至網域中，電腦必須成員網域的電腦和使用者 account 必須建立在嘗試登入之前 AD DS。

> [!NOTE]
> 個人電腦正在執行 Windows 有 [本機使用者和群組使用者帳號，稱為安全性帳號 Manager（坡）使用者帳號資料庫資料庫。 當您在本機電腦坡資料庫中建立帳號時，您可以登入本機電腦，但您無法登入網域。 網域帳號網域控制站，無法使用 [本機使用者和群組本機電腦上的建立的 Active Directory 使用者和電腦 Microsoft Management Console (MMC)。

在第一次成功登入後的網域登入認證，除非您的電腦已移除網域，或登入設定的手動變更保存登入的設定。

之前您登入網域：

-   建立帳號 Active Directory 使用者與電腦。 每個使用者必須 Active Directory Domain Services 使用者帳號 Active Directory 使用者與電腦。 如需詳細資訊，請查看[在 Active Directory 使用者和電腦建立帳號](#BKMK_createUA)。

-   請確定正確的 IP 位址設定。 若要將電腦加入網域，電腦必須 IP 位址。 本指南，伺服器靜態 IP 位址設定，並 client 電腦 IP 位址租用收到 DHCP 伺服器。 基於這個原因，必須先戶端加入網域部署 DHCP 伺服器。 如需詳細資訊，請查看[部署 DHCP1](#BKMK_deployDHCP01)。

-   加入網域的電腦。 必須加入網域的電腦提供或存取網路資源。 如需詳細資訊，請查看[的伺服器電腦加入網域並登入](#BKMK_joinlogserver)和[的 Client 電腦加入網域並登入](#BKMK_joinlogclients)。

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>規劃 DHCP1 部署
以下是金鑰規劃步驟之前 DHCP1 上安裝 DHCP 伺服器角色。

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>規劃伺服器 DHCP 和 DHCP 轉接
由於 DHCP 訊息廣播的訊息，它們不轉送之間路由器子網路。 如果您有多個子網路，並想要提供 DHCP 為每個子網路的服務，您必須執行下列其中一個動作：

-   安裝每個子網路 DHCP 伺服器

-   設定路由器 DHCP 廣播的郵件轉寄子網路上並設定多個領域子網路每一個範圍 DHCP 伺服器上。

在大部分案例中，設定，將 DHCP 廣播的郵件轉寄路由器有更多成本效益的比 DHCP 伺服器每個區段實體網路上的部署。

#### <a name="planning-ip-address-ranges"></a>規劃 IP 位址範圍
每個子網路中必須有它自己獨特的 IP 位址。 這些範圍表示範圍 DHCP 伺服器上。

領域是使用 DHCP 服務子網路上的電腦的 IP 位址管理群組。 系統管理員會先建立的每個實體的子網路的範圍，然後使用範圍定義用所使用的參數。

領域具有下列屬性：

-   IP 位址，包括或排除使用 DHCP 服務租用提供的地址。

-   子網路遮罩] 判斷子網路首碼指定 IP 位址。

-   指派建立時領域名稱。

-   租用期間值，已指派給 DHCP 戶端接收動態配置的 IP 位址。

-   指派給 DHCP 戶端，例如 DNS 伺服器的 IP 位址和路由器] / [預設閘道 IP 位址設定的任何 DHCP 範圍選項。

-   保留也會用來確保 DHCP client 收到相同的 IP 位址。

部署之前您的伺服器，會列出您子網路和您想要使用的每個子網路的 IP 位址範圍。

#### <a name="planning-subnet-masks"></a>子網路遮罩計劃
使用 [子網路遮罩分辨網路 Id 和主機 Id 中 IP 位址。 每個子網路遮罩是 32 位元數字使用連續元群組的所有的網路找出 (1) ID 和所有零 (0) 找出主機 ID 部分的 IP 位址。

例如，常用的 IP 位址 131.107.16.200 子網路遮罩是下列 32 位元二進位數字：

```
11111111 11111111 00000000 00000000
```

此子網路遮罩數字是 16 一位元後面 16 零位元，指出這個 IP 位址的網路 ID 和主機 ID 區段這兩個 16 位元的長度。 一般而言，這個子網路遮罩小數點標記中顯示為 255.255.0.0。

下表顯示子網路遮罩網際網路位址類別。

|地址課|子網路遮罩的位元|子網路遮罩|
|-----------------|------------------------|---------------|
|A 課|11111111 00000000 00000000 00000000|255.0.0.0|
|B|11111111 11111111 00000000 00000000|255.255.0.0|
|C 課|11111111 11111111 11111111 00000000|255.255.255.0|

當您建立領域 DHCP 中，輸入 ip 範圍 DHCP 提供這些預設子網路遮罩值。 一般而言，子網路遮罩的預設值是適用於任何特殊需求大部分網路與其中每個 IP 網路區段對應單一實體網路。

有時候，您可以使用 [自訂子網路遮罩實作 IP 子網路。 IP 子網路，您也可以細分預設主機 ID 部分指定的原始課程為基礎的網路 ID 量度子網路的 IP 位址

自訂子網路遮罩長度，您可以減少用於實際主機收到的位元

若要防止地址和路由問題，您應該確定區段網路上的所有 TCP/IP 電腦都使用相同的子網路遮罩與每個電腦或裝置具有獨特的 IP 位址。

#### <a name="planning-exclusion-ranges"></a>規劃範圍排除項目
當您建立範圍 DHCP 伺服器時，您可以指定 IP 位址範圍包含所有的租用 DHCP 戶端，例如電腦與其他裝置以允許 DHCP 伺服器的 IP 位址。 如果您然後並手動設定某些伺服器與其他裝置靜態相同的 IP 位址範圍使用 DHCP 伺服器的 IP 位址，您不小心可以建立 IP 位址衝突，有您和 DHCP 伺服器兩指派相同的 IP 位址不同的裝置。

若要解開這個問題，您可以建立 DHCP 範圍排除項目範圍。 排除項目範圍是介於領域的 IP 位址 DHCP 伺服器不受允許使用連續 IP 位址。 如果您建立排除範圍，DHCP 伺服器不會指派範圍，讓您以手動方式將這些位址指派而不需要建立 IP 位址衝突中的位址。

您可以從排除 IP 位址 distribution DHCP 伺服器建立的每個領域排除範圍。 適用於所有裝置與靜態 IP 位址設定，您應該使用排除項目。 排除的位址應該會包含所有伺服器，非 DHCP 戶端、無磁碟工作站，或其他路由並遠端存取和 PPP 手動指派的 IP 位址。

建議您在使用額外的地址，以配合未來網路成長設定您的範圍排除項目。 下表 ip 10.0.0.1-10.0.0.254 和 255.255.255.0 子網路遮罩領域提供的範例排除範圍。

|設定項目|範例值|
|-----------------------|------------------|
|排除項目範圍開始 IP 位址|10.0.0.1|
|排除項目範圍結束 IP 位址|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>規劃靜態的 TCP/IP 設定
特定裝置，例如路由器、DHCP 伺服器和 DNS 伺服器，必須使用靜態 IP 位址設定。 此外，您可能有其他裝置，例如印表機、想要確保永遠具有相同的 IP 位址。 列出的裝置，您靜態想要設定的每個子網路，並規劃排除範圍您想要使用 DHCP 伺服器上，以確保 DHCP 伺服器不會租用靜態設定裝置的 IP 位址。 排除項目範圍是有限的一連串中排除 DHCP 服務方案的範圍的 IP 位址。 排除項目範圍確保給您網路上的 DHCP 戶端伺服器不提供任何這些範圍中的位址。

例如，如果子網路的 IP 位址範圍是透過 192.168.0.254 192.168.0.1 10 個裝置您想要使用靜態 IP 位址設定，您可以建立排除項目範圍 192.168.0 的。*x*包含十部或多個 IP 位址的範圍：192.168.0.1 透過 192.168.0.15。

在此範例中，使用 10 排除 IP 位址伺服器和其他裝置設定成靜態 IP 位址，還有適用的新裝置，您可能想要新增未來靜態設定五個其他的 IP 位址。 使用這個排除項目範圍，透過 192.168.0.254 192.168.0.16 位址集區與剩餘 DHCP 伺服器。

下表中提供 AD DS 和 DNS 範例額外的設定項目。

|設定項目|範例值|
|-----------------------|------------------|
|網路連接繫結|乙太網路|
|DNS 伺服器設定|DC1.corp.contoso.com|
|慣用的 DNS 伺服器的 IP 位址|10.0.0.2|
|新增範圍對話方塊的值<br /><br />1.範圍名稱<br />2.開始 IP 位址<br />3.結束 IP 位址<br />4.子網路遮罩<br />5.預設閘道（選擇性）<br />6.租用期間|1.主要子網路<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6.8 天|
|IPv6 DHCP 伺服器操作模式|不支援|

## <a name="BKMK_deployment"></a>核心網路部署
若要部署的核心網路，基本步驟如下：

1.  [設定所有伺服器]](#BKMK_configuringAll)

2.  [部署 DC1](#BKMK_deployADDNS01)

3.  [加入網域的電腦伺服器，並登入](#BKMK_joinlogserver)

4.  [部署 DHCP1](#BKMK_deployDHCP01)

5.  [加入網域的電腦 Client 並登入](#BKMK_joinlogclients)

6.  [部署選擇性功能網路存取驗證和 Web 服務](#BKMK_optionalfeatures)

> [!NOTE]
> -   本指南大部分程序提供相同的 Windows PowerShell 命令。 之前，請先執行 Windows PowerShell cmdlet 這些，適用於您的網路部署值取代範例值。 此外，您必須輸入每個 cmdlet 在同一行 Windows PowerShell 中。 在本指南個人 cmdlet 可能會出現在幾個行因為格式限制和文件中的顯示設定您的瀏覽器或其他應用程式。
> -   本文中的程序中不包含那些案例指示**使用者 Account 控制項**對話方塊要求您的權限才能繼續。 如果此對話方塊同時執行程序本指南，如果對話方塊一個因應以您的動作，按一下 [**繼續**。

### <a name="BKMK_configuringAll"></a>設定所有伺服器]
其他技術，例如 Active Directory Domain Services 或 DHCP，在安裝之前請務必進行下列項目。

-   [將電腦重新命名](#BKMK_rename)

-   [設定靜態 IP 位址](#BKMK_ip)

每個伺服器執行這些動作，您可以使用下列的各節。

資格在**系統管理員**，或相當於，才能執行這些程序最小值。

#### <a name="BKMK_rename"></a>將電腦重新命名
若要變更電腦的名稱，您可以在本區段中使用此程序。 重新命名電腦適合用於環境中的作業系統已自動建立您不想要使用的電腦名稱。

> [!NOTE]
> 使用 Windows PowerShell 來執行這個程序、開放 PowerShell 及在不同行中輸入下列 cmdlet，然後按 ENTER。 您還必須更換*電腦名稱*以您想要使用的名稱。
>
> `Rename-Computer`*電腦名稱*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>若要重新命名電腦是執行 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012

1.  在伺服器管理員中，按一下**本機伺服器**。 電腦**屬性**會顯示在詳細資料窗格中。

2.  在**屬性**，請在**電腦名稱**，按一下 [現有的電腦名稱。 **系統屬性**對話方塊。 按一下**變更**。 **電腦名稱日網域變更**對話方塊。

3.  在**電腦名稱日網域變更**對話方塊中，在**電腦名稱**，輸入您的電腦的新名稱。 例如，如果您想要為電腦 DC1，輸入**DC1**。

4.  按一下**[確定]**兩次，然後按**關閉**。 如果您想要重新開機立即，完成名稱的變更，請按一下**現在重新開機**。 否則，請按**重新開機之後**。

> [!NOTE]
> 如何重新命名執行其他 Microsoft 作業系統的電腦上的資訊，請查看[附錄 A-重新命名電腦](#BKMK_A)。

#### <a name="BKMK_ip"></a>設定靜態 IP 位址
您可以設定「網際網路通訊協定第 4 (IPv4) 本主題中使用的程序的靜態 IP，以上網屬性地址執行 Windows Server 2016 的電腦。

> [!NOTE]
> 使用 Windows PowerShell 來執行這個程序、開放 PowerShell 及在不同行中輸入下列 cmdlet，然後按 ENTER。 您必須也取代介面名稱與 IP 位址，在此範例中您想要設定您的電腦使用的值。
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>若要設定執行 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 的電腦上的靜態 IP 位址

1.  工作列，在 [網路] 圖示，以滑鼠右鍵按一下，然後按一下**開放式網路和共用中心]**。

2.  在**網路和共用中心]**，按一下 [**變更介面卡設定**。 **裝置管理員**資料夾開啟並顯示 [可用的網路連接。

3.  在**裝置管理員**，以滑鼠右鍵按一下您想要設定，然後按一下 [連接**屬性**。 網路**屬性**對話方塊。

4.  在網路連接**屬性**對話方塊中，在**此連接使用下列項目]**，請選取**網際網路通訊協定第 4 版本 (TCP 日 IPv4)**，，然後按一下**屬性**。 **網際網路通訊協定第 4 版本 (TCP 日 IPv4) 屬性**對話方塊。

5.  在**網際網路通訊協定第 4 版本 (TCP 日 IPv4) 屬性**，在**一般**索引標籤上，按一下 [**使用下列的 IP 位址**。 在**的 IP 位址**，輸入您想要使用 IP 位址。

6.  按] 索引標籤，將游標在**子網路遮罩**。 會自動輸入子網路遮罩的預設值。 接受預設子網路遮罩，或輸入您想要使用的子網路遮罩。

7.  在**預設閘道**，輸入您的預設閘道 IP 位址。

    > [!NOTE]
    > 您必須設定**預設閘道**以相同的 IP 位址，您的區域網路（區域網路）介面您的路由器上使用。 例如，如果您的路由器連接到網際網路，以及您區域網路至於例如寬區域網路 (WAN)，設定的區域網路介面使用相同的 IP 位址，您將會指定為**預設閘道**。 在另一部範例中，如果您已連接到兩個的區域網路位置區域網路使用位址範圍 10.0.0.0 月 24，B 區域網路使用位址範圍 192.168.0.0 月 24 路由器設定的區域網路 A 路由器 IP 位址該位址範圍，例如 10.0.0.1 地址。 此外，在這個位址 DHCP 範圍，請設定**預設閘道**以 10.0.0.1 的 IP 位址。 區域網路 B 設定的區域網路 B 路由器介面的地址範圍，例如 192.168.0.1、地址，然後再設定區域網路 B 範圍 192.168.0.0 月 24 使用**預設閘道**192.168.0.1 的值。

8.  在**慣用 DNS 伺服器]**，輸入您的 DNS 伺服器的 IP 位址。 如果您打算使用為慣用 DNS 伺服器的本機電腦，請輸入本機電腦的 IP 位址。

9. 在**其他 DNS 伺服器**，如果有的話，輸入您替代的 DNS 伺服器的 IP 位址。 如果您想要使用做為備用 DNS 伺服器的本機電腦，請輸入本機電腦的 IP 位址。

10. 按一下**[確定]**，然後按**關閉**。

> [!NOTE]
> 如需如何設定執行其他 Microsoft 作業系統的電腦上的靜態 IP 位址資訊，請查看[附錄 B-設定靜態 IP 位址](#BKMK_B)。

#### <a name="BKMK_deployADDNS01"></a>部署 DC1
若要部署 DC1 的電腦執行 Active Directory Domain Services (AD DS) 和 DNS，您必須先完成下列步驟進行以下列順序：

-   一節中執行步驟[設定所有伺服器]](#BKMK_configuringAll)。

-   [安裝 AD DS，以及新的樹系的 DNS](#BKMK_installAD-DNS)

-   [建立帳號 Active Directory 使用者與電腦](#BKMK_createUA)

-   [指派群組成員資格](#BKMK_assigngroup)

-   [設定 DNS 反向對應區域](#BKMK_reverse)

**系統管理員權限**

如果您正在安裝小的網路，而且不只系統管理員的網路，建議您建立帳號，並再成員企業系統管理員和網域系統管理員的身分加入您的使用者帳號。 這樣將會讓您做的所有網路資源，系統管理員的身分變得更容易。 我們也建議您登入此帳號只有在您需要執行管理工作，並建立執行非 IT 不同的使用者負責相關工作時。

如果您有較大的公司使用多個管理員，請參考 AD DS 文件，以判斷組織員工的最佳群組成員資格。

**網域帳號與帳號本機電腦上不同**

其中一個優點網域為基礎的是，您不需要帳號建立網域中的每一部電腦上。 電腦是否 client 的電腦或伺服器是如此。

因此，您不應該帳號建立網域中的每一部電腦上。 建立 Active Directory 使用者和電腦所有使用者帳號，並使用上述程序指派群組成員資格。 根據預設，所有使用者帳號都的網域使用者群組成員。

使用者網域群組的所有成員可以都登入 client 的任何電腦之後該已經加入網域。

您可以設定帳號，若要指定的使用者可以登入電腦的日期和時間。 您也可以指定每個使用者可以使用哪一台電腦。 這些設定，開放 Active Directory 使用者和電腦，找出您想要設定帳號，按兩下 [account。 在帳號**屬性**，按一下 [ **Account**索引標籤，然後再按一下**登入小時**或**來登入**。

#### <a name="BKMK_installAD-DNS"></a>安裝 AD DS，以及新的樹系的 DNS

您可以使用下列程序的其中一個安裝 Active Directory Domain Services (AD DS) 和 DNS，並在新的樹系建立新的網域。 

第一個步驟的指示在第二個程序會顯示您如何使用伺服器管理員安裝 AD DS 和 DNS 使用 Windows PowerShell 中，執行下列動作。

>[!IMPORTANT]
>步驟執行此程序完成之後，會自動重新啟動電腦。

**安裝 AD DS 和 DNS 使用 Windows PowerShell**

您可以使用下列命令來安裝和 AD DS 和 DNS 設定。 您必須在此範例中的網域名稱取代您想要為您的網域使用的值。

>[!NOTE]
>如需 Windows PowerShell 命令，查看下列的參考主題。
>- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)
>- [Install-ADDSForest](https://technet.microsoft.com/itpro/powershell/windows/adds/deployment/install-addsforest)

資格在**系統管理員**，至少需要執行這個程序。

- 系統管理員身分執行 Windows PowerShell 中輸入下列命令，，然後按 ENTER 鍵：  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

安裝完成後成功，下列訊息會顯示在 Windows PowerShell 中。

    
    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...
    

- Windows PowerShell 中，輸入下列命令，更換文字**corp.contoso.com**與您的網域名稱，然後按 ENTER 鍵：

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- 安裝和設定程序期間，才會顯示在頂端的 Windows PowerShell 視窗，會顯示下列命令提示字元。 它會顯示之後，輸入密碼，然後按 ENTER 鍵。

    **SafeModeAdministratorPassword:**

- 您輸入密碼並按下 ENTER 後，會顯示下列確認的提示。 輸入相同的密碼，然後按 ENTER 鍵。

    **確認 SafeModeAdministratorPassword:**

- 下列提示出現時，輸入的字母**Y**，然後按 ENTER 鍵。

    
    將會設定為網域控制站目標伺服器，並重新啟動完成這項作業時。
    您想要進行此操作嗎？
    [Y] 是 [A] 是所有的 [N] 不 [L] 不到 [所有 [S] 暫停 [？]協助（預設值為 [Y」）：
    
- 如果您想要您可以讀取 AD DS 和 DNS 的標準模式、成功安裝期間會顯示警告訊息。 這些訊息是標準，無法安裝失敗的指示。

- 在成功安裝之後，則會顯示訊息，您將會使電腦可以重新登入電腦使用。 如果您按一下**關閉**、立即登出電腦，以及電腦重新開機。 如果您不要按**關閉**，在電腦重新開機之後預設一段時間。

- 伺服器會重新之後，您可以檢查 Active Directory Domain Services 和 DNS 成功的安裝。 打開 Windows PowerShell，輸入下列命令，然後按 ENTER。

````
Get-WindowsFeature
````

在這個命令的結果，Windows PowerShell 中會顯示，並且應該是類似下列影像中的結果。 適用於已安裝的技術，技術名稱的左括弧包含字元**X**，以及的值**安裝的狀態**是**已安裝**。

![Get-WindowsFeature 命令的結果](../media/Core-Network-Guide/server-roles-installed.jpg)

**安裝 AD DS 和 DNS 使用伺服器管理員**

1.  DC1，在中**伺服器管理員**，按一下 [**管理**，然後按一下 [**新增角色與功能**。 新增角色與功能精靈開啟。

2.  在**在您開始之前，請先**，按一下 [**下**。

    > [!NOTE]
    > **在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。

3.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。

4.  在**選取目的伺服器**，確保**選取伺服器伺服器集區的**選取。 在**伺服器集區**，請確定已選取 [本機電腦。 按一下**下一步**。

5.  在**選擇伺服器角色**，請在**角色**，按一下 [ **Active Directory Domain Services**。 在**新增所需的 Active Directory Domain Services 功能**，按一下 [**新增功能**。 按一下**下一步**。

6.  在**選擇功能**，按一下 [**下一步**，在**Active Directory Domain Services**，檢視的資訊，提供，然後按一下**下一步**。

7.  在**確認安裝選項**，按一下 [**安裝**。 安裝進度頁面會顯示在安裝期間狀態。 當程序完成時，訊息詳細資訊，按一下**這個網域控制站伺服器升級**。 Active Directory Domain Services 組態精靈開啟。

8.  在**部署組態**、**新增新的樹系**。 在**根網域名稱**，輸入您的網域的完整的網域名稱 (FQDN)。 例如，如果您 FQDN corp.contoso.com，輸入**corp.contoso.com**。按一下**下一步**。

9. 在**網域控制站選項**，請在**選取新的樹系和根網域功能等級**，選取樹系功能等級和網域您想要使用的功能等級。 在**指定網域控制站功能**，確保**網域名稱系統」(DNS) 伺服器**並**全球 Catalog (GC)**選取。 在**密碼**和**確認密碼**，輸入您想要使用的 Directory 服務還原模式 (DSRM) 密碼。 按一下**下一步**。

10. 在**DNS 選項**，按一下 [**下**。

11. 在**的其他選項**、驗證 NetBIOS 名稱指定給網域中，並視需要變更該只。 按一下**下一步**。

12. 在**路徑**，請在**指定的位置資料庫 AD DS，登入檔案，以及 SYSVOL**，執行下列其中一個動作：

    -   接受預設值。

    -   輸入您想要使用的資料夾位置**資料夾資料庫**，**登入檔案的資料夾**，並**SYSVOL 資料夾**。

13. 按一下**下一步**。

14. 在**檢視選項**，檢視您的選擇。

15. 如果您想要設定匯出到 Windows PowerShell 指令碼，請按一下**檢視指令碼**。 指令碼開啟在「記事本」中，您可以將它儲存到您想要的資料夾位置。 按一下**下一步**。 在**必要條件檢查**、進行驗證您的選擇。 檢查完成時，按**安裝**。 Windows 的提示，請按一下**關閉**。 伺服器重新開機才能完成安裝 AD DS 和 DNS。

16. 伺服器重新開機之後，若要確認成功安裝，檢視伺服器管理員」主控台。 AD DS 和 DNS 應該會顯示在左窗格中，例如反白顯示下列影像中的項目。

![AD DS 和 DNS 伺服器管理員中](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>建立帳號 Active Directory 使用者與電腦
Active Directory 使用者電腦 Microsoft Management Console (MMC) 中建立新的網域帳號，您可以使用此程序。

資格在**網域系統管理員**，或相當於，才能執行此程序最小值。

> [!NOTE]
> 使用 Windows PowerShell 來執行這個程序，開放 PowerShell 和一行，輸入下列 cmdlet，然後按 ENTER。 您也必須取代使用者 account 名稱在此範例中為您想要使用的值。
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> 按下 ENTER 後，請輸入密碼帳號。 Account 建立，預設授與成員資格加入網域使用者群組。
>
> 使用下列 cmdlet，您可以指定新的使用者 account 其他群組成員資格。 以下範例會 User1 加入網域系統管理員」及企業系統管理員」群組。 請確定之前，請先執行此命令的變更 account 的使用者名稱、網域名稱和群組]，以符合您的需求。
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>若要建立的使用者 account

1.  在 DC1，在伺服器管理員中，按一下**工具**，然後按一下 [ **Active Directory 使用者和電腦**。 Active Directory 使用者和電腦 MMC 開啟。 如果您未選取，按一下您的網域節點。 例如，如果您的網域 corp.contoso.com，請按一下**corp.contoso.com**。

2.  在詳細資料窗格中，以滑鼠右鍵按一下您要新增的使用者帳號的資料夾。

    **何處？**

    -   Active Directory 使用者和電腦日*網域節點*/*資料夾*

3.  移至**新**，然後按一下 [**使用者**。 **新物件-使用者**對話方塊。

4.  在**第一個名稱**，輸入第一的使用者的名稱。

5.  在**簡稱**，輸入使用者的簡稱。

6.  在**姓氏**，輸入使用者的最後一個名稱。

7.  修改**的全名**來新增簡稱或反向排序的名字與姓氏。

8.  在**登入的使用者名稱**，輸入登入的使用者名稱。 按一下**下一步**。

9. 在**新物件-使用者**，請在**的密碼**並**確認密碼**、輸入使用者的密碼，然後選取適當的密碼的選項。

10. 按一下**下一步**，檢視的新使用者 account 設定，然後按**完成]**。

##### <a name="BKMK_assigngroup"></a>指派群組成員資格
您可以使用此程序群組 Active Directory 使用者電腦 Microsoft Management Console (MMC) 中新增的使用者，電腦上或群組。

資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。

###### <a name="to-assign-group-membership"></a>若要指定群組成員資格

1.  在 DC1，在伺服器管理員中，按一下**工具**，然後按一下 [ **Active Directory 使用者和電腦**。 Active Directory 使用者和電腦 MMC 開啟。 如果您未選取，按一下您的網域節點。 例如，如果您的網域 corp.contoso.com，請按一下**corp.contoso.com**。

2.  在詳細資料窗格中，按兩下您想新增成員群組所在的資料夾。

    何處？

    -   **Active Directory 使用者和電腦**/*網域節點*/*群組所在的資料夾*

3.  在詳細資料窗格中，以滑鼠右鍵按一下您想要新增到群組，例如使用者或電腦，然後按一下 [物件**屬性**。 物件的**屬性**對話方塊。 按一下**的成員**索引標籤。

4.  在**的成員**索引標籤上，按一下 [**新增]**。

5.  在**[輸入物件名稱來選取**，輸入您想要新增的物件，然後按一下 [的群組的名稱**[確定]**。

6.  若要指定其他使用者、群組或電腦群組成員資格，重複步驟 4 和 5 此程序。

##### <a name="BKMK_reverse"></a>設定 DNS 反向對應區域
您可以使用此程序，設定反向對應區域在「網域名稱系統」(DNS)。

資格在**網域系統管理員**，至少需要執行這個程序。

> [!NOTE]
> -   媒體和大型的組織，建議您設定並使用 Active Directory 使用者和電腦 DNSAdmins 群組。 如需詳細資訊，請查看[額外的技術資源](#BKMK_resources)
> -   使用 Windows PowerShell 來執行這個程序，開放 PowerShell 和一行，輸入下列 cmdlet，然後按 ENTER。 您也必須取代 DNS 反向對應區域和 zonefile 名稱在此範例中您想要使用的值。 請確定您回復網路 ID 反向區域的名稱。 如果網路 ID 192.168.0，，例如建立反向尋找區域名稱**0.168.192.in in-addr.arpa**。
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>若要設定 DNS 反向對應區域

1.  DC1，在伺服器管理員中，按一下 [**工具**，然後按**DNS**。 DNS MMC 開啟。

2.  在 DNS，如果未展開，按兩下以展開樹伺服器名稱。 例如，如果 DC1 DNS 伺服器名稱，按兩下 [ **DC1**。

3.  選取 [**反向對應區域**，以滑鼠右鍵按一下**反向對應區域**，然後按一下 [**新增區域**。 新的時區精靈開啟。

4.  在**歡迎使用新的時區精靈**，按一下 [**下**。

5.  在**區域類型**、**主要區域**。

6.  如果您的 DNS 伺服器寫入網域控制站，確保**在 Active Directory 中儲存區域**選取。 按一下**下一步**。

7.  在**Active Directory 區域複寫領域**、**執行網域控制站在這個網域中的所有 DNS 伺服器**，除非您有特定的理由，來選擇不同的選項。 按一下**下一步**。

8.  第一次**反向對應區域的名稱**頁面上，選取**回復對應區域 IPv4**。 按一下**下一步**。

9. 在第二個**反向對應區域的名稱**頁面上，執行下列其中一個動作：

    -   在**網路 ID**，輸入您的 IP 位址各種不同的網路來電顯示。 例如，如果您的 IP 位址範圍是透過 10.0.0.254 10.0.0.1，輸入**10.0.0**。

    -   在**反向對應區域名稱**，會自動新增您區域 IPv4 反向對應的名稱。 按一下**下一步**。

10. 在**動態更新**，選取您想要允許的動態更新的類型。 按一下**下一步**。

11. 在**完成新增區精靈**，檢視您的選擇，然後按**完成]**。

#### <a name="BKMK_joinlogserver"></a>加入網域的電腦伺服器，並登入
您有安裝 Active Directory Domain Services (AD DS)，並建立一個或多個使用者帳號已經加入網域的電腦的權限之後，您可以加入網域並登入伺服器核心網路伺服器以安裝其他技術，例如「動態主機設定通訊協定」(DHCP)。

在 [所有伺服器，您的部署，除了伺服器執行 AD DS，執行下列動作：

1.  完成的程序，以提供[設定所有伺服器]](#BKMK_configuringAll)。

2.  使用下列兩個程序的指示，加入您的伺服器網域並登入執行其他部署工作伺服器：

> [!NOTE]
> 使用 Windows PowerShell 來執行這個程序，開放 PowerShell 輸入下列 cmdlet，並再按下 ENTER。 您必須也將您想要使用名稱的網域名稱取代。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 當您接到這樣做時，請輸入的使用者名稱和密碼 account 的權限加入網域的電腦。 若要重新開機，輸入下列命令，然後按 ENTER。
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>若要加入網域執行 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 的電腦

1.  在伺服器管理員中，按一下**本機伺服器**。 在詳細資料窗格中，按一下**群組**。 **系統屬性**對話方塊。

2.  在**系統屬性**對話方塊中，按**變更**。 **電腦名稱日網域變更**對話方塊。

3.  在**電腦名稱**，請在**的成員**，按一下 [**網域**，然後輸入您想要加入的網域名稱。 如果 corp.contoso.com 的網域名稱，例如，輸入**corp.contoso.com**。

4.  按一下**[確定]**。 **Windows 安全性**對話方塊。

5.  在**電腦名稱日網域變更**，請在**使用者名稱**，輸入使用者名稱，並在**密碼**，請輸入密碼，然後按一下**[確定]**。 **電腦名稱日網域變更**對話方塊，一則歡迎您的網域。 按一下**[確定]**。

6.  **電腦名稱日網域變更**對話方塊中，會顯示訊息表示，您必須重新開機，適用於所做的變更。 按一下**[確定]**。

7.  在**系統屬性**對話方塊中，於**電腦名稱**索引標籤上，按一下 [**關閉**。 **Microsoft Windows**對話方塊中開啟，且會顯示訊息，再試一次表示，您必須重新開機，適用於所做的變更。 按一下**現在重新**。

> [!NOTE]
> 如需如何加入網域執行其他 Microsoft 作業系統的電腦上的資訊，請[附錄 C-加入網域的電腦](#BKMK_C)。

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>若要登入執行 Windows Server 2016 的電腦網域

1.  登入電腦，或重新開機。

2.  按下 CTRL + ALT + DELETE。 登入畫面顯示。

3.  在 [左下角，按一下 [**以其他使用者**。

4.  在**的使用者名稱**，輸入您的使用者名稱。

5.  在**密碼**、輸入您的網域密碼，然後按一下箭頭，或按下 ENTER。

> [!NOTE]
> 如何登入以使用電腦執行其他 Microsoft 作業系統的網域資訊，請查看[附錄 D-登入網域](#BKMK_D)。

#### <a name="BKMK_deployDHCP01"></a>部署 DHCP1
您必須先部署的核心網路這個元件，執行下列動作：

-   一節中執行步驟[設定所有伺服器]](#BKMK_configuringAll)。

-   一節中執行步驟[的伺服器電腦加入網域並登入](#BKMK_joinlogserver)。

若要部署 DHCP1，也就是在電腦執行的是「動態主機設定通訊協定」(DHCP) 伺服器角色，您必須先完成下列步驟進行以下列順序：

-   [安裝動態主機設定通訊協定」(DHCP)](#BKMK_installDHCP)

-   [建立和啟動 DHCP 新的領域](#BKMK_newscopeDHCP)

> [!NOTE]
> 若要使用 Windows PowerShell 來執行這些程序，開放 PowerShell 不同行，輸入下列 cmdlet，然後按 ENTER 鍵。 您必須也取代領域名稱、IP 位址開始和結束範圍、子網路遮罩和其他值在此範例中您想要使用的值。
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

##### <a name="BKMK_installDHCP"></a>安裝動態主機設定通訊協定」(DHCP)
安裝和使用新增角色及功能精靈 DHCP 伺服器角色的設定，您可以使用此程序。

資格在**網域系統管理員**，或相當於，才能執行此程序最小值。

###### <a name="to-install-dhcp"></a>若要安裝 DHCP

1.  DHCP1，在伺服器管理員中，按一下 [**管理**，然後按**新增角色與功能**。 新增角色與功能精靈開啟。

2.  在**在您開始之前，請先**，按一下 [**下**。

    > [!NOTE]
    > **在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。

3.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。

4.  在**選取目的伺服器**，確保**選取伺服器伺服器集區的**選取。 在**伺服器集區**，請確定已選取 [本機電腦。 按一下**下一步**。

5.  在**選取伺服器角色**，請在**角色**、選取**DHCP 伺服器**。 在**加入的功能需要 DHCP 伺服器的**，按一下 [**新增功能**。 按一下**下一步**。

6.  在**選擇功能**，按一下 [**下一步**，在**DHCP 伺服器**，檢視的資訊，提供，然後按一下**下一步**。

7.  在**確認安裝選項**，按一下 [**必要時自動重新開機目的伺服器**。 當系統提示您確認選擇時，請按一下**[是]**，然後按一下 [**安裝**。 **安裝進度**頁面上的安裝程序期間會顯示狀態。 當程序完成時，該訊息「所需的設定。 成功安裝*電腦名稱*「會顯示在*電腦名稱*電腦時，您可以安裝 DHCP 伺服器的名稱。 在視窗訊息中，按一下 [**完成 DHCP 設定**。 開啟 DHCP 後 Post-Install 設定精靈。 按一下**下一步**。

8.  在**授權**，指定您要用來授權在 Active Directory Domain Services，DHCP 伺服器，然後按一下 [認證**認可**。 授權完成後，請按一下**關閉**。

##### <a name="BKMK_newscopeDHCP"></a>建立和啟動 DHCP 新的領域
您可以使用此程序，以建立新的 DHCP 領域使用 DHCP Microsoft Management Console (MMC)。 當您完成程序時，範圍便會觸動，之後您建立排除範圍防止 DHCP 伺服器租借，建議您使用靜態設定伺服器的 IP 位址和其他裝置，需要靜態 IP 位址。

資格在**DHCP 系統管理員**，或相當於，才能執行此程序最小值。

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>若要建立並啟用 DHCP 新的領域

1.  在 DHCP1，在伺服器管理員中，按一下**工具**，然後按**DHCP**。 DHCP MMC 開啟。

2.  在**DHCP**，展開 [伺服器名稱。 例如，如果 DHCP1.corp.contoso.com DHCP 伺服器名稱，按一下向下箭號下一步**DHCP1.corp.contoso.com**。

3.  在 [伺服器名稱，以滑鼠右鍵按一下**IPv4**，然後按一下 [**新的領域**。 新的領域精靈開啟。

4.  在**歡迎使用新的領域精靈**，按一下 [**下**。

5.  在**範圍名稱**，請在**名稱**，輸入名稱的範圍。 例如，輸入**子網路 1**。

6.  在**描述**，輸入新的領域，描述，然後按**下**。

7.  在**Ip**，執行下列動作：

    1.  在**開始 IP 位址**，輸入 IP 位址的範圍中的第一個 IP 位址。 例如，輸入**10.0.0.1**。

    2.  在**結束 IP 位址**，輸入 IP 位址的範圍中的最後一個 IP 位址。 例如，輸入**10.0.0.254**。 值適用於**長度**和**子網路遮罩**會自動輸入，根據您所輸入的 IP 位址**開始 IP 位址**。

    3.  如有需要，修改中的值**長度**或**子網路遮罩**，視您位址配置。

    4.  按一下**下一步**。

8.  在**[新增排除項目**，執行下列動作：

    1.  在**開始 IP 位址**，輸入 IP 位址的範圍排除項目中的第一個 IP 位址。 例如，輸入**10.0.0.1**。

    2.  在**結束 IP 位址**，輸入 IP 位址的最後一個 IP 位址範圍排除項目，例如，輸入**10.0.0.15**。

9. 按一下**新增**，然後按一下 [**下**。

10. 在**租用期間**，修改預設值的**天**，**時間**，和**分鐘**，視您的網路，然後再按一下**下一步**。

11. 在**設定 DHCP 選項**，請選取**是，我要設定現在這些選項**，然後按一下 [**下一步**。

12. 在**（預設閘道）路由器**，執行下列其中一個動作：

    -   如果您尚未路由器您網路上，按一下 [**下一步**。

    -   在**的 IP 位址**中，輸入 IP 位址，您的路由器或預設閘道。 例如，輸入**10.0.0.1**。 按一下**新增**，然後按一下 [**下**。

13. 在**的網域名稱和 DNS 伺服器]**，執行下列動作：

    1.  在**父系網域**，輸入名稱解析戶端使用 DNS 網域名稱。 例如，輸入**corp.contoso.com**。

    2.  在**伺服器名稱**，輸入名稱解析戶端使用 DNS 電腦的名稱。 例如，輸入**DC1**。

    3.  按一下**解析**。 DNS 伺服器的 IP 位址會新增**的 IP 位址**。 按一下**新增**，等待 DNS 伺服器 IP 位址驗證，才能完成，然後按**下**。

14. 在**WINS 伺服器]**，因為您未在您的網路，需要 WINS 伺服器按**下一步**。

15. 在**啟動範圍**、**是，我想要立即啟動這個領域**。

16. 按一下**下一步**，然後按**完成**。

> [!IMPORTANT]
> 若要建立新的領域的其他子網路，請重複此程序。 想要部署，並確保 DHCP 郵件轉寄可以在所有的路由器會導致其他子網路，每個子網路使用不同的 IP 位址。

### <a name="BKMK_joinlogclients"></a>加入網域的電腦 Client 並登入

> [!NOTE]
> 使用 Windows PowerShell 來執行這個程序，開放 PowerShell 輸入下列 cmdlet，並再按下 ENTER。 您必須也將您想要使用名稱的網域名稱取代。
>
> `Add-Computer -DomainName corp.contoso.com`
>
> 當您接到這樣做時，請輸入的使用者名稱和密碼 account 的權限加入網域的電腦。 若要重新開機，輸入下列命令，然後按 ENTER。
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>若要加入的網域執行 Windows 10 的電腦

1.  登入本機電腦。

2.  在**[搜尋網路與 Windows**，輸入**系統**。 在搜尋結果中，按一下 [**系統（控制台）**。 **系統**對話方塊。

3.  在**系統**，按一下 [**進階系統設定**。 **系統屬性**對話方塊。 按一下**電腦名稱**索引標籤。

4.  在**電腦名稱**，按一下 [**變更**。 **電腦名稱日網域變更**對話方塊。

5.  在**電腦名稱日網域變更**，請在**的成員**，按一下 [**網域**，然後輸入您想要加入的網域名稱。 如果 corp.contoso.com 的網域名稱，例如，輸入**corp.contoso.com**。

6.  按一下**[確定]**。 **Windows 安全性**對話方塊。

7.  在**電腦名稱日網域變更**，請在**使用者名稱**，輸入使用者名稱，並在**密碼**，請輸入密碼，然後按一下**[確定]**。 **電腦名稱日網域變更**對話方塊，一則歡迎您的網域。 按一下**[確定]**。

8.  **電腦名稱日網域變更**對話方塊中，會顯示訊息表示，您必須重新開機，適用於所做的變更。 按一下**[確定]**。

9. 在**系統屬性**對話方塊中，於**電腦名稱**索引標籤上，按一下 [**關閉**。 **Microsoft Windows**對話方塊中開啟，且會顯示訊息，再試一次表示，您必須重新開機，適用於所做的變更。 按一下**現在重新**。

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>若要加入的網域執行 Windows 8.1 的電腦

1.  登入本機電腦。

2.  以滑鼠右鍵按一下**[開始]**，然後按**系統**。 **系統**對話方塊。

3.  在**系統**，按一下 [**進階系統設定**。 **系統屬性**對話方塊。 按一下**電腦名稱**索引標籤。

4.  在**電腦名稱**，按一下 [**變更**。 **電腦名稱日網域變更**對話方塊。

5.  在**電腦名稱日網域變更**，請在**的成員**，按一下 [**網域**，然後輸入您想要加入的網域名稱。 如果 corp.contoso.com 的網域名稱，例如，輸入**corp.contoso.com**。

6.  按一下**[確定]**。 **Windows 安全性**對話方塊。

7.  在**電腦名稱日網域變更**，請在**使用者名稱**，輸入使用者名稱，並在**密碼**，請輸入密碼，然後按一下**[確定]**。 **電腦名稱日網域變更**對話方塊，一則歡迎您的網域。 按一下**[確定]**。

8.  **電腦名稱日網域變更**對話方塊中，會顯示訊息表示，您必須重新開機，適用於所做的變更。 按一下**[確定]**。

9. 在**系統屬性**對話方塊中，於**電腦名稱**索引標籤上，按一下 [**關閉**。 **Microsoft Windows**對話方塊中開啟，且會顯示訊息，再試一次表示，您必須重新開機，適用於所做的變更。 按一下**現在重新**。

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>若要登入執行 Windows 10 電腦網域

1.  登入電腦，或重新開機。

2.  按下 CTRL + ALT + DELETE。 登入畫面顯示。

3.  在 [左下，按一下 [**以其他使用者**。

4.  在**的使用者名稱**，輸入您的網域和使用者名稱的格式*網域使用者*。 來登入網域 corp.contoso.com 名帳號，例如**使用者-01**，輸入**CORP\User-01**。

5.  在**密碼**、輸入您的網域密碼，然後按一下箭頭，或按下 ENTER。

### <a name="BKMK_optionalfeatures"></a>部署選擇性功能網路存取驗證和 Web 服務
如果您想要部署的網路存取伺服器，例如 wireless 存取點或 VPN 伺服器安裝核心網路之後，建議您部署 NPS 伺服器和網頁伺服器。 對於網路存取部署，建議使用的安全性憑證架構的驗證方法。 您可以使用 NPS 管理存取權的網路原則和部署安全的驗證方法。 您可以使用 Web 伺服器發行您憑證授權單位提供安全驗證憑證的憑證撤銷清單 (CRL)。

> [!NOTE]
> 您可以使用核心網路小幫手指南部署伺服器的憑證和其他功能。 如需詳細資訊，請查看[額外的技術資源](#BKMK_resources)。

下圖顯示拓撲新增 NPS 與 Web 伺服器與 Windows Server Core 網路。

![Windows Server Core 網路拓撲新增 NPS 與 Web 伺服器](../media/Core-Network-Guide/cng16_overview_2.jpg)

下列章節提供 NPS 與 Web 伺服器新增到您的網路的資訊。

-   [部署 NPS1](#BKMK_deployNPS1)

-   [部署 WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>部署 NPS1
準備用來部署其他網路存取技術，例如私人網路 virtual (VPN) 伺服器、wireless 存取點，並 802.1 X 驗證的參數步驟以安裝網路原則 Server (NPS) 伺服器。

網路原則 Server (NPS) 可讓您集中設定及管理網路原則，使用下列功能：遠端驗證 Dial 使用者服務 (RADIUS) 伺服器與 RADIUS proxy。

NPS 核心網路，選擇性元件，但下列其中一項時，您應該會安裝 NPS:

-   您計畫以展開網路包含遠端存取伺服器 RADIUS 通訊協定，例如電腦執行的是 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 和路由並遠端存取服務，車票服務閘道或遠端桌面閘道相容。


-   想要部署 802.1 X 驗證的有線或無線存取。

部署之前的角色這項服務，您必須為 NPS 伺服器設定您的電腦上執行下列步驟。

-   一節中執行步驟[設定所有伺服器]](#BKMK_configuringAll)。

-   一節中執行步驟[的伺服器電腦加入網域並登入](#BKMK_joinlogserver)

若要部署，也就是在電腦 NPS1 伺服器角色網路原則與服務存取權的網路原則 Server (NPS) 角色服務執行的是，您必須先完成此步驟：

-   [規劃 NPS1 部署](#bkmk_NetFndtn_Pln_NPS-01)

-   [安裝網路原則伺服器 (NPS)](#BKMK_installNPS)

-   [NPS 伺服器登記預設網域中](#BKMK_registerNPS)

> [!NOTE]
> 本指南部署 NPS 獨立伺服器上的指示或 VM 名 NPS1。  另一個建議的部署模型是網域控制站 NPS 安裝。 如果您偏好網域控制站而不是在獨立的伺服器上安裝 NPS，安裝 NPS DC1 上。

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>規劃 NPS1 部署
如果您想要部署的網路存取伺服器，例如 wireless 存取點或 VPN 伺服器部署核心網路之後，建議您部署 NPS。

當您使用遠端驗證 Dial 使用者服務 (RADIUS) 伺服器 NPS 時，NPS 執行驗證和授權連接到您的網路存取伺服器的要求。 NPS 也可讓您集中設定及管理原則的網路存取網路的人、它們如何存取網路及時，他們可以存取網路可用來判斷。

以下是金鑰計劃的步驟，安裝 NPS 之前。

- 規劃安全性。 根據預設，如果您加入執行 Active Directory domain、NPS 伺服器 NPS 會執行驗證，AD DS 使用者帳號資料庫授權。 有時候，像是使用大型網路使用 NPS RADIUS proxy 為轉送連接要求其他 RADIUS 伺服器，您可能要安裝 NPS 成員非網域的電腦上。

- 規劃 RADIUS 計量。 NPS 可讓您登入計量資料 SQL Server 資料庫或在本機電腦上的文字檔案。 如果您想要使用 SQL Server 登入，計劃安裝及執行 SQL Server server 的設定。

##### <a name="BKMK_installNPS"></a>安裝網路原則伺服器 (NPS)
若要使用的新增角色與功能精靈安裝網路原則 Server (NPS)，您可以使用此程序。 NPS 是以角色服務的網路原則與服務存取伺服器角色。

> [!NOTE]
> 根據預設，NPS RADIUS 連接埠 1812 年，1813 年、1645 年 1646 年所有安裝的網路介面卡上的資料傳輸接聽。 使用進階安全性 Windows 防火牆尚未安裝 NPS 時，如果上述連接埠防火牆例外會自動建立網際網路通訊協定第 6 版 \(IPv6\) 和 IPv4 流量的安裝程序期間。 如果您的網路存取伺服器設定 RADIUS 流量傳送到以外這些預設的連接埠，移除例外建立 NPS 在安裝期間，Windows 防火牆使用進階安全性，並建立例外 RADIUS 流量您使用的連接埠。

**管理認證**

若要完成此程序，您必須成員的**網域系統管理員**群組。

> [!NOTE]
> 若要使用 Windows PowerShell 來執行這個程序，開放 PowerShell 輸入下列命令，並再按下 ENTER。
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>若要安裝 NPS

1.  NPS1，在伺服器管理員中，按一下 [**管理**，然後按**新增角色與功能**。 新增角色與功能精靈開啟。

2.  在**在您開始之前，請先**，按一下 [**下**。

    > [!NOTE]
    > **在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。

3.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。

4.  在**選取目的伺服器**，確保**選取伺服器伺服器集區的**選取。 在**伺服器集區**，請確定已選取 [本機電腦。 按一下**下一步**。

5.  在**選取伺服器角色**，請在**角色**、選取**網路原則與服務存取**。 對話方塊詢問是否它應該會新增所需的網路原則與服務存取的功能。 按一下**新增功能**，然後按一下 [**下**。

6.  在**選擇功能**，按一下 [**下一步**，在**網路原則與服務存取**，檢視的資訊，提供，然後按一下**下一步**。

7.  在**選擇角色服務**，按一下 [**的網路原則伺服器**。  在**新增所需的網路原則伺服器功能**，按一下 [**新增功能**。 按一下**下一步**。

8.  在**確認安裝選項**，按一下 [**必要時自動重新開機目的伺服器**。 當系統提示您確認選擇時，請按一下**[是]**，然後按一下 [**安裝**。 安裝進度頁面會顯示在安裝期間狀態。 此程序完成時，該訊息」成功安裝*電腦名稱*「出現時，其中*電腦名稱*電腦時，您的網路原則伺服器的名稱。 按一下**關閉**。

##### <a name="BKMK_registerNPS"></a>NPS 伺服器登記預設網域中
您可以使用此程序位於網域中伺服器網域成員登記 NPS 伺服器。

NPS 伺服器必須登記完畢 Active Directory 中，讓他們使用的是讀取的帳號撥號屬性授權程序期間的權限。 登記 NPS 伺服器新增伺服器**RAS 及 IAS 伺服器]**群組中 Active Directory。

**管理認證**

若要完成此程序，您必須成員的**網域系統管理員**群組。

> [!NOTE]
> 若要執行此程序使用 Windows PowerShell 中的網路介面 (Netsh) 命令、開放 PowerShell 輸入下列命令，並再按下 ENTER。
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-server-in-its-default-domain"></a>若要在其預設網域登記 NPS 伺服器

1.  在 NPS1，在伺服器管理員中，按一下 [工具]，然後按一下**的網路原則伺服器**。 網路原則伺服器 MMC 開啟。

2.  以滑鼠右鍵按一下**NPS（本機）**，然後按一下 [**登記伺服器 Active Directory 中的**。 **的網路原則伺服器**對話方塊。

3.  在**的網路原則伺服器**，按一下 [ **[確定]**，，然後按一下 [ **[確定]**再試一次。

如需的網路原則伺服器，查看[的網路原則 Server (NPS)](../technologies/nps/nps-top.md)。

#### <a name="BKMK_IIS"></a>部署 WEB1

在 Windows Server 2016 網頁伺服器 (IIS) 角色提供安全，輕鬆管理、模組且最具擴充性的平台會可靠地裝載的網站、服務和應用程式。 使用網際網路資訊服務 (IIS)，您可以分享網際網路、內部網路，或外部網路使用者的資訊。 IIS 是整合 IIS、ASP.NET、FTP 服務、PHP 及 Windows 通訊基本知識 (WCF) 的整合的 web 平台。

網頁伺服器 (IIS) 伺服器角色除了可讓您存取 CRL 發行網域成員電腦，可讓您設定及管理多個網站、web 應用程式，以及 FTP 網站。 IIS 也提供下列優點：

-   最大化 web 透過減少的伺服器英呎列印和自動應用程式隔離的安全性。

-   輕鬆地部署與執行 ASP.NET、傳統型 ASP 和 PHP web 應用程式在相同的伺服器上。

-   隔離的應用程式提供同事處理程序的唯一身分和沙箱設定預設進一步減少安全性風險。

-   輕鬆地新增、移除及自訂模組，適用於客戶需求甚至取代建 IIS 元件。

-   來加速您透過建動態快取和美化的壓縮的網站。

您必須部署 WEB1，是執行的網頁伺服器 (IIS) 伺服器角色電腦，請執行下列動作：

-   一節中執行步驟[設定所有伺服器]](#BKMK_configuringAll)。

-   一節中執行步驟[的伺服器電腦加入網域並登入](#BKMK_joinlogserver)

-   [安裝網頁伺服器 (IIS) 伺服器角色](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>安裝網頁伺服器 (IIS) 伺服器角色
若要完成此程序，您必須成員的**系統管理員**群組。

> [!NOTE]
> 若要使用 Windows PowerShell 來執行這個程序，開放 PowerShell 輸入下列命令，並再按下 ENTER。
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  在**伺服器管理員**，按一下 [**管理**，然後按一下 [**新增角色與功能**。 新增角色與功能精靈開啟。

2.  在**在您開始之前，請先**，按一下 [**下**。

    > [!NOTE]
    > **在您開始之前，請先**頁面上新增角色與精靈中的功能不顯示如果先前已選取**預設略過此頁面**功能精靈與新增的角色執行。

3.  在**選擇安裝類型**頁面上，按一下 [**下**。

4.  在**選擇目的伺服器**頁面中，確定本機電腦已選取，然後按**下一步**。

5.  在**選擇伺服器角色**頁面上，捲動到 [，然後選取**網頁伺服器 (IIS)**。 **新增所需的網頁伺服器 (IIS) 功能**對話方塊。 按一下**新增功能**，然後按一下 [**下**。

6.  按一下**下一步**直到您接受預設的所有網頁伺服器設定，然後按一下 [**安裝**。

7.  安裝所有已成功，請確認，然後按一下**關閉**。

## <a name="BKMK_resources"></a>其他技術資源
如需本指南技術的詳細資訊，查看下列的資源：

 Windows Server 2016、Windows Server 2012 R2 和 Server 2012 技術文件庫的 Windows 資源

-   [Windows Server 2016 中的 Active Directory Domain Services (AD DS) 中的新功能](https://technet.microsoft.com/en-us/library/mt163897.aspx)

-   [Active Directory 網域服務概觀](https://technet.microsoft.com/library/hh831484.aspx)以https://technet.microsoft.com/library/hh831484.aspx。

-   [網域名稱系統」(DNS) 概觀](https://technet.microsoft.com/library/hh831667.aspx)以https://technet.microsoft.com/library/hh831667.aspx。

-   [執行 DNS 管理員角色](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [動態主機設定通訊協定」(DHCP) 概觀](https://technet.microsoft.com/library/hh831825.aspx)以https://technet.microsoft.com/library/hh831825.aspx。

-   [網路原則與服務存取概觀](https://technet.microsoft.com/library/hh831683.aspx)以https://technet.microsoft.com/library/hh831683.aspx。

-   [Web 伺服器 (IIS) 概觀](https://technet.microsoft.com/library/hh831725.aspx)以https://technet.microsoft.com/library/hh831725.aspx。

## <a name="BKMK_appendix"></a>透過電子附錄 A
下列章節包含額外的設定電腦正在執行 Windows Server 2016、Windows 10、Windows Server 2012 和 Windows 8 以外的作業系統資訊。 此外，可以協助您處理您的部署提供的網路準備試算表。

1.  [附錄 A-重新命名電腦](#BKMK_A)

2.  [附錄 B-設定靜態 IP 位址](#BKMK_B)

3.  [附錄 C-加入網域的電腦](#BKMK_C)

4.  [附錄 D-登入網域](#BKMK_D)

5.  [附錄 E-核心網路規劃準備工作表](#BKMK_E)

## <a name="BKMK_A"></a>附錄 A-重新命名電腦
您可以使用在本區段中程序，以提供使用不同的電腦名稱執行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的電腦。

-   [Windows Server 2008 R2 和 Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 和 Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 和 Windows 7
資格在**系統管理員**，或相當於，才能執行這些程序最小值。

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>若要重新命名執行 Windows Server 2008 R2 和 Windows 7 的電腦

1.  按一下**[開始]**，以滑鼠右鍵按一下**電腦**，然後按一下 [**屬性**。 **系統**對話方塊。

2.  在**電腦名稱、網域及工作群組設定**，按一下 [**變更設定**。 **系統屬性**對話方塊。

    > [!NOTE]
    > 在電腦上執行 Windows 7 之前**系統屬性**對話方塊，**使用者 Account 控制項**對話方塊，要求權限才能繼續。 按一下**繼續**以繼續。

3.  按一下**變更**。 **電腦名稱日網域變更**對話方塊。

4.  在**電腦名稱**，輸入您的電腦的名稱。 例如，如果您想要為電腦 DC1，輸入**DC1**。

5.  按一下**[確定]**兩次，按一下 [**關閉**，然後按一下 [**立即重新開機**重新開機。

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 和 Windows Vista
資格在**系統管理員**，或相當於，才能執行這些程序最小值。

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>若要重新命名電腦執行的 Windows Server 2008 和 Windows Vista

1.  按一下**[開始]**，以滑鼠右鍵按一下**電腦**，然後按一下 [**屬性**。 **系統**對話方塊。

2.  在**電腦名稱、網域及工作群組設定**，按一下 [**變更設定**。 **系統屬性**對話方塊。

    > [!NOTE]
    > 在電腦上執行 Windows Vista、前**系統屬性**對話方塊，**使用者 Account 控制項**對話方塊，要求權限才能繼續。 按一下**繼續**以繼續。

3.  按一下**變更**。 **電腦名稱日網域變更**對話方塊。

4.  在**電腦名稱**，輸入您的電腦的名稱。 例如，如果您想要為電腦 DC1，輸入**DC1**。

5.  按一下**[確定]**兩次，按一下 [**關閉**，然後按一下 [**立即重新開機**重新開機。

## <a name="BKMK_B"></a>附錄 B-設定靜態 IP 位址
本主題提供程序如何設定執行下列作業系統的電腦上的靜態 IP 位址：

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
資格在**系統管理員**，或相當於，才能執行此程序最小值。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>若要設定的電腦執行的 Windows Server 2008 R2 上的靜態 IP 位址

1.  按一下**[開始]**，然後按**[控制台]**。

2.  在**[控制台]**，按一下 [**網路和網際網路**。 **網路和網際網路**開啟。

    在**網路和網際網路**，按一下 [**網路和共用中心]**。 **網路和共用中心]**開啟。

3.  在**網路和共用中心]**，按一下 [**變更介面卡設定**。 **網路連接**開啟。

4.  在**裝置管理員**，以滑鼠右鍵按一下您想要設定，然後按一下 [網路**屬性**。

5.  在**本機區域連接屬性**，請在**此連接使用下列項目**、選取**網際網路通訊協定第 4 版本 (TCP 日 IPv4)**，，然後按一下**屬性**。 **網際網路通訊協定第 4 版本 (TCP 日 IPv4) 屬性**對話方塊。

6.  在**網際網路通訊協定第 4 版本 (TCP 日 IPv4) 屬性**，在**一般**索引標籤上，按一下 [**使用下列的 IP 位址**。 在**的 IP 位址**，輸入您想要使用 IP 位址。

7.  按] 索引標籤，將游標在**子網路遮罩**。 會自動輸入子網路遮罩的預設值。 接受預設子網路遮罩，或輸入您想要使用的子網路遮罩。

8.  在**預設閘道**，輸入您的預設閘道 IP 位址。

9. 在**慣用 DNS 伺服器]**，輸入您的 DNS 伺服器的 IP 位址。 如果您打算使用為慣用 DNS 伺服器的本機電腦，請輸入本機電腦的 IP 位址。

10. 在**其他 DNS 伺服器**，如果有的話，輸入您替代的 DNS 伺服器的 IP 位址。 如果您想要使用做為備用 DNS 伺服器的本機電腦，請輸入本機電腦的 IP 位址。

11. 按一下**[確定]**，然後按**關閉**。

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
資格在**系統管理員**，或相當於，才能執行這些程序最小值。

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>若要設定執行 Windows Server 2008 的電腦上的靜態 IP 位址

1.  按一下**[開始]**，然後按**[控制台]**。

2.  在**[控制台]**，確認**傳統檢視**已選取，然後按兩下 [**網路和共用中心]**。

3.  在**網路和共用中心]**，請在**工作**，按一下 [**管理網路連接**。

4.  在**裝置管理員**，以滑鼠右鍵按一下您想要設定，然後按一下 [網路**屬性**。

5.  在**本機區域連接屬性**，請在**此連接使用下列項目**、選取**網際網路通訊協定第 4 版本 (TCP 日 IPv4)**，，然後按一下**屬性**。 **網際網路通訊協定第 4 版本 (TCP 日 IPv4) 屬性**對話方塊。

6.  在**網際網路通訊協定第 4 版本 (TCP 日 IPv4) 屬性**，在**一般**索引標籤上，按一下 [**使用下列的 IP 位址**。 在**的 IP 位址**，輸入您想要使用 IP 位址。

7.  按] 索引標籤，將游標在**子網路遮罩**。 會自動輸入子網路遮罩的預設值。 接受預設子網路遮罩，或輸入您想要使用的子網路遮罩。

8.  在**預設閘道**，輸入您的預設閘道 IP 位址。

9. 在**慣用 DNS 伺服器]**，輸入您的 DNS 伺服器的 IP 位址。 如果您打算使用為慣用 DNS 伺服器的本機電腦，請輸入本機電腦的 IP 位址。

10. 在**其他 DNS 伺服器**，如果有的話，輸入您替代的 DNS 伺服器的 IP 位址。 如果您想要使用做為備用 DNS 伺服器的本機電腦，請輸入本機電腦的 IP 位址。

11. 按一下**[確定]**，然後按**關閉**。

## <a name="BKMK_C"></a>附錄 C-加入網域的電腦
若要加入的網域執行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的電腦，您可以使用下列程序。

-   [Windows Server 2008 R2 和 Windows 7](#BKMK_c1)

-   [Windows Server 2008 和 Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> 若要加入網域的電腦，您必須登入本機電腦，或者，如果您登入的電腦，而不需要本機電腦的系統管理員認證帳號，您必須提供認證本機電腦加入網域的程序期間。 此外，您必須使用者帳號您要將電腦加入網域中。 在的電腦加入網域過程中，將會提示您輸入您的網域 account 認證（的使用者名稱和密碼）。

### <a name="BKMK_c1"></a>Windows Server 2008 R2 和 Windows 7
資格在**網域使用者**，或相當於，才能執行此程序最小值。

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>若要加入的網域執行 Windows Server 2008 R2 和 Windows 7 的電腦

1.  登入本機電腦。

2.  按一下**[開始]**，以滑鼠右鍵按一下**電腦**，然後按一下 [**屬性**。 **系統**對話方塊。

3.  在**電腦名稱、網域及工作群組設定**，按一下 [**變更設定**。 **系統屬性**對話方塊。

    > [!NOTE]
    > 在電腦上執行 Windows 7 之前**系統屬性**對話方塊，**使用者 Account 控制項**對話方塊，要求權限才能繼續。 按一下**繼續**以繼續。

4.  按一下**變更**。 **電腦名稱日網域變更**對話方塊。

5.  在**電腦名稱**，請在**的成員**、選取**網域**，然後輸入您想要加入的網域名稱。 如果 corp.contoso.com 的網域名稱，例如，輸入**corp.contoso.com**。

6.  按一下**[確定]**。 **Windows 安全性**對話方塊。

7.  在**電腦名稱日網域變更**，請在**使用者名稱**，輸入使用者名稱，並在**密碼**，請輸入密碼，然後按一下**[確定]**。 **電腦名稱日網域變更**對話方塊，一則歡迎您的網域。 按一下**[確定]**。

8.  **電腦名稱日網域變更**對話方塊中，會顯示訊息表示，您必須重新開機，適用於所做的變更。 按一下**[確定]**。

9. 在**系統屬性**對話方塊中，於**電腦名稱**索引標籤上，按一下 [**關閉**。 **Microsoft Windows**對話方塊中開啟，且會顯示訊息，再試一次表示，您必須重新開機，適用於所做的變更。 按一下**現在重新**。

### <a name="BKMK_c2"></a>Windows Server 2008 和 Windows Vista
資格在**網域使用者**，或相當於，才能執行此程序最小值。

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>若要加入的網域執行 Windows Server 2008 和 Windows Vista 的電腦

1.  登入本機電腦。

2.  按一下**[開始]**，以滑鼠右鍵按一下**電腦**，然後按一下 [**屬性**。 **系統**對話方塊。

3.  在**電腦名稱、網域及工作群組設定**，按一下 [**變更設定**。 **系統屬性**對話方塊。

4.  按一下**變更**。 **電腦名稱日網域變更**對話方塊。

5.  在**電腦名稱**，請在**的成員**、選取**網域**，然後輸入您想要加入的網域名稱。 如果 corp.contoso.com 的網域名稱，例如，輸入**corp.contoso.com**。

6.  按一下**[確定]**。 **Windows 安全性**對話方塊。

7.  在**電腦名稱日網域變更**，請在**使用者名稱**，輸入使用者名稱，並在**密碼**，請輸入密碼，然後按一下**[確定]**。 **電腦名稱日網域變更**對話方塊，一則歡迎您的網域。 按一下**[確定]**。

8.  **電腦名稱日網域變更**對話方塊中，會顯示訊息表示，您必須重新開機，適用於所做的變更。 按一下**[確定]**。

9. 在**系統屬性**對話方塊中，於**電腦名稱**索引標籤上，按一下 [**關閉**。 **Microsoft Windows**對話方塊中開啟，且會顯示訊息，再試一次表示，您必須重新開機，適用於所做的變更。 按一下**現在重新**。

## <a name="BKMK_D"></a>附錄 D-登入網域
您可以使用下列程序網域使用執行 Windows Server 2008 R2、Windows 7、Windows Server 2008 和 Windows Vista 的電腦登入。

-   [Windows Server 2008 R2 和 Windows 7](#BKMK_d1)

-   [Windows Server 2008 和 Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 和 Windows 7
資格在**網域使用者**，或相當於，才能執行此程序最小值。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>登入以使用執行 Windows Server 2008 R2 或 Windows 7 的網域

1.  登入電腦，或重新開機。

2.  按下 CTRL + ALT + DELETE。 登入畫面顯示。

3.  按一下**切換使用者**，然後按**其他使用者**。

4.  在**的使用者名稱**，輸入您的網域和使用者名稱的格式*網域使用者*。 來登入網域 corp.contoso.com 名帳號，例如**使用者-01**，輸入**CORP\User-01**。

5.  在**密碼**、輸入您的網域密碼，然後按一下箭頭，或按下 ENTER。

### <a name="BKMK_d2"></a>Windows Server 2008 和 Windows Vista
資格在**網域使用者**，或相當於，才能執行此程序最小值。

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>登入以使用電腦執行的 Windows Server 2008 和 Windows Vista 的網域

1.  登入電腦，或重新開機。

2.  按下 CTRL + ALT + DELETE。 登入畫面顯示。

3.  按一下**切換使用者**，然後按**其他使用者**。

4.  在**的使用者名稱**，輸入您的網域和使用者名稱的格式*網域使用者*。 來登入網域 corp.contoso.com 名帳號，例如**使用者-01**，輸入**CORP\User-01**。

5.  在**密碼**、輸入您的網域密碼，然後按一下箭頭，或按下 ENTER。

## <a name="BKMK_E"></a>附錄 E-核心網路規劃準備工作表
您可以使用此網路規劃準備表收集安裝核心網路所需的資訊。 本主題提供包含個人設定項目，您必須提供的資訊或特定值安裝或設定程序期間各伺服器電腦的資料表。 每個設定的項目提供範例值。

規劃和追蹤用途，空格是您輸入用來部署的值為每個表格中所提供。 如果您登入時這些表格與安全性相關的值，您應該將資訊儲存在安全的位置。

下列連結，會導致本主題中的區段，可提供範例值這個節目表中顯示的部署程序相關聯的設定項目。

1.  [Active Directory Domain Services 和 DNS 安裝](#BKMK_FndtnPrep_InstallAD)

    -   [設定 DNS 反向對應區域](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [安裝 DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [建立排除範圍 dhcp](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [建立新的 DHCP 領域](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [安裝網路原則伺服器（選擇性）](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Active Directory Domain Services 和 DNS 安裝
在本區段中表格列出設定項目預先安裝並安裝 Active Directory Domain Services (AD DS) 和 DNS。

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>AD DS 和 DNS 預先安裝設定項目
下列表格清單預先安裝的設定項目中所述[設定所有伺服器]](#BKMK_configuringAll):

-   [設定靜態 IP 位址](#BKMK_ip)

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|IP 位址|10.0.0.2||
|子網路遮罩|255.255.255.0||
|預設閘道|10.0.0.1||
|慣用的 DNS 伺服器|127.0.0.1||
|其他 DNS 伺服器|10.0.0.15||

-   [將電腦重新命名](#BKMK_rename)

|設定項目|範例值。|值。|
|----------------------|-----------------|---------|
|電腦名稱|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>AD DS 和 DNS 安裝設定項目
設定項目適用於 Windows Server Core 網路部署程序[安裝 AD DS 和 DNS 新的樹系的](#BKMK_installAD-DNS):

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|完整的 DNS 名稱|corp.contoso.com||
|森林功能層級|Windows Server 2003||
|Active Directory Domain Services 資料庫資料夾位置|E:\Configuration\\<br /><br />或接受預設的位置。||
|Active Directory Domain Services 登入檔案的資料夾位置|E:\Configuration\\<br /><br />或接受預設的位置。||
|Active Directory Domain Services SYSVOL 資料夾位置|E:\Configuration\\<br /><br />或接受預設的位置||
|Directory 還原模式系統管理員密碼|J * p2leO4$ F||
|回應檔案名稱（選擇性）|廣告 DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>設定 DNS 反向對應區域

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|時區類型：|-主要區域<br />-次要區域<br />-Stub 區域||
|輸入區<br /><br />**在 Active Directory 中存放區**|選取<br />-未選取||
|Active Directory 區域複寫領域|-到此森林中的所有 DNS 伺服器<br />在這個網域中的所有 DNS 伺服器地<br />對所有網域控制站在這個網域中<br />所有網域控制站在這個 directory 磁碟分割的範圍中指定地||
|反向尋找區域名稱<br /><br />（IP 鍵入）|-IPv4 反向對應區域<br />-IPv6 反向對應區域||
|反向尋找區域名稱<br /><br />(網路 ID)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>安裝 DHCP
在本區段中表格列出預先安裝並安裝 DHCP 設定項目。

##### <a name="pre-installation-configuration-items-for-dhcp"></a>預先安裝的組態 DHCP 的項目
下列表格清單預先安裝的設定項目中所述[設定所有伺服器]](#BKMK_configuringAll):

-   [設定靜態 IP 位址](#BKMK_ip)

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|IP 位址|10.0.0.3||
|子網路遮罩|255.255.255.0||
|預設閘道|10.0.0.1||
|慣用的 DNS 伺服器|10.0.0.2||
|其他 DNS 伺服器|10.0.0.15||

-   [將電腦重新命名](#BKMK_rename)

|設定項目|範例值。|值。|
|----------------------|-----------------|---------|
|電腦名稱|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>DHCP 安裝設定項目
設定項目適用於 Windows Server Core 網路部署程序[安裝動態主機設定通訊協定 (DHCP)](#BKMK_installDHCP):

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|網路連接繫結|乙太網路||
|DNS 伺服器設定|DC1||
|慣用的 DNS 伺服器的 IP 位址|10.0.0.2||
|其他 DNS 伺服器的 IP 位址|10.0.0.15||
|範圍名稱|Corp1||
|開始 IP 位址|10.0.0.1||
|結束 IP 位址|10.0.0.254||
|子網路遮罩|255.255.255.0||
|預設閘道（選擇性）|10.0.0.1||
|租用期間|8 天||
|IPv6 DHCP 伺服器操作模式|不支援||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>建立排除範圍 dhcp
設定項目時，建立範圍 dhcp 建立排除範圍。

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|範圍名稱|Corp1||
|範圍描述|主要辦公室子網路 1||
|排除項目範圍 [開始] 畫面的 IP 位址|10.0.0.1||
|排除項目範圍結束 IP 位址|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>建立新的 DHCP 領域
設定項目適用於 Windows Server Core 網路部署程序[建立及啟動 DHCP 新的領域](#BKMK_newscopeDHCP):

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|新的領域名稱|Corp2||
|範圍描述|主要辦公室子網路 2||
|(Ip)<br /><br />[開始] 畫面的 IP 位址|10.0.1.1||
|(Ip)<br /><br />結束 IP 位址|10.0.1.254||
|長度|8||
|子網路遮罩|255.255.255.0||
|（排除項目範圍）[開始] 畫面的 IP 位址|10.0.1.1||
|排除項目範圍結束 IP 位址|10.0.1.15||
|租用期間<br /><br />天<br /><br />小時<br /><br />分鐘|-   8<br />-   0<br />-   0||
|路由器（預設閘道）<br /><br />IP 位址|10.0.1.1||
|家長的 DNS 網域|corp.contoso.com||
|DNS 伺服器<br /><br />IP 位址|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>安裝網路原則伺服器（選擇性）
在本區段中表格列出預先安裝並安裝 NPS 設定項目。

##### <a name="pre-installation-configuration-items"></a>預先安裝的設定項目
下列三種表格列出預先安裝的設定項目中所述[設定所有伺服器]](#BKMK_configuringAll):

-   [設定靜態 IP 位址](#BKMK_ip)

|設定項目|範例值|值|
|-----------------------|------------------|----------|
|IP 位址|10.0.0.4||
|子網路遮罩|255.255.255.0||
|預設閘道|10.0.0.1||
|慣用的 DNS 伺服器|10.0.0.2||
|其他 DNS 伺服器|10.0.0.15||

-   [將電腦重新命名](#BKMK_rename)

|設定項目|範例值。|值。|
|----------------------|-----------------|---------|
|電腦名稱|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>網路原則伺服器安裝設定項目
設定項目適用於 Windows Server Core 網路 NPS 部署程序[安裝網路原則 Server (NPS)](#BKMK_installNPS)和[登記 NPS 伺服器預設網域中](#BKMK_registerNPS)。

-   安裝和登記 NPS 需要額外的設定項目。

