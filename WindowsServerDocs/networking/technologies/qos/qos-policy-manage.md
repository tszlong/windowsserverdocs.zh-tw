---
title: 管理 QoS 原則
description: 本主題提供如何建立及管理品質服務 (QoS) 原則，在 Windows Server 2016 上的指示。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5baa83e41c79a558f5bd32f77a234367726592c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-qos-policy"></a>管理 QoS 原則

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何使用 QoS 原則精靈來建立、編輯，或移除 QoS 原則。

>[!NOTE]
>  本主題中，除了下列 QoS 原則管理文件會提供。
> 
>  - [事件 QoS 原則與錯誤](qos-policy-errors.md)

Windows 作業系統，在 QoS 原則，請使用群組原則管理性結合標準型 QoS 的功能。 輕鬆 QoS 原則應用程式可設定的這個組合群組原則物件。 Windows 包含 QoS 原則精靈可協助您進行下列工作。

-  [建立 QoS 原則](#bkmk_createpolicy)

-  [檢視、編輯，或移除 QoS 原則](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>建立 QoS 原則

建立 QoS 原則之前，請務必您了解可用來管理網路流量的兩個按鍵 QoS 控制項：

- DSCP 值。

-   調節率

### <a name="prioritizing-traffic-with-dscp"></a>設定資料傳輸與 DSCP 優先順序

在前一個範例業務的應用程式中所述，您可以使用定義輸出網路流量的優先順序**指定 DSCP 值**來設定 QoS 原則的特定 DSCP 值。 

RFC 2474 所述，DSCP 可 0 63 IPv4 封包 TO 欄位中和 IPv6 中流量課程欄位中指定的值。 網路路由器使用 DSCP 值分類網路封包並適當地佇列它們。
  
> [!NOTE]
>  根據預設，Windows 流量有 DSCP 設定為 0。
  
佇列和優先順序行為的數目需要為您的組織 QoS 策略的一部分。 例如，您的組織可能選擇五個佇列：延遲顧慮流量、控制流量、關鍵性流量、最佳流量，以及大量資料傳輸交通。  
  
### <a name="throttling-traffic"></a>節流流量

加上 DSCP 值，節流是管理頻寬另一個按鍵的控制項。 如之前所述，您可以使用**指定節流閥速率**來設定 QoS 原則的特定節流閥的輸出資料傳輸速率的設定。 藉由使用節流，QoS 原則限制指定的節流閥速率傳出網路流量。 DSCP 標示和節流可以管理流量有效一起使用。

>[!NOTE]
>根據預設，**指定調節速率**未選取核取方塊。

若要建立 QoS 原則，編輯的群組原則物件 (GPO) 從「群組原則管理主控台 (GPMC)」工具中的設定。 GPMC 開啟群組原則物件編輯器。

您必須唯一 QoS 原則的名稱。 伺服器並終端使用者如何套用原則而定 QoS 原則儲存在群組原則物件編輯器：

- 在 [電腦設定 windows 設定 Settings\QoS 原則 QoS 原則適用於電腦，無論目前登入的使用者。 您通常可以使用電腦的 QoS 原則伺服器電腦。

- 使用者 \windows Settings\QoS 原則 QoS 原則套用到使用者他們已登入之後，無論哪一部電腦他們已登入。

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>建立新原則 QoS QoS 原則精靈

-   群組原則物件編輯器] 中，將其中一項步驟以滑鼠右鍵按一下**QoS 原則**節點，然後再按一下**建立新原則**。

### <a name="wizard-page-1---policy-profile"></a>1-原則設定檔] 精靈頁面

在 QoS 原則精靈中的第一個頁面上，您可以指定原則名稱，並設定 QoS 控制項傳出網路流量的方式。

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>設定原則設定檔] 頁面的 [QoS 型原則精靈

1. 在**原則的名稱**，輸入「QoS 原則的名稱。 名稱唯一必須找出原則。

2. 或者，使用**指定 DSCP 值**可讓 DSCP 標記，並設定 DSCP 介於 0 和 63。

3. 或者，使用**指定節流閥速率**可讓您交通節流和設定節流閥速率。 必須大於 1 流速速率值，您可以指定的第二個 \(KBps\) 每 kb 為單位或每個第二個 \(MBps\) mb 單位。

4. 按一下**下一步**。

### <a name="wizard-page-2---application-name"></a>精靈頁面 2-應用程式的名稱

在第二個頁面的 [QoS 原則精靈您可以到 [所有應用程式特定的應用程式套用原則有如由可執行檔名稱，路徑和應用程式名稱，或針對特定 URL 處理要求 HTTP 伺服器應用程式。

- **所有應用程式**指定 QoS 原則精靈中的第一個頁面上的資料傳輸管理設定套用到所有應用程式。

- **這可執行檔名稱的應用程式**指定 QoS 原則精靈中的第一個頁面上的資料傳輸管理設定都是針對特定應用程式。 可執行檔命名，並.exe 副檔名必須結束。

- **僅限 HTTP 伺服器應用程式的此 URL 回應要求**指定 QoS 原則精靈中的第一個頁面上的資料傳輸管理設定適用於特定 HTTP 伺服器應用程式。

或者，您可以輸入應用程式路徑。 若要指定的應用程式路徑，包括應用程式名稱與路徑。 路徑可以包含環境變數。 例如，%ProgramFiles%\My Path\MyApp.exe 應用程式或 c:\program files\my application path\myapp.exe。

>[!NOTE]
>應用程式路徑無法包含路徑解析符號的連結。

URL 必須符合[RFC 1738](http://tools.ietf.org/html/rfc1738)，格式為`http[s]://<hostname\>:<port\>/<url-path>`。 您可以使用萬用字元，`‘*’`，適用於`<hostname>`和（或) `<port>`，例如`http://training.\*/, https://\*.\*`，但萬用字元不能代表的子`<hostname>`或`<port>`。

不亦即，`http://my\*site/`或`https://\*training\*/`是有效的。 

或者，您可以檢查**包含子目錄和檔案**來執行符合所有子目錄與檔案下列 URL。 例如，如果選取此選項且 URL `http://training`，QoS 原則會考慮要求適用於` http://training/video`告訴您一個好相符項目。

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>設定原則 QoS 精靈中的應用程式名稱頁面

1. 在**這個 QoS 原則適用於**，選取**的所有應用程式**或**可執行檔名稱的應用程式**。

2. 如果您選取 [**這個可執行檔名稱的應用程式**，指定結束.exe 副檔名可執行檔名稱。

3. 按一下**下一步**。

### <a name="wizard-page-3---ip-addresses"></a>3-IP 位址] 精靈頁面

第三個頁面 QoS 原則精靈中，您可以指定 IP 位址 QoS 原則，包括下列條件：

- 所有來源 IPv4 或 IPv6 位址或特定的來源 IPv4 或 IPv6 位址

- 全部目的 IPv4 或 IPv6 位址] 或 [特定目的 IPv4 或 IPv6 位址

如果您選取 [**來源下列的 IP 位址的僅適用於**或**的目的地下列的 IP 位址只**，您必須輸入下列其中一個動作：

- 例如，IPv4 位址 `192.168.1.1`

- 使用網路首碼長度標記，例如 IPv4 位址前置詞 `192.168.1.0/24`

- 例如 IPv6 位址 `3ffe:ffff::1`

- IPv6 位址前置詞例如 `3ffe:ffff::/48`

如果您同時選取**來源下列的 IP 位址的僅適用於**和**的目的地下列的 IP 位址只**，同時地址或地址前置詞必須任一 IPv4 或 IPv6 為基礎。

如果您在精靈上一頁指定的應用程式 HTTP 伺服器的 URL，您會注意到來源 QoS 原則精靈本頁上的 IP 位址灰色。 

因為來源 IP 位址 HTTP 伺服器位址，它可設定此處不是如此。 手動，您仍然可以自訂原則藉由目的地 IP 位址。 這可讓您可以使用相同的 HTTP 伺服器應用程式的不同戶端建立不同的原則。

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>若要設定 QoS 原則精靈的 IP 位址] 頁面

1. 在**此 QoS 原則適用於**（來源），選取 [**任何來源 IP 位址**或**僅供下列的 IP 來源位址**。

2. 如果您選取 [**只下列的 IP 來源位址**，指定 IPv4 或 IPv6 位址，或是前置詞。

3. 在**此 QoS 原則適用於**（目的地），選取 [**任何目的地位址**或**僅供下列的 IP 位址目的。**

4. 如果您選取 [**下列的 IP 位址目的地的僅適用於**，指定 IPv4 或 IPv6 位址前置詞位址的類型或指定來源位址前置詞。

5.  按一下**下一步**。  

### <a name="wizard-page-4---protocols-and-ports"></a>精靈頁面 4-通訊協定與連接埠

在 [QoS 原則精靈第四個頁面上，您可以指定的類型的資料傳輸與精靈中的第一個頁面上的設定所控制連接埠。 您可以指定：  
-   TCP 流量、UDP 交通，或兩者  

-   所有來源連接埠，各種來源的連接埠或特定來源連接埠

-   所有目的地連接埠，有各種不同的目的地連接埠或特定目的連接埠  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>設定原則 QoS 精靈的通訊協定與連接埠] 頁面

1. 在**選取通訊協定 QoS 這項原則套用到**、**TCP**、**UDP**，或**TCP 與 UDP**。

2. 在**指定來源連接埠號碼**，請選取**的任何來源連接埠**或**從這個來源連接埠號碼**。

3. 如果您選取 [**這個來源連接埠號碼的**，輸入 1 之間 65535 連接埠號碼。

     或者，您可以指定連接埠各種不同的格式」*低*:*高*，「其中*低*和*高*代表下限和上限的連接埠範圍，並依。 *低*與*高*每一個必須是介於 1 與 65535。 允許不空間之間分號（:）的字元和數字。

4. 在**指定目的地連接埠號碼**，請選取**以任何目的地連接埠**或**到此目標連接埠號碼**。

5. 如果您選取 [**到此目標連接埠號碼**在上一個步驟中，輸入 1 之間 65535 連接埠號碼。

若要完成建立新的 QoS 原則，按一下 [**完成**在**通訊協定與連接埠**QoS 原則精靈的頁面。 完成時，在詳細資料窗格中的 [群組原則物件編輯器列出的新 QoS 原則。  
  
若要套用 QoS 原則設定的使用者或電腦，在其中的 QoS 原則位於 Active Directory Domain Services 容器，例如 「 網域、 網站或組織單位 （組織單位） 將 GPO 連結。  
  
##  <a name="bkmk_editpolicy"></a>檢視、 編輯，或移除 QoS 原則

當您檢視或編輯的原則時所顯示網頁屬性先前對應的 QoS 原則精靈中所述之。  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>若要檢視的屬性 QoS 原則  
  
-   在詳細資料窗格中的群組原則物件編輯器原則的名稱上按一下滑鼠右鍵，然後按一下**屬性**。  
  
     群組原則物件編輯器會顯示下列索引標籤的屬性頁面：  
  
    -   原則設定檔  
  
    -   應用程式的名稱  
  
    -   IP 位址  
  
    -   通訊協定與連接埠  
  
### <a name="to-edit-a-qos-policy"></a>若要編輯 QoS 原則  
  
-   在詳細資料窗格中的群組原則物件編輯器原則的名稱上按一下滑鼠右鍵，然後按一下**編輯現有的原則**。  
  
     群組原則物件編輯器會顯示**編輯現有 QoS 原則**對話方塊。  
  
### <a name="to-delete-a-qos-policy"></a>若要 delete QoS 原則  
  
-   在詳細資料窗格中的群組原則物件編輯器原則的名稱上按一下滑鼠右鍵，然後按一下**Delete 原則**。  
  
### <a name="qos-policy-gpmc-reporting"></a>QoS 原則 GPMC 報告 

在您的組織套用 QoS 原則數目之後，可能需要定期檢查如何套用原則甚至。 使用 GPMC 報告可以檢視 QoS 原則的特定的使用者或電腦的摘要。  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>若要執行 QoS 原則的報告的群組原則結果精靈  
  
-   在 gpmc 中，以滑鼠右鍵按一下**群組原則結果**節點，然後再選取功能表選項的**群組原則結果精靈。**  
  
群組原則結果專之後，請按一下**設定**索引標籤。在**設定**索引標籤上，可以在 [電腦設定 windows 設定 Settings\QoS 原則」和「使用者 \windows Settings\QoS 原則」節點找到 QoS 原則。  
  
在**設定**索引標籤上，QoS 原則會列出它們 DSCP 值、 節流閥速率、 原則條件，QoS 原則的名稱來和贏得 GPO 列在同一行。  
  
群組原則結果檢視辨識獲勝 GPO。 當多個 Gpo 有 QoS 原則相同的 QoS 原則名稱時，以最高優先順序 GPO GPO 會套用。 這是獲勝 GPO。 發生衝突 QoS 原則 （由原則的名稱），已連接到較低優先順序 GPO 並不會套用。 請注意 GPO 優先順序定義哪些 QoS 原則適當地部署網站、 網域，或滑鼠。 部署之後，在使用者或電腦的層級， [QoS 原則優先順序規則](#BKMK_precedencerules)允許的流量並封鎖判斷。  
  
QoS 原則的 DSCP 值、 節流閥速率和原則條件也是顯示在群組原則物件編輯器 (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>進階漫遊和遠端使用者的設定  
使用 QoS 原則，目標是管理企業的網路流量。 在行動裝置版案例中，使用者可能會傳送資料傳輸或企業網路關閉。 由於 QoS 原則不相關的企業網路離開時，才只在適用於 Windows 8、 Windows 7 或 Windows Vista 連接到企業網路介面 QoS 原則。  
  
例如，使用者可能會她筆記型電腦透過 virtual 私人網路 (VPN) 他們企業網路從連接咖啡館。 Vpn、 實體網路介面 （例如無線） 並不會套用 QoS 原則。 不過，VPN 介面會有套用，因為它連接到企業 QoS 原則。 如果使用者稍後輸入不需要 AD DS 信任關係的另一個企業網路，將不支援 QoS 原則。  
  
請注意，在這些案例中行動裝置版不會套用到伺服器工作負載。 例如，有多個網路介面卡的伺服器可能坐邊緣的企業網路。 選擇 IT 部門可能 QoS 原則節流閥流量之 egresses 企業;不過，這傳送此輸出流量網路介面卡不一定連接到企業網路。 基於這個原因，QoS 原則的隨時支援所有的網路介面執行 Windows Server 2012 的電腦上。  
  
> [!NOTE]
>  選擇性提供僅適用於 QoS 原則，而非討論本文件中的下一步進階 QoS 設定。  
  
### <a name="advanced-qos-settings"></a>進階的 QoS 設定

進階的設定] QoS 提供額外控制，IT 系統管理員，管理電腦網路消耗和 DSCP 標示為。 而 QoS 原則可以在電腦和使用者層級套用只能在電腦層級，適用於進階的 QoS 設定。

#### <a name="to-configure-advanced-qos-settings"></a>若要設定 QoS 的進階的設定

1.  按一下**電腦設定**，然後按一下 [**群組原則中的 Windows 設定**。
  
2.  以滑鼠右鍵按一下**QoS 原則**，然後按一下 [**進階 QoS 設定]**。

     下圖顯示這兩個進階 QoS 設定] 索引標籤：**輸入 TCP 流量**和**將會覆寫 DSCP**。
  
> [!NOTE]
>  進階的 QoS 設定的層級電腦群組原則設定。
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>進階 QoS 設定： 輸入 TCP 流量

**靠近花朵的 TCP 流量**而 QoS 原則影響輸出的 TCP 與 UDP 傳輸控制項一邊收件者的 TCP 頻寬使用量。 

設定較低處理能力層級在**輸入 TCP 流量**索引標籤，將會限制 TCP 其此處刊登的 TCP 大小接收視窗。 這項設定的效果將會增加的輸送量並連結以更高頻寬或延遲 (頻寬延遲 product) 的 TCP 連接的使用量。 根據預設，在執行 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的電腦設定為最大的輸送量層級。
  
TCP 收到視窗已經在 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 從先前的 Windows 版本。 舊版 Windows 設計的有限的 TCP 接收端視窗最多 64 (kb)，而 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 動態接收端調整視窗大小達 16 (mb)。 在輸入的 TCP 傳輸控制項中，您可以控制輸入的輸送量層級設定 TCP 收到視窗可以一起拓展的最大值。 層級對應至下列最大值。 
  
|輸入的輸送量層級|最大值|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

實際的視窗大小可能等於或小於根據網路條件，最大值。

###### <a name="to-set-the-tcp-receive-side-window"></a>若要設定的 TCP 接收端視窗

1. 群組原則物件編輯器] 中，按一下 [**本機電腦原則**，按一下 [ **Windows 設定**，以滑鼠右鍵按一下**QoS 原則**，，然後按一下 [**進階 QoS 設定]**。
  
2. 在**TCP 接收輸送量**，請選取**設定 TCP 接收輸送量**，然後選取 [輸送量您想要的層級。

3.  將 GPO 連結到組織單位。

#### <a name="advanced-qos-settings-dscp-marking-override"></a>進階 QoS 設定： 將會覆寫 DSCP

將會覆寫 DSCP 限制指定的應用程式的能力，或 「 標記 」-DSCP 值以外 QoS 原則中指定的。 藉由應用程式可設定 DSCP 值，應用程式可以設定為零 DSCP 值。 

藉由**忽略]**，使用 QoS Api 的應用程式將會有設定為零，其 DSCP 值，並只 QoS 原則設定 DSCP 值。 

根據預設，可執行 Windows Server 2016、 Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012、 Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的電腦讓應用程式指定 DSCP 值。不覆寫應用程式和裝置，請不要使用 QoS Api。

##### <a name="wireless-multimedia-and-dscp-values"></a>Wireless 多媒體及 DSCP 值

[Alliance Wi ‑ Fi](https://go.microsoft.com/fwlink/?LinkId=160769)已建立憑證 Wireless 多媒體 \(WMM\) 定義四個存取分類 \(WMM_AC\) 排列網路流量傳輸 wireless Wi\ Wi-fi 網路。 存取分類包括 \ （依的最高的最低 priority\）： 語音、 視訊、 最佳努力及背景。分別縮寫 VO、 VI、 相當，以及亮度。 WMM 規格會定義哪個 DSCP 每個的四個存取分類的對應的值：
  
|DSCP 值。|WMM 存取明細|
|----------|-------------------|
|48-63|語音 (VO)|
|32-47|影片 (VI)|
|24-31, 0-7|最佳效果 （相當）|
|8-23|背景 （亮度）|

您可以建立 QoS 原則，以確保 WMM wireless 卡筆記型電腦與 Wi\-Fi Certified™ 收到時相關聯 Wi\ Wi-fi 認證 WMM 存取點的優先順序的處理使用這些 DSCP 值。
  
### <a name="BKMK_precedencerules"></a>QoS 原則優先順序規則

類似 GPO 的優先順序 QoS 原則有衝突時多個 QoS 原則套用到特定的設定的資料傳輸到優先順序規則。 輸出 TCP 或 UDP 流量，可以一次，這表示 QoS 原則，不需要累積效果，例如想加總節流閥率套用只有一個 QoS 原則。

一般而言，贏得 QoS 原則與最符合的條件。 規則時多個 QoS 原則適用的選項，可分成三種： 使用者層級與電腦層級。應用程式與網路 quintuple;並在網路 quintuple。

藉由*網路 5 倍*，我們來源 IP 位址，目的地 IP 位址、來源連接埠，目的連接埠，表示和 \(TCP/UDP\) 通訊協定。  

 **使用者層級 QoS 原則優先順序高於電腦層級 QoS 原則**

此規則會大幅幫助 QoS Gpo，尤其是使用者型群組原則的網路管理員管理。 例如網路系統管理員想要定義 QoS 使用者群組原則，他們可以只建立並散發 GPO 以該群組。 他們不用擔心有關電腦的這些使用者登入並是否這些電腦將會有定義，QoS 原則衝突，因為如果衝突，使用者層級原則一律優先。

> [!NOTE]
>  使用者層級 QoS 原則僅適用於流量使用者所。 其他使用者的特定的電腦和電腦本身不會受任何 QoS 原則使用者所定義。

 **特定應用程式和網路 quintuple 拍攝優先**

多個 QoS 原則符合特定流量時, 詳細特定原則。 之間找出應用程式的原則，被視為只辨識的應用程式名稱（無路徑）的另一個原則比更為明確的原則，包括傳送的應用程式的檔案路徑。 如果仍然適用的應用程式使用多個原則，優先順序規則使用網路 5 倍尋找最佳比對。

或者，多 QoS 原則可能會套用到相同的流量藉由不重疊的條件。 條件的應用程式和網路 quintuple，指定的應用程式原則會被視為詳細特定並套用。 

例如 policy_A 只指定的應用程式名稱 (app.exe)，和 policy_B 指定目的地 IP 位址 192.168.1.0 月 24。 在這些 QoS 原則衝突 \（app.exe 會傳送資料傳輸到範圍 192.168.4.0 日 24\ 的 IP 位址，）policy_A 取得套用。

 **更多的細節優先網路 5 倍**

在網路 quintuple 原則衝突的原則與最符合的條件優先順序。 例如，假設 policy_C 指定來源 IP 位址] 任何」，目的地 IP 位址來源連接埠，10.0.0.1」任何」，目的地連接埠「任何，「通訊協定」TCP」。 

接下來，假設 policy_D 指定來源 IP 位址] 任何」、目的地 IP 位址來源連接埠，10.0.0.1」任何」，目的地連接埠 80，和通訊協定」TCP」。 然後 policy_C 和 policy_D 符合目的地 10.0.0.1:80 連接。 因為 QoS 原則套用原則的特定最符合的條件，policy_D 優先在此範例中。  
  
不過，QoS 原則可能會有相同數量的條件。 例如，數個原則可能每指定只有一個（但不是相同）網路 quintuple 有如。 網路 quintuple，下列順序是來自高優先順序較低：

- 來源 IP 位址

- 目的地 IP 位址

- 來源連接埠

- 目的地連接埠

- 通訊協定（TCP 或 UDP）

特定的條件，例如 IP 位址，更特定的 IP 位址都會被視為優先順序較高。例如，IP 位址 192.168.4.1 是詳細特定超過 192.168.4.0 月 24。

設計原則 QoS 儘可能針對特定簡化您組織的能力的原則才會生效。

本指南下一步主題，請查看[QoS 原則事件和錯誤](qos-policy-errors.md)。

本指南中第一次主題，請查看[品質服務 (QoS) 原則](qos-policy-top.md)。
