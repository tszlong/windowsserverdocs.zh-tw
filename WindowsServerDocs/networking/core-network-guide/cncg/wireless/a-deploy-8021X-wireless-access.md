---
title: 部署密碼型 802.1X 驗證無線存取
description: 本主題是 Windows Server 2016 網路功能指南 」 部署密碼型的 802.1 X 驗證無線存取 」 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f34580f4a0fd92c6f059d19d09a6926fc2775960
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840009"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>部署密碼\-型 802.1x 驗證無線存取

>適用於：Windows Server （半年通道），Windows Server 2016

這是 Windows Server 的附屬指南&reg;2016年核心網路指南 》。 核心網路指南提供規劃和部署全功能網路與新樹系中的新 Active Directory® 網域所需元件的指示。

本指南說明如何建置在核心網路時，所提供的指示，有關如何部署美國電機暨電子工程師\(IEEE\) 802.1x\-驗證 IEEE 802.11 無線存取使用受保護的可延伸驗證通訊協定 – Microsoft Challenge Handshake 驗證通訊協定第 2 版\(PEAP\-MS\-MS-CHAP v2\)。

因為 PEAP\-MS\-MS-CHAP v2 可讓您要求使用者提供密碼\-根據的認證而不是不是驗證程序期間的憑證，它是通常更容易、 成本較低部署比 EAP\-TLS 或 PEAP\-TLS。

>[!NOTE]
>在本指南中，IEEE 802.1x 驗證無線存取與 PEAP\-MS\-MS-CHAP v2 縮寫為 「 無線存取 」 和 「 WiFi 存取。 」

## <a name="about-this-guide"></a>關於本指南
本指南中，結合，如下所述的必要條件指南提供有關如何部署下列部署 WiFi 存取基礎結構的指示。  

- 一或多個 802.1 X\-支援的 802.11 無線存取點\(APs\)。

- Active Directory 網域服務\(AD DS\)使用者和電腦。

- 群組原則管理。

- 一或多個網路原則伺服器\(NPS\)伺服器。

- 執行 NPS 之電腦的伺服器憑證。

- 無線用戶端電腦執行 Windows&reg; 10，Windows 8.1 或 Windows 8。

### <a name="dependencies-for-this-guide"></a>本指南中的相依性

若要成功部署使用本指南的已驗證無線網路連線，您必須具有網路與網域環境所需的技術部署的所有。 您也必須擁有伺服器憑證部署到您的驗證 NPSs。

下列各節會提供說明如何部署這些技術的文件的連結。

#### <a name="network-and-domain-environment-dependencies"></a>網路與網域環境相依性

本指南專為網路和系統管理員已遵循 Windows Server 2016 中的指示**核心網路指南**部署核心網路，或先前已部署的核心技術的人包含在核心網路，包括 AD DS 中，網域名稱系統\(DNS\)，動態主機設定通訊協定\(DHCP\)，TCP\/IP、 NPS 以及 Windows 網際網路名稱服務\(WINS\)。  

核心網路指南位於下列位置：

- Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)位於 Windows Server 2016 技術文件庫。 

- [核心網路指南](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)也會提供以 Word 格式，在 TechNet 資源庫，在[ https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683 ](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

#### <a name="server-certificate-dependencies"></a>伺服器憑證的相依性
有兩個可用選項的註冊伺服器憑證用於 802.1x 驗證-驗證伺服器使用 Active Directory 憑證服務來部署您自己公開金鑰基礎結構\(AD CS\)或使用公開憑證授權單位所註冊的伺服器憑證\(CA\)。

##### <a name="ad-cs"></a>AD CS
已驗證的無線部署的網路和系統管理員必須遵循在 Windows Server 2016 核心網路附屬指南，指示**部署 802.1x 有線和無線部署的伺服器憑證**。 本指南說明如何部署和使用 AD CS，將執行 NPS 之電腦的自動註冊伺服器憑證。

本指南可在下列位置。

- Windows Server 2016 核心網路附屬指南[部署 802.1x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396)技術文件庫中 HTML 格式。 

##### <a name="public-ca"></a>公用 CA
您可以購買伺服器憑證，從公用 CA，例如 VeriSign，用戶端電腦已經信任。 

用戶端電腦會信任 CA 的 CA 憑證安裝在受信任的根憑證授權單位憑證存放區時。 根據預設，執行 Windows 的電腦會有多個公用 CA 憑證安裝在其信任的根憑證授權單位憑證存放區。  

建議您檢閱此部署案例中所用每個技術的設計和部署指南。 這些指南可協助您判斷此部署案例是否提供貴組織的網路需要的服務與設定。

### <a name="requirements"></a>需求
以下是使用此指南中所述的案例中部署無線存取基礎結構的需求：

- 在部署此案例中，您必須先購買 802.1x\-無線存取點，以提供所需的位置，在您的網站中的無線涵蓋範圍。 本指南的規劃一節可協助判斷您的 Ap 必須支援的功能。

- Active Directory 網域服務\(AD DS\)已安裝，正如其他的所需的網路技術，根據 Windows Server 2016 核心網路指南中的指示。  

- 部署 AD CS 時，並向 NPSs 註冊伺服器憑證。 這些憑證是必要的當您部署 PEAP\-MS\-CHAP v2 憑證\-基礎本指南中使用的驗證方法。

- 您組織的成員是熟悉您的無線 Ap 和無線網路配接器安裝在用戶端電腦和網路上的裝置所支援的 IEEE 802.11 標準。 例如，您組織中有人已熟悉使用無線電頻率類型，802.11 無線驗證\(WPA2 或 WPA\)，和編碼器\(AES] 或 [TKIP\)。

## <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容  
以下是本指南不提供某些項目：

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>選取 802.1x 的完整指導\-無線存取點

因為品牌和機型的 802.1x 之間存在的許多差異\-無線 Ap，本指南並未提供詳細的資訊，關於：  

- 判斷哪一個品牌或型號的無線 AP 適合您的需求。

- 在您網路上的無線 Ap 實際的部署。

- 無線 AP 設定，例如進階無線虛擬區域網路\(Vlan\)。

- 有關如何設定的無線 AP 廠商\-NPS 中的特定屬性。

此外，術語和名稱設定的無線 AP 品牌和模型，而有所不同，而且可能不符合此指南中使用泛型的設定名稱。 無線 AP 設定詳細資料，您必須檢閱您的無線 Ap 的製造商所提供的產品文件。

### <a name="instructions-for-deploying-nps-certificates"></a>部署 NPS 憑證的指示
  
有兩個部署 NPS 憑證的替代方式。 本指南不提供完整的指引，可協助您判斷哪一個替代方案最符合您的需求。 一般情況下，不過，您所面對的選項如下：

- 從公用 CA，例如 VeriSign，購買憑證，已經信任的 Windows\-架構用戶端。 較小的網路通常建議使用此選項。

- 部署公開金鑰基礎結構\(PKI\)使用 AD CS 網路上。 這建議用於大部分的網路，並使用先前所述的部署指南中的指示如何部署使用 AD CS 的伺服器憑證。

### <a name="nps-network-policies-and-other-nps-settings"></a>NPS 網路原則和其他 NPS 設定

當您執行時所做的組態設定除外**設定 802.1x**精靈，本指南中所述，本指南不提供手動設定 NPS 條件、 條件約束或其他 NPS 的詳細的資訊設定。

### <a name="dhcp"></a>DHCP

此部署指南不提供設計或部署無線區域網路的 DHCP 子網路的相關資訊。

## <a name="technology-overviews"></a>技術概觀
以下是部署無線存取的技術概觀：

### <a name="ieee-8021x"></a>IEEE 802.1X
IEEE 802.1x 標準定義的連接埠\-型用來提供乙太網路的已驗證的網路存取的網路存取控制。 此連接埠\-架構的網路存取控制利用切換式 LAN 基礎結構的實體特性來驗證連接到 LAN 連接埠的裝置。 如果驗證處理程序失敗，將拒絕存取這個連接埠。 雖然這個標準設計用於有線乙太網路，已經過適用於 802.11 無線 Lan。

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1x\-無線存取點\(Ap\)
此案例中需要部署一或多個 802.1 X\-能夠與遠端驗證撥相容的無線 Ap\-使用者服務\(RADIUS\)通訊協定。

802.1x 和 RADIUS\-相容的 Ap，搭配如 NPS RADIUS 伺服器使用 RADIUS 基礎結構中部署時稱為*RADIUS 用戶端*。

### <a name="wireless-clients"></a>無線用戶端
本指南提供全方位的組態詳細資料，來提供 802.1x 驗證存取網域\-與執行 Windows 10，Windows 8.1 和 Windows 8 的無線用戶端電腦連線到網路的成員使用者。 電腦必須加入網域，才能成功建立的已驗證的存取權。

>[!NOTE]
>您也可以使用執行 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 做為無線用戶端的電腦。

### <a name="support-for-ieee-80211-standards"></a>Ieee 802.11 標準的支援
支援的 Windows 和 Windows Server 作業系統提供內建\-支援的 802.11 無線網路。 在這些作業系統中，已安裝的 802.11 無線網路配接器會顯示為網路和共用中心中的無線網路連線。 

雖然那里內建\-802.11 無線網路支援，Windows 無線元件會相依於下列：

- 無線網路配接器的功能。 已安裝的無線網路介面卡必須支援無線區域網路或您所需要的無線網路安全性標準。 例如，如果無線網路介面卡不支援 Wi-fi\-Wi-fi Protected Access \(WPA\)，您無法啟用或設定 WPA 安全性選項。

- 無線網路介面卡驅動程式的功能。 若要允許您設定無線網路的選項，無線網路介面卡的驅動程式必須報告其功能的所有 Windows 所都支援的。 請確認您的無線網路介面卡的驅動程式會寫入您的作業系統的功能。 也請確定該驅動程式是最新版本，藉由檢查 Microsoft Update 或無線網路介面卡廠商的網站。

下表顯示的傳輸速率和常見的 IEEE 802.11 無線標準的頻率。

|標準|頻率|位元的傳輸速率|使用量|
|-------------|---------------|--------------------------|---------|  
|802.11|S\-頻外工業、 科學，以及醫療\(ISM\)頻率範圍\(至 2.5 2.4 GHz\)|每秒 2 mb \(Mbps\)|已過時。 不常使用。|  
|802.11b|S\-ISM 以頻外方式|11 Mbps|常用。|  
|802.11a|C\-ISM 以頻外方式\(5.725 至 5.875 GHz\)|54 Mbps|不常用的費用和有限範圍內到期。|  
|802.11g|S\-ISM 以頻外方式|54 Mbps|廣泛使用。 802.11g 裝置可與 802.11b 相容裝置。|  
|802.11n \2.4 和 5.0 GHz|C\-頻外和 S\-ISM 以頻外方式|250 Mbps|裝置會根據前置\-年底正式批准 IEEE 802.11 標準推出是在 2007 年 8 月。 許多的 802.11 裝置可與 802.11a，相容 b 和 g 裝置。|
|802.11ac |5 GHz |6.93 Gbps |802.11ac，IEEE 核准在 2014 年，是更靈活也比 802.11n，更快，且部署其中 Ap 和無線用戶端都支援它。|

### <a name="wireless-network-security-methods"></a>無線網路安全性方法
**無線網路安全性方法**是無線驗證的非正式分組\(有時稱為無線安全性\)和無線安全性加密。 無線驗證與加密會在配對來防止未經授權的使用者存取的無線網路，並保護無線傳輸。 

當設定無線網路安全性設定的無線網路原則的群組原則中時，有多個的組合，可從中選擇。 不過，只有 WPA2\-企業版、 WPA\-Enterprise 和 Open with 802.1x 驗證標準支援 802.1 X 驗證的無線部署。

>[!NOTE]
>在設定無線網路原則時，您必須選取**WPA2\-企業**， **WPA\-Enterprise**，或**開啟 802.1x**中才能取得存取權的 EAP 設定所需的 802.1x 驗證無線部署。  

#### <a name="wireless-authentication"></a>無線驗證
本指南建議使用下列的無線驗證標準 802.1x 驗證無線部署。  

**Wi\-Wi-fi 保護的存取 – Enterprise \(WPA\-Enterprise\)**  wpa 鎯  WiFi 聯盟，以符合 802.11 無線安全性通訊協定所開發中期標準。 WPA 通訊協定所開發以回應上述有線等位私密中探索到的嚴重錯誤的數目\(WEP\)通訊協定。

WPA\-企業透過 WEP 所提供改善的安全性：  

1. 需要驗證，以確保集中化的相互驗證和動態的金鑰管理基礎結構的一部分使用 802.1x 的 X EAP framework  

2. 增強完整性檢查值\(ICV\)與訊息完整性檢查\(MIC\)，若要保護的標頭和內容  

3. 實作框架計數器，以防止重新執行攻擊  

**Wi\-Wi-fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)** 像 WPA\-Enterprise standard、 WPA2\-企業使用 802.1x 和 EAP 架構。 WPA2\-企業提供更強的資料保護多個使用者和大型的受管理網路。 WPA2\-Enterprise 是強大的通訊協定，為了防止未經授權的網路存取驗證透過驗證伺服器的網路使用者。  

#### <a name="wireless-security-encryption"></a>無線安全性加密
無線安全性加密來保護傳輸與無線用戶端與無線 AP 之間傳送的無線。 搭配使用無線網路安全性加密與所選的網路安全性驗證方法。 根據預設，執行 Windows 10，Windows 8.1 和 Windows 8 的電腦會支援兩個加密的標準：

1. **暫時金鑰完整性通訊協定** \(TKIP\)是較舊的加密通訊協定，則原本設計為提供比原本就是弱式有線等位私密所提供更安全的無線加密\(WEP\)通訊協定。 IEEE 802.11i 已設計 [tkip] 工作群組和 Wi-fi\-Wi-fi Alliance 來取代 WEP，而不需要傳統的硬體取代。 [Tkip] 是一套演算法，以封裝 WEP 承載，而且可讓使用者的舊版 WiFi 設備可以升級到 TKIP 無須更換硬體。 像 WEP，TKIP 會做為基礎，使用 RC4 資料流加密演算法。 新的通訊協定，不過，每個資料封包會使用加密，唯一的加密金鑰，，而且遠高於這些 WEP 金鑰。 雖然 TKIP 適合用來升級較舊的裝置已設計成使用只有 WEP 的安全性，但它不會處理所有安全性問題，無線區域網路，而且在大部分情況下不夠穩固來保護機密的政府或公司資料傳輸。  

2. **進階加密標準** \(AES\)是慣用的加密通訊協定，商業及政府的資料加密。 AES 提供比 TKIP 或 WEP 高層級的無線傳輸安全性。 不同於 TKIP 和 WEP，AES 需要支援 AES 標準的無線硬體。 是對稱的 AES\-金鑰加密標準使用三個區塊編碼器，AES\-128，AES\-192 和 AES\-256。

在 Windows Server 2016 中，下列的 AES\-無線為基礎的加密方法都適用無線設定檔屬性中的組態，當您選取驗證方法的 WPA2\-企業，建議使用。

1. **AES\-CCMP**。 計數器模式加密區塊鏈結訊息驗證碼通訊協定\(CCMP\)實作 802.11i 標準和專為比 WEP，所提供還要更高的安全性加密，並使用 128 位元 AES 加密金鑰。
2. **AES\-GCMP**。 Galois 計數器模式通訊協定\(GCMP\)支援 802.11ac、 更有效率，比 AES\-CCMP，並提供更佳的效能，無線用戶端。 GCMP 會使用 256 位元 AES 加密金鑰。

> [!IMPORTANT]
> 有線等位私密\(WEP\)是原始的無線網路安全性標準，用來加密網路流量。 您應該不 WEP 在網路上部署因為有確實\-已知在這種安全性的過時形式的安全性弱點。

### <a name="active-directorydoman-services-adds"></a>Active Directory 網域服務\(AD DS\)
AD DS 提供分散式的資料庫來儲存和管理網路資源和應用程式的相關資訊\-目錄中的特定資料\-功能的應用程式。 系統管理員可以使用 AD DS 將網路項目 (像是使用者、電腦以及其他裝置) 組織成階層式的內含項目結構。 階層式內含項目結構包含 Active Directory 樹系中的網域樹系，以及組織單位\(Ou\)每個網域中。 執行 AD DS 的伺服器稱為*網域控制站*。  

AD DS 包含使用者帳戶、 電腦帳戶和帳戶內容所需的 IEEE 802.1x 與 PEAP\-MS\-MS-CHAP v2 來驗證使用者認證，並評估授權的無線連線。

### <a name="active-directory-users-and-computers"></a>Active Directory 使用者和電腦
Active Directory 使用者和電腦是元件的 AD DS 中包含代表實體的實體，例如電腦、 個人、 群組或安全性群組帳戶。 A*安全性群組*是系統管理員可以為單一單位管理的使用者或電腦帳戶的集合。 屬於特定群組的使用者和電腦帳戶指*群組成員*。  

### <a name="group-policy-management"></a>群組原則管理
群組原則管理 可讓目錄\-基礎變更和組態管理的使用者和電腦的設定，包括安全性和使用者的資訊。 您可以使用群組原則來定義群組的使用者和電腦組態。 使用群組原則，您可以指定登錄項目、 安全性、 軟體安裝、 指令碼、 資料夾重新導向、 遠端安裝服務，以及 Internet Explorer 維護設定。 群組原則物件中包含您所建立的群組原則設定\(GPO\)。 由關聯所選取的 Active Directory 系統容器中的 GPO — 網站、 網域及 Ou — 您可以將 GPO 的設定套用至使用者和電腦的 Active Directory 容器中。 若要管理整個企業的群組原則物件，您可以使用群組原則管理編輯器 Microsoft Management Console \(MMC\)。  

本指南提供有關如何在無線網路中指定設定的詳細的指示\(IEEE 802.11\)原則延伸的群組原則管理。 無線網路\(IEEE 802.11\)原則設定網域\-成員無線用戶端電腦必要的連線與無線設定 802.1 X 驗證無線存取。

### <a name="server-certificates"></a>伺服器憑證
此部署案例中需要執行 802.1x 驗證每個 NPS 伺服器憑證。  

伺服器憑證是數位文件，通常用於驗證以及在開放網路上的安全資訊。 憑證將公開金鑰安全地繫結到保存對應私密金鑰的實體。 憑證進行數位簽署所發行的 CA，並可以發給使用者、 電腦或服務。  

憑證授權單位\(CA\)是負責建立和證明屬於主體的公開金鑰的真實性的實體\(通常是使用者或電腦\)或其他 Ca。 憑證授權單位的活動可以包含透過簽署的憑證、 管理憑證序號以及撤銷憑證的辨別名稱將公開金鑰繫結。  

Active Directory 憑證服務\(AD CS\)是做為網路 CA 發行憑證的伺服器角色。 AD CS 憑證基礎結構，也稱為*公開金鑰基礎結構\(PKI\)*，提供自訂的服務，用於發行和管理企業的憑證。

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP，PEAP 與 PEAP\-MS\-MS-CHAP v2
可延伸驗證通訊協定\(EAP\)擴充點\-要\-點的通訊協定\(PPP\)藉由使用認證和資訊的其他驗證方法任意長度的交換。 利用 EAP 驗證，這兩個網路存取用戶端與驗證器\(例如 NPS\)必須支援相同的 EAP 類型才能成功完成驗證發生。 Windows Server 2016 包含一個 EAP 基礎結構，支援兩種 EAP 類型，以及傳送 EAP 訊息至 NPSs 的能力。 藉由使用 EAP，您可以支援額外的驗證配置，稱為*EAP 類型*。 Windows Server 2016 支援的 EAP 類型是：  

- 傳輸層安全性\(TLS\)

- Microsoft Challenge Handshake 驗證通訊協定第 2 版\(MS\-MS-CHAP v2\)

>[!IMPORTANT]
>強式 EAP 類型\(例如以憑證為基礎\)提供更佳安全性，以防止暴力密碼破解\-強制攻擊、 字典攻擊和密碼猜測攻擊比密碼\-基礎驗證通訊協定\(例如 CHAP 或 MS\-CHAP 第 1 版\)。

受保護的 EAP \(PEAP\)使用 TLS 來建立驗證 PEAP 用戶端，如無線電腦和 PEAP 驗證器，例如 NPS 或其他 RADIUS 伺服器之間的加密的通道。 PEAP 不指定驗證方法，但是它提供額外的安全性給其他 EAP 驗證通訊協定\(例如 EAP\-MS\-MS-CHAP v2\) TLS 加密通道可以運作透過 PEAP 提供。 PEAP 可做為驗證方法透過連接到您的組織網路下列類型的網路存取伺服器的存取用戶端\(Nas\):

- 802.1x\-無線存取點

- 802.1x\-能夠驗證交換器

- 執行 Windows Server 2016 和遠端存取服務的電腦\(RAS\)設定為虛擬私人網路\(VPN\)伺服器、 DirectAccess 伺服器，或兩者

- 執行 Windows Server 2016 和遠端桌面服務的電腦

PEAP\-MS\-MS-CHAP v2 是您更輕鬆地部署比 EAP\-TLS 執行使用者驗證使用的密碼因為\-基礎認證\(使用者名稱和密碼\)，而不是憑證或智慧卡。 只有 NPS 或其他 RADIUS 伺服器，才能擁有的憑證。 NPS 憑證是由 NPS 中，驗證程序期間用來對 PEAP 用戶端證明其識別身分。

本指南提供指示來設定無線用戶端與您的 NPS\(s\)搭配使用 PEAP\-MS\-MS-CHAP v2，802.1 X 驗證的存取。

### <a name="network-policy-server"></a>網路原則伺服器
網路原則伺服器\(NPS\)可讓您集中設定和管理網路原則，使用遠端驗證撥\-使用者服務\(RADIUS\)伺服器與 RADIUS proxy。 當您部署 802.1x 無線存取需要 NPS。

當您設定您 802.1x 無線存取點做為 NPS 中的 RADIUS 用戶端時，NPS 會處理 APs 所傳送的連線要求。 連接要求在處理期間，NPS 會執行驗證和授權。 驗證會判斷用戶端是否已提供有效的認證。 如果成功，NPS 會驗證要求的用戶端，NPS 會判斷用戶端獲授權對要求的連線，並允許或拒絕連線。 這是更詳細說明，如下所示：

#### <a name="authentication"></a>驗證

成功的相互 PEAP\-MS\-CHAP v2 驗證有兩個主要部分：

1.  用戶端驗證 NPS。  在這個階段中的相互驗證中，NPS 會將其伺服器憑證傳送至用戶端電腦，以便用戶端可以驗證 NPS 的身分識別的憑證。 若要成功地驗證 NPS，用戶端電腦必須信任發出 NPS 憑證的 CA。 用戶端電腦上的受信任的根憑證授權單位憑證存放區中 CA 的憑證時，用戶端會信任此 CA。

    如果您部署您自己的私人 CA 時，會自動安裝的 CA 憑證的受信任的根憑證授權單位憑證存放區目前的使用者和本機電腦的網域成員用戶端電腦上重新整理群組原則。 如果您決定要部署從公用 CA 的伺服器憑證，請確定公用 CA 憑證已在受信任的根憑證授權單位憑證存放區。  

2.  NPS 驗證使用者。 用戶端順利驗證 NPS 之後，用戶端會傳送使用者的密碼\-基礎 NPS 中，確認使用者的認證，對 Active Directory 網域服務中的使用者帳戶資料庫的認證\(ADDS\)。

如果是有效的認證和驗證成功時，NPS 就會開始處理連線要求的授權階段。 如果不是有效的認證和驗證失敗時，NPS 會傳送拒絕存取訊息，並連接要求遭到拒絕。  

#### <a name="authorization"></a>Authorization

執行 NPS 的伺服器會執行授權，如下所示：  

1. NPS 檢查是否有限制的使用者或電腦帳戶的撥\-在 AD DS 中的屬性。 在 [Active Directory 使用者和電腦的每個使用者和電腦帳戶包含多個屬性，包括上找到**撥\-在**] 索引標籤。此索引標籤上，在**網路存取權限**，如果值是**允許存取**，授權使用者或電腦是連線到網路。 如果值為**拒絕存取**，使用者或電腦未獲授權可連線到網路。 如果值為**透過 NPS 網路原則控制存取**，NPS 會評估設定的網路原則，以判斷是否授權使用者或電腦連線到網路。 

2. 然後 NPS 會處理其網路原則，以尋找符合連線要求原則。 如果找到相符的原則，NPS 會授與或拒絕根據該原則設定的連接。  

如果驗證和授權都順利進行，而且相符網路原則授與存取權，NPS 會授與存取網路，和使用者和電腦可以連線到網路資源，他們擁有的權限。  

>[!NOTE]
>若要部署無線存取，您必須設定 NPS 原則。 本指南提供使用指示**設定 802.1 X 精靈**中若要建立 NPS 原則，802.1 X 驗證無線存取的 NPS。  

### <a name="bootstrap-profiles"></a>啟動程序的設定檔
802.1x\-驗證的無線網路，無線用戶端必須提供由 RADIUS 伺服器驗證才能連接到網路的安全性認證。 受保護的 eap \[PEAP\]\-Microsoft Challenge Handshake 驗證通訊協定第 2 版\[MS\-MS-CHAP v2\]，安全性認證是使用者名稱和密碼。 Eap\-傳輸層安全性\[TLS\]或 PEAP\-TLS 安全性認證是憑證，例如用戶端使用者和電腦憑證或智慧卡。

當連線到網路設定為執行 PEAP\-MS\-MS-CHAP v2，PEAP\-TLS 或 EAP\-TLS 驗證，根據預設，Windows 無線用戶端也必須驗證的電腦憑證由 RADIUS 伺服器傳送。 RADIUS 伺服器所傳送的每個驗證工作階段的電腦憑證通常稱為 「 伺服器憑證。

如先前所述，您可以發出 RADIUS 伺服器其伺服器憑證中有兩種： 從商業 CA \(VeriSign，Inc.，例如\)，或從您在網路部署的私人 CA。 如果 RADIUS 伺服器傳送電腦憑證已安裝在用戶端的受信任的根憑證授權單位憑證存放區的根憑證的商業 CA 所發出，則無線用戶端可以驗證 RADIUS 伺服器電腦憑證，不論無線用戶端是否加入 Active Directory 網域。 在此情況下，無線用戶端可以連線到無線網路，，然後您可以將電腦加入網域。

>[!NOTE]
>可以停用要求用戶端驗證伺服器憑證的行為，但在生產環境中不建議停用伺服器憑證驗證。

啟動程序的無線設定檔是一種讓連線至 802.1x 的無線用戶端使用者設定的暫存設定檔\-驗證的無線網路，才能將電腦加入網域，以及\/或之前使用者已成功登入網域使用指定的無線電腦第一次。  此區段會摘列時，嘗試將無線電腦加入網域，或使用網域使用者遇到什麼問題\-聯結的無線電腦第一次登入網域。

部署所在的使用者或 IT 系統管理員無法實際將電腦連線到有線的乙太網路，將電腦加入網域，以及電腦沒有必要的發出根 CA 憑證安裝在其**受信任的根憑證授權單位**憑證存放區，您可以使用暫存的無線連線設定檔，稱為 「 設定無線用戶端*啟動設定檔*，以連線到無線網路。

A*啟動設定檔*，就不需要驗證 RADIUS 伺服器的電腦憑證。 此暫存組態可讓無線使用者將電腦加入網域，在這段網路無線\(IEEE 802.11\)會套用原則，並在自動安裝適當的根 CA 憑證電腦。

電腦上，套用群組原則時，會套用強制執行相互驗證需求的一或多個無線連線設定檔啟動程序的設定檔已不再需要，並移除。 將電腦加入網域並重新啟動電腦之後, 使用者就可以使用無線連線來登入網域。

如需使用這些技術的無線存取部署程序的概觀，請參閱 <<c0> [ 無線存取部署概觀](b-wireless-access-deploy-overview.md)。
