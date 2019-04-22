---
title: 無線存取部署概觀
description: 本主題是 Windows Server 2016 網路功能指南 」 部署密碼型的 802.1 X 驗證無線存取 」 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6658c4750ba2f71b24acd4f7da02029da63179bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818919"
---
# <a name="wireless-access-deployment-overview"></a>無線存取部署概觀

>適用於：Windows Server （半年通道），Windows Server 2016

下圖顯示的元件，才能部署 802.1x 驗證與 PEAP 搭配使用的無線存取\-MS\-MS-CHAP v2。  

![802.1x 部署基礎結構概觀](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>無線存取部署元件
下列的基礎結構是為了此無線存取部署：

### <a name="8021x-capable-wireless-access-points"></a>802.1x\-支援的無線存取點
支援無線區域網路所需的網路基礎結構服務已就緒之後，您就可以開始設計程序，無線 Ap 的位置。 無線 AP 部署設計程序包含下列步驟：

- 識別為無線使用者的涵蓋範圍的區域。 識別的區域的涵蓋範圍，務必找出您是否想要提供無線服務外部建置，而如果是的話，判斷特別是那些外部區域的位置。

- 判斷多少的無線 Ap，以部署以確保有足夠的涵蓋範圍。

- 決定要放置無線 Ap 的位置。

- 選取無線 Ap 的通道頻率。

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
無線存取部署需要下列項目，AD ds。

#### <a name="users-and-computers"></a>使用者和電腦

使用 Active Directory 使用者和電腦嵌入式管理單元\-中建立及管理使用者帳戶，以及建立無線網路安全性的群組，其中包含您要授與無線存取每個網域成員。

#### <a name="wireless-network-ieee-80211-policies"></a>無線網路\(IEEE 802.11\)原則

您可以使用無線網路\(IEEE 802.11\)原則延伸的群組原則管理，以設定無線電腦嘗試存取網路時所套用的原則。

在群組原則管理編輯器中，當您以滑鼠右鍵\-按一下 **無線網路\(IEEE 802.11\)原則**，您有下列兩個選項，如您所建立的無線原則的類型。

- **建立新的無線網路原則，適用於 Windows Vista 和更新版本**

- **建立新的 Windows XP 原則**

>[!TIP]
>設定新的無線網路原則時，您可以選擇變更名稱和原則的描述。 如果您變更原則的名稱，變更會反映在**詳細資料**窗格的 群組原則管理編輯器和無線網路的 原則 對話方塊的標題列上。 不論您如何重新命名您的原則，新的 XP 無線原則會一律列在群組原則管理編輯器與**型別**顯示**XP**。 其他原則會列出**型別**顯示**Vista 和更新的版本**。  

無線網路原則適用於 Windows Vista 和更新的版本可讓您設定、 設定優先順序，以及管理多個無線設定檔。 無線設定檔是連線能力和用來連線到特定的無線網路的安全性設定的集合。 當無線用戶端電腦更新群組原則時，您建立無線網路原則中的設定檔會自動新增至您要套用無線網路原則的無線用戶端電腦上的設定。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>允許多個無線網路連線

如果您有貴組織中的實體位置間移動的無線用戶端，例如總公司與分公司辦公室，您可能想電腦連線到一個以上的無線網路。 在此情況下，您可以設定包含特定的連線和安全性設定，每個網路的無線設定檔。

例如，假設您的公司有一個無線網路，為主要的公司辦公室，與服務組識別元\(SSID\) WlanCorp。

您的分公司也有您也要連接的無線網路。 分公司已設定為 WlanBranch SSID。

在此案例中，您可以設定每個網路，以及電腦或其他裝置，可在公司辦公室的設定檔，所以它們實際上是在範圍內的網路涵蓋範圍時，將可以連線到其中一個無線網路的分公司。

##### <a name="mixed-mode-wireless-networks"></a>混合\-模式之無線網路

或者，假設您的網路有混合的無線電腦和裝置支援不同的安全性標準的裝置。 某些較舊的電腦可能有只能使用 WPA 的無線介面卡\-Enterprise，而較新的裝置可以使用更強的 WPA2\-企業標準。

您可以建立兩個不同的設定檔使用相同的 SSID 和幾乎完全相同的連線和安全性設定。

在一個設定檔，您可以設定無線驗證至 WPA2\-Enterprise 與 AES，和其他設定檔中，您就可以指定 WPA\-Enterprise 含 [tkip]。

這通常就是為混合\-模式部署，並能讓不同的型別及共用相同的無線網路的無線功能的電腦。

### <a name="network-policy-server-nps"></a>網路原則伺服器\(NPS\)
NPS 可讓您建立和強制執行連線要求驗證和授權的網路存取原則。

當您使用 NPS 做為 RADIUS 伺服器時，您會設定為 NPS 中的 RADIUS 用戶端的網路存取伺服器，例如無線存取點。 您也可以設定 NPS 用來驗證存取用戶端和授權連線要求的網路原則。  

### <a name="wireless-client-computers"></a>無線用戶端電腦
為了本指南中，無線用戶端電腦會是電腦與其他裝置，配備 IEEE 802.11 無線網路介面卡，以及執行 Windows 用戶端或 Windows Server 作業系統。

#### <a name="server-computers-as-wireless-clients"></a>為無線用戶端的伺服器電腦

根據預設，802.11 無線功能已停用執行 Windows Server 的電腦上。

若要啟用執行伺服器作業系統的電腦上的無線連線，您必須安裝並啟用無線區域網路\(WLAN\)使用可能是 Windows PowerShell 或 [新增角色及功能精靈] 在伺服器中的服務功能管理員。

當您安裝**無線區域網路服務**功能，新的服務**WLAN 自動設定**安裝在**Services**。 安裝完成時，您必須重新啟動伺服器。

重新啟動伺服器之後，您就可以存取 WLAN 自動設定，當您按一下 **開始**， **Windows 系統管理工具**，並**Services**。

安裝和伺服器重新啟動，WLAN 自動設定服務處於停止狀態的啟動類型的後**自動**。 若要啟動服務，按兩下**WLAN 自動設定**。 在上**一般**索引標籤上，按一下**開始**，然後按一下 **確定**。

WLAN 自動設定服務列舉無線介面卡，並管理無線連線與無線設定檔包含設定伺服器連線到無線網路所需的設定。

如需無線存取部署的概觀，請參閱 <<c0> [ 無線存取部署程序](c-wireless-access-deploy-process.md)。
