---
title: 部署密碼型 802.1X 驗證無線存取
description: 本主題是 Windows Server 2016 網路指南「部署以密碼為基礎的 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a03d9d1b0532c846e8514ca904d38ea825baf4d4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318121"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>根據 802.1 X 驗證無線存取部署密碼\-

>適用於：Windows Server (半年通道)、Windows Server 2016

這是《 Windows Server&reg; 2016 核心網路指南》的附屬指南。 《核心網路指南》提供在新樹系中規劃及部署完全正常運作的網路和新的 Active Directory®網域所需元件的指示。

本指南說明如何使用受保護的可延伸驗證通訊協定（Microsoft 挑戰交握驗證通訊協定第2版 \(PEAP\-MS\-CHAP v2\)），以核心網路為基礎來建立 \(IEEE\) 802.1 X\-驗證的 IEEE 802.11 無線存取的相關指示。

因為 PEAP\-MS\-CHAP v2 要求使用者在驗證程式期間提供密碼\-的認證，而不是憑證，所以部署的成本通常比 EAP\-TLS 或 PEAP\-TLS 更簡單且較便宜。

>[!NOTE]
>在本指南中，使用 PEAP\-MS\-CHAP v2 的 IEEE 802.1 X 驗證無線存取會縮寫為「無線存取」和「WiFi 存取」。

## <a name="about-this-guide"></a>關於本指南
本指南結合下面所述的必要條件指南，提供如何部署下列 WiFi 存取基礎結構的指示。  

- 一或多個 802.1 X\-支援802.11 無線存取點\)\(的 Ap。

- Active Directory Domain Services \(AD DS\) 使用者和電腦。

- 群組原則管理。

- 一或多個網路原則伺服器 \(NPS\) 伺服器。

- 執行 NPS 之電腦的伺服器憑證。

- 執行 Windows&reg; 10、Windows 8.1 或 Windows 8 的無線用戶端電腦。

### <a name="dependencies-for-this-guide"></a>本指南的相依性

若要使用本指南成功部署已驗證的無線網路，您必須擁有已部署所有必要技術的網路和網域環境。 您也必須將伺服器憑證部署至驗證 Nps。

下列各節提供說明如何部署這些技術的檔連結。

#### <a name="network-and-domain-environment-dependencies"></a>網路和網域環境的相依性

本指南是針對已遵循 Windows Server 2016**核心網路指南**中的指示部署核心網路的網路和系統管理員所設計，也適用于先前已部署核心網路所包含之核心技術的使用者，包括 AD DS、網域名稱系統 \(DNS\)、動態主機設定通訊協定 \(DHCP\)、TCP\/IP、NPS 和 Windows 網際網路名稱服務 \(WINS\)。  

核心網路指南可在下列位置取得：

- Windows server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)可在 windows Server 2016 技術文件庫中取得。 

- 《[核心網路指南》](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)也以 Word 格式提供，位於 TechNet 圖庫的[https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

#### <a name="server-certificate-dependencies"></a>伺服器憑證相依性
有兩個選項可用來註冊具有伺服器憑證的驗證服務器以搭配 802.1 X 驗證使用-使用 Active Directory 憑證服務 \(AD CS 來部署您自己的公開金鑰基礎結構\) 或使用由公開憑證授權單位單位 \(CA\)註冊的伺服器憑證。

##### <a name="ad-cs"></a>AD CS
網路和系統管理員部署已驗證的無線必須遵循《 Windows Server 2016 核心網路附屬指南》中的指示，**部署 802.1 x 有線和無線部署的伺服器憑證**。 本指南說明如何部署和使用 AD CS，將伺服器憑證自動註冊到執行 NPS 的電腦。

本指南可在下列位置取得。

- 技術文件庫中的 Windows Server 2016 核心網路附屬指南以 HTML 格式[部署 802.1 x 有線和無線部署的伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396)。 

##### <a name="public-ca"></a>公用 CA
您可以從用戶端電腦已信任的公用 CA （例如 VeriSign）購買伺服器憑證。 

當 CA 憑證安裝在「信任的根憑證授權單位」憑證存放區時，用戶端電腦會信任 CA。 根據預設，執行 Windows 的電腦會在其「信任的根憑證授權單位」憑證存放區中安裝多個公用 CA 憑證。  

建議您檢閱此部署案例中所用每個技術的設計和部署指南。 這些指南可協助您判斷此部署案例是否提供貴組織的網路需要的服務與設定。

### <a name="requirements"></a>需求
以下是使用本指南所述的案例來部署無線存取基礎結構的需求：

- 在部署此案例之前，您必須先購買 802.1 X\-支援的無線存取點，才能在您的網站上的所需位置提供無線涵蓋範圍。 本指南的規劃一節可協助您判斷您的 Ap 必須支援的功能。

- 根據《 Windows Server 2016 核心網路指南》中的指示，安裝的 Active Directory Domain Services \(AD DS\) 是其他必要的網路技術。  

- 已部署 AD CS，並已向 Nps 註冊伺服器憑證。 當您部署本指南中所使用的 PEAP\-MS\-CHAP v2 憑證\-型驗證方法時，就需要這些憑證。

- 您組織的成員熟悉您的無線 Ap 所支援的 IEEE 802.11 標準，以及安裝在網路上用戶端電腦和裝置中的無線網路介面卡。 例如，組織中的某個人熟悉無線電頻率類型、802.11 無線驗證 \(WPA2 或 WPA\)，以及 \(AES 或 TKIP\)的密碼。

## <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容  
以下是本指南未提供的一些專案：

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>選取 802.1 X\-功能的無線存取點的完整指引

由於 802.1 X\-支援的無線 Ap 的品牌和型號之間有許多差異，因此本指南不會提供有關下列各項的詳細資訊：  

- 判斷哪一種無線 AP 的品牌或型號最適合您的需求。

- 網路上無線 Ap 的實體部署。

- Advanced 無線 AP 設定，例如適用于無線虛擬區域網路的網路 \(Vlan\)。

- 有關如何在 NPS 中設定無線 AP 廠商\-特定屬性的指示。

此外，設定的術語和名稱會因無線 AP 品牌和型號而異，而且可能不符合本指南中所使用的一般設定名稱。 如需無線 AP 設定的詳細資料，您必須參閱無線 Ap 製造商所提供的產品檔。

### <a name="instructions-for-deploying-nps-certificates"></a>部署 NPS 憑證的指示
  
部署 NPS 憑證有兩種替代方案。 本指南不提供完整的指引，可協助您判斷哪一種替代方案最符合您的需求。 不過，一般情況下，您所面臨的選擇如下：

- 向已受 Windows\-為基礎的用戶端信任的公用 CA （例如 VeriSign）購買憑證。 此選項通常建議用於較小的網路。

- 使用 AD CS，在您的網路上部署 \(PKI\) 的公開金鑰基礎結構。 這適用于大部分的網路，以及如何使用 AD CS 部署伺服器憑證的指示，請前往先前提到的部署指南。

### <a name="nps-network-policies-and-other-nps-settings"></a>NPS 網路原則和其他 NPS 設定

除了在執行 [**設定 802.1 x** ] 時所做的設定（如本指南所述），本指南並未提供手動設定 nps 條件、條件約束或其他 nps 設定的詳細資訊。

### <a name="dhcp"></a>DHCP

此部署指南不會提供設計或部署無線區域網路之 DHCP 子網的相關資訊。

## <a name="technology-overviews"></a>技術概觀
以下是部署無線存取的技術概覽：

### <a name="ieee-8021x"></a>IEEE 802.1X
IEEE 802.1 X standard 會定義通訊埠\-網路存取控制，用來為乙太網路提供已驗證的網路存取。 此埠\-網路存取控制會使用交換器 LAN 基礎結構的實體特性來驗證連接至 LAN 埠的裝置。 如果驗證處理程序失敗，將拒絕存取這個連接埠。 雖然此標準是針對有線 Ethernet 網路所設計，但已經過調整，可用於802.11 無線 Lan。

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1 x\-支援的無線存取點 \(Ap\)
此案例需要部署一或多個 802.1 X\-支援的無線 Ap，其與使用者服務 \(RADIUS\) 通訊協定中的遠端驗證撥號\-相容。

802.1 x 和 RADIUS\-相容的 Ap，部署在具有 RADIUS 伺服器（例如 NPS）的 RADIUS 基礎結構時，稱為*radius 用戶端*。

### <a name="wireless-clients"></a>無線用戶端
本指南提供完整的設定詳細資料，以針對使用執行 Windows 10、Windows 8.1 和 Windows 8 的無線用戶端電腦連線到網路的網域\-成員使用者，提供 802.1 X 驗證的存取權。 電腦必須加入網域，才能成功建立已驗證的存取。

>[!NOTE]
>您也可以使用執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的電腦作為無線用戶端。

### <a name="support-for-ieee-80211-standards"></a>支援 IEEE 802.11 標準
支援的 Windows 和 Windows Server 作業系統提供802.11 無線網路支援的內建\-。 在這些作業系統中，已安裝的802.11 無線網路介面卡在 [網路和共用中心] 中會顯示為無線網路連線。 

雖然有內建的\-支援802.11 無線網路，但 Windows 的無線元件會根據下列各項而定：

- 無線網路介面卡的功能。 已安裝的無線網路介面卡必須支援您需要的無線區域網路 或無線安全性標準。 例如，如果無線網路介面卡不支援 \(WPA\)的 Wi-fi\-Wi-fi 保護的存取，您就無法啟用或設定 WPA 安全性選項。

- 無線網路介面卡驅動程式的功能。 若要讓您設定無線網路選項，無線網路介面卡的驅動程式必須支援將其所有功能報告到 Windows。 確認您的無線網路介面卡的驅動程式是針對作業系統的功能所撰寫。 也請檢查 Microsoft Update 或無線網路介面卡廠商的網站，以確定驅動程式是最新版本。

下表顯示一般 IEEE 802.11 無線標準的傳輸速率和頻率。

|標準|線|位傳輸速率|使用方式|
|-------------|---------------|--------------------------|---------|  
|802.11|\-頻外工業、科學和醫療 \(ISM\) 頻率範圍 \(2.4 到 2.5 GHz\)|每秒 2 mb \(Mbps\)|已經過時： 不常使用。|  
|802.11b|S\-波段 ISM|11 Mbps|常用。|  
|a|C\-波段 ISM \(5.725 到 5.875 GHz\)|54 Mbps|不常因為支出和有限範圍而使用。|  
|g|S\-波段 ISM|54 Mbps|廣泛使用。 802.11 g 裝置與 802.11 b 裝置相容。|  
|802.11 n \ 2.4 和 5.0 GHz|C\-寬線和 S\-頻外 ISM|250 Mbps|以預先\-ratification IEEE 802.11 n 標準為基礎的裝置，將于2007年8月推出。 許多 802.11 n 裝置都與 802.11 a、b 和 g 裝置相容。|
|802.11ac |5 GHz |6.93 Gbps |802.11 c 是由 IEEE 在2014中核准，比 802.11 n 更具擴充性且速度更快，而且會在 Ap 和無線用戶端支援的位置進行部署。|

### <a name="wireless-network-security-methods"></a>無線網路安全性方法
**無線網路安全性方法**是一組非正式的無線驗證 \(有時稱為無線安全性\) 和無線安全性加密。 無線驗證和加密會成對用來防止未經授權的使用者存取無線網路，以及保護無線傳輸。 

在群組原則的無線網路原則中進行無線安全性設定時，有多個組合可供選擇。 不過，針對 802.1 X 驗證的無線部署，僅支援 WPA2\-Enterprise、WPA\-Enterprise 和 Open with 802.1 X 驗證標準。

>[!NOTE]
>設定無線網路原則時，您必須選取 [ **WPA2\-enterprise**]、[ **WPA\-enterprise**] 或 [**以 802.1 x 開啟**]，才能存取 802.1 x 驗證無線部署所需的 EAP 設定。  

#### <a name="wireless-authentication"></a>無線驗證
本指南建議您針對 802.1 X 驗證無線部署使用下列無線驗證標準。  

**Wi-fi\-Wi-fi 保護的存取– enterprise \(WPA\-enterprise\)** WPA 是由 WiFi 聯盟所開發的過渡標準，以符合802.11 無線安全性通訊協定的規範。 WPA 通訊協定的開發目的，是為了回應先前的有線對等隱私權 \(WEP\) 通訊協定中所發現的許多嚴重瑕疵。

WPA\-Enterprise 透過下列方式提供更佳的 WEP 安全性：  

1. 需要使用 802.1 X EAP 架構做為基礎結構一部分的驗證，以確保集中式相互驗證和動態金鑰管理  

2. 以訊息完整性檢查 \(MIC\)來增強完整性檢查值 \(ICV\)，以保護標頭和承載  

3. 執行框架計數器以防止重新執行攻擊  

**Wi-fi\-Wi-fi 保護的存取 2-enterprise \(WPA2\-enterprise\)** 就像 WPA\-Enterprise standard 一樣，WPA2\-Enterprise 會使用 802.1 X 和 EAP 架構。 WPA2\-Enterprise 為多個使用者和大型受控網路提供更強大的資料保護。 WPA2\-Enterprise 是一種健全的通訊協定，其設計目的是要透過驗證服務器來驗證網路使用者，以防止未經授權的網路存取。  

#### <a name="wireless-security-encryption"></a>無線安全性加密
無線安全性加密是用來保護在無線用戶端與無線 AP 之間傳送的無線傳輸。 無線安全性加密會與選取的網路安全性驗證方法搭配使用。 根據預設，執行 Windows 10、Windows 8.1 和 Windows 8 的電腦支援兩種加密標準：

1. **時態性金鑰完整性通訊協定**\(TKIP\) 是較舊的加密通訊協定，原先設計來提供比原本就弱式有線對等隱私權 \(WEP\) 通訊協定所提供更安全的無線加密。 TKIP 是由 IEEE 802.11 i 工作組和 Wi-fi\-Fi 聯盟所設計，可取代 WEP，而不需要更換舊版硬體。 TKIP 是封裝 WEP 承載的演算法套件，可讓舊版 WiFi 設備的使用者升級至 TKIP，而不需要更換硬體。 就像 WEP 一樣，TKIP 會使用 RC4 串流加密演算法做為基礎。 不過，新的通訊協定會使用唯一的加密金鑰來加密每個資料封包，而金鑰比 WEP 更強。 雖然 TKIP 在專為僅使用 WEP 的舊版裝置上升級安全性很有用，但它並不能解決所有對無線區域網路的安全性問題，而且在大部分情況下，保護敏感性政府或公司資料的情況並不夠強大。均.  

2. **進階加密標準**\(AES\) 是用於加密商業和政府資料的慣用加密通訊協定。 AES 提供的無線傳輸安全性層級高於 TKIP 或 WEP。 與 TKIP 和 WEP 不同的是，AES 需要支援 AES 標準的無線硬體。 AES 是對稱\-金鑰加密標準，其使用三個區塊密碼、AES\-128、AES\-192 和 AES\-256。

在 Windows Server 2016 中，當您選取 WPA2\-Enterprise 的驗證方法時，可以在無線配置檔案屬性中設定下列 AES\-型無線加密方法，這是建議的做法。

1. **AES\-CCMP**。 計數器模式加密區塊鏈訊息驗證碼通訊協定 \(CCMP\) 會執行 802.11 i 標準，而且是針對比 WEP 所提供更高的安全性加密所設計，並使用128位 AES 加密金鑰。
2. **AES\-GCMP**。 Galois 計數器模式通訊協定 \(GCMP\) 受到 802.11 ac 的支援，比 AES\-CCMP 更有效率，並為無線用戶端提供較佳的效能。 GCMP 使用256位 AES 加密金鑰。

> [!IMPORTANT]
> 有線對等隱私權 \(WEP\) 是用來加密網路流量的原始無線安全性標準。 您不應該在您的網路上部署 WEP，因為這種過期的安全性形式有\-已知的弱點。

### <a name="active-directorydoman-services-adds"></a>Active Directory 網域 Services \(AD DS\)
AD DS 提供分散式資料庫，可儲存和管理網路資源和應用程式的相關資訊，\-來自目錄\-啟用應用程式的特定資料。 系統管理員可以使用 AD DS 將網路項目 (像是使用者、電腦以及其他裝置) 組織成階層式的內含項目結構。 階層式內含專案結構包括 Active Directory 樹系、樹系中的網域，以及每個網域中\) \(Ou 的組織單位。 執行 AD DS 的伺服器稱為*網域控制站*。  

AD DS 包含 IEEE 802.1 X 和 PEAP\-MS\-CHAP v2 所需的使用者帳戶、電腦帳戶和帳戶內容，以驗證使用者認證及評估無線連線的授權。

### <a name="active-directory-users-and-computers"></a>Active Directory 使用者和電腦
Active Directory 使用者和電腦是 AD DS 的元件，其中包含代表實體實體的帳戶，例如電腦、個人或安全性群組。 *安全性群組*是使用者或電腦帳戶的集合，系統管理員可將其管理為單一單位。 屬於特定群組的使用者和電腦帳戶稱為*群組成員*。  

### <a name="group-policy-management"></a>群組原則管理
群組原則管理可讓使用者和電腦設定以目錄\-為基礎的變更和設定管理，包括安全性和使用者資訊。 您可以使用群組原則來定義使用者群組和電腦的設定。 使用群組原則，您可以指定登錄專案、安全性、軟體安裝、腳本、資料夾重新導向、遠端安裝服務，以及 Internet Explorer 維護的設定。 您所建立的群組原則設定會包含在群組原則物件 \(GPO\)中。 藉由將 GPO 與所選 Active Directory 系統容器（網站、網域和 Ou）產生關聯，您可以將 GPO 的設定套用到這些 Active Directory 容器中的使用者和電腦。 若要管理整個企業的群組原則物件，您可以使用群組原則管理編輯器 Microsoft Management Console \(MMC\)。  

本指南提供有關如何在群組原則管理的無線網路 \(IEEE 802.11\) 原則延伸中指定設定的詳細指示。 無線網路 \(IEEE 802.11\) 原則會使用 802.1 X 驗證無線存取所需的連線和無線設定，來設定網域\-成員無線用戶端電腦。

### <a name="server-certificates"></a>伺服器憑證
此部署案例需要執行 802.1 X 驗證之每個 NPS 的伺服器憑證。  

「伺服器憑證」是一份數位檔，通常用於驗證以及保護開放式網路上的資訊。 憑證將公開金鑰安全地繫結到保存對應私密金鑰的實體。 發行 CA 會以數位方式簽署憑證，而且可以針對使用者、電腦或服務發出。  

\(CA\) 的憑證授權單位單位是負責建立及保證屬於主旨的公開金鑰真實性的實體，\(通常是\) 或其他 Ca 的使用者或電腦。 憑證授權單位單位的活動可以包含透過簽署的憑證將公開金鑰系結到辨別名稱、管理憑證序號，以及撤銷憑證。  

Active Directory 憑證服務 \(AD CS\) 是一個伺服器角色，會將憑證頒發為網路 CA。 AD CS 憑證基礎結構（也稱為*公開金鑰基礎結構 \(PKI\)* ）提供了可自訂的服務，可用來發行和管理企業的憑證。

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP、PEAP 和 PEAP\-MS\-CHAP v2
可延伸的驗證通訊協定 \(EAP\) 會藉由允許使用任意長度的認證和資訊交換的其他驗證方法，將點\-擴充至\-點通訊協定 \(PPP\)。 使用 EAP 驗證時，網路存取用戶端和驗證器 \(（例如 NPS\)）都必須支援相同的 EAP 類型，才能成功進行驗證。 Windows Server 2016 包括 EAP 基礎結構、支援兩種 EAP 類型，以及將 EAP 訊息傳遞至 Nps 的功能。 藉由使用 EAP，您可以支援其他驗證配置，也稱為*EAP 類型*。 Windows Server 2016 支援的 EAP 類型如下：  

- \(TLS\) 的傳輸層安全性

- Microsoft 挑戰交握驗證通訊協定第2版 \(MS\-CHAP v2\)

>[!IMPORTANT]
>強式 EAP 類型 \(例如以憑證為基礎的\) 提供比密碼\-型驗證通訊協定（例如 CHAP 或 MS \(CHAP 版本 1\-）更佳的安全性，以防止暴力\-強制攻擊、字典攻擊和密碼猜測攻擊。\)

受保護的 EAP \(PEAP\) 使用 TLS 來建立驗證 PEAP 用戶端（如無線電腦）和 PEAP 驗證器（例如 NPS 或其他 RADIUS 伺服器）之間的加密通道。 PEAP 不指定驗證方法，但它會為其他 EAP 驗證通訊協定提供額外的安全性 \(例如 EAP\-MS\-CHAP v2\)，其可透過 PEAP 提供的 TLS 加密通道來操作。 PEAP 是用來作為透過下列網路存取伺服器類型（\(Nas\)）連線到您組織網路之用戶端的驗證方法：

- 802.1 x\-支援的無線存取點

- 802.1 x\-能夠驗證交換器

- 執行 Windows Server 2016 和遠端存取服務 \(RAS\) 設定為虛擬私人網路 \(VPN\) 伺服器、DirectAccess 伺服器，或兩者皆執行的電腦

- 執行 Windows Server 2016 和遠端桌面服務的電腦

PEAP\-MS\-CHAP v2 比 EAP\-TLS 更容易部署，因為使用者驗證是使用密碼\-型認證 \(使用者名稱和密碼\)（而不是憑證或智慧卡）來執行。 只有 NPS 或其他 RADIUS 伺服器需要有憑證。 Nps 會在驗證程式期間使用 NPS 憑證，向 PEAP 用戶端證明其身分識別。

本指南提供的指示說明如何設定您的無線用戶端和 NPS\(s\) 使用 PEAP\-MS\-CHAP v2 來進行 802.1 X 驗證的存取。

### <a name="network-policy-server"></a>Network Policy Server
網路原則伺服器 \(NPS\) 可讓您使用使用者服務 \(RADIUS\) 伺服器和 RADIUS proxy 中的遠端驗證撥號\-，集中設定和管理網路原則。 當您部署 802.1 X 無線存取時，需要 NPS。

當您將 802.1 X 無線存取點設定為 NPS 中的 RADIUS 用戶端時，NPS 會處理 Ap 所傳送的連線要求。 在連接要求處理期間，NPS 會執行驗證和授權。 驗證會判斷用戶端是否已出示有效的認證。 如果 NPS 成功驗證要求的用戶端，則 NPS 會決定是否授權用戶端進行要求的連線，並允許或拒絕連線。 其詳細說明如下：

#### <a name="authentication"></a>驗證

成功的相互 PEAP\-MS\-CHAP v2 驗證有兩個主要部分：

1.  用戶端會驗證 NPS。  在相互驗證的這個階段中，NPS 會將其伺服器憑證傳送到用戶端電腦，讓用戶端可以使用憑證來驗證 NPS 的身分識別。 若要成功驗證 NPS，用戶端電腦必須信任發行 NPS 憑證的 CA。 當 CA 的憑證出現在用戶端電腦上的「信任的根憑證授權單位」憑證存放區中時，用戶端會信任此 CA。

    如果您部署自己的私用 CA，CA 憑證會自動安裝在目前使用者的「信任的根憑證授權單位」憑證存放區中，而在網域成員用戶端電腦上重新整理群組原則時，則會將其儲存在本機電腦上。 如果您決定從公用 CA 部署伺服器憑證，請確定公用 CA 憑證已經在「信任的根憑證授權單位」憑證存放區中。  

2.  NPS 會驗證使用者。 在用戶端成功驗證 NPS 之後，用戶端會將使用者的密碼\-型認證傳送到 NPS，這會在 Active Directory 網域 Services \(AD DS\)中，針對使用者帳戶資料庫驗證使用者的認證。

如果認證有效且驗證成功，NPS 就會開始處理連線要求的授權階段。 如果認證無效且驗證失敗，NPS 會傳送拒絕存取訊息，且連線要求會遭到拒絕。  

#### <a name="authorization"></a>Authorization

執行 NPS 的伺服器會執行授權，如下所示：  

1. NPS 會在 AD DS 的屬性中檢查使用者或電腦帳戶撥號\-的限制。 Active Directory 使用者和電腦中的每個使用者和電腦帳戶都包含多個屬性，包括在 [**撥號\-** ] 索引標籤中找到的內容。在此索引標籤的 [**網路存取權限**] 中，如果值為 [**允許存取**]，則使用者或電腦會獲得連線到網路的授權。 如果值為 [**拒絕存取**]，表示使用者或電腦未獲授權連線到網路。 如果值是 [**透過 NPS 網路原則控制存取**]，NPS 會評估已設定的網路原則，以判斷使用者或電腦是否有權連線到網路。 

2. NPS 接著會處理其網路原則，以尋找符合連線要求的原則。 如果找到相符的原則，NPS 會根據該原則的設定來授與或拒絕連接。  

如果驗證和授權都成功，而且相符的網路原則會授與存取權，NPS 會授與存取權給網路，而使用者和電腦可以連線到他們具有許可權的網路資源。  

>[!NOTE]
>若要部署無線存取，您必須設定 NPS 原則。 本指南提供使用 NPS 中的**Configure 802.1 x wizard**來建立 nps 原則以進行 802.1 x 驗證無線存取的指示。  

### <a name="bootstrap-profiles"></a>啟動程式設定檔
在 802.1 X\-已驗證的無線網路中，無線用戶端必須提供由 RADIUS 伺服器驗證的安全性認證，才能連線到網路。 針對受保護的 EAP \[PEAP\]\-Microsoft 挑戰交握驗證通訊協定第2版 \[MS\-CHAP v2\]，安全性認證是使用者名稱和密碼。 針對 EAP\-傳輸層安全性 \[TLS\] 或 PEAP\-TLS，安全性認證是憑證，例如用戶端使用者和電腦憑證或智慧卡。

當連接到設定為執行 PEAP\-MS\-CHAP v2、PEAP\-TLS 或 EAP\-TLS 驗證的網路時，Windows 無線用戶端預設也必須驗證 RADIUS 伺服器所傳送的電腦憑證。 RADIUS 伺服器為每個驗證會話傳送的電腦憑證，通常稱為「伺服器憑證」。

如先前所述，您可以使用下列兩種方式之一來發出 RADIUS 伺服器的伺服器憑證：從商業 CA \(例如 VeriSign、Inc.、\)，或從您部署在網路上的私人 CA。 如果 RADIUS 伺服器傳送的電腦憑證是由已在用戶端的「信任的根憑證授權單位」憑證存放區中安裝根憑證的商業 CA 所發行，則無線用戶端可以驗證 RADIUS 伺服器的電腦憑證，不論無線用戶端是否已加入 Active Directory 網域。 在此情況下，無線用戶端可以連線到無線網路，然後您就可以將電腦加入網域。

>[!NOTE]
>需要用戶端驗證伺服器憑證的行為可以停用，但不建議在生產環境中停用伺服器憑證驗證。

無線啟動程式設定檔是設定的暫存設定檔，可讓無線用戶端使用者在電腦加入網域之前，連線到 802.1 X\-已驗證的無線網路，以及在使用者第一次使用特定的無線電腦成功登入網域之前，\/或之前。  本節將摘要說明在嘗試將無線電腦加入網域時，或讓使用者第一次使用網域\-加入的無線電腦登入網域時，會發生什麼問題。

對於使用者或 IT 系統管理員無法實際將電腦連線到有線 Ethernet 網路以將電腦加入網域的部署，而且電腦沒有安裝在其「**信任的根憑證授權**」憑證存放區中的必要發行根 CA 憑證時，您可以使用暫時的無線連線設定檔（稱為「*啟動程式配置*檔」）來設定無線用戶端，以連接到無線網路。

*啟動程式設定檔*會移除驗證 RADIUS 伺服器電腦憑證的需求。 這種暫時設定可讓無線使用者將電腦加入網域，此時會套用無線網路 \(IEEE 802.11\) 原則，並將適當的根 CA 憑證自動安裝在電腦上。

套用群組原則時，會在電腦上套用一或多個強制執行相互驗證需求的無線連線設定檔;已不再需要啟動程式設定檔，而且已將其移除。 將電腦加入網域並重新啟動電腦之後，使用者可以使用無線連線來登入網域。

如需使用這些技術的無線存取部署程式總覽，請參閱[無線存取部署總覽](b-wireless-access-deploy-overview.md)。
