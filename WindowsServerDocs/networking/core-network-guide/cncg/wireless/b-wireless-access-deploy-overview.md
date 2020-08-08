---
title: 無線存取部署概觀
description: 本主題是 Windows Server 2016 網路指南「部署以密碼為基礎的 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: eross-msft
ms.author: lizross
ms.openlocfilehash: ab135c7a30a1930fef58fae357a38510eab711eb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969615"
---
# <a name="wireless-access-deployment-overview"></a>無線存取部署概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

下圖顯示使用 PEAP \- MS CHAP v2 部署 802.1 x 驗證無線存取所需的元件 \- 。

![802.1 x 部署基礎結構總覽](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>無線存取部署元件
此無線存取部署需要下列基礎結構：

### <a name="8021x-capable-wireless-access-points"></a>802.1 x \- 支援的無線存取點
在支援無線區域網路的必要網路基礎結構服務就緒之後，您就可以開始設計無線 Ap 位置的程式。 無線 AP 部署設計程式牽涉到下列步驟：

- 識別無線使用者涵蓋範圍的區域。 識別涵蓋範圍的區域時，請務必識別您是否要在大樓外提供無線服務，如果是的話，請特別判斷這些外部區域的所在位置。

- 決定要部署多少個無線 Ap，以確保有足夠的涵蓋範圍。

- 決定無線 Ap 的放置位置。

- 選取無線 Ap 的通道頻率。

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
以下是無線存取部署所需的 AD DS 元素。

#### <a name="users-and-computers"></a>使用者和電腦

使用 [Active Directory 使用者和電腦] 嵌入式管理單元 \- 來建立和管理使用者帳戶，以及建立包含您要授與無線存取權之每個網域成員的無線安全性群組。

#### <a name="wireless-network-ieee-80211-policies"></a>無線網路 \( IEEE 802.11 \) 原則

您可以使用群組原則管理的無線網路 \( IEEE 802.11 \) 原則延伸，設定在無線電腦嘗試存取網路時套用的原則。

在群組原則管理編輯器中，當您以滑鼠右鍵 \- 按一下 [**無線網路 \( IEEE 802.11 \) 原則**] 時，您會有下列兩個選項可供您建立的無線原則類型使用。

- **為 Windows Vista 和更新版本建立新的無線網路原則**

- **建立新的 Windows XP 原則**

>[!TIP]
>設定新的無線網路原則時，您可以選擇變更原則的名稱和描述。 如果您變更原則的名稱，變更就會反映在 [群組原則管理編輯器的**詳細資料**窗格和 [無線網路原則] 對話方塊的標題列上。 無論您重新命名原則的方式為何，新的 XP 無線原則一律會列在 [群組原則管理編輯器，且顯示的**類型**為 [ **xp**]。 其他原則會以顯示**Vista 和更新**版本的**類型**列出。

適用于 Windows Vista 和更新版本的無線網路原則可讓您設定、排定優先順序，以及管理多個無線設定檔。 無線設定檔是用來連線到特定無線網路的連線能力和安全性設定集合。 當您的無線用戶端電腦上更新群組原則時，您在無線網路原則中建立的設定檔會自動新增至無線用戶端電腦上套用無線網路原則的設定。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>允許連接到多個無線網路

如果您的無線用戶端跨組織的實體位置（例如總公司與分公司之間）移動，您可能會想要讓電腦連接到多個無線網路。 在此情況下，您可以設定無線設定檔，其中包含每個網路的特定連線能力和安全性設定。

例如，假設您的公司有一個用於主要公司辦公室的無線網路，並具有服務集合識別碼 \( SSID \) WlanCorp。

您的分公司也有一個您也想要連接的無線網路。 分公司的 SSID 設定為 WlanBranch。

在此案例中，您可以設定每個網路的設定檔，而當公司辦公室和分公司所使用的電腦或其他裝置實際位於網路涵蓋範圍內時，可以連線到其中一個無線網路。

##### <a name="mixed-mode-wireless-networks"></a>混合 \- 模式無線網路

或者，假設您的網路有混合的無線電腦和支援不同安全性標準的裝置。 也許有些較舊的電腦具有只能使用 WPA enterprise 的無線介面卡 \- ，但較新的裝置可以使用更強的 WPA2 \- enterprise 標準。

您可以建立兩個不同的設定檔，使用相同的 SSID 和幾乎相同的連線能力和安全性設定。

在一個設定檔中，您可以使用 AES 將無線驗證設定為 WPA2 \- enterprise，而在另一個設定檔中，您可以 \- 使用 TKIP 指定 WPA enterprise。

這通常稱為混合 \- 模式部署，可讓不同類型和無線功能的電腦共用相同的無線網路。

### <a name="network-policy-server-nps"></a>網路原則伺服器 \( NPS\)
NPS 可讓您建立和強制執行連線要求驗證和授權的網路存取原則。

當您使用 NPS 做為 RADIUS 伺服器時，您會將網路存取伺服器（例如無線存取點）設定為 NPS 中的 RADIUS 用戶端。 您也可以設定 NPS 用來驗證存取用戶端及授權其連線要求的網路原則。

### <a name="wireless-client-computers"></a>無線用戶端電腦
基於本指南的目的，無線用戶端電腦是配備 IEEE 802.11 無線網路介面卡且執行 Windows 用戶端或 Windows Server 作業系統的電腦和其他裝置。

#### <a name="server-computers-as-wireless-clients"></a>做為無線用戶端的伺服器電腦

根據預設，執行 Windows Server 的電腦會停用802.11 無線功能。

若要在執行伺服器作業系統的電腦上啟用無線連線，您必須 \( \) 使用 Windows PowerShell 或伺服器管理員中的 [新增角色及功能]，安裝並啟用無線區域網路 WLAN 服務功能。

當您安裝**無線局域網路服務**功能時，會在**服務**中安裝新的服務**wlan**自動設定。 安裝完成時，您必須重新開機伺服器。

重新開機伺服器之後，您可以在按一下 [**啟動**]、[Windows 系統**管理工具**] 和 [**服務**] 時，存取 WLAN 自動設定。

安裝和伺服器重新開機之後，WLAN 自動設定服務會處於 [已停止] 狀態，且啟動類型為 [**自動**]。 若要啟動服務，請按兩下 [ **WLAN**自動設定]。 在 [**一般**] 索引標籤上，按一下 [**開始**]，然後按一下 **[確定]**。

WLAN 自動設定服務會列舉無線介面卡，並管理無線連線和無線設定檔，其中包含設定伺服器連線到無線網路所需的設定。

如需無線存取部署的總覽，請參閱[無線存取部署流程](c-wireless-access-deploy-process.md)。
