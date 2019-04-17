---
title: Wireless 存取部署概觀
description: 本主題是 Windows Server 2016 網路指南「部署密碼基礎 802.1 X 驗證無線存取」的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 11d87254d8819606a06acedd407bb2e09a45cb60
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-overview"></a>Wireless 存取部署概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

下圖顯示元件所需部署 802.1 X 驗證 PEAP\-MS\-CHAP v2 wireless 存取。  

![802.1 x 部署基礎結構概觀](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Wireless 存取部署元件
以下基礎結構是此 wireless 存取部署所需項目：

### <a name="8021x-capable-wireless-access-points"></a>802.1X\-能無線存取點
支援 wireless 區域網路所需的網路基礎結構服務會在位置之後，您就可以開始的位置，wireless Ap 設計程序。 Wireless AP 部署設計程序包含下列步驟：

- 找出 wireless 使用者涵蓋的範圍。 檢測軍人區域的涵蓋範圍時，請務必找出您是否想要提供的建築物、 外的電信業雖然，如果有的話判斷特定位置外部的區域。

- 判斷多少 wireless Ap，以確保適當的涵蓋範圍部署。

- 判斷放置 wireless Ap 的位置。

- 選取 wireless Ap 通道的頻率。

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
下列項目 AD ds 部署所需的 wireless 存取。

#### <a name="users-and-computers"></a>使用者和電腦

使用 Active Directory 使用者和電腦 snap\ 中建立及管理使用者帳號，並建立包含每個您要權限授與 wireless 的網域成員 wireless 安全性群組。

#### <a name="wireless-network-ieee-80211-policies"></a>無線網路 \ (IEEE 802.11\) 原則

您可以使用 Wireless 網路 \ (IEEE 802.11\) 原則設定原則套用到 wireless 電腦嘗試存取網路時，群組原則管理的擴充功能。

群組原則管理編輯器] 中，當您 right\ 按一下**Wireless 網路 \ (IEEE 802.11\) 原則**，您有下列兩個選項 wireless 原則您所建立的類型。

- **建立新的網路 Wireless 原則適用於 Windows Vista 和較新版本**

- **建立新的 Windows XP 原則**

>[!TIP]
>在設定新網路 wireless 原則時，您可以選擇變更名稱與原則的描述。 如果您變更原則的名稱，變更會反映在**的詳細資料**窗格中的群組原則管理編輯器和 wireless 的網路原則對話方塊中的標題列。 無論您重新命名您的原則為何，新 XP 無線原則一律會列在群組原則管理編輯器的**輸入**顯示**XP**。 其他原則所列的**輸入**顯示**Vista 和稍後發行**。  

Wireless 網路原則適用於 Windows Vista 和稍後發行，可讓您設定、 排定優先順序，並管理多個 wireless 設定檔。 Wireless 設定檔是連接和安全性設定連接到特定的 wireless 網路所使用的集合。 群組原則 wireless client 電腦上的更新，當您建立網路 Wireless 原則中的設定檔會自動新增 wireless client Wireless 的網路原則適用於電腦的設定。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>讓連接多部 wireless 網路

如果您有 wireless 戶端您在組織中的所在位置間移動，例如主要辦公室之間分公司，您可能想電腦連接到更多個 wireless 網路。 此時，您可以設定 wireless 包含每個網路的特定連接和安全性設定的設定檔。

例如，假設您的公司有一個 wireless 主要公司 office，服務設定識別碼與網路 \(SSID\) WlanCorp。

您分公司也有 wireless 網路，您也想連接。 分公司已設定為 WlanBranch SSID。

在本案例中，您可以設定為每個網路，以及電腦或其他裝置在公司辦公室所使用的設定檔並分公司可以連接到任一 wireless 網路時實體在各種不同的網路的涵蓋範圍。

##### <a name="mixed-mode-wireless-networks"></a>Mixed\ 模式 wireless 網路

或者，假設您網路上有多種 wireless 電腦與裝置支援標準不同的安全。 加勒比某些較舊的電腦有只能使用 WPA\-企業版時有較新的裝置可以使用較 WPA2\ 企業標準 wireless 介面卡。

您可以建立兩個不同的設定檔，使用相同的 SSID 和幾乎連接和安全性設定。

一個設定檔，wireless 驗證為 WPA2\ 企業有好一段，及其他設定檔中您可以指定 WPA\ 企業與 TKIP。

此程序通常稱為 mixed\ 模式部署，並可讓不同類型及 wireless 功能來分享 wireless 在相同網路的電腦。

### <a name="network-policy-server-nps"></a>網路原則伺服器 \(NPS\)
NPS 可讓您建立並執行適用於連接要求驗證與授權網路存取原則。

當您使用 NPS RADIUS 伺服器時，您可以設定 wireless 存取點，例如網路存取伺服器中 NPS RADIUS 戶端為。 您也需要驗證存取戶端和授權連接要求 NPS 使用的網路原則設定。  

### <a name="wireless-client-computers"></a>Wireless client 電腦
本指南，wireless client 電腦的電腦和的配備 IEEE 802.11 wireless 網路介面卡及可執行 Windows client 或 Windows Server 作業系統的其他裝置。

#### <a name="server-computers-as-wireless-clients"></a>Wireless 戶端為伺服器電腦

根據預設，802.11 wireless 的功能已停用電腦正在執行 Windows Server。

若要讓 wireless 連接執行 server 作業系統的電腦上，您必須安裝以及 Wireless 區域網路 \(WLAN\) 服務使用 「 Windows PowerShell 或新增角色與精靈中的功能在伺服器管理員中的功能。

當您安裝**無線區域網路服務**功能，新的服務**WLAN 自動設定**中安裝**服務**。 安裝完成時，您必須重新開機。

當您按一下伺服器會重新之後，您可以存取 WLAN 自動設定**[開始]**， **Windows 系統管理工具]**，並**服務**。

安裝和伺服器重新開機，是停止狀態的開機類型 service WLAN 自動設定後**自動**。 若要開始服務，按兩下 [ **WLAN 自動設定**。 在**一般**索引標籤上，按一下 [ **[開始]**，然後按一下 [ **[確定]**。

WLAN 自動設定服務列舉 wireless 介面卡及管理同時 wireless 連接和 wireless 包含所需設定伺服器連接 wireless 網路設定的設定檔。

Wireless 存取部署概觀，請查看[Wireless 存取部署程序](c-wireless-access-deploy-process.md)。
