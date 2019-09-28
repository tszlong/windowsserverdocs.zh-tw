---
ms.assetid: ''
title: 設定系統的高精確度
description: 已大幅改善 Windows 10 和 Windows Server 2016 中的時間同步處理。  在合理的作業狀況下，系統可以設定為維持1毫秒（毫秒）精確度或更佳（相對於 UTC）。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405193"
---
# <a name="configuring-systems-for-high-accuracy"></a>設定系統的高精確度
>適用於：Windows Server 2016 和 Windows 10 1607 版或更新版本

已大幅改善 Windows 10 和 Windows Server 2016 中的時間同步處理。  在合理的作業狀況下，系統可以設定為維持1毫秒（毫秒）精確度或更佳（相對於 UTC）。

下列指導方針可協助您設定系統，以達到高準確度。  本文討論下列需求：

- 支援的作業系統
- 系統設定 

> [!WARNING]
> **先前的作業系統精確度目標**<br>
>Windows Server 2012 R2 和以下不能達到相同的高精確度目標。 這些作業系統不支援高準確度。
>
>在這些版本中，Windows Time 服務滿足下列需求：
>
> - 提供必要的時間精確度以滿足 Kerberos 第5版的驗證需求。
> - 為已加入 common Active Directory 樹系的 Windows 用戶端和伺服器提供了鬆散精確的時間。
>
>2012 R2 和更新版本的更大容限超出 Windows 時間服務的設計規格。

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 和 Windows Server 2016 預設設定

雖然我們在 Windows 10 或 Windows Server 2016 上支援最高1毫秒的精確度，但大部分的客戶並不需要非常精確的時間。

因此，**預設**設定的目的是要滿足與先前作業系統相同的需求，如下所示：

- 提供必要的時間精確度，以滿足 Kerberos 第5版的驗證需求。
- 為已加入 common Active Directory 樹系的 Windows 用戶端和伺服器提供鬆散準確的時間。

## <a name="how-to-configure-systems-for-high-accuracy"></a>如何設定系統的高精確度

>[!IMPORTANT]
>**有關高準確度系統的可支援性注意事項**<br>
> 時間精確度需要端對端將正確時間從授權時間來源發佈到終端裝置。  在此路徑中，以測量方式新增 assymetry 的任何專案將會對精確度造成負面影響，而會影響裝置上的精確度。
>
>基於這個理由，我們已記載[支援界限來設定高精確度環境的 Windows 時間服務](support-boundary.md)，並概述必須滿足才能達到高準確度目標的環境需求。

### <a name="operating-system-requirements"></a>作業系統需求

高精確度設定需要 Windows 10 或 Windows Server 2016。  時間拓撲中的所有 Windows 裝置都必須符合這項需求，包括較高層級的 Windows 時間伺服器，以及虛擬化案例中執行時間緊迫虛擬機器的 Hyper-v 主機。 所有這些裝置都必須至少是 Windows 10 或 Windows Server 2016。

如下圖所示，需要高精確度的虛擬機器執行 Windows 10 或 Windows Server 2016。  同樣地，虛擬機器所在的 Hyper-v 主機，以及上游 Windows 時間伺服器也必須執行 Windows Server 2016。

![時間拓撲-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**判斷 Windows 版本**<br>
> 您可以在命令提示字元中執行命令 `winver`，以確認作業系統版本是1607（或更高版本），而 OS 組建是14393（或更新版本），如下所示：
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>系統設定

達到高準確度目標需要系統組態。  有各種方式可以執行這項設定，包括直接在登錄中或透過群組原則。  如需每個設定的詳細資訊，請參閱 Windows 時間服務技術參考– [Windows 時間服務工具](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools)。

#### <a name="windows-time-service-startup-type"></a>Windows 時間服務啟動類型

Windows 時間服務（W32Time）必須持續執行。  若要這麼做，請將 Windows 時間服務的啟動類型設定為「自動」啟動。

![自動設定](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>累計單向網路延遲

當網路延遲增加時，測量不確定性和「雜訊」時間。  因此，網路延遲必須在合理的界限內。  特定需求取決於您的目標精確度，並在支援界限中概述，[以設定高準確度環境的 Windows 時間服務](support-boundary.md)一文。

若要計算累計單向網路延遲，請在時間拓撲中的一組 NTP 用戶端-伺服器節點之間新增個別的單向延遲，從目標開始，並以高準確度層級1時間來源結束。

例如: 假設有一個時間同步階層，其具有高精確度的來源、兩個中繼 NTP 伺服器 A 和 B，以及該順序的目的電腦。 若要取得目標和來源之間的累計網路延遲，請測量平均個別 NTP 往返時間（RTTs）：

- 目標和時間伺服器 B
- 時間伺服器 B 和時間伺服器 A
- 時間伺服器 A 和來源

您可以使用收件匣的 w32tm 工具來取得這項測量。  請這樣做：

1. 從目標和時間伺服器 B 執行計算。
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. 從時間伺服器 b 執行計算（指向）時間伺服器 a。
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. 針對來源從時間伺服器 a 執行計算。
 
4. 接下來，新增在上一個步驟中測量的平均 RoundTripDelay，並除以2來取得目標和來源之間的累計網路延遲。

#### <a name="registry-settings"></a>登錄設定

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
設定系統輪詢所允許的最小間隔（以 log2 秒為單位）。

|  |  | 
|---------|---------|
|金鑰位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|設定    | 6        |
|結果 | 最小輪詢間隔現在為64秒。 |

下列命令會指示 Windows 時間取得已更新的設定：

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
設定允許進行系統輪詢的最大間隔（以 log2 秒為單位）。

|  |  |  
|---------|---------|
|金鑰位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|設定    | 6        |
|結果 | 輪詢間隔上限現在為64秒。  |

下列命令會指示 Windows 時間取得已更新的設定：

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
階段更正調整之間的時鐘刻度數目。

|  |  |  
|---------|---------|
|金鑰位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|設定    | 100        |
|結果 | 階段更正調整之間的時鐘刻度數目現在是100刻度。 |

下列命令會指示 Windows 時間取得已更新的設定：

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
設定啟用 SpecialInterval 0x1 旗標時的輪詢間隔（以秒為單位）。

|  |  |  
|---------|---------|
|金鑰位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|設定    | 64        |
|結果 | 輪詢間隔現在為64秒。 |

下列命令會重新開機 Windows 時間以挑選更新的設定：

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|金鑰位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|設定    | 2        |


---
