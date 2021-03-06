---
title: 部署密碼型 802.1X 驗證無線存取
description: 瞭解如何使用受保護的可延伸驗證通訊協定（Microsoft 挑戰交握驗證通訊協定第2版） 802.1 X 驗證的 IEEE 802.11 無線存取，來部署電氣和電子工程師協會。
manager: brianlic
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 6a861bf9df9d80efc5fd82b8c74ea54b71c8aef5
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038398"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>部署密碼型 \- 802.1 x 驗證無線存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

這是《 Windows Server &reg; 2016 核心網路指南》的附屬指南。 《核心網路指南》提供在新樹系中規劃和部署全功能網路所需的元件，以及新的 Active Directory®網域的指示。

本指南說明如何 \( \) \- 使用受保護802.11 的可延伸驗證通訊協定（Microsoft 挑戰交握驗證通訊協定第2版 \( PEAP \- MS \- CHAP v2） \) ，透過核心網路來建立核心網路的相關指示。

因為 PEAP \- MS \- CHAP v2 要求使用者在驗證程式期間提供密碼型認證 \- ，而不是憑證，所以部署的部署通常比 EAP \- tls 或 PEAP tls 更簡單且成本更低 \- 。

>[!NOTE]
>在本指南中，使用 PEAP MS CHAP v2 的 IEEE 802.1 X 驗證無線存取 \- \- 會縮寫為「無線存取」和「WiFi 存取」。

## <a name="about-this-guide"></a>關於本指南
本指南與下面所述的必要條件指南結合，提供有關如何部署下列 WiFi 存取基礎結構的指示。

- 一或多個具有 802.1 X \- 功能的802.11 無線存取點 \( ap \) 。

- Active Directory Domain Services \( AD DS \) 使用者和電腦。

- 群組原則管理。

- 一或多個網路原則伺服器 \( NPS \) 伺服器。

- 執行 NPS 之電腦的伺服器憑證。

- 執行 Windows &reg; 10、Windows 8.1 或 Windows 8 的無線用戶端電腦。

### <a name="dependencies-for-this-guide"></a>本指南的相依性

若要使用本指南成功部署已驗證的無線，您必須擁有網路和網域環境，並已部署所有必要的技術。 您也必須將伺服器憑證部署至驗證 NPSs。

下列各節提供說明如何部署這些技術的檔連結。

#### <a name="network-and-domain-environment-dependencies"></a>網路和網域環境相依性

本指南是專為已遵循《 Windows Server 2016 **核心網路指南》** 中的指示來部署核心網路的網路和系統管理員所設計，也適用于先前已部署核心網路內含核心技術的使用者，包括 AD DS、網域名稱系統 \( DNS \) 、動態主機設定通訊協定 \( DHCP \) 、TCP \/ IP、NPS，以及 Windows 網際網路名稱服務 \( 勝出 \) 。

您可以在下列位置取得核心網路指南：

- Windows server 2016 [核心網路指南](../../core-network-guide.md) 適用于 windows Server 2016 技術文件庫。

- 《 [核心網路指南》](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) 也可在 TechNet 資源庫的 Word 格式中取得 [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) 。

#### <a name="server-certificate-dependencies"></a>伺服器憑證相依性
有兩個可用的選項可用來向伺服器憑證註冊驗證服務器，以便與 802.1 X 驗證搭配使用-使用 Active Directory 憑證服務 AD CS 來部署您自己的公開金鑰基礎結構 \( \) ，或使用由公開憑證授權單位單位 CA 註冊的伺服器憑證 \( \) 。

##### <a name="ad-cs"></a>AD CS
部署已驗證之無線網路的網路和系統管理員必須遵循 Windows Server 2016 核心網路附屬指南中的指示， **部署 802.1 x 有線和無線部署的伺服器憑證**。 本指南說明如何部署和使用 AD CS，將伺服器憑證自動註冊到執行 NPS 的電腦。

您可以在下列位置取得本指南。

- Windows Server 2016 核心網路附屬指南以 HTML 格式在技術文件庫中 [部署 802.1 x 有線和無線部署的伺服器憑證](../server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments.md?f=255&MSPPError=-2147217396) 。

##### <a name="public-ca"></a>公用 CA
您可以從用戶端電腦已經信任的公用 CA （例如 VeriSign）購買伺服器憑證。

當 CA 憑證安裝在 [受信任的根憑證授權單位] 憑證存放區時，用戶端電腦會信任 CA。 依預設，執行 Windows 的電腦會在其 [受信任的根憑證授權單位] 憑證存放區中安裝多個公用 CA 憑證。

建議您檢閱此部署案例中所用每個技術的設計和部署指南。 這些指南可協助您判斷此部署案例是否提供貴組織的網路需要的服務與設定。

### <a name="requirements"></a>需求
以下是使用本指南中記載的案例部署無線存取基礎結構的需求：

- 在部署此案例之前，您必須先購買 802.1 X \- 功能的無線存取點，以在您網站上的所需位置提供無線涵蓋範圍。 本指南的規劃章節可協助您判斷您的 Ap 必須支援的功能。

- \( \) 根據《 Windows Server 2016 核心網路指南》中的指示，安裝 Active Directory Domain Services 的 AD DS，如同其他必要的網路技術。

- 系統會部署 AD CS，並向 NPSs 註冊伺服器憑證。 當您部署本指南所使用的 PEAP \- MS \- CHAP v2 憑證型驗證方法時，需要這些憑證 \- 。

- 您組織的成員熟悉 IEEE 802.11 標準，您的無線 Ap 以及安裝在網路上用戶端電腦和裝置上的無線網路介面卡。 例如，您組織中的某個人熟悉無線電頻率類型、802.11 無線驗證 \( WPA2 或 WPA \) ，以及加密 \( AES 或 TKIP \) 。

## <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容
以下是本指南未提供的一些專案：

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>選取 802.1 X \- 功能的無線存取點的完整指導方針

由於 802.1 X 可用無線 Ap 的品牌和型號之間有許多差異 \- ，因此本指南不會提供有關下列各項的詳細資訊：

- 判斷最適合您需求的無線 AP 品牌或型號。

- 網路上無線 Ap 的實體部署。

- Advanced wireless AP 設定，例如無線虛擬區域網路絡 \( vlan \) 。

- 有關如何在 NPS 中設定無線 AP 廠商 \- 特定屬性的指示。

此外，設定的術語和名稱會隨著無線 AP 品牌和型號而有所不同，而且可能不符合本指南中使用的一般設定名稱。 如需無線 AP 設定的詳細資料，您必須參閱您的無線 Ap 製造商所提供的產品檔。

### <a name="instructions-for-deploying-nps-certificates"></a>部署 NPS 憑證的指示

部署 NPS 憑證有兩個替代方案。 本指南不提供完整的指引，可協助您判斷最符合您需求的替代方案。 但一般而言，您所面對的選擇如下：

- 從以 Windows 為基礎的用戶端已信任的公用 CA （例如 VeriSign）購買憑證 \- 。 針對較小的網路，通常建議使用這個選項。

- \( \) 使用 AD CS 在您的網路上部署公開金鑰基礎結構 PKI。 大部分的網路都建議使用這項功能，並提供如何使用 AD CS 部署伺服器憑證的指示，可在先前所述的部署指南中取得。

### <a name="nps-network-policies-and-other-nps-settings"></a>NPS 網路原則和其他 NPS 設定

除了您執行 **設定 802.1 x** wizard （如本指南所述）時所做的設定以外，本指南不提供手動設定 nps 條件、條件約束或其他 nps 設定的詳細資訊。

### <a name="dhcp"></a>DHCP

此部署指南不提供針對無線區域網路設計或部署 DHCP 子網的相關資訊。

## <a name="technology-overviews"></a>技術概觀
以下是部署無線存取的技術概述：

### <a name="ieee-8021x"></a>IEEE 802.1X
IEEE 802.1 X 標準定義了以埠為 \- 基礎的網路存取控制，用來為 Ethernet 網路提供驗證的網路存取。 這個以埠為 \- 基礎的網路存取控制會使用切換區域網路基礎結構的實體特性來驗證連接至 lan 埠的裝置。 如果驗證處理程序失敗，將拒絕存取這個連接埠。 雖然此標準是針對有線 Ethernet 網路所設計，但已經過調整，可用於802.11 的無線區域網路。

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1 x \- 功能的無線存取點 \( ap\)
此案例需要部署一或多個具有 802.1 X \- 功能的無線 ap，以與 \- 使用者服務 \( RADIUS 通訊協定中的遠端驗證撥號相容 \) 。

802.1 x 和 RADIUS \- 相容的 ap 部署在 radius 伺服器（例如 NPS）的 radius 基礎結構中時，稱為 *radius 用戶端*。

### <a name="wireless-clients"></a>無線用戶端
本指南提供完整的設定詳細資料，為 \- 使用 Windows 10、Windows 8.1 和 Windows 8 的無線用戶端電腦連線到網路的網域成員使用者，提供 802.1 x 驗證的存取。 電腦必須加入網域，才能成功建立驗證的存取權。

>[!NOTE]
>您也可以使用執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的電腦做為無線用戶端。

### <a name="support-for-ieee-80211-standards"></a>IEEE 802.11 標準的支援
支援的 Windows 和 Windows Server 作業系統提供 \- 802.11 無線網路功能的內建支援。 在這些作業系統中，已安裝的802.11 無線網路介面卡在網路和共用中心中會顯示為無線網路連線。

雖然 \- 內建支援802.11 無線網路功能，但 Windows 的無線元件相依于下列各項：

- 無線網路介面卡的功能。 已安裝的無線網路介面卡必須支援您需要的無線區域網路或無線安全性標準。 例如，如果無線網路介面卡不支援 Wi-fi \- 保護 \( 的存取 WPA \) ，您就無法啟用或設定 wpa 安全性選項。

- 無線網路介面卡驅動程式的功能。 為了讓您能夠設定無線網路選項，無線網路介面卡的驅動程式必須支援將其所有功能報告到 Windows。 確認您的無線網路介面卡驅動程式是針對作業系統的功能所撰寫的。 也請檢查無線網路介面卡廠商的 Microsoft Update 或網站，以確定驅動程式是最新的版本。

下表顯示常見 IEEE 802.11 無線標準的傳輸速率和頻率。

|標準|頻率|位傳輸速率|使用方式|
|-------------|---------------|--------------------------|---------|
|802.11|\-頻外產業、科學及醫療 \( ISM \) 頻率範圍 \( 2.4 到 2.5 GHz\)|每秒 2 mb \( mbps\)|已過時。 不常使用。|
|802.11b|S \- 頻內 ISM|11 Mbps|常用。|
|802.11a|C \- 頻外 ISM \( 5.725 至 5.875 GHz\)|54 Mbps|由於費用和有限範圍，因此不常使用。|
|802.11g|S \- 頻內 ISM|54 Mbps|廣泛使用。 802.11 g 裝置與 802.11 b 裝置相容。|
|802.11 n \ 2.4 和 5.0 GHz|C \- 頻外和 S \- 頻內 ISM|250 Mbps|\-以預先 RATIFICATION IEEE 802.11 n 標準為基礎的裝置，在2007年8月變成可供使用。 許多 802.11 n 裝置都與 802.11 a、b 和 g 裝置相容。|
|802.11ac |5 GHz |6.93 Gbps |802.11 a c （由 IEEE 在2014中核准）比 802.11 n 更具擴充性且速度更快，而且會部署在 Ap 和無線用戶端兩者皆支援的位置。|

### <a name="wireless-network-security-methods"></a>無線網路安全性方法
**無線網路安全性方法** 是一組非正式的無線驗證 \( ，有時也稱為無線安全性 \) 和無線安全性加密。 無線驗證和加密會成對使用，以防止未經授權的使用者存取無線網路，以及保護無線傳輸。

在群組原則的無線網路原則中設定無線安全性設定時，有多個組合可供選擇。 不過，只有 WPA2 \- enterprise、WPA \- enterprise 和開啟檔案 802.1 x 驗證標準支援 802.1 x 驗證的無線部署。

>[!NOTE]
>設定無線網路原則時，您必須選取 **WPA2 \- enterprise**、 **WPA \- enterprise** 或 **開啟檔案 802.1 x** ，才能存取 802.1 x 驗證無線部署所需的 EAP 設定。

#### <a name="wireless-authentication"></a>無線驗證
本指南針對 802.1 X 驗證的無線部署，建議使用下列無線驗證標準。

**Wi-fi \-Wi-fi 保護的存取– \( Enterprise \- wpa \) enterprise** Wpa 是由 WiFi 聯盟所開發的過渡標準，可符合802.11 無線安全性通訊協定。 WPA 通訊協定的開發目的是為了回應先前有線對等隱私權 \( WEP 通訊協定中發現的一些嚴重瑕疵 \) 。

WPA \- Enterprise 透過下列方式，提供優於 WEP 的增強安全性：

1. 需要使用 802.1 X EAP 架構做為基礎結構一部分的驗證，以確保集中式相互驗證和動態金鑰管理

2. \( \) 使用訊息完整性檢查 MIC 增強完整性檢查值 ICV \( \) ，以保護標頭和承載

3. 執行框架計數器以防止重新執行攻擊

**Wi-fi \-Wi-fi Protected Access 2 – Enterprise \( wpa2 \- enterprise \)** （例如 WPA \- enterprise standard），WPA2 \- enterprise 使用 802.1 x 和 EAP 架構。 WPA2 \- Enterprise 針對多個使用者和大型受控網路提供更強大的資料保護。 WPA2 \- Enterprise 是一種健全的通訊協定，其設計目的是要透過驗證服務器驗證網路使用者，以防止未經授權的網路存取。

#### <a name="wireless-security-encryption"></a>無線安全性加密
無線安全性加密是用來保護無線用戶端與無線 AP 之間傳送的無線傳輸。 無線安全性加密是與選取的網路安全性驗證方法搭配使用。 依預設，執行 Windows 10、Windows 8.1 和 Windows 8 的電腦支援兩種加密標準：

1. **時態性金鑰完整性通訊協定** \(TKIP \) 是一種較舊的加密通訊協定，其設計目的是為了提供比本質上弱式有線對等隱私權 \( WEP 通訊協定所提供更安全的無線加密 \) 。 TKIP 是由 IEEE 802.11 i 工作組和 Wi-fi 聯盟所設計， \- 不需要取代舊版硬體就能取代 WEP。 TKIP 是封裝 WEP 承載的演算法套件，可讓舊版 WiFi 設備的使用者升級至 TKIP，而不需要更換硬體。 就像 WEP 一樣，TKIP 使用 RC4 串流加密演算法作為基礎。 不過，新的通訊協定會使用唯一的加密金鑰來加密每個資料封包，而且金鑰比 WEP 更強。 雖然 TKIP 適用于在設計為只使用 WEP 的舊版裝置上升級安全性，但是在大部分情況下，它並不會解決保護敏感性政府或公司資料傳輸的所有安全性問題。

2. **進階加密標準** \(AES \) 是用於加密商業和政府資料的慣用加密通訊協定。 AES 提供比 TKIP 或 WEP 更高層級的無線傳輸安全性。 與 TKIP 和 WEP 不同的是，AES 需要支援 AES 標準的無線硬體。 AES 是對稱 \- 金鑰加密標準，使用三個封鎖密碼、aes \- 128、aes \- 192 和 aes \- 256。

在 Windows Server 2016 中， \- 當您選取 [WPA2 Enterprise] 的驗證方法 \- （建議使用）時，可使用下列 AES 型無線加密方法來設定無線設定檔內容。

1. **AES \-CCMP**。 計數器模式加密區塊鏈訊息驗證碼 Protocol \( CCMP \) 會實作為 802.11 i 標準，而且是針對比 WEP 提供的更高安全性加密所設計，並使用128位 AES 加密金鑰。
2. **AES \-GCMP**。 Galois Counter Mode Protocol \( GCMP \) 是由 802.11 a CCMP 所支援，比 AES 更有效率， \- 並為無線客戶提供更佳的效能。 GCMP 使用256位 AES 加密金鑰。

> [!IMPORTANT]
> 有線等 \( 的隱私權 WEP \) 是用來加密網路流量的原始無線安全性標準。 您不應該在網路上部署 WEP，因為這種 \- 過期的安全性形式有已知的弱點。

### <a name="active-directory-domain-services-ad-ds"></a>Active Directory Domain Services \( AD DS\)
AD DS 提供分散式資料庫，可儲存和管理 \- 從啟用目錄之應用程式的網路資源和應用程式特定資料的相關資訊 \- 。 系統管理員可以使用 AD DS 將網路項目 (像是使用者、電腦以及其他裝置) 組織成階層式的內含項目結構。 階層式內含專案結構包括 Active Directory 樹系、樹系中的網域，以及 \( \) 每個網域中的組織單位 ou。 正在執行 AD DS 的伺服器稱為 *網域控制站*。

AD DS 包含 IEEE 802.1 X 和 PEAP MS CHAP v2 所需的使用者帳戶、電腦帳戶和帳戶內容 \- ， \- 以驗證使用者認證，並評估無線連線的授權。

### <a name="active-directory-users-and-computers"></a>Active Directory 使用者及電腦
Active Directory 消費者和電腦是 AD DS 的元件，其中包含代表實體實體的帳戶，例如電腦、個人或安全性群組。 *安全性群組* 是一組使用者或電腦帳戶的集合，系統管理員可以將其當作單一單位來管理。 屬於特定群組的使用者和電腦帳戶稱為 *群組成員*。

### <a name="group-policy-management"></a>群組原則管理
群組原則管理可啟用以目錄為 \- 基礎的變更和設定管理的使用者和電腦設定，包括安全性和使用者資訊。 您可以使用群組原則來定義使用者和電腦群組的設定。 使用群組原則，您可以指定登錄專案、安全性、軟體安裝、腳本、資料夾重新導向、遠端安裝服務和 Internet Explorer 維護的設定。 您所建立的群組原則設定會包含在群組原則物件 \( GPO 中 \) 。 藉由將 GPO 與選取的 Active Directory 系統容器（網站、網域和 Ou）產生關聯，您就可以將 GPO 的設定套用至這些 Active Directory 容器中的使用者和電腦。 若要管理整個企業的群組原則物件，您可以使用群組原則管理編輯器 Microsoft Management Console \( MMC \) 。

本指南提供有關如何在群組原則管理的無線網路 \( IEEE 802.11 原則延伸中指定設定的詳細指示 \) 。 無線網路 \( IEEE 802.11 \) 原則會 \- 使用 802.1 x 驗證無線存取的必要連線和無線設定來設定網域成員無線用戶端電腦。

### <a name="server-certificates"></a>伺服器憑證
此部署案例需要每個執行 802.1 X 驗證的 NPS 的伺服器憑證。

伺服器憑證是一份數位檔，通常用於驗證以及保護開放式網路上的資訊。 憑證將公開金鑰安全地繫結到保存對應私密金鑰的實體。 憑證由發行 CA 以數位方式簽署，而且可以針對使用者、電腦或服務發出。

憑證授權單位單位 \( CA \) 是一種實體，負責建立和保證屬於 \( 一般使用者或電腦 \) 或其他 ca 的公用金鑰的真實性。 憑證授權單位單位的活動可以透過簽署的憑證來將公開金鑰系結至辨別名稱、管理憑證序號，以及撤銷憑證。

Active Directory 憑證服務 \( AD CS \) 是一種伺服器角色，會將憑證發行為網路 CA。 AD CS 憑證基礎結構（也稱為 *公開金鑰基礎結構 \( PKI \)*）提供可自訂的服務來發行和管理企業的憑證。

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP、PEAP 和 PEAP \- MS \- CHAP v2
\(可延伸的驗證通訊協定 EAP \) \- \- \( \) 會允許使用任意長度的認證和資訊交換的其他驗證方法，藉此擴充點對點通訊協定 PPP。 使用 EAP 驗證時，網路存取用戶端和驗證器 \( （例如 NPS）都 \) 必須支援相同的 EAP 類型，才能成功進行驗證。 Windows Server 2016 包含 EAP 基礎結構、支援兩種 EAP 類型，以及將 EAP 訊息傳遞至 NPSs 的能力。 您可以使用 EAP 來支援其他驗證配置，稱為 *EAP 類型*。 Windows Server 2016 支援的 EAP 類型為：

- 傳輸層安全性 \( TLS\)

- Microsoft 挑戰交握驗證通訊協定第2版 \( MS \- CHAP v2\)

>[!IMPORTANT]
>強式 EAP 類型（例如以憑證為基礎的 EAP 類型） \( \) 提供更佳的安全性，以防範暴力 \- 密碼破解攻擊、字典攻擊和密碼猜測攻擊，而非 \- 以密碼為基礎的驗證通訊協定， \( 例如 CHAP 或 MS \- CHAP 版本 1 \) 。

受保護 \( 的 EAP peap \) 使用 TLS 來建立驗證 PEAP 用戶端（如無線電腦）和 PEAP 驗證器（例如 NPS 或其他 RADIUS 伺服器）之間的加密通道。 PEAP 不指定驗證方法，但是它會為其他 EAP 驗證通訊協定（例如 EAP MS CHAP v2）提供額外的安全性，這些通訊協定 \( \- \- \) 可透過 PEAP 提供的 TLS 加密通道來操作。 PEAP 可作為存取用戶端的驗證方法，而用戶端會透過下列類型的網路存取伺服器 nas 來連線至您的組織網路 \( \) ：

- 802.1 x \- 功能的無線存取點

- 802.1 x \- 可用驗證參數

- 執行 Windows Server 2016 和遠端存取服務 \( RAS 的電腦， \) 這些電腦設定為虛擬私人網路 \( VPN \) 伺服器、DirectAccess 伺服器或兩者

- 執行 Windows Server 2016 和遠端桌面服務的電腦

\- \- \- 因為使用者驗證是透過使用密碼型認證的 \- \( 使用者名稱和密碼 \) ，而不是憑證或智慧卡來執行，所以 PEAP MS CHAP v2 比 EAP TLS 更容易部署。 只有 NPS 或其他 RADIUS 伺服器需要擁有憑證。 Nps 會在驗證程式期間使用 NPS 憑證，向 PEAP 用戶端證明其身分識別。

本指南提供將您的無線用戶端和 NPS 設定 \( \) 為使用 PEAP \- MS \- CHAP v2 進行 802.1 x 驗證存取的指示。

### <a name="network-policy-server"></a>網路原則伺服器
網路原則伺服器 \( NPS 可 \) 讓您使用 \- 使用者服務 \( radius \) 伺服器和 RADIUS Proxy 中的遠端驗證撥號來集中設定和管理網路原則。 當您部署 802.1 X 無線存取時，需要 NPS。

當您將 802.1 X 無線存取點設定為 NPS 中的 RADIUS 用戶端時，NPS 會處理由 Ap 傳送的連線要求。 在連接要求處理期間，NPS 會執行驗證和授權。 驗證會判斷用戶端是否已出示有效的認證。 如果 NPS 成功驗證要求的用戶端，則 NPS 會判斷是否授權用戶端進行要求的連接，以及允許或拒絕連線。 這會更詳細地說明，如下所示：

#### <a name="authentication"></a>驗證

成功的相互 PEAP \- MS \- CHAP v2 驗證有兩個主要部分：

1.  用戶端會驗證 NPS。  在此相互驗證階段中，NPS 會將其伺服器憑證傳送到用戶端電腦，讓用戶端可以使用憑證來驗證 NPS 的身分識別。 若要成功驗證 NPS，用戶端電腦必須信任發出 NPS 憑證的 CA。 用戶端會在用戶端電腦上的 [信任的根憑證授權單位] 憑證存放區中有 CA 的憑證時，信任此 CA。

    如果您部署自己的私人 CA，CA 憑證會自動安裝在目前使用者的「信任的根憑證授權單位」憑證存放區中，而在網域成員用戶端電腦上重新整理群組原則時，則會自動安裝在本機電腦上。 如果您決定要從公用 CA 部署伺服器憑證，請確定公用 CA 憑證已在 [受信任的根憑證授權單位] 憑證存放區中。

2.  NPS 會驗證使用者。 用戶端成功驗證 NPS 之後，用戶端會將使用者的密碼型 \- 認證傳送給 nps，以針對 Active Directory Domain Services AD DS 中的使用者帳戶資料庫驗證使用者的認證 \( \) 。

如果認證有效且驗證成功，NPS 就會開始處理連接要求的授權階段。 如果認證無效且驗證失敗，NPS 會傳送拒絕存取訊息，並拒絕連線要求。

#### <a name="authorization"></a>授權

執行 NPS 的伺服器會執行授權，如下所示：

1. NPS 會在 AD DS 中檢查使用者或電腦帳戶撥入屬性的限制 \- 。 Active Directory 消費者和電腦中的每個使用者和電腦帳戶包含多個屬性，包括在 [ **撥 \- 入** ] 索引標籤上找到的內容。在此索引標籤的 [ **網路存取權限**] 中，如果值為 [ **允許存取**]，則會授權使用者或電腦連線到網路。 如果值為 [ **拒絕存取**]，則使用者或電腦沒有連線到網路的授權。 如果此值是 **透過 NPS 網路原則控制存取權**，NPS 會評估設定的網路原則，以判斷使用者或電腦是否有權連線到網路。

2. NPS 接著會處理其網路原則，以尋找符合連接要求的原則。 如果找到相符的原則，NPS 會根據該原則的設定來授與或拒絕連接。

如果驗證和授權都成功，且相符的網路原則授與存取權，NPS 會授與網路的存取權，而且使用者和電腦可以連線到他們有許可權的網路資源。

>[!NOTE]
>若要部署無線存取，您必須設定 NPS 原則。 本指南提供在 NPS 中使用 **設定 802.1 x wizard** 來建立 802.1 x 驗證無線存取之 nps 原則的指示。

### <a name="bootstrap-profiles"></a>啟動程式設定檔
在 802.1 X \- 驗證無線網路中，無線用戶端必須提供由 RADIUS 伺服器驗證的安全性認證，才能連接到網路。 針對受保護的 EAP \[ PEAP \] \- Microsoft 挑戰交握驗證通訊協定第2版 \[ MS \- CHAP v2 \] ，安全性認證是使用者名稱和密碼。 對於 EAP \- 傳輸層安全性 \[ TLS \] 或 PEAP \- tls，安全性認證是憑證，例如用戶端使用者和電腦憑證或智慧卡。

當連接到設定為執行 PEAP \- MS \- CHAP V2、PEAP \- tls 或 EAP tls 驗證的網路時 \- ，Windows 無線用戶端預設也必須驗證 RADIUS 伺服器所傳送的電腦憑證。 RADIUS 伺服器針對每個驗證會話傳送的電腦憑證通常稱為伺服器憑證。

如先前所述，您可以使用下列兩種方式之一來發出 RADIUS 伺服器的伺服器憑證：從 \( 像是 VeriSign、inc.、的商業 ca， \) 或從您部署在網路上的私人 ca。 如果 RADIUS 伺服器傳送的電腦憑證是由已在用戶端的「受信任的根憑證授權單位」憑證存放區中安裝根憑證的商業 CA 所發行，則無論無線用戶端是否已加入 Active Directory 網域，無線用戶端都可以驗證 RADIUS 伺服器的電腦憑證。 在此情況下，無線用戶端可以連線到無線網路，然後您可以將電腦加入網域。

>[!NOTE]
>您可以停用要求用戶端驗證伺服器憑證的行為，但不建議在生產環境中停用伺服器憑證驗證。

無線啟動設定檔是一種暫時設定檔，其設定方式是讓無線用戶端使用者在 \- 電腦加入網域之前， \/ 或在使用者第一次使用指定的無線電腦成功登入網域之前，先連線到 802.1 x 驗證無線網路。  本節摘要說明嘗試將無線電腦加入網域時所發生的問題，或使用者第一次登入網域時，是否要使用已加入網域的 \- 無線電腦。

對於使用者或 IT 系統管理員無法將電腦實際連線到有線乙太網路以將電腦加入網域，而且電腦沒有在其 **受信任的根憑證授權** 單位憑證存放區中安裝必要的發行根 CA 憑證的部署，您可以使用暫時性的無線連線設定檔（稱為 *啟動設定檔*）來設定無線用戶端，以連接到無線網路。

*啟動程式設定檔* 會移除驗證 RADIUS 伺服器電腦憑證的需求。 此暫時設定可讓無線使用者將電腦加入網域，此時會套用無線網路 \( IEEE 802.11 原則，並在 \) 電腦上自動安裝適當的根 CA 憑證。

套用群組原則時，會在電腦上套用強制執行相互驗證需求的一或多個無線連線設定檔;啟動程式設定檔已不再需要且已移除。 將電腦加入網域並重新啟動電腦之後，使用者就可以使用無線連線來登入網域。

如需使用這些技術的無線存取部署程式總覽，請參閱 [無線存取部署總覽](b-wireless-access-deploy-overview.md)。