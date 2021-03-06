---
title: 設定網路原則
description: 本主題提供 Windows Server 2016 中網路原則伺服器的網路原則設定總覽。
manager: brianlic
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3b14d290a1bf80b0f72abb2b2043be2e5e53516a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946684"
---
# <a name="configure-network-policies"></a>設定網路原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題在 NPS 中設定網路原則。

## <a name="add-a-network-policy"></a>新增網路原則

網路原則伺服器 \( NPS \) 會使用網路原則和使用者帳戶的撥入屬性來判斷連線要求是否已獲授權連接到網路。

您可以使用此程式，在 NPS 主控台或遠端存取主控台中設定新的網路原則。

### <a name="performing-authorization"></a>執行授權

當 NPS 執行連線要求的授權時，會將要求與已排序的原則清單中的每個網路原則做比較，從第一個原則開始，依序往下比較已設定原則的清單。 如果 NPS 找到其條件符合連線要求的原則，NPS 會使用該使用者帳戶的比對原則和撥入屬性來執行授權。 如果使用者帳戶的撥入內容是設定為透過網路原則來授與存取權或控制存取權，而連線要求已獲得授權，NPS 就會將網路原則中設定的設定套用到連線。

如果 NPS 找不到符合連線要求的網路原則，就會拒絕連線要求，除非使用者帳戶的撥入內容設定為授與存取權。

如果使用者帳戶的撥入內容是設定為拒絕存取，NPS 就會拒絕連線要求。

### <a name="key-settings"></a>主要設定

當您使用 [新增網路原則嚮導] 建立網路原則時，您在 [ **網路連線方法** ] 中指定的值會用來自動設定 **原則類型** 條件：

- 如果您將預設值保留為 [未指定]，NPS 就會針對所有使用網路存取伺服器 (NAS) 的網路連線類型，評估您所建立的網路原則。
- 如果您指定網路連線方法，則只有在連線要求來自您指定的網路存取伺服器類型時，NPS 才會評估網路原則。

在 [ **存取權限** ] 頁面上，如果您想要允許使用者連線到您的網路，則必須選取 [ **授與存取權** ]。 如果您想要讓原則防止使用者連接到您的網路，請選取 [ **拒絕存取**]。

如果您想要在 Active Directory 網域服務 AD DS 的使用者帳戶撥入內容中決定存取權限 &reg; \( \) ，您可以選取 [ **存取是由使用者撥入屬性決定** ] 核取方塊。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

### <a name="to-add-a-network-policy"></a>新增網路原則

1. 開啟 NPS 主控台，然後按兩下 [ **原則**]。

2. 在主控台樹中，以滑鼠右鍵按一下 [ **網路原則**]，然後按一下 [ **新增**]。 此時會開啟新增網路原則精靈。

3. 使用新增網路原則精靈來建立原則。

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>使用 Wizard 建立撥號或 VPN 的網路原則

您可以使用此程式來建立連線要求原則和網路原則，以將撥號伺服器或虛擬私人網路 VPN 伺服器部署為 \( \) \( \) NPS radius 伺服器遠端驗證撥入消費者服務 RADIUS 用戶端。

>[!NOTE]
>用戶端電腦（例如，膝上型電腦和其他執行用戶端作業系統的電腦）不是 RADIUS 用戶端。 RADIUS 用戶端是網路存取伺服器（例如無線存取點、802.1 X 驗證交換器、虛擬私人網路 \( VPN \) 伺服器和撥號伺服器），因為這些裝置會使用 radius 通訊協定與 radius 伺服器（例如 NPSs）進行通訊。

此程式說明如何在 NPS 中開啟新的撥號或虛擬私人網路連線 wizard。

在執行精靈之後，會建立下列原則：

- 一個連線要求原則
- 一個網路原則

您可以在每次需要為撥號伺服器與 VPN 伺服器建立新原則時，執行新增撥號或虛擬私人網路連線精靈。

執行新的撥號或虛擬私人網路連線嚮導，不是將撥號或 VPN 伺服器做為 RADIUS 用戶端部署到 NPS 的唯一必要步驟。 這兩個網路存取方法都需要您部署其他的硬體與軟體元件。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>使用精靈建立撥號或 VPN 原則

1. 開啟 NPS 主控台。 如果尚未選取，請按一下 [ **NPS \( 本機 \)**]。 如果您想要在遠端 NPS 上建立原則，請選取伺服器。

2. 在 [ **消費者入門** ] 和 [ **標準**] 設定中，選取 [ **撥號或 VPN 連線的 RADIUS 伺服器**]。 文字與文字底下的連結會變更以反映選取。

3. 按一下 [ **設定 VPN] 或 [使用 Wizard 撥號]**。 此時會開啟新增撥號或虛擬私人網路連線精靈。

4. 遵循嚮導中的指示，完成新原則的建立。

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>使用 Wizard 建立 802.1 X 有線或無線的網路原則

您可以使用此程式來建立連線要求原則和網路原則，以將 802.1 X 驗證交換器或 802.1 X 無線存取點部署為遠端驗證撥入消費者服務 (RADIUS) 用戶端加入 NPS RADIUS 伺服器。

此程式說明如何在 NPS 中啟動新的 IEEE 802.1 X Secure 有線和無線連線嚮導。

在執行精靈之後，會建立下列原則：

- 一個連線要求原則
- 一個網路原則

您可以在每次需要建立 802.1X 存取的新原則時，執行 [新增 IEEE 802.1X 的安全有線及無線連線] 精靈。

執行 [新增 IEEE 802.1 X 安全有線及無線連線] 嚮導不是將 802.1 X 驗證交換器和無線存取點作為 RADIUS 用戶端部署至 NPS 的唯一必要步驟。 這兩個網路存取方法都需要您部署其他的硬體與軟體元件。

至少要有 **Domain Admins** 的成員資格或是對等成員資格，才能完成此程序。

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>使用精靈建立 802.1X 有線或無線的原則

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 如果尚未選取，請按一下 [ **NPS \( 本機 \)**]。 如果您想要在遠端 NPS 上建立原則，請選取伺服器。

3. 在 [ **消費者入門** ] 和 [ **標準**] 設定中，選取 [ **802.1 x 無線或有線連線的 RADIUS 伺服器**]。 文字與文字底下的連結會變更以反映選取。

4. 按一下 [ **使用 Wizard 設定 802.1 x]**。 此時會開啟 [新增 IEEE 802.1X 的安全有線及無線連線] 精靈。

5. 遵循嚮導中的指示，完成新原則的建立。

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>設定 NPS 略過使用者帳戶撥入屬性

您可以使用此程式來設定 NPS 網路原則，以在授權程式期間忽略 Active Directory 中使用者帳戶的撥入屬性。 除非使用者帳戶的 [ **網路存取權限** ] 屬性設定為 [ **透過 NPS 網路原則控制存取**]，否則 Active Directory 消費者和電腦中的使用者帳戶會有 NPS 在授權程式期間評估的撥入屬性。

在兩種情況下，您可能會想要將 NPS 設定為忽略 Active Directory 中使用者帳戶的撥入屬性：

- 當您想要使用網路原則簡化 NPS 授權時，但並非所有的使用者帳戶都將 [ **網路存取權限** ] 屬性設定為 [ **透過 NPS 網路原則控制存取**]。 例如，某些使用者帳戶可能會將使用者帳戶的 [ **網路存取權限** ] 屬性設定為 [ **拒絕存取** ] 或 [ **允許存取**]。

- 當使用者帳戶的其他撥入屬性不適用於網路原則中設定的連線類型時。 例如， **網路存取權限** 設定以外的屬性僅適用于撥入或 VPN 連線，但您所建立的網路原則是用於無線或驗證交換器連線。

您可以使用此程式，將 NPS 設定為略過使用者帳戶的撥入屬性。 如果連線要求符合選取此核取方塊的網路原則，NPS 就不會使用使用者帳戶的撥入內容來判斷使用者或電腦是否有權存取網路。只有網路原則中的設定會用來決定授權。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 按兩下 [ **原則**]，按一下 [ **網路原則**]，然後在詳細資料窗格中，按兩下您要設定的原則。

3. 在 **[原則內容** ] 對話方塊的 [ **總覽** ] 索引標籤的 [ **存取權限**] 中，選取 [ **略過使用者帳戶撥入** 內容] 核取方塊，然後按一下 **[確定]**。

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>設定 NPS 略過使用者帳戶撥入屬性



## <a name="configure-nps-for-vlans"></a>設定 Vlan 的 NPS

藉由在 Windows Server 2016 中使用 VLAN 感知的網路存取伺服器和 NPS，您可以提供使用者群組只能存取其安全性許可權所適用的網路資源。 例如，您可以為訪客提供網際網路的無線存取，而不允許他們存取您的組織網路。

此外，Vlan 也可讓您以邏輯方式將存在於不同實體位置或不同實體子網的網路資源分組。 例如，銷售部門的成員及其網路資源（例如用戶端電腦、伺服器和印表機）可能位於您組織中的數個不同大樓，但您可以將這些資源全部放在一個使用相同 IP 位址範圍的 VLAN 上。 然後 VLAN 會以單一子網的形式，從使用者的觀點來看。

當您想要在不同的使用者群組之間隔離網路時，也可以使用 Vlan。 決定您要如何定義群組之後，您可以在 Active Directory 消費者和電腦嵌入式管理單元中建立安全性群組，然後將成員新增至群組。

### <a name="configure-a-network-policy-for-vlans"></a>設定 Vlan 的網路原則

您可以使用這個程式來設定網路原則，以將使用者指派給 VLAN。 當您使用可感知 VLAN 的網路硬體（例如路由器、交換器和存取控制器）時，您可以設定網路原則，以指示存取伺服器將特定 Active Directory 群組的成員放在特定的 Vlan 上。 以邏輯方式將網路資源群組在 Vlan 上的功能，可在設計和執行網路解決方案時提供彈性。

當您設定要搭配 Vlan 使用的 NPS 網路原則設定時，您必須設定通道 **-中型類型**、通道 **-PVT-群組識別碼**、通道 **類型** 和通道 **標記** 的屬性。

此程式是以指導方針的形式提供;您的網路設定可能需要與下面所述不同的設定。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-configure-a-network-policy-for-vlans"></a>設定 Vlan 的網路原則

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 按兩下 [ **原則**]，按一下 [ **網路原則**]，然後在詳細資料窗格中，按兩下您要設定的原則。

3. 在 [原則 **屬性** ] 對話方塊中，按一下 [ **設定** ] 索引標籤。

4. 在 [原則 **屬性**] 的 [ **設定**] 的 [ **RADIUS 屬性**] 中，確定已選取 [ **標準** ]。

5. 在詳細資料窗格的 [ **屬性**] 中，[ **服務類型** ] 屬性會設定為 [ **框架**] 的預設值。 根據預設，對於具有 VPN 和撥號存取方法的原則， **框架通訊協定** 屬性會設定為 [ **PPP**] 的值。 若要指定 Vlan 所需的其他連接屬性，請按一下 [ **新增**]。 [ **新增標準 RADIUS 屬性** ] 對話方塊隨即開啟。

6. 在 [ **新增標準 RADIUS 屬性**] 的 [屬性] 中，向下滾動至並新增下列屬性：

    - 通道 **-中類型**。 選取適合您為原則所做的先前選取專案的值。 例如，如果您要設定的網路原則是無線原則，請選取 [ **值： 802 (包含所有802媒體 Plus Ethernet 標準格式])**。

    - 通道 **-Pvt-群組識別碼**。 輸入代表將指派給群組成員的 VLAN 編號的整數。

    - 通道 **類型**。 選取 [ **虛擬 lan] (VLAN)**。


7. 在 [ **新增標準 RADIUS] 屬性** 中，按一下 [ **關閉**]。

8. 如果您的網路存取伺服器 (NAS) 需要使用通道 **標記** 屬性，請使用下列步驟將通道卷 **標屬性新增** 至網路原則。 如果您的 NAS 檔未提及此屬性，請不要將它新增至原則。 如有需要，請新增屬性，如下所示：

    - 在 [原則 **屬性**] 的 [ **設定**] 的 [ **RADIUS 屬性**] 中，按一下 [ **廠商特定**]。

    - 在詳細資料窗格中，按一下 [ **新增**]。 [ **新增廠商專屬屬性** ] 對話方塊隨即開啟。

    - 在 [ **屬性**] 中，向下滾動至並選取 [通道 **-標記**]，然後按一下 [ **新增**]。 [ **屬性資訊** ] 對話方塊隨即開啟。

    - 在 [ **屬性值**] 中，輸入您從硬體檔中取得的值。

## <a name="configure-the-eap-payload-size"></a>設定 EAP 承載大小

在某些情況下，路由器或防火牆會捨棄封包，因為它們已設定為捨棄需要片段的封包。

當您使用可延伸驗證通訊協定 \( EAP （ \) 具有傳輸層安全性 \( TLS \) ，或 eap-tls）作為驗證方法的網路原則部署 NPS 時，nps 針對 EAP 承載使用的預設最大傳輸單位 \( MTU \) 為1500個位元組。

此 EAP 承載的大小上限可以建立 RADIUS 訊息，要求 NPS 與 RADIUS 用戶端之間的路由器或防火牆分散。 如果是這種情況，則在 RADIUS 用戶端與 NPS 之間的路由器或防火牆可能會以無訊息方式捨棄某些片段，而導致驗證失敗，以及存取用戶端無法連線到網路。

使用下列程式，將網路原則中的框架-MTU 屬性調整為不大於1344的值，以降低 NPS 針對 EAP 承載使用的大小上限。

若要完成此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-configure-the-framed-mtu-attribute"></a>設定框架-MTU 屬性

1. 在 NPS 的伺服器管理員中，按一下 [ **工具**]，然後按一下 [ **網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 按兩下 [ **原則**]，按一下 [ **網路原則**]，然後在詳細資料窗格中，按兩下您要設定的原則。

3. 在 [原則 **屬性** ] 對話方塊中，按一下 [ **設定** ] 索引標籤。

4. 在 [ **設定**] 的 [ **RADIUS 屬性**] 中，按一下 [ **標準**]。 在詳細資料窗格中，按一下 [ **新增**]。 [ **新增標準 RADIUS 屬性** ] 對話方塊隨即開啟。

5. 在 [ **屬性**] 中，向下滾動至，然後按一下 [ **框架-MTU**]，再按一下 [ **新增**]。 [ **屬性資訊** ] 對話方塊隨即開啟。

6. 在 [ **屬性值**] 中，輸入等於或小於 **1344** 的值。 按一下 **[確定]**，按一下 [ **關閉**]，然後按一下 **[確定]**。



如需網路原則的詳細資訊，請參閱 [網路原則](nps-np-overview.md)。

如需模式比對語法指定網路原則屬性的範例，請參閱 [在 NPS 中使用正則運算式](nps-crp-reg-expressions.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。
