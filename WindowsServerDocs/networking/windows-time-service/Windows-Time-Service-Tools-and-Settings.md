---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Windows 時間服務工具和設定
author: Teresa-Motiv
description: 描述可用於 Windows Time 服務 (W32Time) 的設定，以及可用來設定這些設定的工具。
ms.author: v-tea
ms.date: 11/20/2020
ms.topic: article
ms.openlocfilehash: 3a9f079160f3af2b175b16654ce5aa77b4dd323e
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330500"
---
# <a name="windows-time-service-tools-and-settings"></a>Windows 時間服務工具和設定

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

在本主題中，您將了解 Windows Time 服務 (W32Time) 的工具和設定。

如果您只想要為已加入網域的用戶端電腦同步時間，請參閱[設定用戶端電腦以便自動同步網域時間](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。 如需有關如何設定 Windows Time 服務的其他主題，請參閱[可在何處找到 Windows Time 服務的設定資訊](windows-time-service-top.md)。

> [!CAUTION]
> 請勿在 Windows 時間服務執行時，使用 **Net time** 命令來設定時間。
>
> 此外，在執行 Windows XP 或更早版本的舊型電腦上，**Net time /querysntp** 命令會顯示電腦所設定用來作為同步依據的網路時間通訊協定 (NTP) 伺服器名稱，但只有在電腦的時間用戶端設定為 NTP 或 AllSync 時，才會使用該 NTP 伺服器。 該命令已遭到取代。

大部分網域成員電腦的時間用戶端類型都是 NT5DS，這表示其會與網域階層的時間同步。 一般來說，關於這一點的唯一例外是作為樹系根網域主要網域控制站 (PDC) 模擬器操作主機的網域控制站。 PDC 模擬器操作主機通常會設定為與外部時間來源的時間同步。 若要檢視電腦的時間用戶端設定 (從 Windows Server 2008 和 Windows Vista 起的版本)，從提升權限的命令提示字元執行 **W32tm /query /configuration** 命令，然後查看命令輸出中的 **Type** 行。 如需詳細資訊，請參閱 [Windows Time 服務的運作方式](How-the-Windows-Time-Service-Works.md)。 此外，您可以執行 **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** 命令，並查看命令輸出中的 **NtpServer** 值。

> [!IMPORTANT]
> 在 Windows Server 2016 之前，W32Time 服務並非設計來滿足有時效性應用程式的需求。 但現在，Windows Server 2016 的更新可讓您在網域中實作需要 1 毫秒準確度的解決方案。 如需詳細資訊，請參閱 [Windows 2016 的精確時間](accurate-time.md)和[可為高準確度環境設定 Windows Time 服務的支援界限](support-boundary.md)。

## <a name="windows-time-service-tools"></a>Windows 時間服務工具

下列工具與 Windows 時間服務相關聯。

### <a name="w32tmexe-windows-time"></a>W32tm.exe：Windows Time

**類別**

此工具是 Windows (Windows XP 和更新版本) 和 Windows Server (Windows Server 2003 和更新版本) 預設安裝的一部分。

**版本相容性**

此工具用於 Windows (Windows XP 和更新版本) 和 Windows Server (Windows Server 2003 和更新版本) 預設安裝。

您可以使用 W32tm.exe 來設定 Windows 時間服務設定，以及診斷時間服務問題。 W32tm.exe 是在針對 Windows 時間服務進行設定、監視或疑難排解時的慣用命令列工具。

下表說明可與 W32tm.exe 搭配使用的參數。

**W32tm.exe 主要參數**

|參數 |說明 |
| --- | --- |
|**w32tm /?** |顯示 W32tm 命令列說明 |
|**w32tm /register** |將時間服務註冊為以服務的形式執行，並將預設設定資訊新增至登錄中。 |
|**w32tm /unregister** |取消註冊時間服務，並從登錄中移除其所有設定資訊。 |
|**w32tm /monitor [/domain:\<*domain name*>] [/computers:\<*name*>[,\<*name*>[,\<*name*>...]]] [/threads:\<*num*>]** |監視 Windows 時間服務。<p>**/domain**:指定所要監視的網域。 如果未給定網域名稱，或 **/domain** 和 **/computers** 這兩項均未指定，則會使用預設網域。 此選項可能會使用多次。<p>**/computers**:監視給定的電腦清單。 電腦名稱會以逗號分隔，且不含空格。 如果名稱前面加上 **\**_，則系統會將其視為 PDC。此選項可能會使用多次。<p>_*/threads**：指定要同時分析的電腦數目。 預設值為 3。 允許的範圍是 1 至 50。 |
|**w32tm /ntte \<NT *time epoch*>** |將 Windows NT 系統時間 (從 0h 1-Jan 1601 開始，以 10<sup>-7</sup> 秒間隔測量) 轉換成可讀取的格式。 |
|**w32tm /ntpte \<NTP *time epoch*>** |將 NTP 系統時間 (從 0h 1-Jan 1900 開始，以 2<sup>-32</sup> 秒間隔測量) 轉換成可讀取的格式。 |
|**w32tm /resync [/computer:\<*computer*>] [/nowait] [/rediscover] [/soft]** |指示電腦應盡快重新同步其時鐘，並丟掉所有累積的錯誤統計資料。<p>**/computer:\<*computer*>** :指定應該重新同步的電腦。 如果未指定，則本機電腦會重新同步。<p>**/nowait**: 不等候重新同步開始；立即傳回。 否則，等候重新同步完成後再傳回。<p>**/rediscover**:重新偵測網路設定，並重新探索網路來源，然後重新同步。<p>**/soft**:使用現有的錯誤統計資料來重新同步。 沒什麼用處，為確保相容性而提供。 |
|**w32tm /stripchart /computer:\<*target*> [/period:\<*refresh*>] [/dataonly] [/samples:\<*count*>] [/rdtsc]** |顯示此電腦與另一部電腦之間的時差帶狀圖。<p>**/computer:\<*target*>** :要對其測量時差的電腦。<p>**/period:\<*refresh*>** :取樣間隔時間，以秒為單位。 預設值為 2 秒。<p>**/dataonly**:僅顯示資料，不包含圖形。<p>**/samples:\<*count*>** :收集 \<*count*> 範例，然後停止。 如果未指定，則會一直收集樣本，直到您按下 **Ctrl+C** 為止。<br/><br/>**/rdtsc**:針對每個樣本，此選項會列印逗號分隔值以及 **RdtscStart**、**RdtscEnd**、**FileTime**、**RoundtripDelay**、**NtpOffset** 等標頭，而不是列印文字圖形。<br/><ul><li>**RdtscStart**:[RDTSC (讀取時間戳記計數器)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) 值，在 NTP 要求產生前一刻進行收集。</li><li>**RdtscEnd**:在收到並處理 NTP 回應之後所立刻收集的 RDTSC 值。</li><li>**FileTime**:NTP 要求中使用的本機 FILETIME 值。</li><li>**RoundtripDelay**:從產生 NTP 要求到處理收到的 NTP 回應所經過的時間 (以秒為單位)，此值會根據 NTP 往返計算來進行計算。</li><li>**NTPOffset**：本機電腦與 NTP 伺服器之間的時差 (以秒為單位)，此值會根據 NTP 時差計算來進行計算。</li></ul> |
|**w32tm /config [/computer:\<*target*>] [/update] [/manualpeerlist:\<*peers*>] [/syncfromflags:\<*source*>] [/LocalClockDispersion:\<*seconds*>] [/reliable:(YES\|NO)] [/largephaseoffset:\<*milliseconds*>]** |**/computer:\<*target*>** :調整 \<*target*> 的設定。 如果未指定，則預設值是本機電腦。<p>**/update**:對時間服務發出通知，讓其知道設定已變更，以使變更生效。<p>**/manualpeerlist:\<*peers*>** :將手動對等清單設定為 \<*peers*>，這是以空格分隔的 DNS 和/或 IP 位址清單。 指定多個對等時，此選項必須以引號括住。<p>**/syncfromflags:\<*source*>** :設定要作為 NTP 用戶端同步依據的來源。 \<*source*> 應該是下列關鍵字的逗號分隔清單 (不區分大小寫)：<ul><li>**MANUAL**:包含手動對等清單中的對等。</li><li>**DOMHIER**:從網域階層中的網域控制站 (DC) 來同步。</li></ul>**/LocalClockDispersion:\<*seconds*>** :設定 W32Time 在無法從其設定的來源取得時間時，所會採用的內部時鐘精確度。<p>**/reliable:(YES\|NO)** :設定此電腦是否為可靠的時間來源。 此設定只有在網域控制站上才有意義。<ul><li>**YES**:此電腦是可靠的時間服務。</li><li>**NO**:此電腦不是可靠的時間服務。</li></ul>**/largephaseoffset:\<*milliseconds*>** : 設定本機與網路時間之間的時差，而 W32Time 會視其為尖峰。 |
|**w32tm /tz** |顯示目前的時區設定。 |
|**w32tm /dumpreg [/subkey:\<*key*>] [/computer:\<*target*>]** |顯示與給定登錄機碼相關聯的值。<p>預設機碼是 **HKLM\System\CurrentControlSet\Services\W32Time** (時間服務的根機碼)。<p>**/subkey:\<*key*>** :顯示與預設機碼的子機碼 <key> 相關聯的值。<p>**/computer:\<*target*>** :查詢電腦 \<*target*>的登錄設定 |
|**w32tm /query [/computer:\<*target*>] {/source \| /configuration \| /peers \| /status} [/verbose]** |顯示電腦的 Windows Time 服務資訊。 此參數最早是在 Windows Vista 和 Windows Server 2008 的 Windows 時間用戶端開始提供的。<p>**/computer:\<*target*>** :查詢 \<*target*>的資訊。 如果未指定，則預設值是本機電腦。<p>**/source**:顯示時間來源。<p>**/configuration**:顯示執行階段的設定和設定的來源。 在詳細資訊模式中，也會顯示未定義或未使用的設定。<p>**/peers**:顯示對等及其狀態的清單。<p>**/status**:顯示 Windows 時間服務狀態。<p>**/verbose**:設定詳細資訊模式以顯示更多資訊。 |
|**w32tm /debug {/disable \| {/enable /file:\<*name*> /size:/<*bytes*> /entries:\<*value*> [/truncate]}}** |啟用或停用本機電腦的 Windows Time 服務私人記錄。 此參數最早是在 Windows Vista 和 Windows Server 2008 的 Windows 時間用戶端開始提供的。<p>**/disable**:停用私人記錄檔。<p>**/enable**:啟用 私人記錄檔。<ul><li>**file:\<*name*>** :指定絕對的檔案名稱。</li><li>**size:\<*bytes*>** :指定循環記錄的大小上限。</li><li>**entries:\<*value*>** :包含以數字指定並以逗號分隔的旗標清單，這些旗標可指定應加以記錄的資訊類型。 有效數字是 0 到 300。 除了單一數字外，使用數字範圍也有效，例如 0-100,103,106。 值 0-300 可用來記錄所有資訊。</li></ul>**/truncate**:截斷檔案 (如果存在的話)。 |

如需 **W32tm.exe** 詳細資訊，請參閱 [Windows 說明](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10)。

**範例**

如果您想將本機 Windows 時間用戶端設定為指向兩部不同的時間伺服器，一個名為 ntpserver.contoso.com，另一個名為 clock.adatum.com，請在命令列中輸入下列命令，然後按 ENTER 鍵：

```cmd
w32tm /config /manualpeerlist:"ntpserver.contoso.com clock.adatum.com" /syncfromflags:manual /update
```

如果您想從主機名稱為 CONTOSOW1 的 Windows 用戶端電腦檢查 Windows 時間用戶端設定，請執行下列命令：

```cmd
W32tm /query /computer:contosoW1 /configuration
```

此命令的輸出是針對 Windows 時間用戶端所設定的設定參數清單。

> [!IMPORTANT]
> [Windows Server 2016 已改善同步處理演算法的時間](./accurate-time.md)，與 RFC 規格一致。 因此，如果您想要將本機 Windows 時間用戶端設定為指向多個對等互連，則強烈建議您準備三個以上不同的時間伺服器。
>
> 如果您只有兩部時間伺服器，您應該指定 **UseAsFallbackOnly** 旗標 (0x2) 來取消設定其中一個優先順序。 例如，如果您想要在 clock.adatum.com 上設定 ntpserver.contoso.com 的優先順序，請執行下列命令。
> ```cmd
> w32tm /config /manualpeerlist:"ntpserver.contoso.com,0x8 clock.adatum.com,0xa" /syncfromflags:manual /update
> ```
> 如需指定旗標的意義，請參閱 ["HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters" subkey entries](#parameters)。

## <a name="using-group-policy-to-configure-the-windows-time-service"></a>使用群組原則設定 Windows 時間服務

Windows 時間服務會將一些設定屬性儲存為登錄項目。 您可以使用群組原則物件來設定這項資訊的大部分內容。 例如，您可以使用群組原則物件 (GPO) 將電腦設定為 NTPServer 或 NTPClient、設定時間同步機制，或將電腦設定為可靠的時間來源。

> [!NOTE]
> Windows Time 服務的群組原則設定可在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 網域控制站上設定，而且只能套用至執行 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 的電腦。

Windows 會將 Windows 時間服務原則資訊儲存在 W32Time.admx 系統管理範本檔案中，位於 **Computer Configuration\Administrative Templates\System\Windows Time Service** 下。 系統會儲存原則在登錄中所定義的設定資訊，並使用這些登錄項目來設定 Windows 時間服務的登錄項目。 因此，由群組原則所定義的值會覆寫登錄之 Windows 時間服務區段中任何預先存在的值。

> [!IMPORTANT]
> 某些預設的群組原則物件 (GPO) 設定與對應的預設登錄項目不同。 如果您打算使用 GPO 來設定任何 Windows Time 設定，請務必要檢閱 [Windows Time 服務群組原則設定的預設值與 Windows Server 2003 中對應的 Windows Time 服務登錄項目不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此問題適用於 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。

例如，假設您 **設定 Windows NTP 用戶端** 原則中編輯原則設定。

您的變更會儲存在系統管理範本中的下列位置：

> **Computer Configuration\Administrative Templates\System\Windows Time Service\Time Providers\ Configure Windows NTP Client**

Windows 會將這些設定載入下列子機碼下的登錄原則區域中：

> **HKLM\Software\Policies\Microsoft\W32time\TimeProviders\NtpClient**

然後 Windows 會使用原則設定，在下列子機碼下設定相關的 Windows 時間服務登錄項目：

> **HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Time Providers\NTPClient\\**

下表列出您可以為 Windows 時間服務設定的原則，以及這些原則影響的登錄子機碼。

> [!NOTE]
> 移除群組原則設定時，Windows 會從登錄的原則區域中移除對應的項目。

|原則<sup>1</sup> |登錄位置<sup>2,</sup> <sup>3</sup> |
| --- | --- |
|全域組態設定 |W32Time<br />W32Time\Config<br />W32Time\Parameters |
|Time Providers\Configure Windows NTP Client |W32Time\TimeProviders\NtpClient |
|Time Providers\Enable Windows NTP Client |W32Time\TimeProviders\NtpClient |
|Time Providers\Enable Windows NTP Server |W32Time\TimeProviders\NtpServer |

> <sup>1</sup> 類別路徑：**Computer Configuration\Administrative Templates\System\Windows Time Service**
> <sup>2</sup> Subkey:**HKLM\SOFTWARE\Policies\Microsoft**
> <sup>3</sup> Subkey:**HKLM\SYSTEM\CurrentControlSet\Services**

## <a name="enabling-w32time-logging"></a>啟用 W32Time 記錄

下列三個登錄項目雖不是 W32Time 預設設定的一部分，但仍可新增至登錄中以提升記錄功能。 您可以變更群組原則物件編輯器中 **EventLogFlags** 設定的值，以修改記錄到系統事件記錄的資訊。 根據預設，時間服務會在每次切換至新的時間來源時記錄事件。

您必須新增下列登錄項目才能啟用 W32Time 記錄：

|登錄項目 |版本 |說明 |
| --- | --- | --- |
|**FileLogEntries** |所有版本 |控制 Windows Time 記錄檔中所建立的項目數量。 預設值是 [無]，也就是不會記錄任何 Windows Time 活動。 有效值是 **0** 到 **300**。 此值不會影響 Windows Time 正常建立的事件記錄項目 |
|**FileLogName** |所有版本 |控制 Windows Time 記錄的位置和檔案名稱。 預設值是空白，除非變更 **FileLogEntries**，否則請勿變更此預設值。 有效值是完整路徑和檔案名稱，Windows Time 會用此值來建立記錄檔。 此值不會影響 Windows Time 正常建立的事件記錄項目。 |
|**FileLogSize** |所有版本 |控制 Windows Time 記錄檔的循環記錄行為。 定義了 **FileLogEntries** 和 **FileLogName** 時，此項目會定義記錄檔在到達多大 (以位元組為單位) 後，才以新的項目覆寫最舊的記錄項目。 請為此設定使用 **1000000** 以上的值。 此值不會影響 Windows Time 正常建立的事件記錄項目。 |

## <a name="configuring-how-windows-time-service-resets-the-computer-clock"></a>設定 Windows 時間服務重設電腦時鐘的方式

為了讓 W32Time 逐漸設定電腦時鐘，時差必須小於 **MaxAllowedPhaseOffset** 值，並同時滿足下列方程式：

- Windows Server 2016 和更新版本：

  > |**CurrentTimeOffset**| &divide; (16 &times; **PhaseCorrectRate** &times; **pollIntervalInSeconds**) &le; **SystemClockRate** &divide; 2

- Windows Server 2012 R2 和較早版本：

  > |**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2

**CurrentTimeOffset** 值會以時鐘刻度為單位來進行測量，在 Windows 系統上，1 毫秒 = 10,000 時鐘刻度。

**SystemClockRate** 和 **PhaseCorrectRate** 也會以時鐘刻度為單位來進行測量。 若要取得 **SystemClockRate** 值，您可以使用下列命令，並使用公式 (秒數 &times;1000 &times; 10000) 將其從秒數轉換成時鐘刻度：

```cmd
W32tm /query /status /verbose
ClockRate: 0.0156000s
```

**SystemClockRate** 是系統上的時脈速率。 以 156000 秒為例，**SystemClockRate** 值會是 0.0156000 &times; 1000 &times; 10000 = 156000 時鐘刻度。

**MaxAllowedPhaseOffset** 也會以秒為單位測量。 若要將其轉換成時鐘刻度，請乘上 **MaxAllowedPhaseOffset** &times; 1000 &times;10000。

下列範例說明在使用 Windows Server 2012 R2 或更早版本時要如何套用這些計算。

**範例 1：時間差四分鐘**

例如，您的時間是 11:05，而您從對等所收到、認為正確無誤的時間樣本是 11:09。

> **PhaseCorrectRate** = 1
>
> **UpdateInterval** = 30000 時鐘刻度
>
> **SystemClockRate** = 156000 時鐘刻度
>
> **MaxAllowedPhaseOffset** = 10 分鐘 = 600 秒 = 600 &times; 1000 &times; 10000 = 6000000000 時鐘刻度
>
> |**CurrentTimeOffset**|= 4 分鐘 = 4 &times; 60 &times; 1000 &times; 10000 = 2400000000 時鐘刻度
>
> Is **CurrentTimeOffset** &le; **MaxAllowedPhaseOffset**?
>
> 2,400,000,000 &le; 6,000,000,000:TRUE

其是否符合上述方程式？

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)

Is 2,400,000,000 / (30,000 &times; 1) &le; 156,000 &divide; 2

80,000 &le; 78,000:FALSE

因此，W32tm 會立即將時鐘回調。

> [!NOTE]
> 在此情況下，如果您想要讓時鐘慢慢回調，則也必須在登錄中調整 **PhaseCorrectRate** 或 **updateInterval** 的值，以確保方程式的結果是 **TRUE**。

**範例 2：時間差三分鐘**

> **PhaseCorrectRate** = 1
>
> **UpdateInterval** = 30000 時鐘刻度
>
> SystemClockRate = 156000 時鐘刻度
>
> **MaxAllowedPhaseOffset** = 10 分鐘 = 600 秒 = 600 &times; 1000 &times; 10000 = 6000000000 時鐘刻度
>
> |**CurrentTimeOffset**|= 3分鐘 = 3 &times; 60 &times; 1,000 &times; 10,000 = 1,800,000,000 時鐘刻度
>
> Is |**CurrentTimeOffset**| &le; **MaxAllowedPhaseOffset**?
>
> 1,800,000,000 &le; 6,000,000,000:TRUE

其是否符合上述方程式？

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)
>
> Is 3 mins &times; (1,800,000,000) &divide; (30,000 &times; 1) &le; 156,000 &divide; 2
>
> Is 60,000 &le; 78,000:TRUE

在此情況下，時鐘會慢慢回調。

## <a name="reference-windows-time-service-registry-entries"></a>參考：Windows 時間服務的登錄項目

> [!WARNING]
> 在針對必要設定是否已完成套用的情形進行疑難排解或確認時，這些登錄項目資訊可供您作為參考。 在登錄的 W32Time 區段中，有許多值會由 W32Time 在內部用來儲存資訊。 請勿手動變更這些值。 在套用登錄的修改之前，登錄編輯程式或 Windows 並不會加以驗證。 如果登錄包含不正確值，Windows 可能會遇到嚴重錯誤。

Windows 時間服務會將資訊儲存在下列登錄子機碼下：

- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config**](#config)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**](#parameters)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient**](#ntpclient)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer**](#ntpserver)

此外，為了進行疑難排解，您可以[新增專案以設定記錄](#enabling-w32time-logging)。

在下表中，「所有版本」是指包含 Windows 7、Windows 8、Windows 10、Windows Server 2008 和 Windows Server 2008 R2、Windows Server 2012 和 Windows server 2012 R2、Windows Server 2016 和 Windows Server 2019 的 Windows 版本。 某些項目僅適用於較新的 Windows 版本。

> [!NOTE]
> 登錄中的某些參數會以時鐘刻度為單位來測量，某些則會以秒為單位來測量。 若要將時間從時鐘刻度轉換為秒，請使用這些轉換因數：
> - 1 分鐘 = 60 秒
> - 1 秒 = 1000 毫秒
> - 1 毫秒 = Windows 系統上的 10,000 時鐘刻度，如 [DateTime.Ticks 屬性](/dotnet/api/system.datetime.ticks)所述。
>
> 例如，5 分鐘會變成 5 &times; 60 &times; 1000 &times; 10000 = 3,000,000,000 時鐘刻度。

### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig-subkey-entries"></a><a id="config"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config" subkey entries

|登錄項目 |版本 |說明 |
| --- | --- | --- |
|**AnnounceFlags** |所有版本 |控制是否要將此電腦標示為可靠的時間伺服器。 除非電腦也標示為時間伺服器，否則不會將其標示為可靠。<ul><li>**0x00**： 不是時間伺服器</li><li>**0x01**： 永遠的時間伺服器</li><li>**0x02**： 自動的時間伺服器</li><li>**0x04**： 永遠可靠的時間伺服器</li><li>**0x08**： 自動且可靠的時間伺服器</li></ul><br />網域成員的預設值是 **10**。 獨立用戶端和伺服器的預設值是 **10**。 |
|**ChainDisable** | |控制是否停用鏈結機制。 如果已停用鏈結 (設為0)，唯讀網域控制站 (RODC) 可以與任何網域控制站同步，但是未在 RODC 上將密碼快取的主機將無法與 RODC 同步。 這是布林值設定，預設值為 **0**。|
|**ChainEntryTimeout** | |指定在項目被視為過期之前，可保留在鏈結資料表中的最長時間量。 處理下一個要求或回應時，可能會移除過期的項目。 預設值為 **16** (秒)。 |
|**ChainLoggingRate** | |控制事件的頻率，指出記錄到事件檢視器中系統記錄檔之成功和失敗的鏈結嘗試次數。 預設值是 **30** (分鐘)。 |
|**ChainMaxEntries** | |控制在鏈結資料表中允許的最大項目數。 如果鏈結資料表已滿，而且沒有任何已過期的項目可以移除，則會捨棄任何連入要求。 預設值是 **128** (項目)。 |
|**ChainMaxHostEntries** | |控制在鏈結資料表中特定主機允許的最大項目數。 預設值是 **4** (項目)。 |
|**ClockAdjustmentAuditLimit** |Windows Server 2016 版本 1709 和更新版本；Windows 10 版本 1709 和更新版本 |指定可能記錄到目的電腦上 W32time 服務事件記錄檔中的最小本機時鐘調整值。 預設值是 **800** (每百萬 -PPM 中的部分)。 |
|**ClockHoldoverPeriod** |Windows Server 2016 版本 1709 和更新版本；Windows 10 版本 1709 和更新版本 |指出系統時鐘可名義上可保留的精確度最大秒數，而不需要與時間來源進行同步處理。 如果在 W32time 並未從其任何輸入提供者取得新樣本的情況下經過了這段時間，則 W32time 會開始重新探索時間來源。 Default：7,800 秒。 |
|**EventLogFlags** |所有版本 |控制時間服務所記錄的事件。<ul><li>**0x1**： 時間跳躍</li><li>**0x2**： 來源變更</li></ul>網域成員上的預設值是 **2**。 獨立用戶端和伺服器上的預設值是 **2**。 |
|**FrequencyCorrectRate** |所有版本 |控制時鐘的校正速率。 如果此值太小，時鐘會不穩定且過度校正。 如果此值太大，則時鐘要很久才會同步。 網域成員上的預設值是 **4**。 獨立用戶端和伺服器上的預設值是 **4**。<p>**注意** <br />0 對於 **FrequencyCorrectRate** 登錄項目來說是無效值。 在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 電腦上，如果此值設定為 **0**，則 Windows Time 服務會自動將其變更為 **1**。 |
|**HoldPeriod** |所有版本 |控制為了讓本機時鐘快速進入同步狀態而將尖峰偵測停用的時間長度。 尖峰是指出時間已偏離一定秒數的時間樣本，且通常會在持續傳回正確的時間樣本之後收到。 網域成員上的預設值是 **5**。 獨立用戶端和伺服器上的預設值是 **5**。 |
|**LargePhaseOffset** |所有版本 |指定當時差大於或等於此值 10<sup>-7</sup> 秒時便會視為尖峰。 網路中斷 (例如大量的流量) 可能會導致尖峰。 除非尖峰長時間存在，否則會加以忽略。 網域成員上的預設值是 **50000000**。 獨立用戶端和伺服器上的預設值是 **50000000**。 |
|**LastClockRate** |所有版本 |由 W32Time 維護。 其包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都可能會導致無法預期的結果。 網域成員上的預設值是 **156250**。 獨立用戶端和伺服器上的預設值是 **156250**。 |
|**LocalClockDispersion** |所有版本 |控制當唯一的時間來源是內建 CMOS 時鐘時，您所必須採用的散佈方式 (以秒為單位)。 網域成員上的預設值是 **10**。 獨立用戶端和伺服器上的預設值是 **10**。 |
|**MaxAllowedPhaseOffset** |所有版本 |指定 W32Time 會在時差達到多大 (以秒為單位) 時，嘗試使用時脈速率來調整電腦時鐘。 當時差超過此速率時，W32Time 便會直接設定電腦時鐘。 網域成員的預設值是 **300**。 獨立用戶端和伺服器的預設值是 **1**。 如需詳細資訊，請參閱[設定 Windows 時間服務重設電腦時鐘的方式](#configuring-how-windows-time-service-resets-the-computer-clock)。 |
|**MaxClockRate** |所有版本 |由 W32Time 維護。 其包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都可能會導致無法預期的結果。 網域成員的預設值是 **155860**。 獨立用戶端和伺服器的預設值是 **155860**。 |
|**MaxNegPhaseCorrection** |所有版本 |指定服務所進行的最大負數時間校正 (以秒為單位)。 如果服務判斷所需的變更大於此值，便會改為記錄事件。<p>**注意**<br />**0xFFFFFFFF** 值是特殊情況。 此值表示服務一律會更正時間。<p>網域成員的預設值是 **0xFFFFFFFF**。 獨立用戶端和伺服器的預設值是 **54,000** (15 小時)。|
|**MaxPollInterval** |所有版本 |指定系統輪詢間隔所允許的最大間隔 (以 log2 秒為單位)。 請注意，雖然系統必須根據排定的間隔進行輪詢，但提供者也可在經要求後拒絕產生樣本。 網域控制站的預設值是 **10**。 網域成員的預設值是 **15**。 獨立用戶端和伺服器的預設值是 **15**。 |
|**MaxPosPhaseCorrection** |所有版本 |指定服務所進行的最大正數時間校正 (以秒為單位)。 如果服務判斷所需的變更大於此值，便會改為記錄事件。<p>**注意**<br />**0xFFFFFFFF** 值是特殊情況。 此值表示服務一律會更正時間。<p>網域成員的預設值是 **0xFFFFFFFF**。 獨立用戶端和伺服器的預設值是 **54,000** (15 小時)。 |
|**MinClockRate** |所有版本 |由 W32Time 維護。 其包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都可能會導致無法預期的結果。 網域成員的預設值是 **155860**。 獨立用戶端和伺服器的預設值是 **155860**。 |
|**MinPollInterval** |所有版本 |指定系統輪詢間隔所允許的最小間隔 (以 log2 秒為單位)。 請注意，雖然系統不會比此值更頻繁地要求樣本，但提供者可以在排定間隔外的時間產生樣本。 網域控制站的預設值是 **6**。 網域成員的預設值是 **10**。 獨立用戶端和伺服器的預設值是 **10**。 |
|**PhaseCorrectRate** |所有版本 |控制相位差的校正速率。 指定較小的值可快速校正相位差，但可能會導致時鐘變得不穩定。 如果此值太大，則需要較長的時間才能校正相位差。<p>網域成員上的預設值是 **1**。 獨立用戶端和伺服器上的預設值是 **7**。<p>**注意**<br />0 對於 **PhaseCorrectRate** 登錄項目來說是無效值。 在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 電腦上，如果此值設定為 **0**，則 Windows Time 服務會自動將其變更為 **1**。 |
|**PollAdjustFactor** |所有版本 |控制是要增加還是減少系統輪詢間隔的決策。 值愈大，導致輪詢間隔減少的錯誤量就會愈小。 網域成員上的預設值是 **5**。 獨立用戶端和伺服器上的預設值是 **5**。 |
|**RequireSecureTimeSyncRequests** |Windows 8 和更新版本 |控制 DC 是否會回應使用舊版驗證通訊協定的時間同步要求。 如果已啟用 (設定為 **1**)，DC 將不會使用這類通訊協定回應要求。 這是布林值設定，預設值為 **0**。 |
|**SpikeWatchPeriod** |所有版本 |指定可疑的時差必須持續多久 (以秒為單位) 才會讓系統接受而將其設為正確值。 網域成員上的預設值是 **900**。 獨立用戶端和工作站上的預設值是 **900**。 |
|**TimeJumpAuditOffset** |所有版本 |不帶正負號的整數，可指出時間跳躍稽核閾值 (以秒為單位)。 如果時間服務會直接設定時鐘來調整本機時鐘，而且時間校正量大於此值，則時間服務會記錄稽核事件。 |
|**UpdateInterval** |所有版本 |指定每次相位校正調整所間隔的時鐘刻度數目。 網域控制站的預設值是 **100**。 網域成員的預設值是 **30,000**。 獨立用戶端和伺服器的預設值是 **360,000**。<p>**注意**<br />0 對於 **UpdateInterval** 登錄項目來說是無效值。 在執行 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 的電腦上，如果此值設定為 **0**，則 Windows Time 服務會自動將其變更為 **1**。|
|**UtilizeSslTimeData** |Windows 10 組建 1511 以後的 Windows 版本 |值為 **1** 表示 W32Time 會使用多個 SSL 時間戳記來植入極其不精確的時鐘。 |

### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters-subkey-entries"></a><a id="parameters"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters" subkey entries

| 登錄項目 | 版本 | 說明 |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |所有版本 |指出對等之間的同步允許非標準模式組合。 網域成員的預設值是 **1**。 獨立用戶端和伺服器的預設值是 **1**。 |
|**NtpServer** |所有版本 |指定可供電腦從中取得時間戳記的對等清單 (以空格分隔)，每行會包含一或多個 DNS 名稱或 IP 位址。 所列出的每個 DNS 名稱或 IP 位址都必須是唯一的。 連線到網域的電腦必須與更可靠的時間來源同步，例如美國官方的時鐘。  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive:如需此模式的詳細資訊，請參閱 [Windows 時間伺服器：3.3 操作模式](https://go.microsoft.com/fwlink/?LinkId=208012)。</li><li>0x08 用戶端</li></ul><br />網域成員上沒有此登錄項目的預設值。 獨立用戶端和伺服器上的預設值是 **time.windows.com,0x1**。 |
|**ServiceDll** |所有版本 |由 W32Time 維護。 其包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都可能會導致無法預期的結果。 此 DLL 在網域成員和獨立用戶端與伺服器上的預設位置是 %windir%\System32\W32Time.dll。 |
|**ServiceMain** |所有版本 |由 W32Time 維護。 其包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都可能會導致無法預期的結果。 網域成員上的預設值是 **SvchostEntry_W32Time**。 獨立用戶端和伺服器上的預設值是 **SvchostEntry_W32Time**。 |
|**類型** |所有版本 |指出要接受作為同步來源的對等：  <ul><li>**NoSync**。 時間服務不會與其他來源同步。</li><li>**NTP**： 時間服務會從 **NtpServer** 中指定的伺服器同步。 登錄項目。</li><li>**NT5DS**。 時間服務會從網域階層同步。  </li><li>**AllSync**。 時間服務會使用所有可用的同步機制。  </li></ul>網域成員上的預設值是 **NT5DS**。 獨立用戶端和伺服器上的預設值是 **NTP**。 |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient-subkey-entries"></a><a id="ntpclient"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient" subkey entries

|登錄項目 |版本 |說明 |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |所有版本 |指出對等之間的同步允許非標準模式組合。 網域成員的預設值是 **1**。 獨立用戶端和伺服器的預設值是 **1**。|
|**CompatibilityFlags** |所有版本 |指定下列相容性旗標和值：<ul><li>**0x00000001** - DispersionInvalid</li><li>**0x00000002** - IgnoreFutureRefTimeStamp</li><li>**0x80000000** - AutodetectWin2K</li><li>**0x40000000** - AutodetectWin2KStage2</li></ul>網域成員的預設值是 **0x80000000**。 獨立用戶端和伺服器的預設值是 **0x80000000**。 |
|**CrossSiteSyncFlags** |所有版本 |決定服務是否要選擇電腦網域以外的同步夥伴。 其選項和值如下：<ul><li>**0** - None</li><li>**1** - PdcOnly</li><li>**2** - All</li></ul>如果未設定 NT5DS 值，則會忽略此項目值。 網域成員的預設值是 **2**。 獨立用戶端和伺服器的預設值是 **2**。 |
|**DllName** |所有版本 |指定時間提供者的 DLL 位置。<p>此 DLL 在網域成員和獨立用戶端與伺服器上的預設位置是 **%windir%\System32\W32Time.dll**。 |
|**已啟用** |所有版本 |指出是否要在目前的 Time 服務中啟用 NtpClient 提供者。<ul><li>**1** - 是</li><li>**0** - 否</li></ul>網域成員上的預設值是 **1**。 獨立用戶端和伺服器上的預設值是 **1**。|
|**EventLogFlags** |所有版本 |指定 Windows Time 服務所記錄的事件。<ul><li>**0x1** - 可連線性變更</li><li>**0x2** - 大型樣本誤差 (此值僅適用於 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2)</li></ul>網域成員上的預設值是 **0x1**。 獨立用戶端和伺服器上的預設值是 **0x1**。|
|**InputProvider** |所有版本 |指出是否要以 InputProvider 的形式來啟用 NtpClient，以從 NtpServer 取得時間資訊。 NtpServer 是時間伺服器，其會傳回可用於同步本機時鐘的時間樣本，以回應網路上的用戶端時間要求。 <ul><li>**1** - 是</li><li>**0** - 否</li></ul>網域成員和獨立用戶端的預設值是 **1**。 |
|**LargeSampleSkew** |所有版本 |指定用於記錄的大型樣本誤差 (以秒為單位)。 為了遵守美國證券交易委員會 (SEC) 的規範，此值應設為三秒。 只有在已為 0x2 大型樣本誤差明確設定 **EventLogFlags** 時，才會記錄此設定的事件。 網域成員上的預設值是 3。 獨立用戶端和伺服器上的預設值是 3。 |
|**ResolvePeerBackOffMaxTimes** |所有版本 |指定當為了找到要與其同步的對等所做的嘗試重複失敗時，要將等候間隔加倍的失敗次數上限。 值為 0 表示等候間隔一律為最小值。 網域成員上的預設值是 **7**。 獨立用戶端和伺服器上的預設值是 **7**。 |
|**ResolvePeerBackoffMinutes** |所有版本 |指定一開始要等候多久的間隔 (以分鐘為單位)，才嘗試找出要與其同步的對等。 網域成員上的預設值是 **15**。 獨立用戶端和伺服器上的預設值是 **15**。  |
|**SpecialPollInterval** |所有版本 |指定手動對等的特殊輪詢間隔 (以秒為單位)。 已啟用 **SpecialInterval** 0x1 旗標時，W32Time 將會使用此輪詢間隔，而不是作業系統所決定的輪詢間隔。 網域成員上的預設值是 **3,600**。 獨立用戶端和伺服器上的預設值是 **604,800**。<br/><br/>**SpecialPollInterval** 對組建 1703 來說是新項目，會包含在 **MinPollInterval** 和 **MaxPollInterval** 設定的登錄值中。|
|**SpecialPollTimeRemaining** |所有版本 |由 W32Time 維護。 其包含 Windows 作業系統所使用的保留資料。 其會指定 W32Time 要在電腦重新啟動後多久 (以秒為單位) 才會重新同步。 對此設定所做的任何變更都會導致無法預期的結果。 網域成員和獨立用戶端與伺服器上的預設值會保留空白。 |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver-subkey-entries"></a><a id="ntpserver"></a>"HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer" 子機碼項目

|登錄項目 |版本 |說明 |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |所有版本 |指出用戶端與伺服器之間的同步允許非標準模式組合。 網域成員的預設值是 **1**。 獨立用戶端和伺服器的預設值是 **1**。 |
|**DllName** |所有版本 |指定時間提供者的 DLL 位置。 此 DLL 在網域成員和獨立用戶端與伺服器上的預設位置是 **%windir%\System32\W32Time.dll**。  |
|**已啟用** |所有版本 |指出是否要在目前的 Time 服務中啟用 NtpServer 提供者。 <ul><li>**1** - 是</li><li>**0** - 否</li></ul>網域成員上的預設值是 **1**。 獨立用戶端和伺服器上的預設值是 **1**。 |
|**InputProvider** |所有版本 |指出是否要以 InputProvider 的形式來啟用 NtpClient，以從 NtpServer 取得時間資訊。 NtpServer 是時間伺服器，其會傳回可用於同步本機時鐘的時間樣本，以回應網路上的用戶端時間要求。 <ul><li>**1** - 是</li><li>**0** - 否 = 0 </li></ul>網域成員和獨立用戶端的預設值：1 |

## <a name="reference-pre-set-values-for-the-windows-time-service-gpo-settings"></a>參考：Windows 時間服務 GPO 設定的預先設定值

下表列出與 Windows Time 服務相關聯的全域群組原則設定，以及與每個設定相關聯的預先設定值。 如需有關每個設定的詳細資訊，請參閱本文先前提到的[參考：Windows 時間服務的登錄項目](#reference-windows-time-service-registry-entries)。 下列設定包含在名為 **全域組態設定** 的單一 GPO 中。

### <a name="pre-set-values-for-global-group-policy-settings"></a>「全域群組原則」設定的預先設定值

|群組原則設定|預先設定值|
| --- | --- |
|**AnnounceFlags**|**10**|
|**EventLogFlags**|**2**|
|**FrequencyCorrectRate**|**4**|
|**HoldPeriod**|**5**|
|**LargePhaseOffset**|**1,280,000**|
|**LocalClockDispersion**|**10**|
|**MaxAllowedPhaseOffset**|**300**|
|**MaxNegPhaseCorrection**|**54,000** (15小時)|
|**MaxPollInterval**|**15**|
|**MaxPosPhaseCorrection**|**54,000** (15小時)|
|**MinPollInterval**|**10**|
|**PhaseCorrectRate**|**7**|
|**PollAdjustFactor**|**5**|
|**SpikeWatchPeriod**|**90**|
|**UpdateInterval**|**100**|

### <a name="pre-set-values-for-configure-windows-ntp-client-settings"></a>「設定 Windows NTP 用戶端」設定的預先設定值

下表列出 **設定 Windows NTP 用戶端** GPO 的可用設定以及與 Windows Time 服務相關聯的預先設定值。 如需有關每個設定的詳細資訊，請參閱本文先前提到的[參考：Windows 時間服務的登錄項目](#reference-windows-time-service-registry-entries)。

|群組原則設定|預先設定值|
|------------------------|-----------------|
|**NtpServer**|time.windows.com, **0x1**|
|**類型**|預設選項：<ul><li>**NTP**。 用於未加入網域的電腦。</li><li>**NT5DS.** 用於已加入網域的電腦。</li></ul>|
|**CrossSiteSyncFlags**|**2**|
|**ResolvePeerBackoffMinutes**|**15**|
|**ResolvePeerBackoffMaxTimes**|**7**|
|**SpecialPollInterval**|**3,600**|
|**EventLogFlags**|**0**|

## <a name="reference-network-ports-that-the-windows-time-service-uses"></a>參考：Windows 時間服務所使用的網路連接埠

Windows Time 遵循 NTP 規範，因此必須使用 UDP 連接埠 123 來進行所有的時間同步通訊。 此連接埠保留供 Windows Time 使用，且會永遠保持保留狀態。 每當電腦同步其時鐘或提供時間給另一部電腦時，就會在 UDP 連接埠 123 上執行通訊。

> [!NOTE]
> 如果您的電腦有多個網路介面卡 (也稱為 *多重主目錄* 電腦)，您就無法根據網路介面卡來選擇性地啟用 Windows Time 服務。

## <a name="related-information"></a>相關資訊

下列資源包含與本節相關的其他資訊。

- IETF RFC 資料庫中的 RFC 1305