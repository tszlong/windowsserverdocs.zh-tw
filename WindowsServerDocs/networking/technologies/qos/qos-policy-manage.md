---
title: 管理 QoS 原則
description: 本主題提供有關如何建立和管理 Windows Server 2016 中的服務品質 (QoS) 原則的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94e5a1832a6c1e160b9cc338d50636026a5eb751
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851669"
---
# <a name="manage-qos-policy"></a>管理 QoS 原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解如何使用 QoS 原則精靈 建立、 編輯或刪除 QoS 原則。

>[!NOTE]
>  本主題中，除了下列的 QoS 原則管理文件使用。
> 
>  - [QoS 原則事件和錯誤](qos-policy-errors.md)

在 Windows 作業系統中，QoS 原則會結合標準為依據的 QoS 功能，與群組原則的管理能力。 這種組合的組態會讓群組原則物件的 QoS 原則的簡單應用程式。 Windows 包含的 QoS 原則精靈可協助您達成下列工作。

-  [建立 QoS 原則](#bkmk_createpolicy)

-  [檢視、 編輯或刪除 QoS 原則](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>建立 QoS 原則

建立 QoS 原則之前，請務必了解兩個金鑰 QoS 控制項用來管理網路流量：

- DSCP 值

-   節流閥速率

### <a name="prioritizing-traffic-with-dscp"></a>使用 DSCP 排定流量

如先前的特定業務應用程式範例中所述，您可以使用定義輸出網路流量的優先順序**指定 DSCP 值**具有特定 DSCP 值設定 QoS 原則。 

RFC 2474 中所述，DSCP 允許從 0 到 63 TOS 欄位在 IPv4 封包內或 ipv6 Traffic Class 欄位內指定的值。 網路路由器使用 DSCP 值，來分類網路封包，並適當地排入佇列。
  
> [!NOTE]
>  根據預設，Windows 流量會有 DSCP 值為 0。
  
組織的 QoS 策略必須設計佇列數目及優先順序行為。 例如，您的組織可以選擇有五個佇列： 延遲敏感流量、 控制流量、 商務關鍵性流量、 最佳流量，以及大量資料傳輸流量。  
  
### <a name="throttling-traffic"></a>流量節流

DSCP 值，以及節流是另一個索引鍵的控制項，來管理網路頻寬。 如先前所述，您可以使用**指定節流閥速率**設定來設定輸出流量的特定的節流速率的 QoS 原則。 藉由使用節流，QoS 原則會限制指定的節流閥速率連出網路流量。 DSCP 標示和節流可以一起用來有效管理流量。

>[!NOTE]
>預設不會選取 [指定節流閥速率] 核取方塊。

若要建立 QoS 原則，請編輯的群組原則物件 (GPO) 從在 「 群組原則管理主控台 (GPMC) 工具中的設定。 接著，GPMC 會開啟 群組原則物件編輯器。

QoS 原則名稱必須是唯一的。 伺服器和終端使用者如何套用原則的 QoS 原則會儲存在群組原則物件編輯器 」 中的位置而定：

- 在 電腦設定 \windows 設定 \ Settings\QoS 原則的 QoS 原則套用到電腦，無論使用者目前登入。 通常對於伺服器電腦，會使用以電腦為依據的 QoS 原則。

- 使用者設定 \windows 設定 \ Settings\QoS 原則中的 QoS 原則套用到使用者，他們必須登入後，不論哪一部電腦使用者登入。

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>若要使用 QoS 原則精靈建立新的 QoS 原則

-   在 「 群組原則物件編輯器 」 中，將其中一種以滑鼠右鍵按一下**QoS 原則**節點，然後再按一下**建立新的原則**。

### <a name="wizard-page-1---policy-profile"></a>精靈頁面 1-原則設定檔

在 [QoS 原則精靈] 的第一個頁面上，您可以指定原則名稱，並設定 QoS 控制傳出網路流量的方式。

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>設定 [以原則為依據的 QoS 精靈] 的 [原則設定檔] 頁面

1. 在 [原則名稱] 中，輸入 QoS 原則的名稱。 名稱必須唯一識別該原則。

2. （選擇性） 使用**指定 DSCP 值**來啟用 DSCP 標示，然後設定 DSCP 值介於 0 和 63 之間。

3. 或者，使用 [指定節流閥速率] 來啟用流量節流，並設定節流閥速率。 節流閥速率值必須是大於 1，而且您可以指定單位的每秒 kb \(KBps\)或 每秒 mb \(MBps\)。

4. 按一下 [下一步] 。

### <a name="wizard-page-2---application-name"></a>精靈頁面 2-應用程式名稱

QoS 原則精靈的第二個頁面中您可以將原則套用至所有應用程式、 特定的應用程式所識別的應用程式名稱，其可執行檔的名稱、 路徑或處理特定的 URL 要求的 HTTP 伺服器應用程式。

- **所有應用程式**指定 QoS 原則精靈的第一頁上的流量管理設定套用到所有的應用程式。

- **僅此可執行檔名稱的應用程式**指定 QoS 原則精靈的第一頁上的流量管理設定為特定的應用程式。 可執行檔名稱必須以 .exe 副檔名結尾。

- **只有 HTTP 伺服器應用程式回應要求，此 url**指定 QoS 原則精靈的第一頁上的流量管理設定套用到特定 HTTP 伺服器應用程式。

您可以選擇輸入應用程式路徑。 若要指定應用程式路徑，請在路徑加入應用程式名稱。 路徑可以包含環境變數。 例如，%ProgramFiles%\My Application Path\MyApp.exe，或 c:\program files\my application path\myapp.exe。

>[!NOTE]
>應用程式路徑不能包含會解析成符號連結的路徑。

URL 必須符合[RFC 1738](https://tools.ietf.org/html/rfc1738)，以`http[s]://<hostname\>:<port\>/<url-path>`。 您可以使用萬用字元， `‘*’`，如`<hostname>`及/或`<port>`，例如`https://training.\*/, https://\*.\*`，但萬用字元無法表示的子字串`<hostname>`或`<port>`。

換句話說，兩者皆非`https://my\*site/`也不`https://\*training\*/`有效。 

（選擇性） 您可以檢查**包含子目錄和檔案**来執行比對所有子目錄和下列 URL 的檔案。 例如，如果勾選此選項，而 URL 是`https://training`，QoS 原則會考慮要求` https://training/video`完全相符。

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>若要設定 QoS 原則精靈 的 應用程式名稱 頁面

1. 在 **此 QoS 原則會套用至**，選取**所有應用程式**或**僅此可執行檔名稱的應用程式**。

2. 如果選取 [僅此可執行檔名稱的應用程式]，請指定以 .exe 附檔名結尾的可執行檔名稱。

3. 按一下 [下一步] 。

### <a name="wizard-page-3---ip-addresses"></a>精靈第 3 頁-IP 位址

QoS 原則精靈的第三個頁面中，您可以指定 IP 位址條件，為 QoS 原則，包括下列：

- 所有來源 IPv4 或 IPv6 位址，或是特定的來源 IPv4 或 IPv6 位址。

- 所有目的地 IPv4 或 IPv6 位址或特定目的地 IPv4 或 IPv6 位址

如果選取 [僅下列來源 IP 位址] 或 [僅下列目的 IP 位址]，就必須輸入下列其中一項：

- IPv4 位址例如 `192.168.1.1`

- 例如使用網路首碼長度表示法的 IPv4 位址首碼 `192.168.1.0/24`

- IPv6 位址例如 `3ffe:ffff::1`

- IPv6 位址前置詞例如 `3ffe:ffff::/48`

如果您同時選取**只會針對下列來源 IP 位址**並**只針對下列目的地 IP 位址**，位址或位址首碼必須是 IPv4 或 IPv6 為基礎。

如果您在前一個精靈頁面中指定的 HTTP 伺服器應用程式的 URL，您會發現，在此精靈頁面的 QoS 原則的來源 IP 位址會變成灰色。 

因為來源 IP 位址是 HTTP 伺服器位址，而且它是無法設定以下，也是如此。 相反地，您仍然可以在指定的目的地 IP 位址，自訂原則。 這項功能可讓您建立不同的用戶端的不同原則，使用相同的 HTTP 伺服器應用程式。

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>若要設定 QoS 原則精靈 的 IP 位址 頁面

1. 在 **此 QoS 原則會套用至**（來源） 中，選取**任何來源 IP 位址**或**只針對下列 IP 來源位址**。

2. 如果您選取**僅下列來源 IP 位址**，或指定的 IPv4 或 IPv6 位址前置詞。

3. 在 **此 QoS 原則會套用至**（目的地） 中，選取**任何目的地位址**或**只針對下列目的地 IP 位址。**

4. 如果您選取**只會針對下列目的地 IP 位址**，指定 IPv4 或 IPv6 位址的型別對應的前置詞或指定來源位址前置詞。

5.  按一下 [下一步] 。  

### <a name="wizard-page-4---protocols-and-ports"></a>精靈第 4 頁-通訊協定和連接埠

在 [QoS 原則精靈] 的第四個頁面上，您可以指定流量和精靈的第一頁上的設定所控制的連接埠的類型。 您可以指定：  
-   TCP 流量、UDP 流量或兩者  

-   所有來源連接埠、某個範圍的來源連接埠，或特定來源連接埠

-   所有目的地連接埠、 目的地連接埠，範圍或特定目的地連接埠  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>若要設定 QoS 原則精靈 的 通訊協定和連接埠 頁面

1. 在 [選取此 QoS 原則套用的通訊協定] 中，選取 [TCP]、[UDP] 或 [TCP 及 UDP]。

2. 在 [指定來源連接埠號碼] 中，選取 [來自任意來源的連接埠] 或 [來自此來源的連接埠號碼]。

3. 如果您選取**來自此來源連接埠號碼**，輸入介於 1 到 65535 之間的連接埠號碼。

     您可以選擇性地指定連接埠範圍，格式為"*低*:*高*，」 位置*低*並*高*代表下限和上限的連接埠範圍、 （含）。 *低*並*高*每個必須是介於 1 到 65535 之間的數字。 冒號 (:) 字元和數字之間不允許有空格。

4. 在 [指定目的地連接埠號碼] 中，選取 [到任何目的地連接埠] 或 [到此目的地連接埠號碼]。

5. 如果在上一個步驟中選取了 [到此目的地連接埠號碼]，請輸入介於 1 和 65535 之間的連接埠號碼。

若要完成建立新的 QoS 原則，按一下**完成**上**通訊協定和連接埠**QoS 原則精靈 頁面。 完成時，新的 QoS 原則會列在 詳細資料 窗格的 群組原則物件編輯器 」 中。  
  
若要將 QoS 原則設定套用至使用者或電腦，連結的 GPO 中的 QoS 原則位於一個 Active Directory 網域服務容器，例如網域、 站台或組織單位 (OU)。  
  
##  <a name="bkmk_editpolicy"></a>檢視、 編輯或刪除 QoS 原則

QoS 原則精靈 所述的頁面之前會對應到當您檢視或編輯原則的內容時所顯示的 屬性 頁面。  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>檢視 QoS 原則的內容  
  
-   以滑鼠右鍵按一下 詳細資料 窗格的 群組原則物件編輯器 」 中中的原則名稱，然後按一下**屬性**。  
  
     群組原則物件編輯器會顯示下列索引標籤的 [屬性] 頁面：  
  
    -   原則設定檔  
  
    -   應用程式名稱  
  
    -   IP 位址  
  
    -   通訊協定及連接埠  
  
### <a name="to-edit-a-qos-policy"></a>編輯 QoS 原則  
  
-   以滑鼠右鍵按一下 詳細資料 窗格的 群組原則物件編輯器 」 中中的原則名稱，然後按一下**編輯現有的原則**。  
  
     群組原則物件編輯器 會顯示**編輯現有的 QoS 原則** 對話方塊。  
  
### <a name="to-delete-a-qos-policy"></a>刪除 QoS 原則  
  
-   以滑鼠右鍵按一下 詳細資料 窗格的 群組原則物件編輯器 」 中中的原則名稱，然後按一下**刪除原則**。  
  
### <a name="qos-policy-gpmc-reporting"></a>QoS 原則 GPMC 報告 

整個組織套用 QoS 原則數目之後，它可能是有效或才需要定期檢閱如何套用原則。 使用 GPMC 報告，可以檢視特定使用者或電腦 QoS 原則的摘要。  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>若要執行群組原則結果精靈，為 QoS 原則的報表  
  
-   在 GPMC 中，以滑鼠右鍵按一下**群組原則結果**節點，然後再選取功能表選項**群組原則結果精靈。**  
  
會產生群組原則結果之後，請按一下**設定** 索引標籤。在 **設定**索引標籤上，可以 「 電腦設定 \windows 設定 \ Settings\QoS 原則 」 和 「 使用者設定 \windows 設定 \ Settings\QoS 原則 節點下找到的 QoS 原則。  
  
在 [**設定**] 索引標籤，QoS 原則會依其 DSCP 值、 節流閥速率、 原則條件、 其 QoS 原則名稱列出並贏得 GPO 列在相同的資料列...  
  
[群組原則結果] 檢視中可唯一識別優勢 GPO。 當多個 Gpo 都有相同的 QoS 原則名稱的 QoS 原則時，則會套用具有最高的 GPO 優先順序的 GPO。 這是優勢 GPO。 衝突的資料會附加至較低的優先順序不會套用 GPO QoS 原則 （原則名稱所識別）。 請注意 GPO 優先順序決定權衡的 QoS 原則會部署在站台、 網域或 OU，適當地。 在部署後，在使用者或電腦層級中， [QoS 原則優先順序規則](#BKMK_precedencerules)決定允許和封鎖的流量。  
  
QoS 原則的 DSCP 值、 節流閥速率和原則條件也會顯示在 群組原則物件編輯器 (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>漫遊和遠端使用者的進階的設定  
QoS 原則，目標是要管理的企業網路上的流量。 在行動裝置的情況下，使用者可能會傳送給流量開啟或關閉企業網路。 因為不相關而遠離企業網路 QoS 原則，只在 Windows 8、 Windows 7 或 Windows Vista 連接到總公司的網路介面啟用 QoS 原則。  
  
例如，使用者可能她可攜式電腦連線到她的企業網路，透過虛擬私人網路 (VPN) 從咖啡館。 Vpn，（例如無線） 的實體網路介面不會有套用 QoS 原則。 不過，VPN 介面必須套用，因為它會連接到企業的 QoS 原則。 如果使用者稍後輸入沒有 AD DS 信任關係的其他企業網路，將不會啟用 QoS 原則。  
  
請注意，這些行動裝置的情況下不會套用至伺服器工作負載。 例如，具有多張網路介面卡的伺服器可能位於企業網路的邊緣。 IT 部門可能會選擇將 egresses 企業; 的 QoS 原則節流流量不過，會將此輸出流量傳送此網路介面卡不會不一定是連回企業網路。 基於這個理由，一定會在執行 Windows Server 2012 的電腦的所有網路介面上啟用 QoS 原則。  
  
> [!NOTE]
>  QoS 原則，而不在本文件接下來討論進階 QoS 設定，僅適用於選擇性啟用。  
  
### <a name="advanced-qos-settings"></a>進階的 QoS 設定

進階的 QoS 設定提供讓 IT 系統管理員管理的電腦網路耗用量和 DSCP 標示其他控制項。 只能在電腦層級的進階的 QoS 設定套用，而可以套用 QoS 原則，電腦和使用者層級。

#### <a name="to-configure-advanced-qos-settings"></a>若要設定進階 QoS 設定

1.  按一下 **電腦組態**，然後按一下**群組原則中的 Windows 設定**。
  
2.  以滑鼠右鍵按一下**QoS 原則**，然後按一下**進階 QoS 設定**。

     下圖顯示兩個進階 QoS 設定 索引標籤：**輸入 TCP 流量**並**DSCP 標示覆寫**。
  
> [!NOTE]
>  進階的 QoS 設定是電腦層級群組原則設定。
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>進階 QoS 設定： 輸入 TCP 流量

**輸入 TCP 流量**控制在接收者端，將 TCP 頻寬耗用量，而 QoS 原則會影響輸出的 TCP 和 UDP 流量。 

層級上設定較低的輸送量**連入 TCP 流量**TCP 會限制索引標籤上，大小其通告的 TCP 接收視窗。 這項設定的效果會更高的輸送量速率並連結使用率，以更高的頻寬或延遲 （頻寬延遲乘積） TCP 連線。 根據預設，執行 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的電腦是設定最大輸送量層級。
  
TCP 接收視窗已從舊版 Windows 的 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 中變更。 舊版 Windows 的有限 TCP 接收端窗，上限為 64 kb，而 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 動態接收端調整視窗大小達 16 mb (MB). 在連入 TCP 流量控制項中，您可以控制的輸入的輸送量層級設定 TCP 接收視窗所能成長的最大值。 等級會對應至下列的最大值。 
  
|輸入的輸送量層級|最大值|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

實際的視窗大小可能的值等於或小於最大值，根據網路狀況。

###### <a name="to-set-the-tcp-receive-side-window"></a>若要設定 TCP 接收端窗口

1. 在 「 群組原則物件編輯器 」 中，按一下**本機電腦原則**，按一下**Windows 設定**，以滑鼠右鍵按一下**QoS 原則**，然後按一下 **進階 QoS設定**。
  
2. 在  **TCP 接收輸送量**，選取**設定 TCP 接收輸送量**，然後選取您想要的輸送量層級。

3.  將 GPO 連結到 OU。

#### <a name="advanced-qos-settings-dscp-marking-override"></a>進階的 QoS 設定：DSCP 標示覆寫

DSCP 標示覆寫會限制指定的應用程式的能力 — 或 「 標記 」 — DSCP QoS 原則中所指定以外的值。 藉由指定應用程式可以設定 DSCP 值，應用程式可以設定非零的 DSCP 值。 

藉由指定**忽略**，使用 QoS Api 應用程式必須設為零，其 DSCP 值，並只 QoS 原則可以設定 DSCP 值。 

根據預設，執行 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的電腦可讓應用程式指定 DSCP 值;應用程式和不使用 QoS Api 的裝置不會覆寫。

##### <a name="wireless-multimedia-and-dscp-values"></a>無線多媒體與 DSCP 值

[Wi-fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769)已建立憑證的無線多媒體\(WMM\)定義四個存取類別\(WMM_AC\)優先排序網路流量Wi-fi 上傳輸\-Wi-fi 無線網路。 存取類別，包括\(中的最高為最低優先順序\)： 語音、 視訊、 最佳投入量及背景，分別為 VO、 VI、 BE 和 BK 縮寫。 WMM 規格會定義的 DSCP 值對應至每個四種存取類別：
  
|DSCP 值|WMM 存取類別目錄|
|----------|-------------------|
|48-63|語音 (VO)|
|32-47|影片 (VI)|
|24-31, 0-7|最大努力 (BE)|
|8-23|背景 (BK)|

您可以建立 QoS 原則使用這些 DSCP 值，以確保該可攜式電腦 Wi\-Fi Certified™ WMM 無線介面卡接收優先處理 Wi-fi 與相關聯時\-WMM 認證 Wi-fi 存取點。
  
### <a name="BKMK_precedencerules"></a>QoS 原則優先順序規則

類似於 GPO 的優先順序，QoS 原則有優先順序規則來解決衝突，當多個 QoS 原則套用至一組特定的流量。 輸出 TCP 或 UDP 流量，只能有一個 QoS 原則可以套用一次，這表示，QoS 原則不會有累加的效果，例如會加總節流閥速率。

一般情況下，贏得 QoS 原則，以處理最相符的情況。 當多個 QoS 原則套用時，規則分為三種類別： 使用者層級與電腦層級;應用程式與網路五重物件。並在網路間 quintuple。

藉由*網路 5 倍*，來源 IP 位址、 目的地 IP 位址、 來源連接埠、 目的地連接埠和通訊協定是指\(TCP/UDP\)。  

 **使用者層級 QoS 原則將優先於電腦層級的 QoS 原則**

此規則會大幅簡化網路系統管理員的管理 QoS Gpo，特別是針對使用者群組為基礎的原則。 例如，如果網路系統管理員想要定義使用者群組的 QoS 原則，可以只建立並發佈至該群組的 GPO。 它們不需要擔心這些使用者登入有關哪些電腦，是否這些電腦會有衝突的 QoS 原則定義，因為使用者層級原則有衝突時，一律會優先。

> [!NOTE]
>  使用者層級的 QoS 原則只適用於該使用者所產生的流量。 其他使用者的特定電腦，與電腦本身，不會針對該使用者所定義的任何 QoS 原則。

 **應用程式明確性和採用的優先順序，高於網路五重物件**

當多個 QoS 原則相符的特定流量時，會套用更具體的原則。 識別應用程式的原則，在包含傳送應用程式的檔案路徑的原則會被視為比另一個原則只會識別應用程式名稱 （無路徑），更明確。 如果應用程式的多個原則仍然適用，優先順序規則會使用 5 倍的網路來尋找最符合項目。

或者，多個 QoS 原則可以套用至相同的流量藉由指定非重疊的條件。 應用程式的條件，網路五重物件之間指定的應用程式的原則會被視為更明確，而且會套用。 

例如，policy_A 只會指定應用程式名稱 (app.exe)，而 policy_B 指定目的地 IP 位址 192.168.1.0/24。 當這些 QoS 原則衝突\(app.exe 將流量傳送至 IP 位址範圍內的 192.168.4.0/24\)，policy_A 會套用。

 **更多的精確性，會在網路中的優先 5 倍**

對於網路五重物件內的原則衝突，會優先使用最符合的條件的原則。 例如，假設 policy_C 指定來源 IP 位址 [任何]、 目的地 IP 位址 10.0.0.1、 來源連接埠 「 任何 」、 目的地連接埠 「 任何 」，並將通訊協定"TCP"。 

接下來，假設 policy_D 指定來源 IP 位址 「 任何 」、 目的地 IP 位址 10.0.0.1、 來源連接埠 [任何]、 目的地連接埠 80，並將通訊協定 「 TCP 」。 然後 policy_C 和 policy_D 都符合目的地 10.0.0.1: 80 的連線。 QoS 原則會套用具有最特定的比對條件的原則，因為 policy_D 會優先使用在此範例中。  
  
不過，QoS 原則可能會有相同數目的條件。 例如，數個原則可能每個指定只有一個 （但不同） 的網路五重物件。 在網路五重物件，以下列順序是從高到低優先順序：

- 來源 IP 位址

- 目的地 IP 位址

- 來源連接埠

- 目的地連接埠

- 通訊協定 (TCP 或 UDP)。

更明確的 IP 位址視為具有較高的優先順序; 內特定的條件，例如 IP 位址例如，IP 位址 192.168.4.1 是比 192.168.4.0/24 更明確。

設計您的 QoS 原則盡量明確地以簡化您的組織能夠了解哪些原則會生效。

如本指南中的下一個主題，請參閱 < [QoS 原則事件和錯誤](qos-policy-errors.md)。

如本指南中的第一個主題，請參閱 <<c0> [ 服務品質 (QoS) 原則](qos-policy-top.md)。
