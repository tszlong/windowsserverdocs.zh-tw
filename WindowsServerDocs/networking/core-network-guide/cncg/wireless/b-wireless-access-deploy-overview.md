---
title: 無線存取部署概觀
description: 本主題是 Windows Server 2016 網路指南「部署 Password-Based 802.1 X 驗證無線存取」的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: eross-msft
ms.author: lizross
ms.date: 08/07/2020
ms.openlocfilehash: 336f8af276e14d0f54702f04539f041cf1c71400
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97943270"
---
# <a name="wireless-access-deployment-overview"></a>無線存取部署概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

下圖顯示使用 PEAP \- MS CHAP v2 部署 802.1 x 驗證無線存取所需的元件 \- 。

![802.1 x 部署基礎結構總覽](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>無線存取部署元件
此無線存取部署需要下列基礎結構：

### <a name="8021x-capable-wireless-access-points"></a>802.1 x \- 功能的無線存取點
在支援無線區域網路的必要網路基礎結構服務就緒之後，您就可以開始進行無線 Ap 位置的設計程式。 無線 AP 部署設計程式涉及下列步驟：

- 識別無線使用者涵蓋範圍的區域。 找出涵蓋範圍時，請務必識別您是否要在大樓之外提供無線服務，如果是，請明確地判斷這些外部區域的位置。

- 決定要部署多少個無線 Ap，以確保有足夠的涵蓋範圍。

- 決定要放置無線 Ap 的位置。

- 選取無線 Ap 的通道頻率。

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
無線存取部署需要 AD DS 的下列元素。

#### <a name="users-and-computers"></a>使用者和電腦

使用 Active Directory 消費者和電腦的嵌入式管理單元 \- 來建立和管理使用者帳戶，以及建立包含您要授與無線存取權的每個網域成員的無線安全性群組。

#### <a name="wireless-network-ieee-80211-policies"></a>無線網路 \( IEEE 802.11 \) 原則

您可以使用群組原則管理的無線網路 \( IEEE 802.11 \) 原則延伸，設定在無線電腦嘗試存取網路時套用的原則。

在群組原則管理編輯器中，當您以滑鼠右鍵 \- 按一下 [ **無線網路 \( IEEE 802.11 \) 原則**] 時，會有下列兩個選項可供您建立的無線原則類型使用。

- **建立適用于 Windows Vista 和更新版本的新無線網路原則**

- **建立新的 Windows XP 原則**

>[!TIP]
>設定新的無線網路原則時，您可以選擇變更原則的名稱和描述。 如果您變更原則的名稱，則變更會反映在群組原則管理編輯器的 **詳細資料** 窗格和 [無線網路原則] 對話方塊的標題列上。 無論您如何重新命名原則，新的 XP 無線原則一律會列在 [群組原則管理編輯器中，並顯示 **xp** 的 **類型**。 其他原則會以顯示 **Vista 和更新** 版本的 **類型** 列出。

適用于 Windows Vista 和更新版本的無線網路原則可讓您設定、設定優先順序，以及管理多個無線設定檔。 無線設定檔是連線和安全性設定的集合，可用來連接到特定的無線網路。 當您在無線用戶端電腦上更新群組原則時，您在無線網路原則中建立的設定檔會自動新增至無線網路原則所套用的無線用戶端電腦上的設定。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>允許連接至多個無線網路

如果您的無線用戶端是在組織中的實體位置（例如總公司與分公司之間）移動，您可能會想要讓電腦連接到多個無線網路。 在此情況下，您可以設定無線設定檔，其中包含每個網路的特定連線能力和安全性設定。

例如，假設您的公司有一個適用于總公司公司的無線網路，並具有服務組識別元 \( SSID \) WlanCorp。

您的分公司也有您要連接的無線網路。 分公司具有設定為 WlanBranch 的 SSID。

在此案例中，您可以為每個網路設定設定檔，而在公司辦公室和分公司使用的電腦或其他裝置，可以在實體于網路涵蓋範圍區域的範圍內時，連接到其中一個無線網路。

##### <a name="mixed-mode-wireless-networks"></a>混合 \- 模式無線網路

或者，假設您的網路具有混合的無線電腦和裝置，以支援不同的安全性標準。 某些較舊的電腦可能會有只能使用 WPA enterprise 的無線介面卡 \- ，而較新的裝置可以使用更強的 WPA2 \- enterprise 標準。

您可以建立兩個不同的設定檔，使用相同的 SSID 和幾乎相同的連線能力和安全性設定。

在一個設定檔中，您可以使用 AES 將無線驗證設定為 WPA2 \- enterprise，而在另一個設定檔中，您可以 \- 使用 TKIP 指定 WPA enterprise。

這通常稱為混合 \- 模式部署，可讓不同類型和無線功能的電腦共用相同的無線網路。

### <a name="network-policy-server-nps"></a>網路原則伺服器 \( NPS\)
NPS 可讓您建立和強制執行連線要求驗證和授權的網路存取原則。

當您使用 NPS 做為 RADIUS 伺服器時，您會將網路存取伺服器（例如無線存取點）設定為 NPS 中的 RADIUS 用戶端。 您也可以設定 NPS 用來驗證存取用戶端的網路原則，並授權其連接要求。

### <a name="wireless-client-computers"></a>無線用戶端電腦
基於本指南的目的，無線用戶端電腦是配備 IEEE 802.11 無線網路介面卡，且執行 Windows 用戶端或 Windows Server 作業系統的電腦和其他裝置。

#### <a name="server-computers-as-wireless-clients"></a>作為無線用戶端的伺服器電腦

預設會在執行 Windows Server 的電腦上停用802.11 無線功能。

若要在執行伺服器作業系統的電腦上啟用無線連線能力，您必須 \( \) 使用 Windows PowerShell 或伺服器管理員中的 [新增角色及功能]，來安裝和啟用無線區域網路 WLAN 服務功能。

當您安裝 **無線局域網路服務** 功能時，會在 **服務** 中安裝新的 Service **WLAN** 自動設定。 當安裝完成時，您必須重新開機伺服器。

重新開機伺服器之後，您可以在按一下 [ **啟動**]、[Windows 系統 **管理工具**] 和 [ **服務**] 時存取 WLAN 自動設定。

安裝和伺服器重新開機之後，WLAN 自動設定服務會處於 [已停止] 狀態，且啟動類型為 [ **自動**]。 若要啟動服務，請按兩下 [ **wlan** 自動設定]。 在 [ **一般** ] 索引標籤上，按一下 [ **開始**]，然後按一下 **[確定]**。

WLAN 自動設定服務會列舉無線介面卡，以及管理無線連線和無線設定檔，其中包含設定伺服器以連線到無線網路所需的設定。

如需無線存取部署的總覽，請參閱 [無線存取部署流程](c-wireless-access-deploy-process.md)。
