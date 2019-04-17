---
title: Wireless 存取部署
description: 本主題是 Windows Server 2016 網路指南「部署密碼基礎 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f78da14b91e5c1e9a2dc22f92ea95c89153befa8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment"></a>Wireless 存取部署

>適用於：Windows Server（以每年次管道）、Windows Server 2016

請依照下列步驟來部署 wireless 存取：

- [部署和設定 Wireless Ap](#bkmk_aps)

- [建立無線使用者安全性群組](#bkmk_groups)

- [設定 Wireless 網路 \ (IEEE 802.11\) 原則](#bkmk_policies)

- [設定 NPS 伺服器](#bkmk_nps)

- [加入網域中的新 Wireless 電腦](#bkmk_domain)

## <a name="bkmk_aps"></a>部署和設定 Wireless Ap

請依照下列步驟來部署，並設定您的 wireless Ap:

- [指定 Wireless AP 通道頻率](#bkmk_channel)

- [設定 Wireless Ap](#bkmk_wirelessaps)

>[!NOTE]
>本文中的程序中不包含案例指示**使用者 Account 控制項**對話方塊要求您的權限才能繼續。 如果此對話方塊同時執行程序本指南，如果對話方塊一個因應以您的動作，按一下 [**繼續**。

### <a name="bkmk_channel"></a>指定 Wireless AP 通道頻率

當您要部署的單一地理網站多個 wireless Ap 時，您必須設定 wireless Ap 已使用獨特的通道頻率減少干擾 wireless Ap 之間重疊訊號。

您可以協助您選擇不衝突 wireless 網路的地理位置其他 wireless 網路的通道頻率使用下列指導方針。

- 如果有其他組織中有辦公室鄰近或做為您的組織相同的建築物、找出是否有任何 wireless 這些公司所擁有的網路。 了解同時位置，指定的通道頻率他們 wireless AP 的因為您需要為您的 AP 不同通道的頻率，您需要來判斷要安裝您的 AP 的最佳位置

- 找出重疊 wireless 訊號在組織中相鄰樓層之間切換。 檢測軍人重疊的涵蓋範圍和外，在組織中通道頻率指派給您的 wireless Ap 之後，確保任何兩重疊的涵蓋範圍 wireless Ap 指派不同通道的頻率。

### <a name="bkmk_wirelessaps"></a>設定 Wireless Ap

設定您的 wireless Ap 使用下列資訊以及 wireless AP 製造商所提供的 product 文件。

此程序列舉通常設定 wireless AP 的項目。 在項目名稱可以視品牌和型號，而且可能從下列清單中的不同。 適用於特定的詳細資訊，查看您 wireless AP 文件。

#### <a name="to-configure-your-wireless-aps"></a>若要設定您的 wireless Ap  

- **SSID**。 Wireless network\(s\) 名稱指定 \ (例如，ExampleWLAN\)。 這是通知給 wireless 戶端名稱。

- **加密**。 指定 WPA2\ 企業 \(preferred\) 或 WPA\-企業版和好一段 \(preferred\) 或 TKIP 加密的密碼，而定，支援的版本，您 wireless client 電腦網路介面卡。

- **無線 AP IP 位址 \(static\)**。 在每個 AP，設定落 DHCP 子網路的範圍排除項目各種唯一靜態 IP 位址。 使用 DHCP，排除指派的地址會防止 DHCP 伺服器相同的 IP 位址指派的電腦或其他裝置。

- **子網路遮罩**。 設定此選項可符合您有連接 wireless AP 區域網路子網路遮罩設定。  

- **DNS 名稱**。 您可以設定部分 wireless Ap DNS 名稱。 網路上的 DNS 服務可以名稱解析為 IP 位址。 在每個 wireless AP 支援此功能，輸入唯一的解析度 DNS 名稱。  

- **DHCP 服務**。 如果您 wireless AP built\ 中 DHCP 服務，來停用它。  

- **RADIUS 共用的密碼**。 除非您計劃設定 Ap 在群組 NPS RADIUS 戶端為每個 wireless AP 使用獨特的 RADIUS 共用的密碼。 如果您計劃中 NPS 設定 Ap 群組，必須是群組的相同的每個成員共用的密碼。 此外，您在使用每個共用的密碼應該隨機一連串至少 22 混合大寫的字元和小寫字母、數字和標點符號。 若要確保隨機，您可以使用發電機隨機字元，例如 NPS 中找到隨機字元發電機**設定 802.1 X**精靈中，以建立共用的密碼。

>[!TIP]
>針對每個 wireless AP 錄製共用的密碼，並將它儲存在安全的位置，例如安全 office。 當您設定 NPS RADIUS 戶端時，您必須知道的每個 wireless AP 共用的密碼。  

- **RADIUS 伺服器的 IP 位址**。 輸入的伺服器的 IP 位址。

- **UDP port\(s\)**。 根據預設，NPS 使用 UDP 連接埠 1812 年和 1645 年驗證訊息與 UDP 連接埠 1813 年和 1646，用於會計訊息。 建議您在您的 Ap，使用這些相同 UDP 連接埠，但如果您有有效的理由使用不同的連接埠，確保您不只使用新的連接埠號碼設定 Ap 也重新設定所有 NPS 伺服器為 Ap 使用相同的連接埠號碼。 如果 Ap 和 NPS 伺服器未與 UDP 連接埠相同設定、NPS 無法接收或處理連接 Ap，要求和網路上的所有 wireless 連接嘗試將會失敗。

- **Vsa**。 部分 wireless Ap 需要 vendor\ 特定屬性 \(VSAs\) 提供完整 wireless AP 功能。 加入 Vsa NPS 的網路原則。

- **篩選 DHCP**。 設定 wireless Ap 封鎖 wireless 戶端從 IP 封包 UDP 連接埠傳送 68 網路，如 wireless AP 製造商所述。

- **篩選 DNS**。 設定 wireless Ap 如 wireless AP 製造商所述封鎖 wireless 戶端從網路、埠 53 傳送 IP 封包。

## <a name="create-security-groups-for-wireless-users"></a>Wireless 使用者建立安全性群組

請依照下列步驟來建立一或多個 wireless 使用者安全性群組，然後將使用者新增至適當的 wireless 使用者安全性群組：

- [建立無線使用者安全性群組](#bkmk_groups)

- [將使用者新增至無線安全性群組](#bkmk_addusers)

### <a name="bkmk_groups"></a>建立無線使用者安全性群組

您可以使用此程序 Active Directory 使用者 \(MMC\) snap\ 電腦 Microsoft Management Console 中建立 wireless 安全性群組-中。  

資格在**網域系統管理員**，或相當於，才能執行此程序最小值。

#### <a name="to-create-a-wireless-users-security-group"></a>若要建立 wireless 使用者安全性群組

1. 按一下**[開始]**，按一下**系統管理工具]**，然後按一下 [ **Active Directory 使用者與電腦**。 Active Directory 使用者和電腦 snap\ 在開啟。 如果您未選取，按一下您的網域節點。 例如，如果您的網域 example.com，請按一下**example.com**。

2. 在詳細資料窗格中，按一下 right\ 資料夾中您要新增新的群組 \ (，例如 right\ 按一下**使用者**\)，指向 [**新**，，然後按一下 [**群組**。

3. 在**新物件 – 群組**，請在**群組名稱**，輸入新群組的名稱。 例如，輸入**群組無線**。

4. 在**群組範圍**，選擇下列其中一個選項：

    - **本機的網域**

    - **全球**

    - **通用**

5. 在**群組類型**、**的安全性**。

6. 按一下**[確定]**。

如果您需要更多個安全性群組 wireless 使用者時，重複這些步驟來建立其他 wireless 使用者群組。 之後，您可以在每個群組，提供不同的存取權限和連接規則套用不同條件和條件 NPS 中建立個人的網路原則。

### <a name="bkmk_addusers"></a>將使用者新增至無線使用者安全性群組

您可以使用此程序將使用者、電腦或群組新增至您 wireless 安全性群組 Active Directory 使用者 \(MMC\) snap\ 電腦 Microsoft Management Console 中-中。

資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。

#### <a name="to-add-users-to-the-wireless-security-group"></a>將使用者新增至 wireless 安全性群組

1. 按一下**[開始]**，按一下**系統管理工具]**，然後按一下 [ **Active Directory 使用者與電腦**。 Active Directory 使用者和電腦 MMC 開啟。 如果您未選取，按一下您的網域節點。 例如，如果您的網域 example.com，請按一下**example.com**。

2. 在詳細資料窗格中，按一下 double\ wireless 安全性群組所在的資料夾。

3. 在 [詳細資料窗格，right\ 按一下 wireless 安全性群組，然後再按一下**屬性**。 **屬性**中的安全性群組。

4. 在**成員**索引標籤上，按一下 [**新增**，並將電腦新增或新增的使用者或群組下列程序完成。

##### <a name="to-add-a-user-or-group"></a>若要新增的使用者或群組

1. 在**[輸入物件名稱來選取**中，輸入名稱的使用者或群組，您想来新增，然後再按一下**[確定]**。

2. 若要指定群組成員資格其他使用者或群組，重複步驟 1 此程序。  

##### <a name="to-add-a-computer"></a>若要將電腦加入

1. 按一下**物件類型**。 **物件類型**對話方塊。

2. 在**物件類型**，請選取**電腦**，然後按一下 [ **[確定]**。

3. 在**[輸入物件名稱來選取**，輸入您想要新增，然後按一下 [電腦名稱**[確定]**。

4. 若要指定的其他電腦群組成員資格，重複步驟 1\-3 此程序。

## <a name="bkmk_policies"></a>設定 Wireless 網路 \ (IEEE 802.11\) 原則

請依照下列步驟來設定無線網路 \ (IEEE 802.11\) 原則群組原則擴充功能：

- [打開或新增並打開群組原則物件](#bkmk_opengpme)

- [啟動預設 Wireless 網路 \ (IEEE 802.11\) 原則](#bkmk_activate)

- [設定新網路 Wireless 原則](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>打開或新增並打開群組原則物件

根據預設時已安裝的 Active Directory Domain Services \(AD DS\) 伺服器角色為網域控制站伺服器設定, 執行 Windows Server 2016 的電腦上安裝的群組原則管理功能。 告訴您如何左網域控制站在群組原則管理主控台 \(GPMC\) 下列程序。 此程序再告訴您如何任一個開放現有 domain\ 層級群組原則物件 \(GPO\) 以供編輯，或建立新的網域 GPO 與開放它來編輯。

資格在**網域系統管理員**，或相當於，才能執行此程序最小值。

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>透過或新增並打開群組原則物件

1. 在您的網域控制站，按一下**[開始]**，按一下 [ **Windows 系統管理工具]**，，然後按一下 [**群組原則管理**。 群組原則管理主控台開啟。  

2. 在左窗格中，按一下 double\ 樹系。 例如，按 double\**的樹系：example.com**。  

3. 在左窗格中，按一下 double\**網域**，再 double\ 按網域您想要管理群組原則物件。 例如，按 double\ **example.com**。  

4. 執行下列其中一個動作：

    -   **若要打開現有的 domain\ 層級 GPO 以供編輯**、按兩下包含您想要管理的群組原則物件的網域 right\ 按一下網域原則您想要管理，例如 [預設網域原則，然後按一下 [**編輯**。 **群組原則編輯器] 管理**開啟。

    -   **建立新的群組原則物件和編輯開放**、right\ 按一下網域您想要建立新的群組原則物件，並按一下 [**在這個網域中建立 GPO 並連結到**。

        在**新的 GPO**，請在**名稱**，輸入新的群組原則物件的名稱，再按**[確定]**。

        Right\ 按一下新的群組原則物件，然後再按一下**編輯**。 **群組原則編輯器] 管理**開啟。

在下一節中，您將使用群組原則編輯器] 管理建立 wireless 原則。

### <a name="bkmk_activate"></a>啟動預設 Wireless 網路 \ (IEEE 802.11\) 原則

此程序告訴您如何啟動預設無線網路 \ (IEEE 802.11\) 使用群組原則編輯器] 管理 \(GPME\) 原則。

>[!NOTE]
>您之後**Windows Vista 和稍後發行**版本無線網路 \ (IEEE 802.11\) 原則或**Windows XP**版，版本選項會自動移除清單選項時您 right\ 按**無線網路 \ (IEEE 802.11\) 原則**。 這是因為選取原則版本之後，已新增原則 GPME 詳細資料窗格中當您選取 [**無線網路 \ (IEEE 802.11\) 原則**節點。 除非您 delete wireless 原則，此時 wireless 原則版本傳回 right\ 按一下功能表會維持此狀態**Wireless 網路 \ (IEEE 802.11\) 原則**中 GPME。 此外，wireless 原則只會列在 GPME 詳細資料窗格時**Wireless 網路 \ (IEEE 802.11\) 原則**選取節點。

資格在**網域系統管理員**，或相當於，才能執行此程序最小值。

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>若要啟動預設無線網路 \ (IEEE 802.11\) 原則  

1. 先前的程序，請遵循**透過或新增並打開群組原則物件**打開 GPME。

2. 在 GPME，請在左窗格中，double\ 按一下**電腦設定**，double\ 按**原則**，double\ 按**Windows 設定**，double\ 再按**的安全性設定**。

![802.1 X 無線群組原則](../../../media/Wireless-GP/Wireless-GP.jpg)

3. 在**安全性設定**，right\ 按一下**無線網路 \ (IEEE 802.11\) 原則**，然後按一下 [**建立新的 Windows Vista 無線原則和稍後發行**。 

![802.1 x Wireless 原則](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. **無線新的網路原則屬性**對話方塊。 在**原則的名稱**，輸入原則的名稱，或讓預設的名稱。 按一下**[確定]**以儲存的原則。 預設的原則啟動和使用您所提供的新名稱，或使用的預設名稱 GPME 的詳細資料窗格中列出**無線新的網路原則**。

![新的網路 Wireless 原則屬性](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. 在詳細資料窗格中，按一下 double\**無線新的網路原則**打開它。

下一節中，您可以執行原則設定、原則處理喜好設定順序和網路權限。

### <a name="bkmk_policyconfig"></a>設定新網路 Wireless 原則

您也可以在本區段中使用程序，設定無線網路 \ (IEEE 802.11\) 原則。 這項原則可讓您設定的安全性，並驗證、管理 wireless 設定檔，以及指定 wireless 網路不會設定為慣用網路的權限。

- [設定 PEAP\-MS\-CHAP v2 Wireless 連接設定檔](#bkmk_configureprofile)  

- [設定的喜好設定順序 Wireless 連接設定檔](#bkmk_preferenceorder)  

- [定義網路權限](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>設定 PEAP\-MS\-CHAP v2 Wireless 連接設定檔

此程序提供所需設定 PEAP\-MS\-CHAP v2 wireless 設定檔的步驟。  

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>若要設定 PEAP\-MS\-CHAP v2 wireless 連接設定檔

1. GPME，為您剛剛建立的原則 wireless 網路屬性對話方塊中的**一般**索引標籤在**描述**，輸入原則的簡短描述。

2. 若要指定 WLAN 自動設定用於 wireless 網路介面卡設定，請確定**使用 Windows WLAN 自動設定戶端服務**選取。  

3. 在**連接到可用的網路設定檔下列之**，按一下 [**新增**，]，然後選取**基礎結構**。 **新的設定檔屬性**對話方塊。

4. 在**新的設定檔屬性**對話方塊中，於**連接**索引標籤**設定檔名稱**] 欄位，輸入新的設定檔的名稱。 例如，輸入**適用於 Windows 10 WLAN 設定檔 Example.com**。

5. 在**網路 Name\(s\) \(SSID\)**，輸入 [對應至設定在您的 wireless Ap，SSID SSID，然後按**新增]**。

    如果您的部署使用多個 Ssid，每個 wireless AP 使用相同的 wireless 安全性設定重複此步驟，即可新增您想要套用此設定擋每個 wireless AP SSID。

    如果您的部署使用多個 Ssid，不符合安全性設定為每個 SSID 設定為使用相同的安全性設定的每個群組不同的設定檔。 例如，如果您有一個群組設定為使用 WPA\-企業版和 TKIP 使用 WPA2\ 企業好一段及其他群 wireless Ap wireless Ap，設定設定檔 wireless Ap 的每個群組。

6. 如果預設文字**NEWSSID**是存在，選取它，然後按一下**移除**。

7. 如果您要部署 wireless 存取點的設定來隱藏廣播的指標，請選取 [**連接即使未廣播網路**。

    > [!NOTE]
    > 因為 wireless 戶端會探查，並嘗試連接到任何 wireless 網路，讓這個選項可以建立是安全性風險。 預設不支援此設定。  

8. 按一下**安全性**索引標籤上，按一下 [**進階]**，然後進行下列設定：

    1. 設定進階 802.1 X 的設定，請在**IEEE 802.1 X**、**動作將使用進階 802.1 X 設定**。

        時進階的 802.1 X 設定正在執行，預設值的**最大的 [開始] 畫面 Eapol\ 訊息**，**保留期間**，**開始期間**，和**驗證期間**的一般 wireless 部署的不足。 因此，您不需要變更的預設值，除非您有特定的原因。

    2. 若要讓單一登入，請選取**這個網路讓單一登入**。

    3. 剩餘的預設值的**單一登入**的一般 wireless 部署的不足。

    4. 在**「快速頻道」漫遊**，如果您 wireless AP 設定 pre\-驗證功能，請選取**此網路使用 pre\ 驗證**。

9. 若要指定 wireless 通訊符合 FIPS 140\-2 標準，請選取 [ **FIPS 140\-2 認證模式中執行密碼編譯**。

10. 按一下**[確定]**以返回**的安全性**索引標籤。在**選取此網路安全性方法**，請在**驗證**、選取**WPA2\ 企業**您 wireless AP 與 wireless client 網路介面卡所支援下，如果。 否則，請選取**WPA\ 企業**。

11. 在**加密**，如果支援您 wireless AP 及 wireless client 網路介面卡，選取**好一段-CCMP**。 如果您使用存取點，並支援 802.11ac wireless 網路介面卡，請選取 [**好一段-GCMP**。 否則，請選取**TKIP**。

    > [!NOTE]  
    > 兩者的設定為**驗證**和**加密**必須符合您 wireless Ap 上的設定。 預設設定**驗證模式**，**驗證失敗的最大**，並**快取的後續連接到這個網路使用者資訊**的典型 wireless 部署滿足。  

12. 在**選取網路驗證方法**，請選取**受保護的 EAP \(PEAP\)**，然後按一下 [**屬性**。 **保護 EAP 屬性**對話方塊。

13. 在**保護 EAP 屬性**，確認已選取**驗證身分伺服器的驗證憑證的**選取。

14. 在**受信任的根憑證授權單位**，請選取 NPS 伺服器的受信任的根憑證授權單位 \(CA\) 發出伺服器的憑證。

    > [!NOTE]  
    > 這個設定限制的根信任選取 Ca 戶端 Ca。 如果未受信任的根 Ca 選取，戶端將信任所有根 Ca 列在其受信任的根憑證授權單位憑證存放區。  

15. 在**選擇驗證方法**清單中，選取 [ **Secured 密碼 \ (EAP\-MS\-CHAP v2\)**。

16. 按一下**設定**。 在**EAP MSCHAPv2 屬性**對話方塊方塊中，請確認**自動使用我的登入 Windows 名稱與密碼 \ (和網域如果 any\)**已選取，然後按一下 [ **[確定]**。

17. 讓 PEAP（fast ring）重新連接，以確保**（fast ring）讓重新連接**選取。

18. 若要需要連接嘗試伺服器加密繫結 TLV，請選取 [**如果伺服器不會顯示加密繫結 TLV 中斷連接**。

19. 若要指定的使用者身分有遮罩驗證的其中一個階段，請選取 [**讓身分隱私權**，並在文字方塊中輸入名稱匿名的身分，或留在文字方塊。

    >[!筆記]
    >- 802.1 X 無線 NPS 原則必須使用 NPS 來建立**連接要求原則**。 如果 NPS 原則以 NPS 建立**的網路原則**，然後將無法運作的身分隱私權。
    >- 空或匿名的身分某些 EAP 方法提供 EAP 身分隱私權 \（不同的實際 identity\）傳送回應 EAP 身分邀請。 PEAP 傳送身分在驗證期間兩次。 在第一階段，身分傳送一般，此身分用於路由，不適用於 client 驗證。 實際的身分，用來驗證-傳送驗證，在安全的通道，會建立在第一階段中的第二個階段。 如果**讓身分隱私權**核取方塊已選取，指定文字方塊中的項目會取代使用者名稱。 例如假設**讓身分隱私權**已選取的身分隱私權別名和**匿名**文字方塊中指定。 實際身分別名使用者的**jdoe@example.com**，將會傳送中第一階段的驗證身分變更為**anonymous@example.com**。為用於路由用途，就不會修改 1 階段身分領域部分。  

20. 按一下**[確定]**以關閉 [**保護 EAP 屬性**對話方塊。
21. 按一下**[確定]**以關閉 [**的安全性**索引標籤。
22. 如果您想要建立額外的設定檔，請按一下**新增**，然後重複上一個步驟，並進行其他選擇以自訂 wireless 戶端和網路您想要套用的設定檔的每個設定檔。 當您新增設定檔完成後時，按一下 [ **[確定]**來關閉對話方塊無線的網路原則屬性。

下一節中，您可以訂購最佳的安全性原則設定檔。

#### <a name="bkmk_preferenceorder"></a>設定的喜好設定順序 Wireless 連接設定檔
如果您已建立多個 wireless 設定檔 wireless 的網路原則中，而您想要訂單的設定檔，如要達到最佳效能和安全性，您可以使用此程序。

若要確保 wireless 戶端連接的安全性，他們可以支援最高層級，將最多限制原則清單的頂端。

例如，如果您有兩種設定檔，一個支援 WPA2 之用戶端，一個用於戶端支援 WPA，放置 WPA2 設定檔更高版本清單。 這樣可確保的支援 WPA2 用方法連接，而不是使用較不安全 WPA。

此程序提供的步驟來指定連接網域成員 wireless 戶端 wireless 網路使用 wireless 連接設定檔的順序。

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>若要設定的喜好設定順序 wireless 連接設定檔

1. 在 [GPME，只要設定原則 wireless 網路屬性對話方塊中按一下**一般**索引標籤。

2. 在**一般**索引標籤的**連接到可用的網路設定檔下列之**、選取在清單中，移至您想要的設定檔，再按一下任一」向上箭號 [按鈕] 或 [向下箭號 [按鈕將個人檔案移至您想要的位置在清單中。

3.  重複步驟 2 的每個您想要移動的清單中的設定檔。  

4.  按一下**[確定]**以儲存的所有變更。

下一節，您可以定義 wireless 原則的網路權限。

#### <a name="bkmk_permissions"></a>定義網路權限
您可以在設定設定**網路權限**索引標籤無線網路網域成員 \ (IEEE 802.11\) 原則套用。

您可以只適用於下列設定 wireless 網路未在設定**一般**索引標籤中**Wireless 的網路原則屬性**頁面：

- 允許或拒絕連接到特定 wireless 網路所指定的網路類型 Service 設定識別碼 \(SSID\)

- 允許或拒絕連接到特定的網路

- 允許或拒絕基礎結構網路來連接情形

- 允許或拒絕使用者檢視的網路類型 \（臨機操作或 infrastructure\）他們無法存取

- 允許或拒絕使用者建立套用到所有使用者的設定檔

- 使用者只能使用群組原則設定檔來連接允許網路

資格在**網域系統管理員**，或相當於，才能完成這些程序最小值。

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>若要允許或拒絕連接到特定 wireless 網路

1. 在 [GPME，在 wireless 網路屬性對話方塊中，按一下 [**網路權限]**索引標籤。

2. 在**網路權限]**索引標籤上，按一下 [**新增]**。 **新的權限的項目**對話方塊。

3. 在**新的權限的項目**對話方塊中，在**的網路名稱 \(SSID\)**欄位中輸入的網路 SSID 網路您想要定義權限。

4.  在**的網路類型**、**基礎結構**或**臨機操作**。

    > [!NOTE]  
    > 如果您不確定是否廣播網路基礎結構或特定網路，您可以設定兩個網路的權限的項目，為每個網路的類型。

5. 在**的權限**、**允許**或**拒絕**。

6. 按一下**[確定]**，以返回**網路權限]**索引標籤。

##### <a name="to-specify-additional-network-permissions-optional"></a>若要指定其他網路權限 \(Optional\)

1.  在**網路權限]**索引標籤上，設定下列一或多個動作：  

    -   若要即可授權您的網域成員至特定網路，選取 [**避免 ad\ 特殊網路來連接**。

    -   若要即可授權您的網域成員網路基礎結構，選取 [**避免基礎結構網路來連接**。  

    -   若要允許您檢視的網路類型網域成員 \（臨機操作或 infrastructure\）到，他們無法存取，選取**允許使用者檢視拒絕的網路**。

    -   若要讓使用者建立套用到所有使用者的設定檔，請選取 [**讓任何人建立所有的使用者設定檔以**。

    -   若要指定您的使用者可以僅限連接到允許使用群組原則設定檔的網路，請選取 [**只能使用群組原則設定檔，允許的網路的**。

## <a name="bkmk_nps"></a>設定您的 NPS 伺服器
請依照下列步驟來設定 NPS 伺服器 wireless 存取執行 802.1 X 驗證：

- [在 Active Directory Domain Services 登記 NPS](#bkmk_npsreg)

- [Wireless AP 設定為 NPS RADIUS Client](#bkmk_radiusclient)

- [使用精靈 802.1 X 無線建立 NPS 原則](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>在 Active Directory Domain Services 登記 NPS
您可以使用此程序登記執行 Active Directory Domain Services \(AD DS\) 位於網域中 NPS 伺服器成員網路原則伺服器 \(NPS\) 伺服器。 NPS 讀取的帳號 dial\ 中屬性授權程序期間的權限授與的伺服器，每個 NPS 伺服器必須在 AD DS 登記完畢。 登記 NPS 伺服器新增伺服器**RAS 及 IAS 伺服器]**中 AD DS 安全性群組。

>[!NOTE]
>您可以安裝 NPS 網域控制站在或專用的伺服器上。 執行下列 Windows PowerShell 命令安裝 NPS，如果您有您尚未執行此動作：
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

#### <a name="to-register-an-nps-server-in-its-default-domain"></a>若要在其預設網域登記 NPS 伺服器

1. NPS 伺服器，在**伺服器管理員**，按一下 [**工具**，，然後按一下 [**的網路原則伺服器**。 NPS snap\ 在開啟。

2. Right\ 按一下**NPS \(Local\)**，然後按一下 [**登記伺服器 Active Directory**。 **的網路原則伺服器**對話方塊。

3. 在**的網路原則伺服器**，按一下 [ **[確定]**，，然後按一下 [ **[確定]**再試一次。

### <a name="bkmk_radiusclient"></a>Wireless AP 設定為 NPS RADIUS Client
您可以使用此程序，設定 AP，也就是*網路存取伺服器 \(NAS\)*，為使用中 snap\ NPS 遠端驗證 Dial\-In 使用者服務 \(RADIUS\) client。 

>[!IMPORTANT]
>Client 電腦，例如 wireless 筆記型電腦與其他執行 client 作業系統的電腦不是 RADIUS 戶端。 RADIUS 戶端的網路存取伺服器，例如 wireless 存取點，802.1X\-能參數、virtual 私人網路 \(VPN\) 伺服器及 dial\ 接伺服器，因為它們可以使用 RADIUS 通訊協定進行通訊例如伺服器 NPS RADIUS 伺服器。

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>新增為中 NPS RADIUS client 的網路存取伺服器

1. NPS 伺服器，在**伺服器管理員**，按一下 [**工具**，，然後按一下 [**的網路原則伺服器**。 NPS snap\ 在開啟。

2. 在 snap\ 中 NPS double\ 按一下**RADIUS 戶端與伺服器**。 Right\ 按一下**RADIUS 戶端**，然後按一下 [**新**。

3. 在**新 RADIUS Client**，確認**可讓這個 RADIUS client**核取方塊。

4. 在**新 RADIUS Client**，請在**的易記名稱**，輸入 wireless 存取點的顯示名稱。

    例如，如果您想要新增的 wireless 存取點 \(AP\) 名 AP\-01，輸入**AP\-01**。

5. 在**位址 \(IP or DNS\)**中，輸入 IP 位址或的完整網域名稱 \(FQDN\) nas。

    如果您輸入 FQDN，若要確認是否正確的名稱，並且對應至有效的 IP 位址，按一下**確認**，然後在**驗證地址**，請在**地址**欄位中，按一下**解析**。 如果名稱 FQDN 對應至有效的 IP 位址，該 NAS 的 IP 位址會自動出現在**的 IP 位址**。 如果 FQDN 無法解析為 IP 位址您將會收到訊息，指出稱為主機。 發生這種情形，如果確認您擁有正確 AP 名稱，且 AP 為電源且連上網路。  

    按一下**[確定]**以關閉 [**驗證地址**。  

6. 在**新 RADIUS Client**，請在**共用密碼**，執行下列其中一個動作：  

    -   若要手動設定 RADIUS 共用的密碼，請選取 [**手動**，然後在**共用密碼**，輸入長，這也 NAS 上輸入密碼。 共用的密碼中重新輸入**確認共用的密碼**。  

    -   若要自動產生共用的密碼，請選取 [**產生**核取方塊，並再按**產生**按鈕。 儲存產生共用的密碼，然後再使用該值來設定 NAS，讓它可以具有 NPS 伺服器通訊。  

        >[!IMPORTANT]
        >您輸入您 virtual AP 的中 NPS RADIUS 共用的密碼完全必須符合您實際 wireless AP 的已 RADIUS 共用的密碼 如果您使用 NPS 選項產生 RADIUS 共用的密碼，您必須使用由 NPS RADIUS 共用密碼設定相符實際 wireless AP。

7. 在**新 RADIUS Client**，在**進階**索引標籤**廠商名稱**，指定 NAS 製造商名稱。 如果您不確定 NAS 製造商名稱，請選取**RADIUS 標準**。

8. 在**其他選項**，如果您使用任何 EAP 和 PEAP，以外的驗證方法，如果您 NAS 支援使用訊息 authenticator 屬性，選取 [**的訊息存取要求必須包含 Message\-Authenticator 屬性**。

9. 按一下**[確定]**。 您 NAS 會出現在清單中的設定伺服器 NPS RADIUS 用。

### <a name="bkmk_npspolicy"></a>使用精靈 802.1 X 無線建立 NPS 原則
您可以使用此程序來建立連接要求原則和網路原則部署任一 802.1X\ 所需的功能 wireless 的存取點，為遠端驗證 Dial\-In 使用者服務 \(RADIUS\) 戶端執行的網路原則伺服器 \(NPS\) RADIUS 伺服器。  
您在執行精靈之後，下列原則建立：

- 有一個連接要求原則

- 有一個網路原則

>[!NOTE]
>您可以在每次您需要為 802.1 X 驗證存取建立新原則執行新 IEEE 802.1 X 的安全有線和無線連接精靈。

資格在**網域系統管理員**，或相當於，才能完成此程序最小值。

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>建立 802.1 X 驗證無線的原則，使用精靈

1. 打開 NPS snap\ 中。 如果您未選取，按一下 [ **NPS \(Local\)**。 如果您 NPS MMC snap\ 中執行，並想要 NPS 遠端伺服器上建立的原則，選取 [伺服器。

2. 在**開始**，請在**標準設定**、選取**802.1 X 無線或有線連接 RADIUS 伺服器**。 文字和連結的文字變更，以反映您的選擇。

3. 按一下**設定 802.1 X**。 設定 802.1 X 精靈開啟。

4.  在**選取 802.1 X 連接類型**精靈] 頁面上，在**類型 802.1 X 連接**，請選取**安全無線連接**，並在**名稱**、輸入您的原則的名稱或離開預設名稱**安全無線連接**。 按一下**下一步**。

5.  在**指定 802.1 X 參數**中精靈頁面**RADIUS 戶端**，所有 802.1 X 參數和 wireless 存取點，您已經新增在 snap\ 中 NPS RADIUS 戶端示。 執行下列其中一項：

    -   中新增額外的網路存取伺服器 \(NASs\)，例如 wireless Ap，**RADIUS 戶端**，按一下 [**新增**，然後在**新 RADIUS client**，輸入的資訊：**易記名稱**，**地址 \(IP or DNS\)**，和**共用密碼**。

    -   若要修改設定的任何 NAS，在**RADIUS 戶端**，選取您要修改設定，然後按一下 [的 AP**編輯**。 修改所需的設定。

    -   中，從清單中移除 NAS **RADIUS 戶端**，選取 [NAS，，然後按**移除**。

        >[!WARNING]
        >移除 RADIUS client 的**設定 802.1 X**精靈將 client 移除 NPS 伺服器設定。 新增項目、修改，以及刪除，可以在**設定 802.1 X**中反映在 snap\ 中 NPS RADIUS 戶端精靈**RADIUS 戶端**下的節點**NPS** \/ **RADIUS 戶端與伺服器**。 例如，如果您使用移除使用 802.1 X 精靈切換，開關切換至也會從 NPS snap\ 中移除。

6. 按一下**下一步**。 在**設定的驗證方法**精靈] 頁面上，在**輸入 \（根據存取和網路 configuration\ 的方法）**，請選取**Microsoft：受保護的 EAP \(PEAP\)**，，然後按一下**設定**。

    >[!TIP]
    >如果您收到錯誤訊息，表示憑證找不到使用的驗證方法、，以及您已設定自動到網路上的 RAS 及 IAS 伺服器發行憑證的 Active Directory 憑證服務，請先確定您有遵循的步驟來登記 NPS，在 Active Directory Domain Services，然後使用下列步驟來更新群組原則：按一下**[開始]**，按一下 [ **Windows 系統**，按一下 [**執行**，在**開放**，輸入**gpupdate**，然後按 ENTER 鍵。 命令時傳回指出使用者和群組原則的電腦已順利更新的結果，請選取**Microsoft：受保護的 EAP \(PEAP\)**，然後按一下 [**設定**。
    >
    >如果在重新整理您持續收到錯誤訊息，表示該憑證找不到使用的驗證方法的群組原則之後, 憑證會不會顯示因為它未符合最低伺服器憑證需求如核心網路小幫手節目表中所述：[適用於 802.1 X 的有線和無線部署部署伺服器憑證](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。 發生這種情形，如果您必須停止 NPS 設定、撤銷發給您 NPS server\(s\)，並依照指示執行設定新的憑證來使用節目表伺服器的憑證部署。

7.  在**編輯保護 EAP 屬性**中精靈頁面**發行憑證**，確定正確 NPS 伺服器的憑證已選取，然後執行下列動作：

    >[!NOTE]
    >確認中的值**發行者**是正確的憑證在選取**發行憑證**。 執行 Active Directory 憑證服務 \(AD CS\) 名 corp\DC1，網域 contoso.com，CA 發行憑證的預期的發行者不，例如**corp\ DC1\ CA**。

    -   若要允許來將他到他們的 wireless 電腦的存取點而不需要每次重新驗證他們建立新的 ap 之間的使用者，請選取 [ **（fast ring）讓重新連接**。

    -   若要指定的連接 wireless 戶端會結束網路驗證程序是否 RADIUS 伺服器不會顯示加密 Type\ Length\ 價值 \(TLV\) 繫結，請選取 [**中斷連接的戶端不加密繫結的**。  

    -   若要修改原則設定 EAP 輸入，在**Eap**，按一下 [**編輯**，請在**EAP MSCHAPv2 屬性**，如有需要修改的設定，然後按一下**[確定]**。  

8.  按一下**[確定]**。 編輯保護 EAP 屬性會關閉對話方塊，讓您以返回**設定 802.1 X**精靈。 按一下**下一步**。

9. 在**指定使用者群組**，按一下 [**新增**，然後輸入您設定的 Active Directory 使用者 snap\ 在電腦中 wireless 戶端安全性群組的名稱。 例如，如果您名為您 wireless 安全性群組無線群組中，輸入**群組無線**。 按一下**下一步**。

10. 按一下**設定**來設定 RADIUS 標準屬性和 vendor\ 特定屬性的 virtual 區域網路 \(VLAN\) 如有需要以及指定 wireless AP 硬體廠商提供的文件。 按一下**下一步**。

11. 檢視設定摘要詳細資料，並再按**完成**。

現在建立 NPS 原則，以及您可以移至 wireless 電腦加入網域的。

## <a name="bkmk_domain"></a>加入網域中的新 Wireless 電腦
將新 wireless 電腦加入網域最簡單的方法就是實際中附加一段有線的區域網路電腦 \（不受 802.1 X switch\ 區段）之前加入網域的電腦。 這是最簡單，因為 wireless 群組原則設定的自動立即套用，如果您有部署自己 PKI，電腦將會收到 CA 憑證，並將它放在受信任的根憑證授權單位憑證存放區，可讓 wireless client 信任 NPS 伺服器伺服器的憑證 CA 您所發行的。

同樣地，新 wireless 電腦已經加入網域之後，使用者來登入網域慣用的方法是使用有線的連接到網路上執行登入。

### <a name="other-domain-join-methods"></a>其他加入網域的方法
位置不實用加入網域的電腦使用有線乙太網路連結，或案例，使用者無法登入網域第一次使用有線的連結，您必須使用備用方法。

- **IT 人員電腦設定**。 Wireless 電腦加入網域 IT 人員的成員，並設定單一登入開機 wireless 設定檔。 使用此方法，IT 系統管理員 wireless 電腦連接到有線乙太網路，並加入網域的電腦。 然後系統管理員散發給使用者的電腦。 當使用者開始，而不使用有線的連結，以手動方式使用者登入指定網域認證的電腦是用來兩連接 wireless 網路並登入網域。

如需詳細資訊，請查看區段[使用 IT 人員的電腦設定方法加入的網域和登入](#bkmk_itstaff)

-   **啟動使用者設定檔 Wireless 設定**。 使用者手動設定 wireless 電腦開機 wireless 設定檔，並加入網域，根據 IT 系統管理員取得的指示操作。 Wireless 開機設定檔，可讓使用者加入網域，並將該名 wireless 連接。 加入網域的電腦，開機之後使用者可以登入網域使用 wireless 連接與他們的網域 account 認證。

如需詳細資訊，請查看區段[使用使用者開機無線設定檔設定加入的網域和登入](#bkmk_userbootstrap)。

### <a name="bkmk_itstaff"></a>藉由 IT 人員的電腦設定方法加入的網域和登入
網域成員 domain\ 加入 wireless client 電腦的使用者可以使用 wireless 暫時設定檔來連接 802.1X\-不需要第一次都連接到有線的區域網路驗證 wireless 網路。 這個暫時 wireless 設定檔稱為*啟動 wireless 設定檔*。

使用者必須手動指定它們 domain 使用者 account 認證，開機 wireless 設定檔，並不驗證執行的網路原則伺服器 \(NPS\) 遠端驗證 Dial\-In 使用者服務 \(RADIUS\) 伺服器的憑證。

建立 wireless 連接之後，套用群組原則 wireless client 在電腦上，並自動發行新的 wireless 設定檔。 使用的電腦和使用者 account 認證 client 驗證新原則。 

此外，部分 PEAP\-MS\-CHAP v2 互加好友驗證使用新的設定檔開機設定檔，而 client 驗證 RADIUS 伺服器的憑證。

加入網域的電腦之後，請散布 domain\ 成員使用者 wireless 電腦之前，請先設定單一登入開機 wireless 設定檔，使用此程序。

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>若要設定的單一登入開機 wireless 設定檔

1. 建立開機設定檔名為本指南使用的程序[設定為 PEAP\-MS\-CHAP v2 無線連接設定檔](#bkmk_configureprofile)，使用下列設定：

    - PEAP\-MS\-CHAP v2 驗證

    - 驗證 RADIUS 伺服器的憑證已停用

    - 單一的登入功能

2. 在屬性無線的網路原則中的新開機設定檔建立的**一般**索引標籤，選取開機設定檔，然後按一下 [**匯出**網路共用匯出個人檔案、USB 快閃磁碟機或其他輕鬆存取的位置。 *.Xml 檔案的位置，指定為已儲存的設定檔。

3. Wireless 新電腦加入網域 \ (例如，透過在毋須 IEEE 802.1 乙太網路連接 X authentication\) 並新增到電腦開機 wireless 設定檔，使用**netsh wlan 新增設定檔**命令。

    >[!NOTE]
    >如需詳細資訊，請查看 Netsh 命令的區域網路無線 \(WLAN\) 在[http:///\/technet.microsoft.com\/library\/dd744890.aspx](https://technet.microsoft.com/library/dd744890)。

4. 散發的新 wireless 電腦給使用者的程序「登入網域使用執行 Windows 10 的電腦」。

當使用者開始電腦時，Windows 會提示輸入網域 account 的使用者名稱與密碼的使用者。 因為支援單一登入時，電腦使用網域使用者 account 認證要先使用 wireless 網路，然後登入網域連接。

#### <a name="bkmk_w10"></a>登入以使用執行 Windows 10 電腦的網域

1. 登入電腦，或重新開機。

2. 請按任意鍵，鍵盤上或在桌面上按一下。 登入畫面會顯示使用者的本機 account 名稱與密碼項目欄位名稱下方的顯示。 不要使用 [本機使用者 account 登入。

3. 中的畫面左下角，按一下**以其他使用者**。 其他使用者登入畫面出現兩個欄位，一的使用者名稱和密碼。 以下密碼欄位會是文字**登入：**和位置電腦所加入的網域名稱。 例如，如果您的網域名稱為 example.com，文字顯示 [**登入：範例**。

4. 在**的使用者名稱**，輸入您的使用者網域名稱。

5. 在**密碼**、輸入您的網域密碼，然後按一下箭頭，或按下 ENTER。

>[!NOTE]
>如果**其他使用者**畫面中不包含文字**登入：**和您的網域名稱，您必須輸入您的使用者名稱的格式*網域 \\ 使用者*。 來登入網域 example.com 名帳號，例如**使用者 \-01**，輸入**example\\User\-01**。

### <a name="bkmk_userbootstrap"></a>使用使用者開機無線設定檔設定加入的網域和登入
使用此方法，您完成一般步驟一節中的步驟，然後您 domain\ 成員使用者提供有關如何手動設定使用開機 wireless 設定檔的 wireless 電腦的指示進行。 Wireless 開機設定檔，可讓使用者加入網域，並將該名 wireless 連接。 加入網域的電腦並重新啟動之後，使用者可以透過 wireless 連接網域登入。

#### <a name="general-steps"></a>一般步驟

1. 設定本機電腦的系統管理員帳號，在**[控制台]**，為使用者。

    >[!IMPORTANT]
    >若要將電腦加入網域，使用者必須登入本機電腦。 或者，使用者必須提供認證的本機電腦加入網域的程序期間。 此外，使用者必須使用者帳號使用者想要將電腦加入的網域中。 在的電腦加入網域過程中，將會提示使用者網域 account 認證 \（使用者名稱和 password\）。

2. 您的網域使用者提供的指示來設定開機 wireless 設定檔，如下列程序中所述**設定開機 wireless 設定檔以**。
3. 此外，為使用者提供這兩個本機電腦的認證 \（使用者名稱和 password\），和網域認證 \（網域 account 的使用者名稱和 password\）在表單中*DomainName\\UserName*，以及」將電腦加入網域，」的程序與「登入網域」與 Windows Server 2016 中的文件[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>將開機 wireless 設定檔

1. 使用您的網路管理員或 IT 支援所提供的認證專業登入本機電腦的系統管理員 account 的電腦。

2. Right\ 按一下網路圖示桌面，然後按一下**開放式網路和共用中心]**。 **網路和共用中心]**開啟。 在**變更您的網路設定**，按一下 [**設定的新連接或網路**。 **設定連接或網路**對話方塊。

3. 按一下**以手動方式連接 wireless 網路**，然後按一下 [**下一步**。

4. 在**以手動方式連接 wireless 網路**，請在**的網路名稱**，輸入 SSID AP 的名稱。  

5. 在**安全性類型**，選取您的系統管理員所提供的設定。

6. 在**加密類型**和**的安全性金鑰**、選取或輸入您的系統管理員所提供的設定。

7. 選取 [**開始這個連接自動**，然後按一下 [**下一步**。

8. 在 **成功新增 * * * 您的網路 SSID*，按一下 [**設定連接**。

9. 按一下**設定連接**。 *您的網路 SSID*無線網路屬性]。

10. 按一下**安全性**索引標籤，然後在**選擇網路驗證方法**，選取**受保護的 EAP \(PEAP\)**。

11. 按一下**設定**。 **受保護的 EAP \(PEAP\) 屬性**認知。

12. 在**受保護的 EAP \(PEAP\) 屬性**頁面上時，請確定**伺服器驗證憑證]**是未選取，按一下 [ **[確定]**兩次，，然後按一下 [**關閉**。

13. Windows 會再嘗試連接 wireless 網路。 設定的設定檔開機 wireless 指定，您必須提供您的網域認證。 Windows 會提示您輸入系統 account 名稱和密碼時, 輸入您的網域 account 認證，如下所示：*網域 Name\\User 名稱*，*網域密碼*。

##### <a name="to-join-a-computer-to-the-domain"></a>若要加入網域的電腦

1. 登入本機電腦。

2. 在 [搜尋] 方塊中，輸入**PowerShell**。 在搜尋結果中，以滑鼠右鍵按一下**Windows PowerShell**，然後按**以系統管理員身分執行**。 Windows PowerShell 開啟提升權限提示。

3. 在 Windows PowerShell 中，輸入下列命令，，然後按 ENTER 鍵。 請確定您想要加入的網域名稱取代變數的網域名稱。
    
    Add-Computer 網域名稱
    
4. 出現提示時，輸入您的網域使用者名稱和密碼，然後按一下**[確定]**。
5. 電腦重新開機。
6. 上一節中的指示，請依照下列[登入執行 Windows 10 電腦網域](#bkmk_w10)。