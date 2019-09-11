---
title: 管理 QoS 原則
description: 本主題提供有關如何在 Windows Server 2016 中建立和管理服務品質（QoS）原則的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3cff51b3cf76d3224832bf99ff966bf473d6ff6c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871855"
---
# <a name="manage-qos-policy"></a>管理 QoS 原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用「QoS 原則」 wizard 來建立、編輯或刪除 QoS 原則。

>[!NOTE]
>  除了本主題之外，還提供下列 QoS 原則管理檔。
> 
>  - [QoS 原則事件和錯誤](qos-policy-errors.md)

在 Windows 作業系統中，QoS 原則結合了以標準為依據的 QoS 功能與群組原則的管理能力。 此組合的設定可讓您輕鬆地將 QoS 原則應用於群組原則物件。 Windows 包含 QoS 原則 Wizard，可協助您執行下列工作。

-  [建立 QoS 原則](#bkmk_createpolicy)

-  [查看、編輯或刪除 QoS 原則](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>建立 QoS 原則

建立 QoS 原則之前，請務必瞭解用來管理網路流量的兩個重要 QoS 控制項：

- DSCP 值

-   節流速率

### <a name="prioritizing-traffic-with-dscp"></a>使用 DSCP 將流量排序

如先前的企業營運應用程式範例所述，您可以使用 [**指定 Dscp 值**] 來設定具有特定 dscp 值的 QoS 原則，以定義輸出網路流量的優先順序。 

如 RFC 2474 中所述，DSCP 允許在 IPv4 封包的 TOS 欄位，以及 IPv6 的 [流量] 類別欄位中，指定0到63之間的值。 網路路由器會使用 DSCP 值來分類網路封包，並適當地將它們排入佇列。
  
> [!NOTE]
>  根據預設，Windows 流量的 DSCP 值為0。
  
組織的 QoS 策略必須設計佇列數目及優先順序行為。 例如，您的組織可能會選擇擁有五個佇列：延遲敏感的流量、控制流量、業務關鍵流量、最佳流量，以及大量資料傳輸流量。  
  
### <a name="throttling-traffic"></a>流量節流

除了 DSCP 值，節流是管理網路頻寬的另一個主要控制項。 如先前所述，您可以使用 [**指定節流閥速率**] 設定，針對輸出流量使用特定的節流速率來設定 QoS 原則。 藉由使用節流，QoS 原則會將傳出的網路流量限制為指定的節流速率。 DSCP 標示和節流可以一起用來有效管理流量。

>[!NOTE]
>預設不會選取 [指定節流閥速率] 核取方塊。

若要建立 QoS 原則，請在群組原則管理主控台（GPMC）工具內編輯群組原則物件（GPO）的設定。 GPMC 接著會開啟群組原則物件編輯器。

QoS 原則名稱必須是唯一的。 原則套用至伺服器和使用者的方式，取決於 QoS 原則儲存在群組原則物件編輯器中的位置：

- 無論目前登入的使用者為何，電腦的 [配置] Settings\QoS 原則中的 QoS 原則都會套用至電腦。 通常對於伺服器電腦，會使用以電腦為依據的 QoS 原則。

- 使用者配置系統 Settings\QoS 原則中的 QoS 原則會在使用者登入之後套用至他們，不論他們登入的是哪一部電腦。

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>使用 QoS 原則嚮導建立新的 QoS 原則

-   在群組原則物件編輯器中，以滑鼠右鍵按一下其中一個**QoS 原則**節點，然後按一下 [**建立新原則**]。

### <a name="wizard-page-1---policy-profile"></a>Wizard 第1頁-原則設定檔

在 [QoS 原則] [wizard] 的第一頁上，您可以指定原則名稱，並設定 QoS 控制傳出網路流量的方式。

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>設定 [以原則為依據的 QoS 精靈] 的 [原則設定檔] 頁面

1. 在 [原則名稱] 中，輸入 QoS 原則的名稱。 此名稱必須唯一識別該原則。

2. （選擇性）使用 [**指定 Dscp 值**] 來啟用 dscp 標示，然後設定介於0到63之間的 dscp 值。

3. 或者，使用 [指定節流閥速率] 來啟用流量節流，並設定節流閥速率。 節流閥速率值必須大於1，而且您可以\(指定每秒 kb 的單位（kbps\) ）或每秒\(mb 的\)MBps。

4. 按一下 [下一步]。

### <a name="wizard-page-2---application-name"></a>Wizard 第2頁-應用程式名稱

在 [QoS 原則] 嚮導的第二個頁面中，您可以將原則套用至 [所有應用程式]、依其可執行檔名稱識別的特定應用程式、路徑和應用程式名稱，或用於處理特定 URL 之要求的 HTTP 伺服器應用程式。

- [**所有應用程式**] 指定 [QoS 原則] 的第一頁上的流量管理設定適用于所有應用程式。

- **只有具有此可執行檔名稱的應用程式**會指定 [QoS 原則] 的第一頁上的流量管理設定適用于特定應用程式。 可執行檔名稱必須以 .exe 副檔名結尾。

- **只有回應此 URL 之要求的 HTTP 伺服器應用程式**會指定 [QoS 原則] 的第一頁上的流量管理設定僅適用于特定的 HTTP 伺服器應用程式。

您可以選擇輸入應用程式路徑。 若要指定應用程式路徑，請在路徑加入應用程式名稱。 路徑可以包含環境變數。 例如，%ProgramFiles%\My Application Path\MyApp.exe，或 c:\program files\my application path\myapp.exe。

>[!NOTE]
>應用程式路徑不能包含解析成符號連結的路徑。

URL 必須符合[RFC 1738](https://tools.ietf.org/html/rfc1738)，格式`http[s]://<hostname\>:<port\>/<url-path>`為。 `‘*'`您可以針對`<hostname>` `<hostname>`和`<port>`/或使用萬用字元，例如`<port>`，但萬用字元不能代表或的子字串。 `https://training.\*/, https://\*.\*`

換句話說， `https://my\*site/`和`https://\*training\*/`都不是有效的。 

（選擇性）您可以勾選 [**包含子目錄和**檔案]，以在 URL 後面的所有子目錄和檔案上執行比對。 例如，如果核取此選項且 URL 為`https://training`，QoS 原則將會考慮要求是否` https://training/video`符合良好。

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>設定 QoS 原則嚮導的 [應用程式名稱] 頁面

1. 在 **此 QoS 原則適用**于 中，選取 **所有應用程式** 或 **僅限此可執行檔名稱的應用程式**

2. 如果選取 [僅此可執行檔名稱的應用程式]，請指定以 .exe 附檔名結尾的可執行檔名稱。

3. 按一下 [下一步]。

### <a name="wizard-page-3---ip-addresses"></a>Wizard 第3頁-IP 位址

在 [QoS 原則嚮導] 的第三個頁面中，您可以指定 QoS 原則的 IP 位址條件，包括下列各項：

- 所有來源 IPv4 或 IPv6 位址，或是特定的來源 IPv4 或 IPv6 位址。

- 所有目的地 IPv4 或 IPv6 位址，或特定目的地 IPv4 或 IPv6 位址

如果選取 [僅下列來源 IP 位址] 或 [僅下列目的 IP 位址]，就必須輸入下列其中一項：

- IPv4 位址，例如`192.168.1.1`

- 使用網路前置長度標記法的 IPv4 位址首碼，例如`192.168.1.0/24`

- IPv6 位址，例如`3ffe:ffff::1`

- IPv6 位址首碼，例如`3ffe:ffff::/48`

如果您同時選取 **[僅下列來源 ip 位址**] 和 **[僅下列目的地 ip**位址]，則位址或位址首碼必須是 IPv4 或 IPv6 型。

如果您在上一個嚮導頁面中指定 HTTP 伺服器應用程式的 URL，您會注意到此 wizard 頁面上 QoS 原則的來源 IP 位址呈現灰色。 

這是因為來源 IP 位址是 HTTP 伺服器位址，而且無法在此設定。 另一方面，您仍然可以藉由指定目的地 IP 位址來自訂原則。 這可讓您使用相同的 HTTP 伺服器應用程式，為不同的用戶端建立不同的原則。

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>設定 QoS 原則嚮導的 [IP 位址] 頁面

1. 在 [**此 QoS 原則會套用至**] （來源）中，選取 [**任何來源 ip 位址**] 或 **[僅下列 ip 來源位址**]。

2. 如果您選取**了 [僅下列 IP 來源位址**]，請指定 IPv4 或 IPv6 位址或首碼。

3. 在 [**此 QoS 原則會套用至**] （目的地）中，選取 [**任何目的地位址**] 或 **[僅下列 IP 目的地位址]。**

4. 如果您選取 **[僅下列 IP 目的地位址**]，請指定 IPv4 或 IPv6 位址或首碼，其對應至為來源位址指定的位址或首碼類型。

5.  按一下 [下一步]。  

### <a name="wizard-page-4---protocols-and-ports"></a>Wizard 第4頁-通訊協定和埠

在 [QoS 原則嚮導] 的第四個頁面上，您可以指定流量的類型，以及由嚮導第一頁上的設定所控制的埠。 您可以指定：  
-   TCP 流量、UDP 流量或兩者  

-   所有來源連接埠、某個範圍的來源連接埠，或特定來源連接埠

-   所有目的地埠、目的地埠範圍或特定目的地埠  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>設定 QoS 原則嚮導的 [通訊協定和埠] 頁面

1. 在 [選取此 QoS 原則套用的通訊協定] 中，選取 [TCP]、[UDP] 或 [TCP 及 UDP]。

2. 在 [指定來源連接埠號碼] 中，選取 [來自任意來源的連接埠] 或 [來自此來源的連接埠號碼]。

3. 如果您選取 [**從此來源埠號碼**]，請輸入介於1到65535之間的埠號碼。

     （選擇性）您可以使用「*低*：*高*」格式指定埠範圍，其中「*低*」和「*高*」代表埠範圍的下限和上限（含）。 *Low*和*High*分別必須是介於1到65535之間的數位。 冒號 (:) 字元和數字之間不允許有空格。

4. 在 [指定目的地連接埠號碼] 中，選取 [到任何目的地連接埠] 或 [到此目的地連接埠號碼]。

5. 如果在上一個步驟中選取了 [到此目的地連接埠號碼]，請輸入介於 1 和 65535 之間的連接埠號碼。

若要完成建立新的 QoS 原則，請在 [QoS 原則] wizard 的 [**通訊協定和埠**] 頁面上按一下 [**完成]** 。 完成時，新的 QoS 原則會列在群組原則物件編輯器的詳細資料窗格中。  
  
若要將 QoS 原則設定套用到使用者或電腦，請將 QoS 原則所在的 GPO 連結到 Active Directory Domain Services 容器，例如網域、網站或組織單位（OU）。  
  
##  <a name="bkmk_editpolicy"></a>查看、編輯或刪除 QoS 原則

先前所述的 QoS 原則 wizard 頁面會對應至您在查看或編輯原則內容時所顯示的屬性頁面。  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>檢視 QoS 原則的內容  
  
-   在群組原則物件編輯器的 [詳細資料] 窗格中，以滑鼠右鍵按一下原則名稱，然後按一下 [**屬性**]。  
  
     群組原則物件編輯器會顯示具有下列索引標籤的 [屬性] 頁面：  
  
    -   原則設定檔  
  
    -   應用程式名稱  
  
    -   IP 位址  
  
    -   通訊協定及連接埠  
  
### <a name="to-edit-a-qos-policy"></a>編輯 QoS 原則  
  
-   在群組原則物件編輯器的詳細資料窗格中，以滑鼠右鍵按一下原則名稱，然後按一下 [**編輯現有的原則**]。  
  
     群組原則物件編輯器會顯示 [**編輯現有的 QoS 原則**] 對話方塊。  
  
### <a name="to-delete-a-qos-policy"></a>刪除 QoS 原則  
  
-   在群組原則物件編輯器的詳細資料窗格中，以滑鼠右鍵按一下原則名稱，然後按一下 [**刪除原則**]。  
  
### <a name="qos-policy-gpmc-reporting"></a>QoS 原則 GPMC 報告 

在您的組織中套用了許多 QoS 原則之後，定期審查原則的套用方式可能會很有用或必要。 您可以使用 GPMC 報告來查看特定使用者或電腦的 QoS 原則摘要。  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>針對 QoS 原則的報告執行 [群組原則結果] Wizard  
  
-   在 GPMC 中，以滑鼠右鍵按一下 [**群組原則結果**] 節點，然後選取 [**群組原則結果] 嚮導**的 [功能表] 選項。  
  
產生群組原則結果之後，請按一下 [**設定**] 索引標籤。在 [**設定**] 索引標籤上，您可以在 [電腦配置設定 Settings\QoS 原則] 和 [使用者設定設定 Settings\QoS 原則] 節點底下找到 QoS 原則。  
  
在 [**設定**] 索引標籤上，qos 原則會依其 [其 DSCP 值]、[節流速率]、[原則條件] 和 [入選的 GPO] 列在相同的資料列中，列出其 qos 原則名稱。  
  
[群組原則結果] 視圖會唯一識別獲勝的 GPO。 當多個 Gpo 具有具有相同 QoS 原則名稱的 QoS 原則時，會套用具有最高 GPO 優先順序的 GPO。 這是獲勝的 GPO。 不會套用附加至較低優先順序 GPO 的衝突 QoS 原則（依原則名稱識別）。 請注意，GPO 優先順序會定義哪些 QoS 原則會適當地部署在網站、網域或 OU 中。 部署之後，在使用者或電腦層級上， [QoS 原則優先順序規則](#BKMK_precedencerules)會決定允許和封鎖的流量。  
  
QoS 原則的 DSCP 值、節流速率和原則條件也會顯示在群組原則物件編輯器（GPOE）中  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>漫遊和遠端使用者的 Advanced 設定  
透過 QoS 原則，其目標是要管理商業網路上的流量。 在行動裝置案例中，使用者可能會在商業網路上傳送或傳出流量。 由於 QoS 原則與商業網路不相關，因此，只有在連線到 Windows 8、Windows 7 或 Windows Vista 企業版的網路介面上，才會啟用 QoS 原則。  
  
例如，使用者可能會透過虛擬私人網路（VPN），將可攜式電腦連接到其商業網路。 若為 VPN，實體網路介面（例如無線）將不會套用 QoS 原則。 不過，VPN 介面將會套用 QoS 原則，因為它會連接到企業。 如果使用者稍後進入另一個沒有 AD DS 信任關係的商業網路，將不會啟用 QoS 原則。  
  
請注意，這些行動案例不適用伺服器工作負載。 例如，具有多個網路介面卡的伺服器可能位於商業網路的邊緣。 IT 部門可能會選擇讓 QoS 原則節流 egresses 企業的流量;不過，此網路介面卡會傳送此輸出流量，不一定會連回商業網路。 基於這個理由，在執行 Windows Server 2012 之電腦的所有網路介面上，一律會啟用 QoS 原則。  
  
> [!NOTE]
>  選擇性啟用僅適用于 QoS 原則，而不適用於本檔中下一節所討論的 Advanced QoS 設定。  
  
### <a name="advanced-qos-settings"></a>Advanced QoS 設定

Advanced QoS 設定可提供額外的控制，讓 IT 系統管理員管理電腦網路耗用量和 DSCP 標記。 Advanced QoS 設定只適用于電腦層級，而 QoS 原則則可以同時套用在電腦和使用者層級。

#### <a name="to-configure-advanced-qos-settings"></a>若要設定 advanced QoS 設定

1.  按一下 [**電腦**設定]，然後按一下**群組原則中的 [Windows 設定**]。
  
2.  在 [ **Qos 原則**] 上按一下滑鼠右鍵，然後按一下 [ **Advanced qos Settings**]。

     下圖顯示兩個 [advanced QoS 設定] 索引標籤：**輸入 TCP 流量**和**DSCP 標記覆寫**。
  
> [!NOTE]
>  Advanced QoS 設定是電腦層級的群組原則設定。
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Advanced QoS 設定：輸入 TCP 流量

**輸入 Tcp 流量**控制接收者端的 TCP 頻寬耗用量，而 QoS 原則則會影響輸出 TCP 和 UDP 流量。 

藉由在 [**輸入 TCP 流量**] 索引標籤上設定較低的輸送量層級，TCP 將會限制其通告的 tcp 接收視窗大小。 這項設定的作用會增加輸送量速率，以及具有較高頻寬或延遲（頻寬延遲產品）之 TCP 連線的連結使用率。 根據預設，執行 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的電腦會設定為最大輸送量層級。
  
Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 中的 TCP 接收視窗已從舊版 Windows 變更。 舊版的 Windows 限制 TCP 接收端視窗的上限為 64 kb，而 Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 會動態調整接收端視窗的大小，最高可達 16 mb （MB）). 在 [輸入 TCP 流量控制] 中，您可以設定 TCP 接收視窗可成長的最大值，以控制輸入輸送量層級。 層級會對應至下列最大值。 
  
|輸入輸送量層級|最大值|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

視網路條件而定，實際的視窗大小可能是等於或小於上限的值。

###### <a name="to-set-the-tcp-receive-side-window"></a>若要設定 TCP 接收端視窗

1. 在群組原則物件編輯器中，依序按一下 [**本機電腦原則**]、[ **Windows 設定**]，以滑鼠右鍵按一下 [ **qos 原則**]，然後按一下 [ **Advanced qos Settings**]。
  
2. 在 [ **TCP 接收輸送量**] 中，選取 [**設定 tcp 接收輸送量**]，然後選取您想要的輸送量層級。

3.  將 GPO 連結至 OU。

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Advanced QoS 設定：DSCP 標記覆寫

DSCP 標記覆寫會限制應用程式指定的能力（或「標記」），而不是在 QoS 原則中指定的 DSCP 值。 藉由指定允許應用程式設定 DSCP 值，應用程式可以設定非零的 DSCP 值。 

藉由指定 [**忽略**]，使用 qos api 的應用程式會將其 DSCP 值設定為零，而且只有 QoS 原則可以設定 dscp 值。 

根據預設，執行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的電腦，可以讓應用程式指定 DSCP 值;不會覆寫未使用 QoS Api 的應用程式和裝置。

##### <a name="wireless-multimedia-and-dscp-values"></a>無線多媒體和 DSCP 值

[Wi-fi 聯盟](https://go.microsoft.com/fwlink/?LinkId=160769)已\(建立無線多媒體 WMM\)的認證，其定義了四個存取類別\(WMM_AC\) ，以排定在 wi-fi\-上傳輸的網路流量優先順序Wi-fi 無線網路。 存取類別包含\(順序最高到最低的優先順序\)：語音、影片、最佳方式和背景; 分別縮寫為 VO、VI、as 和 BK。 WMM 規格會定義哪些 DSCP 值會對應到四個存取類別中的每一個：
  
|DSCP 值|WMM 存取類別|
|----------|-------------------|
|48-63|語音（VO）|
|32-47|影片（VI）|
|24-31、0-7|最佳做法（是）|
|8-23|背景（BK）|

您可以建立使用這些 DSCP 值的 QoS 原則，以確保在 wmm 無線適配\-卡上，具有 wi-fi 認證™的可攜式電腦會在與適用\-于 wmm 存取點的 wi-fi 認證相關聯時，收到優先順序的處理。
  
### <a name="BKMK_precedencerules"></a>QoS 原則優先順序規則

與 GPO 的優先順序類似，QoS 原則具有優先順序規則，可在多個 QoS 原則套用至一組特定的流量時解決衝突。 針對輸出 TCP 或 UDP 流量，一次只能套用一個 QoS 原則，這表示 QoS 原則沒有累積的效果，例如節流速率會加總。

一般而言，具有最符合條件的 QoS 原則會獲勝。 套用多個 QoS 原則時，規則會分成三個類別：使用者層級與電腦層級;應用程式與網路 quintuple;而在網路 quintuple 中。

藉由*網路 quintuple*，我們表示來源 ip 位址、目的地 ip 位址、來源埠、目的地埠，以及通訊\(協定 TCP/\)UDP。  

 **使用者層級的 QoS 原則優先于電腦層級的 QoS 原則**

此規則會大幅簡化網路系統管理員的 QoS Gpo 管理，特別是針對以使用者群組為基礎的原則。 例如，如果網路系統管理員想要定義使用者群組的 QoS 原則，他們可以只建立 GPO 並將其發佈至該群組。 他們不需要擔心這些使用者登入的電腦，以及這些電腦是否會定義衝突的 QoS 原則，因為如果有衝突存在，使用者層級原則一律會優先使用。

> [!NOTE]
>  使用者層級的 QoS 原則僅適用于該使用者所產生的流量。 特定電腦和電腦本身的其他使用者將不會受到針對該使用者定義的任何 QoS 原則。

 **應用程式的明確性並優先于網路 quintuple**

當有多個 QoS 原則符合特定流量時，就會套用更具體的原則。 在識別應用程式的原則中，包含傳送應用程式檔案路徑的原則會被視為比另一個只識別應用程式名稱（沒有路徑）的原則更明確。 如果有多個應用程式的原則仍然適用，優先順序規則會使用網路 quintuple 來尋找最符合的項。

或者，您也可以藉由指定非重迭的狀況，將多個 QoS 原則套用至相同的流量。 在應用程式的條件和網路 quintuple 之間，指定應用程式的原則會被視為更明確且會套用。 

例如，policy_A 只會指定應用程式名稱（app.config），而 policy_B 會指定目的地 IP 位址 192.168.1.0/24。 當這些 QoS 原則衝突\(時，app-v 會將流量傳送到 192.168.4.0/24\)範圍內的 IP 位址，並套用 policy_A。

 **在網路 quintuple 中的優先順序更高**

針對網路 quintuple 內的原則衝突，具有最符合條件的原則會優先使用。 例如，假設 policy_C 指定來源 IP 位址 "any"、目的地 IP 位址10.0.0.1、來源埠 "any"、目的地埠 "any" 和通訊協定 "TCP"。 

接下來，假設 policy_D 指定來源 IP 位址 "any"、目的地 IP 位址10.0.0.1、來源埠 "any"、目的地埠80和通訊協定 "TCP"。 然後，policy_C 和 policy_D 都符合目的地10.0.0.1：80的連接。 由於 QoS 原則會套用具有最符合特定比對條件的原則，因此在此範例中，policy_D 的優先順序較高。  
  
不過，QoS 原則可能會有相同數目的條件。 例如，數個原則可能只會指定網路 quintuple 的一個（但不是相同的）部分。 在網路 quintuple 中，下列順序是從較高到低的優先順序：

- 來源 IP 位址

- 目的地 IP 位址

- 來源埠

- 目的地連接埠

- 通訊協定 (TCP 或 UDP)。

在特定情況下（例如 IP 位址），較特定的 IP 位址會以較高的優先順序處理;例如，IP 位址192.168.4.1 比 192.168.4.0/24 更具體。

盡可能地設計您的 QoS 原則，以簡化組織能夠瞭解哪些原則已生效。

如需本指南中的下一個主題，請參閱[QoS 原則事件和錯誤](qos-policy-errors.md)。

如需本指南的第一個主題，請參閱[服務品質（QoS）原則](qos-policy-top.md)。
