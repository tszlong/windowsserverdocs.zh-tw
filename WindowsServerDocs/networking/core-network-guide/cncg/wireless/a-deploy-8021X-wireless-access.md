---
title: 部署密碼為基礎的 802.1 X 驗證 Wireless 存取
description: 本主題是 Windows Server 2016 網路指南「部署密碼基礎 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ded8a273a9ad464b44fa7245db58d0fd05f06a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>部署 Password\ 為基礎的 802.1 X 驗證 Wireless 存取

>適用於：Windows Server（以每年次管道）、Windows Server 2016

這是 Windows Server 小幫手指南&reg;2016 年核心網路指南。 核心網路指南指示計畫和部署正常運作的網路和新 Active Directory® 網域中新的樹系的必要元件。

本指南如何建置提供有關如何將協會和電子工程師 \(IEEE\) 802.1X\ 部署核心網路-驗證 IEEE 802.11 wireless 存取使用受延伸驗證通訊協定 – Microsoft 挑戰交換驗證通訊協定第 2 \ (PEAP\-MS\-CHAP v2\)。

PEAP\-MS\-CHAP v2 需要使用者驗證程序期間 password\ 認證，而非憑證提供，因為它是通常會更簡單且更比 EAP\ TLS 或 PEAP\ TLS 部署。

>[!NOTE]
>本指南，IEEE 802.1 X 驗證的無線存取 PEAP\-MS\-CHAP v2 被縮寫」wireless 存取」和「WiFi 存取」。

## <a name="about-this-guide"></a>有關本指南
本指南，如下所述，必要條件指南搭配提供了解如何部署下列 WiFi 存取基礎結構的指示操作。  

- 一或多個 802.1X\-能 802.11 wireless 存取點 \(APs\)。

- Active Directory Domain 服務 \(AD DS\) 使用者與電腦。

- 群組原則管理。

- 一或多個網路原則伺服器 \(NPS\) 伺服器。

- 電腦執行 NPS 伺服器的憑證。

- 無線 client 電腦執行的 Windows&reg; 10、Windows 8.1 或 Windows 8。

### <a name="dependencies-for-this-guide"></a>本指南相依性

成功部署本指南使用的驗證的無線，您必須擁有網路和網域環境所有部署所需的技術。 您必須同時伺服器的憑證部署至您的驗證 NPS 伺服器。

下列章節提供您顯示如何部署這些技術文件的連結。

#### <a name="network-and-domain-environment-dependencies"></a>網路和網域環境相依性

本指南針對網路和系統管理員稍 Windows Server 2016 中的步驟**核心網路指南**部署核心網路，或對於先前已部署核心網路，包括 AD DS，包含核心技術網域名稱系統 \(DNS\)、動態主機設定通訊協定 \(DHCP\)、TCP\ 日 IP、NPS 及 Windows 網際網路名稱服務 \(WINS\)。  

核心網路指南可在下列位置：

- Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)Windows Server 2016 技術文件庫中可使用。 

- [核心網路指南](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)也可 TechNet 庫，在 Word 格式，[https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

#### <a name="server-certificate-dependencies"></a>伺服器的憑證相依性
有兩個提供選項的伺服器的憑證用於使用 802.1 X 驗證的註冊驗證伺服器使用 Active Directory 憑證服務 \(AD CS\) 部署自己公用基礎結構或使用伺服器的憑證公開憑證授權單位已退出 \(CA\)。

##### <a name="ad-cs"></a>AD CS
網路和系統管理員已驗證的無線部署必須依照指示 Windows Server 2016 核心網路系列指南，**適用於 802.1 X 的有線和 Wireless 部署部署伺服器憑證**。 本指南如何部署及使用電腦執行 NPS 註冊伺服器的憑證 AD CS。

本指南可在下列位置。

- Windows Server 2016 核心網路系列指南[適用於 802.1 X 的有線和無線部署部署伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396)技術文件庫 HTML 格式。 

##### <a name="public-ca"></a>公開 CA
您可以 client 電腦已信任購買 VeriSign，例如公用加拿大伺服器的憑證。 

Client 電腦信任 CA 時 CA 憑證已安裝的受信任的根憑證授權單位憑證存放區。 根據預設，執行 Windows 的電腦有多個公開 CA 憑證安裝在其受信任的根憑證授權單位憑證市集。  

您檢視的每一種技術，此部署案例中所使用的設計和部署指南至於。 這些指南可協助您判斷這個部署案例提供服務和設定，您需要針對您組織的網路。

### <a name="requirements"></a>需求
以下是使用本文中的案例部署 wireless 存取基礎結構的需求：

- 部署之前此案例，您必須先購買 802.1X\-能 wireless 存取點，以提供您想要的位置在網站中 wireless 涵蓋範圍。 本指南計劃一節協助判斷您的 Ap 必須支援的功能。

- Active Directory Domain Services \(AD DS\) 安裝，在其他網路所需的技術，根據 Windows Server 2016 核心網路節目表中的指示操作。  

- AD CS 部署，及伺服器的憑證已退出 NPS 伺服器。 本指南使用的 PEAP\-MS\-CHAP v2 certificate\ 為基礎的驗證方法部署時，所需這些憑證。

- 您成員是組織的熟悉 IEEE 802.11 標準您 wireless Ap client 電腦及網路上的裝置在安裝 wireless 網路介面卡的支援。 例如，您在組織中其他人是熟悉廣播頻率類型，802.11 wireless 驗證 \（WPA2 或 WPA\），以及加密 \(AES or TKIP\)。

## <a name="what-this-guide-does-not-provide"></a>未提供哪些本指南  
以下是此指南不提供的部分項目：

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>完整的指導方針選取 802.1X\-能 wireless 存取點

因為之間品牌與機型的 802.1X\ 有許多不同的功能 wireless Ap，本指南不提供詳細的資訊，有關：  

- 適用於您的需求來判斷哪一個品牌或 wireless AP 型號最好的作法。

- Wireless Ap 您網路上的實體部署。

- 進階 wireless AP 設定，例如的 wireless virtual 區域網路 \(VLANs\)。

- 如何設定 wireless AP vendor\ 特定屬性 NPS 中的指示操作。

此外，詞彙和設定的名稱 wireless AP 品牌與型號而有所不同，可能不符合本指南使用一般設定名稱。 適用於 wireless AP 設定的詳細資訊，您必須檢視您 wireless Ap 的製造商所提供的 product 文件。

### <a name="instructions-for-deploying-nps-server-certificates"></a>部署 NPS 伺服器的憑證的指示
  
有兩個選擇部署 NPS 伺服器的憑證。 本指南不提供完整的指導方針，以協助您判斷的替代方案最符合您的需求。 一般而言，但是，您所遇到的選項︰

- 購買公用 CA，例如 VeriSign，已經 Windows\ 型用的受信任的憑證。 較小的網路，通常會建議使用此選項。

- 使用 AD CS 部署公用基礎結構 \(PKI\) 在您的網路。 我們建議針對大部分的網路，和之前所述的部署節目表中可使用如何部署伺服器的憑證 AD CS 的指示執行。

### <a name="nps-network-policies-and-other-nps-settings"></a>NPS 的網路原則和其他 NPS 設定

除了對當您執行的組態設定**設定 802.1 X**精靈，如此節目表中所述，此指南不提供手動設定 NPS 條件、限制或其他 NPS 設定的詳細的資訊。

### <a name="dhcp"></a>DHCP

本部署指南不提供設計或部署 DHCP 子網路 wireless 區域網路的相關資訊。

## <a name="technology-overviews"></a>技術概觀
以下是用來部署 wireless 存取技術概觀：

### <a name="ieee-8021x"></a>IEEE 802.1 X
IEEE 802.1 X 的一般定義用來提供的已驗證的網路存取權乙太網路 port\ 為基礎的網路存取控制。 這個 port\ 為基礎的網路存取控制驗證裝置連接到連接埠區域網路使用切換的區域網路基礎結構實體特性。 連接埠可以會存取如果驗證程序失敗。 雖然這個標準固定乙太網路的設計，它已調整 802.11 wireless 區域網路上使用。

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-功能 wireless 的存取點 \(APs\)
本案例需要一或多個 802.1X\ 部署的功能 wireless Ap 遠端驗證 Dial\-In 使用者服務 \(RADIUS\) 通訊協定與相容。

802.1 X 和 RADIUS\ 相容 Ap，例如 server NPS RADIUS 伺服器的基礎結構 RADIUS 部署時稱為*RADIUS 戶端*。

### <a name="wireless-clients"></a>Wireless 戶端
本指南提供完整設定的詳細資訊，以提供 802.1 X 驗證 domain\ 成員使用者 wireless client 電腦執行的 Windows 10、Windows 8.1 和「Windows 8 的連上網路的存取。 電腦必須加入網域，以便成功地建立驗證的存取。

>[!NOTE]
>您也可以使用電腦正在執行 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 為 wireless 戶端。

### <a name="support-for-ieee-80211-standards"></a>支援 IEEE 802.11 標準
支援的 Windows 和 Windows Server 作業系統提供 802.11 wireless 網路 built\ 中支援。 在下列作業系統中，安裝 802.11 wireless 網路介面卡，會顯示為 wireless 網路，在 [網路和共用中心。 

雖然 built\ 中的支援 802.11 wireless 網路功能，Windows 的 wireless 元件是根據下列：

- Wireless 網路介面卡的功能。 安裝 wireless 網路介面卡必須支援 wireless 區域網路或 wireless 安全標準您需要的。 例如，如果 wireless 網路介面卡不支援 \(WPA\) Wi\ Wi-fi 保護的存取，無法讓或設定 WPA 安全性選項。

- Wireless 網路介面卡驅動程式的功能。 為了讓您設定的選項 wireless 網路，必須支援 wireless 網路介面卡驅動程式 windows 的所有功能都報告。 確認寫入 wireless 網路介面卡驅動程式是為您的作業系統功能。 也請確定驅動程式的最新版本檢查 Microsoft Update 或 wireless 網路介面卡廠商的網站。

下表顯示傳輸速率和一般 IEEE 802.11 wireless 標準的頻率。

|標準|頻率|傳輸速率的位元|使用|
|-------------|---------------|--------------------------|---------|  
|802.11|S\ 頻業界、[工程] 及醫療 \(ISM\) 頻率範圍 \ (2.4 到 2.5 GHz\)|第二個 \(Mbps\) 每 2 mb|過時。 不常使用。|  
|802.11|ISM S\ 頻|11 Mbps|常用。|  
|802.11 a|C\ 頻 ISM \ (5.725 以 5.875 GHz\)|54 Mbps|不常用的費用及障礙到期。|  
|802.11 g|ISM S\ 頻|54 Mbps|常用。 802.11 g 裝置的 802.11 相容裝置。|  
|802.11 n \2.4 和 5.0 GHz|ISM C\-Band 和 S\ 頻|250 Mbps|根據 pre\-ratification IEEE 802.11 n 標準裝置變得 2007 年 8 月中可使用。 許多 802.11 n 裝置的相容 802.11 a b 和 g 裝置。|
|802.11ac |5 GHz |6.93 Gbps |802.11ac，IEEE 核准 2014，更延展性，而且可以更快速地比 802.11 n，且該位置支援 Ap 和 wireless 戶端部署。|

### <a name="wireless-network-security-methods"></a>Wireless 網路安全性方法
**無線網路安全性方法**是 wireless 驗證非正式群組 \（有時稱為 wireless security\）和 wireless 安全性加密。 Wireless 驗證及加密用於配對以防止未經授權的使用者存取 wireless 網路，並保護 wireless 傳輸。 

設定時 wireless 安全性 Wireless 網路原則的群組原則中，有多個組合，可從中。 不過，支援只 WPA2\ 企業、WPA\-企業版和使用 802.1 X 驗證標準開放 802.1 X 驗證的 wireless 部署。

>[!NOTE]
>Wireless 的網路原則設定時，您必須選取**WPA2\ 企業**，**WPA\ 企業**，或**開放使用 802.1 X**以取得所需的 802.1 X 驗證的 wireless 部署 EAP 設定的存取權。  

#### <a name="wireless-authentication"></a>Wireless 驗證
本指南建議下列 wireless 驗證標準使用 802.1 X 驗證 wireless 部署。  

**Wi\ Wi-fi 保護的存取 – 企業 \(WPA\-Enterprise\)** WPA 是由遵守 802.11 wireless 安全性通訊協定 WiFi Alliance 暫時標準。 回應上述電傳同樣的隱私權 \(WEP\) 通訊協定發現嚴重問題的一些已開發 WPA 通訊協定。

企業 WPA\ 透過 WEP 來提供改善的安全性：  

1. 要求，以確保集中互加好友的驗證並動態金鑰管理基礎結構的一部分使用 802.1 X EAP 架構的驗證  

2. 美化訊息完整性檢查 \(MIC\)，保護標頭和承載完整性檢查值的 \(ICV\)  

3. 實作防止重新執行攻擊畫面計時器  

**Wi\ Wi-fi 保護的存取 2 – 企業 \(WPA2\-Enterprise\)**如 WPA\ 企業標準，WPA2\ 企業使用 802.1 X 和 EAP 架構。 WPA2\-企業版提供多個使用者和大型受管理的網路較資料保護。 企業 WPA2\ 是穩固的通訊協定是設計用來防止未經授權的網路存取權的網路使用者透過驗證伺服器的驗證。  

#### <a name="wireless-security-encryption"></a>Wireless 安全性加密
Wireless 安全性加密用來保護 wireless client 與 wireless AP 之間傳送 wireless 傳輸。 Wireless 安全性加密是一起使用選取的網路安全性驗證方法。 根據預設，執行 Windows 10、Windows 8.1 和「Windows 8 電腦支援兩個加密標準：

1. **暫時鍵完整性通訊協定**\(TKIP\) 是舊版原先提供更安全的 wireless 加密比所提供的原本弱電傳同樣的隱私權 \(WEP\) 通訊協定的設計加密通訊協定。 TKIP 設計用來 IEEE 802.11 工作群組，而不需要更換舊版硬體更換 WEP Wi\ Wi-fi Alliance。 TKIP 是一套演算法封裝 WEP 承載，且可讓使用者的舊版 WiFi 設備升級至 TKIP，而不會取代硬體。 WEP，例如 TKIP 會使用 RC4 串流加密演算法做為基礎。 新的通訊協定，但是加密每個資料封包唯一加密金鑰，而這些 WEP，許多較下的按鍵。 雖然 TKIP 適合用來升級是設計用來使用只 WEP 舊款裝置上的安全性，它不處理所有面對 wireless 的區域網路的安全性問題，在大部分案例中不這些穩定保護的機密政府或公司資料傳輸。  

2. **進階加密標準**\(AES\) 是慣用的加密通訊協定的商業和政府資料加密。 好一段提供較高的安全性 wireless 傳輸比 TKIP 或 WEP。 然而 TKIP，WEP 好一段需要 wireless 硬體的支援好一段標準。 好一段是 symmetric\ 鍵加密標準使用三個封鎖加密 AES\-128、AES\-192 和 AES\-256。

Windows Server 2016 中 AES\ 型 wireless 加密下列方法可供設定中設定檔 wireless 屬性當您選取 WPA2\-企業版，建議使用的驗證方法。

1. **AES\-CCMP**。 對抗模式加密區鏈結訊息驗證碼通訊協定 \(CCMP\) 實作標準 802.11 適用於更高安全性加密比所提供的 WEP，並使用 128 元好一段加密金鑰。
2. **AES\-GCMP**。 Galois 計數器模式通訊協定 \(GCMP\) 802.11ac 支援、比 AES\-CCMP 更有效率並提供針對 wireless 戶端更好的效能。 GCMP 使用 256 元好一段加密金鑰。

> [!IMPORTANT]
> 有線相等隱私權 \(WEP\) 是原始 wireless 安全性標準加密網路流量是使用。 您不應該部署 WEP 您網路上因為有 well\ 不為人知此過時表單的安全性弱點。

### <a name="active-directory-doman-services-ad-ds"></a>Active Directory 網域服務 \(AD DS\)
AD DS 提供分散式的資料庫來儲存及管理網路資源和 application\ 特定資料的相關資訊從 directory\ 功能的應用程式。 系統管理員可以使用 AD DS 成階層包含結構組織使用者、電腦及其他裝置，例如網路的項目。 階層包含結構包含 Active Directory 樹系的樹系，網域和組織單位 \(OUs\) 每個網域。 執行 AD DS 伺服器稱為*網域控制站*。  

AD DS 包含帳號，電腦帳號及驗證使用者憑證和評估 wireless 連接的授權 IEEE 802.1 X 和 PEAP\-MS\-CHAP v2 所需 account 屬性。

### <a name="active-directory-users-and-computers"></a>Active Directory 使用者與電腦
Active Directory 使用者和電腦是 AD DS 包含帳號，後者實體項目，例如電腦、某人或安全性群組」的元件。 A*安全性群組*是帳號使用者或電腦的系統管理員可以管理單位的集合。 使用者和電腦帳號屬於特定群組稱為*群組成員*。  

### <a name="group-policy-management"></a>群組原則管理
群組原則管理可讓 directory\ 型變更和設定的管理使用者及電腦的設定，包括安全性和使用者資訊。 您可以使用群組原則來定義設定的使用者及電腦的群組。 使用群組原則中，您可以指定登錄項目、安全性、安裝的軟體、指令碼、資料夾重新導向遠端安裝的服務及維護 Internet Explorer 設定。 群組原則設定，您可以建立包含在群組原則物件 \(GPO\)。 選取 Active Directory 系統容器與關聯 GPO — 網站、網域及 Ou，您可以將這些 Active Directory 容器中的 [電腦與使用者套用 GPO 的設定。 若要管理企業的群組原則物件，您可以使用群組原則編輯器] 管理 Microsoft Management Console \(MMC\)。  

本指南指定無線網路設定的相關詳細的指示 \ (IEEE 802.11\) 原則延伸的群組原則管理。 Wireless 網路 \ (IEEE 802.11\) 原則設定需要連接 domain\ 成員 wireless client 電腦和 wireless 設定 802.1 X 驗證 wireless 存取。

### <a name="server-certificates"></a>伺服器的憑證
本案例中部署需要執行 802.1 X 驗證的每個 NPS 伺服器伺服器的憑證。  

伺服器的憑證是一種數位件常用的驗證並保護的開放網路上的資訊。 憑證確實繫結到對應私密金鑰實體公用按鍵。 透過 CA，以數位簽署的憑證，它們可以發行的使用者，電腦上或服務。  

憑證授權單位 \(CA\) 是負責建立和 vouching 公用按鍵屬於主題的真確性的實體 \（通常為使用者或 computers\）或其他 Ca。 憑證授權單位活動可以包含繫結公用按鍵分辨透過簽署的憑證，管理憑證序號和撤銷憑證的名稱。  

Active Directory 憑證服務 \(AD CS\) 是憑證問題與網路 CA 伺服器角色。 AD CS 憑證基礎結構，也就是*公用基礎結構 \(PKI\)*，提供自訂服務發行和管理企業的憑證。

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP、PEAP，以及 PEAP\-MS\-CHAP v2
延伸驗證通訊協定 \(EAP\) 延伸 Point\ to\ 點的通訊協定，允許使用 credential 和資訊的額外的驗證方法 \(PPP\) 交換的任意長度。 EAP 驗證，這兩個網路存取 client 和 authenticator \（例如 NPS server\) 必須支援成功驗證相同 EAP 類型發生。 Windows Server 2016 包含 EAP 基礎結構，支援兩種 EAP 類型，以及 EAP 訊息通過 NPS 伺服器的能力。 您可以藉由使用 EAP，支援額外的驗證方式，稱為*eap*。 支援的 Windows Server 2016 EAP 類型︰  

- 傳輸層安全性 \(TLS\)

- Microsoft 挑戰交換驗證通訊協定第 2 \ (MS\-CHAP v2\)

>[!IMPORTANT]
>強 eap \（例如這些 certificates\ 為基礎）提供更好的安全性防護 brute\ 攻擊、字典攻擊及密碼猜測比 password\ 驗證通訊協定攻擊 \（例如 CHAP 或 MS\-CHAP 版本 1\)。

保護的 EAP \(PEAP\) 使用 TLS 建立加密的通道之間驗證 PEAP client，例如 wireless 電腦，以及 PEAP authenticator，例如 NPS 伺服器或其他 RADIUS 伺服器。 PEAP 未指定的驗證方法，但其他 EAP 驗證通訊協定提供額外的安全性 \（例如 EAP\-MS\-CHAP v2\)，可以透過提供 PEAP TLS 加密通道運作。 PEAP 連接到透過下列類型的網路存取伺服器 \(NASs\) 貴組織的網路的存取戶端會使用的驗證方法為：

- 802.1X\-能 wireless 存取點

- 802.1X\-能驗證切換

- 電腦執行的 Windows Server 2016 和遠端存取服務 \(RAS\) 設定為 virtual 私人網路 \(VPN\) 伺服器，DirectAccess 伺服器或兩者

- 電腦執行的 Windows Server 2016 和遠端桌面服務

PEAP\-MS\-CHAP v2 是更輕鬆地部署比 EAP\ TLS 因為來使用 password\ 認證執行使用者驗證 \（使用者名稱和 password\），而不是憑證或智慧卡。 只有 NPS 或其他 RADIUS 伺服器需要有憑證。 NPS 伺服器的憑證在會使用 NPS 伺服器的驗證程序將其身份 PEAP 戶端。

本指南指示來設定您 wireless 戶端和您 NPS server\(s\) PEAP\-MS\-CHAP v2 使用 802.1 X 驗證的存取。

### <a name="network-policy-server"></a>網路原則伺服器
網路原則伺服器 \(NPS\) 可讓您集中設定和管理使用遠端驗證 Dial\-In 使用者服務 \(RADIUS\) 伺服器與 RADIUS proxy 的網路原則。 當您部署 802.1 X wireless 存取需要 NPS。

當您在 NPS RADIUS 戶端以設定您 802.1 X wireless 存取點時，NPS 處理連接要求 Ap 所傳送。 連接要求處理，期間 NPS 會執行驗證授權。 驗證判斷是否 client 提供有效的憑證。 如果 NPS 成功驗證要求 client、NPS 會判斷是否 client 授權可要求的連接，並且可讓或拒絕連接。 這是所述更多詳細資料，如下所示：

#### <a name="authentication"></a>驗證

成功互加好友的 PEAP\-MS\-CHAP v2 驗證有兩個主要部分：

1.  Client 驗證 NPS 伺服器。  在此階段的互加好友的驗證，NPS 伺服器傳送它伺服器的憑證 client 的電腦，以便 client 可以驗證憑證的 NPS 伺服器的身分。 若要通過 NPS 伺服器，client 的電腦必須信任 CA 發出 NPS 伺服器的憑證。 Client 信任此 CA 憑證時 client 電腦上的受信任的根憑證授權單位憑證存放區中。

    如果您要部署私人授權，CA 憑證會自動安裝目前使用者和本機電腦的受信任的根憑證授權單位憑證存放區網域成員 client 電腦上重新整理群組原則時。 如果您要部署公開 CA 憑證伺服器，請確定公開 CA 憑證已受信任的根憑證授權單位憑證存放區中。  

2.  NPS 伺服器驗證使用者。 Client 成功驗證 NPS 伺服器之後，client 會傳送至 NPS 伺服器，確認使用者在 Active Directory 網域服務 \(AD DS\) 帳號資料庫的使用者的認證的使用者的認證 password\-為基礎。

如果是有效的認證成功驗證，NPS 伺服器開始處理連接要求的授權階段。 如果是無效的憑證，並驗證失敗，NPS 傳送存取拒絕訊息和遭拒連接要求。  

#### <a name="authorization"></a>授權

執行 NPS 伺服器會執行授權，如下所示：  

1. NPS 檢查中的使用者或電腦 account dial\ 屬性 AD DS 中的限制。 Active Directory 使用者電腦中的每個使用者和電腦 account 包括多個屬性，包括位於**Dial\ 在**索引標籤。在這個] 索引標籤，在**網路存取權限**，如果的值**可讓存取**，授權的使用者或電腦已連上網路。 如果價值，是**拒絕**，連上網路未經授權的使用者或電腦。 如果價值，是**控制透過 NPS 的網路原則**、NPS 評估以判斷您連上網路獲得授權的使用者或電腦的設定的網路原則。 

2. NPS 然後處理尋找符合連接要求原則的網路原則。 如果找不到對應的原則，NPS 授與或拒絕連接依照該原則的設定。  

如果驗證與授權使用成功，且對應的網路原則授與的存取權，NPS 授與的存取權的網路，並電腦與使用者都可以連接到網路資源的他們的權限。  

>[!NOTE]
>若要部署 wireless 存取，您必須設定 NPS 原則。 本指南使用的指示執行**設定 802.1 X 精靈**中建立 NPS 原則，針對 802.1 X 驗證 wireless 存取 NPS。  

### <a name="bootstrap-profiles"></a>開機設定檔
在 [802.1X\-驗證 wireless 網路 wireless 戶端必須提供 RADIUS 伺服器的驗證為了連上網路的安全性憑證。 為保護 EAP \[PEAP\]\-Microsoft 挑戰交換驗證通訊協定第 2 \[MS\-CHAP v2\] 的安全性憑證的使用者名稱和密碼。 適用於 EAP\-Tls \[TLS\] 或 PEAP\ TLS 的安全性憑證中的憑證，例如憑證的 client 使用者與電腦或智慧卡。

連接到執行 PEAP\-MS\-CHAP v2、PEAP\ TLS 或 EAP\ 進行驗證，預設設定網路時，Windows wireless 戶端也必須驗證電腦傳送 RADIUS 伺服器的憑證。 每個驗證工作階段 RADIUS 伺服器來傳送電腦憑證通常稱為伺服器的憑證。

如之前所述，您可以選擇發行 RADIUS 伺服器中有兩種他們伺服器的憑證：從 commercial CA \ (VeriSign，Inc.，例如 \)，或從您網路部署私人 CA。 如果 RADIUS 伺服器傳送電腦憑證是核發給 commercial ca 已經安裝 client 的受信任的根憑證授權單位憑證存放區中的根憑證、wireless client 可以驗證 RADIUS 伺服器電腦憑證，無論 wireless client 是否已加入 Active Directory domain。 在這種情形下 wireless client 可以連接到 wireless 網路，然後您可以加入網域的電腦。

>[!NOTE]
>您可以停用需要驗證伺服器的憑證 client 的行為，但 production 環境中不建議停用伺服器的憑證驗證。

Wireless 開機設定檔會暫時在這種方式可以連接到 802.1X\ wireless client 使用者設定的設定檔-驗證 wireless 網路之前電腦已經加入網域，and\ 日或之前使用者成功登入網域第一次使用特定 wireless 的電腦。  本章節摘要 wireless 的電腦加入網域，或的使用者來登入網域使用 domain\ 加入 wireless 電腦中的第一次嘗試時遇到問題。

針對部署 IT 系統管理員的使用者無法實際電腦連接到有線乙太網路加入網域的電腦和電腦不需要必要發行根憑證安裝在其**受信任的根憑證授權單位**憑證存放區，您可以設定 wireless 戶端暫時 wireless 連接設定檔名為*啟動設定檔*、連接到 wireless 網路。

A*啟動設定檔*不需要驗證電腦的 RADIUS 伺服器的憑證。 此暫存設定可讓 wireless 使用者將電腦加入的網域，此時網路無線 \ (IEEE 802.11\) 套用原則和適當 ca 憑證會自動安裝在電腦上。

執行互加好友的驗證的需求一或多個連接 wireless 設定檔套用群組原則時，會套用到電腦。已不再需要的開機設定檔，並且移除。 加入網域的電腦，開機之後使用者可以使用 wireless 連接到網域登入。

概觀使用這些技術 wireless 存取部署程序，請查看[Wireless 存取部署概觀](b-wireless-access-deploy-overview.md)。
