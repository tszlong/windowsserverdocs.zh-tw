---
ms.assetid: ''
title: 設定系統的高精確度
description: 在 Windows 10 和 Windows Server 2016 中的時間同步處理已經過大幅改良。  在合理的運作狀況，系統可以設定為維護 1 毫秒 （毫秒） 或 （相對於 UTC) 的更佳的精確度。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: d872252180d49bd41a91e9a8eaf516b834ed242a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814669"
---
# <a name="configuring-systems-for-high-accuracy"></a>設定系統的高精確度
>適用於：Windows Server 2016 和 Windows 10 版本 1607年或更新版本

在 Windows 10 和 Windows Server 2016 中的時間同步處理已經過大幅改良。  在合理的運作狀況，系統可以設定為維護 1 毫秒 （毫秒） 或 （相對於 UTC) 的更佳的精確度。

下列指引將協助您設定您的系統，以達到高精確度。  這篇文章討論下列需求：

- 支援的作業系統
- 系統設定 

> [!WARNING]
> **舊版作業系統精確度目標**<br>
>Windows Server 2012 R2 和以下不符合相同的高精確度目標。 這些作業系統不支援的高精確度。
>
>在這些版本中，Windows 時間服務滿足下列需求：
>
> - 提供所需的時間精確度，以滿足 Kerberos 版本 5 驗證需求。
> - Windows 用戶端與伺服器加入常見的 Active Directory 樹系提供鬆散準確的時間。
>
>Windows Time 服務的設計規格之外，會於 2012 R2 和以下更高的容錯。

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 和 Windows Server 2016 預設設定

雖然我們在 Windows 10 或 Windows Server 2016 上支援最多 1 毫秒的精確度，大部分的客戶不需要高精確度的時間。

因此，**預設組態**滿足與舊版作業系統為相同的需求：

- 提供所需的時間精確度，以滿足 Kerberos 版本 5 驗證需求。
- Windows 用戶端與伺服器加入常見的 Active Directory 樹系提供鬆散準確的時間。

## <a name="how-to-configure-systems-for-high-accuracy"></a>如何設定高精確度的系統

>[!IMPORTANT]
>**高度精確的系統可支援性的相關附註**<br>
> 時間精確度需要端對端分佈的精確時間從的授權時間來源裝置。  任何項目加入 assymetry 在沿著此路徑的測量會造成負面影響精確度會影響您的裝置上可達成的精確度。
>
>基於這個理由，我們已記載[支援的界限，以設定 Windows 時間服務的高精確度的環境](support-boundary.md)大綱環境的需求，也必須滿足才能達到高精確度的目標。

### <a name="operating-system-requirements"></a>作業系統需求

高精確度組態需要 Windows 10 或 Windows Server 2016。  「 時間 」 拓撲中的所有 Windows 裝置必須都符合此需求包括較高的 stratum Windows 時間伺服器，以及在虛擬化案例中，執行時間緊迫的虛擬機器的 HYPER-V 主機。 所有這些裝置必須至少為 Windows 10 或 Windows Server 2016。

在圖中，如下所示，需要高精確度的虛擬機器正在執行 Windows 10 或 Windows Server 2016。  同樣地，虛擬機器所在，HYPER-V 主機和上游的 Windows 時間伺服器也必須執行 Windows Server 2016。

![時間拓樸-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**判斷 Windows 版本**<br>
> 您可以執行命令`winver`命令提示字元，若要確認 OS 版本是 1607年 （或更新版本），OS 組建 14393 （或更新版本），如下所示：
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>系統設定

達到高精確度目標需要系統設定。  有各種不同的方式來執行這項設定，包括在登錄中直接或透過群組原則。  所有這些設定的詳細資訊可在 Windows 時間服務技術參考 – [Windows 時間服務工具](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools)。

#### <a name="windows-time-service-startup-type"></a>Windows 時間服務啟動類型

Windows 時間服務 (W32Time) 必須持續執行。  若要這樣做，請設定 Windows 時間服務的啟動類型設定為 'Automatic' 的開始。

![自動設定](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>累計單向的網路延遲

測量不確定性和 「 雜訊 」 浮現為網路延遲會增加。  因此，務必網路延遲是合理的界限內。  特定的需求取決於您目標的精確度和所述[支援的界限，以設定 Windows 時間服務的高精確度的環境](support-boundary.md)文章。

若要計算的累計單向的網路延遲，個別單向之間加入延遲成對 NTP 用戶端-伺服器節點，在時間拓撲中，從目標開始和結束時間高精確度 stratum 1 時間來源。

例如: 請考慮使用高度精確的來源、 兩個中繼 NTP 伺服器 A 和 B 和該訂單中的目標電腦的時間同步處理階層。 若要取得的目標和來源之間的累計的網路延遲，測量個別 NTP 往返時間 (RTTs) 之間的平均：

- 目標和時間伺服器 B
- 時間伺服器 B 與時間伺服器 A
- 時間伺服器 A 和來源

您可以使用收件匣 w32tm.exe 工具取得這個度量單位。  請這樣做：
<!-- Use PowerShell to import the CSV then average the RTT Column -->

1. 從目標與時間伺服器 b 執行計算
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. 執行計算對的時間伺服器 b （指向） 時間伺服器。
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. 時間伺服器從執行計算對來源。
 
4. 接下來，新增 平均 RoundTripDelay 測量上一個步驟中，並除以 2，以取得目標與來源之間的累計的網路延遲。

#### <a name="registry-settings"></a>登錄設定

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
設定最小的間隔，以允許系統輪詢 log2 秒為單位。

|  |  | 
|---------|---------|
|索引鍵位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|設定    | 6        |
|結果 | 最短的輪詢間隔現在是 64 的秒數。 |

下列命令會告知 Windows 時間，以取得更新的設定：

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
設定以允許系統輪詢 log2 秒為單位的最大時間間隔。

|  |  |  
|---------|---------|
|索引鍵位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|設定    | 6        |
|結果 | 輪詢間隔上限現在是 64 的秒數。  |

下列命令會告知 Windows 時間，以取得更新的設定：

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
階段更正調整之間的時鐘刻度數目。

|  |  |  
|---------|---------|
|索引鍵位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|設定    | 100        |
|結果 | 100 刻度的現在階段更正調整之間的時鐘刻度數目。 |

下列命令會告知 Windows 時間，以取得更新的設定：

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
以秒為單位的 SpecialInterval 0x1 旗標在啟用時，會設定輪詢間隔。

|  |  |  
|---------|---------|
|索引鍵位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|設定    | 64        |
|結果 | 輪詢間隔現在是 64 的秒數。 |

下列命令會重新啟動 Windows 時間，以取得更新的設定：

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|索引鍵位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|設定    | 2        |


---
