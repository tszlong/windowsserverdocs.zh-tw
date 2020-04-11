---
ms.assetid: ''
title: 設定高準確度的系統
description: Windows 10 和 Windows Server 2016 中的時間同步功能已大幅改善。  在合理的作業狀況下，系統可以設定為維持 1 ms (毫秒) 或更高的準確度 (以 UTC 為準)。
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 25472e4ba4837bd68c9b6914e22c2219c91d3ac0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861651"
---
# <a name="configuring-systems-for-high-accuracy"></a>設定高準確度的系統
>適用於：Windows Server 2016 與 Windows 10 版本 1607 或更新版本

Windows 10 和 Windows Server 2016 中的時間同步功能已大幅改善。  在合理的作業狀況下，系統可以設定為維持 1 ms (毫秒) 或更高的準確度 (以 UTC 為準)。

下列指導方針可協助您設定系統以達到高準確度。  本文會討論下列需求：

- 支援的作業系統
- 系統設定 

> [!WARNING]
> **先前作業系統的準確度目標**<br>
>Windows Server 2012 R2 和較舊版本無法達到同樣的高準確度目標。 這些作業系統不支援高準確度。
>
>在這些版本中，Windows Time 服務滿足下列需求：
>
> - 提供必要的時間準確度以滿足 Kerberos 第 5 版的驗證需求。
> - 為已加入一般 Active Directory 樹系的 Windows 用戶端和伺服器提供精確度沒那麼高的時間。
>
>2012 R2 和較舊版本的更大允差則不在 Windows Time 服務的設計規範內。

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 和 Windows Server 2016 的預設設定

雖然我們在 Windows 10 或 Windows Server 2016 上支援最高 1 毫秒的準確度，但大部分的客戶並不需要非常精確的時間。

因此，**預設設定**的目的就是要滿足與較舊作業系統相同的需求，以便：

- 提供必要的時間準確度以滿足 Kerberos 第 5 版的驗證需求。
- 為已加入一般 Active Directory 樹系的 Windows 用戶端和伺服器提供精確度沒那麼高的時間。

## <a name="how-to-configure-systems-for-high-accuracy"></a>如何設定高準確度的系統

>[!IMPORTANT]
>**關於高精確度系統支援性的注意事項**<br>
> 時間準確度必須以端對端的方式將精確時間從授權時間來源發佈到終端裝置。  在此路徑中，任何會對測量增添偏差的因素，都會對準確度造成負面影響，而影響裝置是否能夠實現準確度。
>
>為此，我們編寫了[可為高準確度環境設定 Windows Time 服務的支援界限](support-boundary.md)，以概述使用者還必須滿足什麼環境需求才能達到高準確度目標。

### <a name="operating-system-requirements"></a>作業系統需求

高準確度設定需要使用 Windows 10 或 Windows Server 2016。  時間拓撲中的所有 Windows 裝置都必須符合這項需求，包括較高層級的 Windows 時間伺服器，而在虛擬化案例中，則包括執行有時效性虛擬機器的 Hyper-V 主機。 這些裝置全都必須至少使用 Windows 10 或 Windows Server 2016。

在下圖中，需要高準確度的虛擬機器所執行的是 Windows 10 或 Windows Server 2016。  同樣地，虛擬機器所在的 Hyper-V 主機，以及上游 Windows 時間伺服器也都必須執行 Windows Server 2016。

![時間拓撲 - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**判斷 Windows 版本**<br>
> 您可以在命令提示字元中執行命令 `winver`，以確認作業系統版本是 1607 (或更新版本)，而 OS 組建是 14393 (或更新版本)，如下所示：
>
> ![Winver - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>系統設定

要達到高準確度目標就需要進行系統設定。  有各種方式可執行這項設定，包括直接在登錄中或透過群組原則來進行。  如需這些設定的詳細資訊，請參閱「Windows Time 服務技術參考資料」– [Windows Time 服務工具](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools)。

#### <a name="windows-time-service-startup-type"></a>Windows Time 服務的啟動類型

Windows Time 服務 (W32Time) 必須持續執行。  若要這麼做，請將 Windows Time 服務的啟動類型設定為「自動」啟動。

![自動設定](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>累計單向網路延遲

網路延遲增加時就會慢慢產生測量不確定性和「雜訊」。  因此，就必須將網路延遲控制在合理界限內。  特定需求則取決於目標準確度，在[可為高準確度環境設定 Windows Time 服務的支援界限](support-boundary.md)一文內會有相關概述。

若要計算累計的單向網路延遲，請在時間拓撲的各 NTP 用戶端-伺服器節點組合之間新增個別的單向延遲，從目標開始，並於高準確度層級 1 時間來源結束。

例如：假設有一個時間同步階層，其具有依該順序的一個高精確度來源、兩個中繼 NTP 伺服器 (A 和 B)，以及一個目標機器。 若要取得目標和來源之間的累計網路延遲，請測量下列項目之間的平均個別 NTP 來回時間 (RTT)：

- 目標和時間伺服器 B
- 時間伺服器 B 和時間伺服器 A
- 時間伺服器 A 和來源

您可以使用收件匣 w32tm.exe 工具來取得此測量值。  若要這樣做：

1. 從目標和時間伺服器 B 執行計算。
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. 從時間伺服器 B 針對 (指向) 時間伺服器 A 執行計算。
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. 從時間伺服器 A 針對來源執行計算。
 
4. 接下來，新增在上一個步驟中測量到的平均 RoundTripDelay，並除以 2 來取得目標和來源之間的累計網路延遲。

#### <a name="registry-settings"></a>登錄設定

# <a name="minpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
設定系統輪詢所允許的最小間隔 (以 log2 秒為單位)。

|  |  | 
|---------|---------|
|機碼位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|設定    | 6        |
|結果 | 最小輪詢間隔現在是 64 秒。 |

下列命令會指示 Windows Time 取得已更新的設定：

`w32tm /config /update`


# <a name="maxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
設定系統輪詢所允許的最大間隔 (以 log2 秒為單位)。

|  |  |  
|---------|---------|
|機碼位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|設定    | 6        |
|結果 | 最大輪詢間隔現在是 64 秒。  |

下列命令會指示 Windows Time 取得已更新的設定：

`w32tm /config /update`

# <a name="updateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
每次相位校正調整所間隔的時鐘刻度數目。

|  |  |  
|---------|---------|
|機碼位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|設定    | 100        |
|結果 | 每次相位校正調整所間隔的時鐘刻度數目是 100 刻度。 |

下列命令會指示 Windows Time 取得已更新的設定：

`w32tm /config /update`

# <a name="specialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
設定啟用了 SpecialInterval 0x1 旗標時的輪詢間隔 (以秒為單位)。

|  |  |  
|---------|---------|
|機碼位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|設定    | 64        |
|結果 | 輪詢間隔現在是 64 秒。 |

下列命令會重新啟動 Windows Time 來取得已更新的設定：

`net stop w32time && net start w32time`

# <a name="frequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|機碼位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|設定    | 2        |


---
