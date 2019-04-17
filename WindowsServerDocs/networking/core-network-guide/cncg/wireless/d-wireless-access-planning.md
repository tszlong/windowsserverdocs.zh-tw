---
title: 無線存取部署計劃
description: 本主題是 Windows Server 2016 網路指南「部署密碼基礎 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1aa7d9fa66c480988ec7e3a97447157bd3eab9c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-planning"></a>無線存取部署計劃

>適用於：Windows Server（以每年次管道）、Windows Server 2016

部署 wireless 存取之前，您必須計劃下列項目：

- 安裝 wireless 存取點 \(APs\) 在您的網路

- Wireless client 設定與存取

下列章節依照計劃提供詳細資訊。

## <a name="planning-wireless-ap-installations"></a>規劃 wireless AP 安裝
當您設計 wireless 網路存取方案時，您必須執行下列動作：

1. 判斷您 wireless Ap 必須支援何種標準
2. 判斷您想要提供 wireless 服務的涵蓋範圍區域
3. 判斷您想要找出 wireless Ap

此外，您必須 wireless ap 計劃的 IP 位址配置和無線戶端。 查看區段**計劃設定的 wireless AP 的 NPS 在**下方的相關資訊。

### <a name="verify-wireless-ap-support-for-standards"></a>請確認標準 wireless AP 支援
一致性之目的，並輕鬆部署及管理 AP，建議您部署 wireless Ap 相同品牌和模型。

您的部署 wireless Ap 必須支援下列項目：

- **IEEE 802.1 X**

- **RADIUS 驗證**

- **Wireless 驗證及密碼。** 列出至少慣用以最多的訂單：

    1.  有好一段 WPA2\ 企業版

    2.  使用 TKIP WPA2\ 企業版

    3.  有好一段 WPA\ 企業版

    4.  使用 TKIP WPA\ 企業版

>[!NOTE]
>若要部署 WPA2，您必須使用 wireless 網路介面卡，也支援 WPA2 wireless Ap。 否則，請使用 WPA\-企業版。

此外，若要提供的網路提高的安全性，wireless Ap 必須支援下列安全性選項：

- **DHCP 篩選。** Wireless AP 必須篩選避免在這些案例中 wireless client 設定為 DHCP 伺服器 DHCP 廣播訊息傳輸 IP 連接埠。 Wireless AP 必須封鎖 client 從網路傳送 UDP 連接埠 68 IP 封包。

- **DNS 篩選。** 若要防止 client 執行為 DNS 伺服器的 IP 連接埠必須篩選 wireless AP。 Wireless AP 必須封鎖 client 傳送 IP 封包 TCP 或 UDP 連接埠 53 網路。

- **Client 隔離**如果 wireless 存取點提供 client 隔離的功能，您應該讓避免詐騙利用可能位址解析度通訊協定 \(ARP\) 的功能。

### <a name="identify-areas-of-coverage-for-wireless-users"></a>找出需 wireless 使用者的涵蓋範圍
使用每個建置針對每個樓層架構的繪圖找出您想要提供 wireless 涵蓋的區域。 例如，找出適當辦公室、會議房間、lobbies、高速公路或 courtyards。

在繪圖，表示任何干擾 wireless 訊號，例如醫療設備 wireless 攝影機、無線電話運作 2.4 透過 2.5 GHz 業界、科學與醫療 \(ISM\) 範圍的裝置和 Bluetooth\ 功能的裝置。

在繪圖，標記方面的建築，可能會干擾 wireless 訊號。用於建置建構金屬物件可能會影響 wireless 訊號。 下列常見物件，例如干擾訊號傳播：電梯、加熱及 air\-穩定導管，以及 girders 具體支援。

指向您 AP 製造商，以取得可能會導致 wireless AP 廣播頻率衰減來源的相關資訊。 大部分的 Ap 提供測試軟體，您可以用來檢查訊號越、錯誤和資料輸送量。

### <a name="determine-where-to-install-wireless-aps"></a>判斷安裝 wireless Ap 的位置
在 [架構繪圖，尋找您 wireless Ap 關閉達到一起提供充裕 wireless 涵蓋範圍，但最不足分開，它們不會干擾彼此。

必要 Ap 距離 AP 類型而定，AP 天線，層面建置封鎖無線訊號及其他來源的干擾。 您可以在每個 wireless AP 不會從任何相鄰 wireless AP 超過 300 英呎的標記 wireless AP 位置。 查看 wireless AP 製造商 AP 規格文件及位置的指導方針。

暫時安裝 wireless Ap 架構繪圖上指定的位置。 然後，使用膝上型電腦配備 wireless 配接器通常會提供調查軟體網站與 802.11 wireless 介面卡，判斷訊號越中每個涵蓋範圍。

在涵蓋範圍低訊號越所在的區域，放在 AP 改善訊號力量的涵蓋範圍時，請安裝其他 wireless Ap 提供所需的涵蓋範圍、重新放置，或移除訊號干擾的來源。  

更新繪圖指出所有 wireless Ap 的最終位置架構。 疑難排解操作期間之後，或您想要升級或更換 Ap，幫助有準確 AP 位置的地圖。

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>規劃 wireless AP 和 NPS RADIUS Client 設定
您可以使用 NPS 設定 wireless Ap 排列或群組中。

如果您要部署大型包含許多 Ap wireless 網路，就能輕鬆設定 Ap 群組中。 若要新增 Ap 為 NPS RADIUS client 群組，您必須設定這些屬性 Ap。

- Wireless Ap 會以相同的 IP 位址範圍的 IP 位址設定。

- Wireless Ap 的所有設定的共用相同的密碼。

### <a name="plan-the-use-of-peap-fast-reconnect"></a>使用快速重新連接 PEAP 計劃
在 802.1 X 基礎結構，wireless 存取點被設定為 RADIUS 戶端 RADIUS 伺服器。 部署時重新連接 PEAP 快速兩個或更多的存取點之間漫遊 wireless client 不需要驗證的每個新的關聯。

重新連接 PEAP 快速因為驗證要求的新的存取點轉寄給最初執行 client 連接要求驗證和授權 NPS 伺服器降低 authenticator client 與驗證的回應時間。

由於 PEAP client 和兩者都使用先前 NPS 伺服器快取 \(TLS\) Tls 連接屬性 \（的集合命名 TLS handle\）、NPS 伺服器快速判斷 client 的重新連線授權。

>[!IMPORTANT]
>快速頻道的重新連接到正確運作，Ap 必須設定為 RADIUS 固定的相同 NPS 伺服器。

如果不使用原始 NPS 伺服器或 client 移至 [設定為到不同的 RADIUS 伺服器 RADIUS client 的存取點，必須 client 之間新 authenticator 來執行完整驗證。

### <a name="wireless-ap-configuration"></a>Wireless AP 設定
下列清單摘要通常 802.1X\ 上設定的項目-能 wireless Ap:

> [!NOTE]
> 在項目名稱可以視品牌和型號，而且可能從下列清單中的不同。 查看您 wireless AP 文件 configuration\ 特定的詳細資料。

- **服務設定識別碼 \(SSID\)**。 這是 wireless 網路的名稱 \ (例如，ExampleWlan\)，以及 wireless 戶端到通知的名稱。 若要減少混淆，通知您選擇 SSID 應該不符合所接收的 wireless 網路的範圍中的任何 wireless 網路廣播 SSID。

    萬一中的多個 wireless Ap 部署 wireless 在相同網路的一部分，請使用相同的 SSID 設定每個 wireless AP。 萬一中的多個 wireless Ap 部署 wireless 在相同網路的一部分，請使用相同的 SSID 設定每個 wireless AP。  

    萬一您有需要部署特定企業需求的不同 wireless 網路位置，您 wireless AP 的上一個網路應該廣播比 SSID 您其他 network\(s\) 不同 SSID。 例如，如果您的員工和來賓需要 wireless 不同的網路，您可能會設定您 wireless Ap 的企業網路 SSID 設為廣播的**ExampleWLAN**。 客體網路，您可以再設定廣播每個 wireless AP SSID **GuestWLAN**。 在這種方式可以連接您的員工和來賓而不需要混淆想要的網路。  

    > [!TIP]  
    > 某些 wireless AP 有廣播多 SSID 的容納 multi\ 網路部署的功能。 Wireless AP 的可以廣播多 SSID，可減少部署及操作維護成本。  

- **無線驗證及加密**。

    Wireless 驗證是時 wireless client 關聯 wireless 存取點，使用安全性驗證。  

    Wireless 加密是安全性加密編碼器 wireless 驗證用來保護 wireless AP 和 wireless client 之間傳送通訊。  

- **無線 AP IP 位址 \(static\)**。 在每個 wireless AP，設定唯一靜態 IP 位址。 如果 DHCP 伺服器由服務子網路，請確定的所有 AP IP 位址都落 DHCP 排除項目範圍，DHCP 伺服器不會嘗試另一部電腦或裝置發行相同的 IP 位址。 排除項目範圍所述程序」來建立及啟動 DHCP 新的領域」[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。 如果您計劃設定的群組 NPS RADIUS 戶端 Ap，在群組中的每個 AP 必須相同的 IP 位址範圍的 IP 位址。

- **DNS 名稱**。 您可以設定部分 wireless Ap DNS 名稱。 設定每個 wireless AP 唯一名稱。 例如擁有部署 wireless Ap multi\-故事建置中，您可能會名稱的第三個樓層 AP3\-01、AP3\ 02 和 AP3\-03 部署前三個 wireless Ap。

- **Wireless AP 子網路遮罩**。 設定的部分的 ip 位址是網路 ID 和 IP 位址的一部分是主機遮罩。

- **AP DHCP 服務**。 如果您 wireless AP built\ 中 DHCP 服務，來停用它。

- **RADIUS 共用的密碼**。 使用獨特的 RADIUS 共用的每個 wireless AP 密碼，除非您計畫將 NPS RADIUS 伺服器中群組-的情況在您必須設定使用相同的共用密碼 Ap 群組中的所有的設定。 共用的密碼應至少 22 字元，同時大寫隨機系列及小寫字母、數字、標點符號。 若要確保隨機，您可以使用隨機字元代程式來建立您的共用的密碼。 建議您的每個 wireless AP 錄製共用的密碼，以及將它儲存在安全的位置，例如安全 office。 當您設定主機 NPS RADIUS 戶端您將會建立每個 AP virtual 的版本。 您每個 virtual AP NPS 中的設定共用的密碼必須符合共用實際、實體 AP 的密碼。

- **RADIUS 伺服器的 IP 位址**。 輸入您想要用來驗證，以及授權連接要求本存取點 NPS 伺服器的 IP 位址。

- **UDP port\(s\)**。 預設 NPS RADIUS 驗證訊息與 UDP 連接埠 1813 年和 RADIUS 計量郵件 1646 年使用 1812 年和 1645 年的 UDP 連接埠。 建議您不要變更預設 RADIUS UDP 連接埠設定。

- **Vsa**。 部分 wireless Ap 需要 vendor\ 特定屬性 \(VSAs\) 提供完整 wireless AP 功能。

- **篩選 DHCP**。 設定 wireless Ap 封鎖 wireless 戶端從網路傳送 UDP 連接埠 68 IP 封包。 查看您 wireless AP 設定 DHCP 篩選的文件。

- **篩選 DNS**。 設定 wireless Ap 封鎖 wireless 戶端從網路埠 53 傳送 IP 封包。 查看您 wireless AP 設定 DNS 篩選的文件。

## <a name="planning-wireless-client-configuration-and-access"></a>規劃 wireless client 設定與存取

規劃 802.1X\ 部署時-驗證 wireless 存取權，您必須考慮因素 client\ 特定：

- **規劃多個標準支援**。

    判斷是否 wireless 電腦的所有使用相同的版本的 Windows，或是否有混合執行其他作業系統的電腦。 如果有不同，請確定您了解任何不同標準支援的作業系統。

    判斷所有 wireless 網路介面卡上的所有 wireless client 電腦是否支援相同 wireless 標準，或是您需要是否支援各種標準。 例如，判斷是否某些網路介面卡的硬體驅動程式支援 WPA2\-企業版和好一段，有些則只 WPA\-企業版和 TKIP 可支援。

- **規劃 client 驗證模式**。 驗證模式定義 Windows 戶端如何處理網域認證。 您可以從下列三種網路驗證模式 wireless 的網路原則中選取。  

    1. **使用者 re\ 驗證**。 此模式指定驗證一律會執行使用安全性憑證根據電腦目前的狀態。 當到電腦不使用者登入時，來透過認證的電腦執行驗證。 當使用者登入電腦時，一律使用使用者的認證來執行驗證。  

    2. **電腦只有**。 電腦只有模式指定驗證一律執行來使用電腦認證。  

    3.  **使用者驗證**。 驗證的使用者模式指定驗證只登入電腦時執行。 不使用者登入電腦時，將不會執行驗證嘗試。  

- **規劃 wireless 限制**。 判斷要所有 wireless 使用者提供的相同層級的存取 wireless 網路，或是要限制適用於某些 wireless 使用者的存取。 您可以適用於特定的 wireless 使用者群組中 NPS 限制。  例如，您可以定義特定天和小時特定群組獲准 wireless 網路的存取權。  

- **規劃方法來新增新的 wireless 電腦**。 Wireless\ 能力的電腦加入您的網域之前部署 wireless 網路，如果電腦已連接至未受 802.1 X 的有線網路的區段，wireless 設定會自動套用設定 Wireless 網路之後 \ (IEEE 802.11\) 網域控制站和群組原則 wireless client 在重新整理之後原則。  

    不已經加入網域的電腦，但是，您必須計劃要套用的設定所需的 802.1X\ 方法-驗證存取。 例如，判斷是否想要使用其中一項下列方法加入網域的電腦。

    1.  將電腦連接到 802.1 X，不受有線網路的區段，然後加入網域的電腦。

    2.  Wireless 為使用者提供的步驟和新增自己 wireless 開機設定檔，可讓他們加入網域的電腦所需的設定。

    3.  指派給 IT wireless 戶端加入網域的人員。

### <a name="planning-support-for-multiple-standards"></a>多個標準計劃的支援

無線網路 \ (IEEE 802.11\) 原則擴充功能在群組原則中提供的各種組態選項，支援各種部署選項。

您可以部署 wireless Ap 標準您想要支援，設定，然後 Wireless 網路設定多個 wireless 設定檔 \ (IEEE 802.11\) 原則，與每個設定檔，您需要標準指定一組。

例如，如果您的網路有 wireless 電腦支援 WPA2\-企業版和好一段，支援 WPA\-企業版和好一段的其他電腦和其他電腦支援只 WPA\-企業版和 TKIP，必須判斷您是否想要：

- 設定單一支援的所有 wireless 電腦使用的弱加密的方式來設定檔，您電腦的所有支援-在本案例中 WPA\-企業版和 TKIP。  
- 設定兩個提供支援的每個 wireless 電腦最佳可能的安全性設定檔。 您會在這個執行個體設定一個指定最穩定加密的設定檔 \（WPA2\-企業版和 AES\），並使用較弱 WPA\-企業版和 TKIP 加密的設定檔。 在此範例中，務必將放 WPA2\-企業版和好一段最高優先順序中使用的設定檔。 電腦不能使用 WPA2\-企業版和好一段自動將會跳到下一步的優先順序設定檔並處理指定 WPA\-企業版和 TKIP 的設定檔。

> [!IMPORTANT]
> 您必須將最安全的標準較高的設定檔放排序清單中的設定檔，因為連接電腦使用的第一個個人檔案，都能使用。

### <a name="planning-restricted-access-to-the-wireless-network"></a>規劃限制 wireless 網路的存取權

很多時候，您可能想要 wireless 使用者提供 wireless 網路的存取權的不同層級。 例如，您可能要允許部分使用者不受限制的存取權，星期幾每一天中的任何一小時。 其他使用者，您可能只想核心時間，每星期五，可讓存取，並在上星期六和星期日拒絕存取。

本指南提供建立存取環境地點 wireless 使用者的所有常見存取 wireless 資源群組中的指示操作。 您 snap\ 中建立 wireless 使用者安全性群組的 Active Directory 使用者和電腦，然後再進行每一位的使用者想要權限授與 wireless 該群組成員。

當您設定 NPS 的網路原則時，您可以指定 wireless 使用者安全性群組為 NPS 處理判斷授權時的物件。

不過，如果您的部署需要支援的不同層級的存取您需要只能執行下列動作：  

1. 建立多個 Wireless 使用者安全性群組 Active Directory 使用者，在電腦中建立其他 wireless 安全性群組。 例如，您可以建立群組，其中包含擁有完整存取權，群組的人員只有在正常運作的時間，存取和其他群組符合其他條件符合您需求的使用者。

2. 將使用者新增到您所建立的適當安全性群組。

3. 設定為每個額外 wireless 安全性群組，其他 NPS 網路原則和設定的條件與限制的每個群組，您需要套用原則。

### <a name="planning-methods-for-adding-new-wireless-computers"></a>新增新的 wireless 電腦計劃的方法

將新 wireless 電腦加入網域，然後登入網域慣用的方法是使用的區域網路存取網域控制站，並使用 802.1 X 驗證乙太網路將不受區段有線的連接。

有時候，不過，它可能不是電腦加入網域，或使用已經加入網域的電腦使用有線的連接上嘗試它們第一次登入的使用者使用有線的連接實用。

若要加入網域的電腦使用 wireless 連接或使用者網域登入第一次使用 domain\ 加入電腦和 wireless 連接，wireless 戶端必須先建立連接 wireless 網路上的存取權的網路網域控制站使用其中一項下列方法區段。

1. **Wireless 電腦加入網域，IT 人員的成員，並設定單一登入開機 wireless 設定檔。** 這種方法，IT 系統管理員 wireless 電腦連接到有線乙太網路，然後將電腦加入的網域。 然後系統管理員散發給使用者的電腦。 當使用者開始電腦時，以手動方式使用者登入處理程序指定網域認證可用同時連接到 wireless 網路並登入的網域。  

2. **使用者手動 wireless 電腦開機 wireless 設定檔，會設定，然後加入網域。** 使用此方法，使用者手動設定 wireless 電腦開機 wireless 設定檔根據 IT 系統管理員的指示操作。 Wireless 設定檔開機可讓使用者建立 wireless 連接，並再加入網域的電腦。 加入網域的電腦，開機之後使用者可以登入網域使用 wireless 連接與他們的網域 account 認證。

部署 wireless 存取，來查看[Wireless 存取部署](e-wireless-access-deployment.md)。
