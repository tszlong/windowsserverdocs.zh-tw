---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Windows 時間服務工具和設定
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 70b7ee4a9955e023d1664a3c29295a22cd5dc75b
ms.sourcegitcommit: fd6a46b702b235f38d90814dd769106ab37cd61b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="windows-time-service-tools-and-settings"></a>Windows 時間服務工具和設定

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


**在本區段中**  
  
-   [Windows 時間服務工具](#w2k3tr_times_tools_dyax)  
  
-   [Windows 時間服務登錄項目](#w2k3tr_times_tools_uhlp)  
  
-   [Windows 時間服務群組原則設定](#w2k3tr_times_tools_vwtt)  
  
-   [Windows 時間服務使用的網路連接埠](#w2k3tr_times_tools_suxb)  
  
-   [相關的資訊](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> 本主題包含工具與設定 Windows 時間服務 (W32Time) 的相關資訊。 如果您只想要同步處理時間加入網域的 client 的電腦，請查看[設定為自動網域時間同步 client 電腦](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。 了解如何設定 Windows 時間服務的其他主題查看主題一節中的清單中的[的位置找到 Windows 時間服務設定資訊，](https://technet.microsoft.com/library/cc773061.aspx)。  
  
> [!CAUTION]  
> 您不應該使用 Net time 命令設定或設定 Windows 時間服務順利執行時的時間。  

此外，在較舊的電腦可執行 Windows XP 或更早版本，命令網路時間 /querysntp 顯示名稱與電腦同步處理，設定，但 NTP 伺服器只適用於電腦的時間 client 設定為 NTP 或 AllSync 網路時間通訊協定 (NTP) 伺服器。 命令已因為取代。  
  
大多數成員網域的電腦有 NT5DS，這表示同步處理時間從網域層的時間 client 類型。 這只有一般例外是網域控制站為的樹系根網域中，這通常設定為使用外部的時間來源同步處理時間主要網域控制站 (PDC) 模擬器操作主機功能。 若要檢視電腦的時間 client 設定，請執行 W32tm//query /configuration 從提升權限的命令提示字元中開始在 Windows Server 2008 和 Windows Vista、命令及朗讀**輸入**列中的命令的輸出。 如需詳細資訊，請查看[Windows 時間服務的運作方式](https://go.microsoft.com/fwlink/?LinkId=117753)。 您可以執行命令**reg 查詢 HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**及朗讀的值**NtpServer**中的命令的輸出。  
  
> [!IMPORTANT]  
> Windows Server 2016 之前 W32Time 服務設計時效性的應用程式需求。  不過，Windows Server 2016 的更新現在可讓您實作的 1ms 方案網域中的準確度。  查看[Windows 2016 正確時間](accurate-time.md)和[設定為高不正確的環境 Windows 時間服務支援邊界](https://go.microsoft.com/fwlink/?LinkID=179459)的詳細資訊。  
  
## <a name="w2k3tr_times_tools_dyax"></a>Windows 時間服務工具  
下列工具會相關 Windows 時間服務使用。  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: Windows 時間  
**明細**  

這項工具已安裝 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 預設安裝的一部分。  
  
**版本的相容性**  
  
這個工具適用於 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 預設安裝。  
  
W32tm.exe 用於設定 Windows 時間服務設定。 它也可以診斷問題時間服務使用。 W32tm.exe 是設定、監視或疑難排解 Windows 時間服務慣用的命令列工具。  
  
下表描述 W32tm.exe 搭配使用的參數。  
  
**W32tm.exe 主要參數**  
  
|參數|描述|  
|-------------|---------------|  
|W32tm 日嗎？|W32tm 命令列協助|  
|W32tm//register|暫存器時間服務執行的服務，並新增登錄預設設定。|  
|W32tm//unregister|移除註冊時間服務，並移除登錄所有設定的資訊。|  
|w32tm//monitor<br /><br />[/domain:<domain name>] [/computers:<name>[，<name>[，<name>...]]][/Threads:<num>]|網域-指定監視的網域。 如果給定不網域名稱，或指定網域都電腦選項，就會使用預設的網域。 此選項可能使用超過一次。<br /><br />電腦-監視給定的電腦清單。 電腦名稱是以逗號分隔，不空間。 如果名稱以使用 ' *'，就會被視為 PDC。 此選項可能使用超過一次。<br /><br />執行緒-指定分析同時的電腦數目。 預設值為 3。 允許的範圍是 1-50。|  
|w32tm /ntte <NT time epoch>|在轉換 NT 系統時間 (10 ^-7) 0 h 1-1 月 1601，可讀取的格式為秒的間隔。|  
|w32tm /ntpte <NTP time epoch>|在轉換 NTP 時間 (2 ^-32) 0 h 1-1 月 1900，可讀取的格式為秒的間隔。|  
|w32tm//resync<br /><br />[/computer:<computer>]<br /><br />[/nowait]<br /><br />[/Rediscover]<br /><br />[/Soft]|它應該時鐘、儘快丟棄所有累積的錯誤統計資料同步處理告訴電腦。<br /><br />電腦：<computer> -指定應該同步處理的電腦。 如果您不指定，將會同步處理本機電腦。<br /><br />[不等待-不等待重新同步化發生。立即退款。 否則，請等候重新同步化傳回之前完成。<br /><br />重新尋找-偵測網路設定與重新尋找網路來源，然後重新同步處理。<br /><br />軟-同步使用現有錯誤的統計資料。 不實用、提供相容性。|  
|w32tm /stripchart<br /><br />/computer:<target><br /><br />[/Period:<refresh>]<br /><br />[/dataonly]<br /><br />[/Samples:<count>]<br/><br/>[/rdtsc]<br/>|顯示在此電腦與另一部電腦之間時差。<br /><br />電腦：<target> -測量時差針對電腦。<br /><br />時間：<refresh> -範例，以秒之間的時間。 預設為 2 秒。<br /><br />dataonly-顯示圖形的資料。<br /><br />範例：<count> -收集<count>範例，然後停止。 如果未指定，會收集範例直到**Ctrl + C**按下按鍵。<br/><br/>rdtsc：範例中，這個選項會列印加上標題 RdtscStart，以逗號分隔值 RdtscEnd，FileTime、RoundtripDelay，而不是文字圖形 NtpOffset。<br/><ul><li>[RdtscStart – RDTSC（讀取戳記計數器）](https://en.wikipedia.org/wiki/Time_Stamp_Counter)產生 NTP 要求之前，所收集的值。</li><li>RdtscEnd – RDTSC（讀取戳記計數器）值收集只之後 NTP 回應已收到及處理。</li><li>FileTime – 用於 NTP 要求本機 FILETIME 值。</li><li>RoundtripDelay – 時間經過秒之間產生 NTP 要求和處理收到的 NTP 回應，根據 NTP 往返計算計算。</li><li>NTPOffset – 時間在本機電腦之間 NTP 伺服器，秒位移計算根據 NTP 位移運算。</li></ul>|
|w32tm /config<br /><br />[/computer:<target>]<br /><br />[/Update]<br /><br />[/manualpeerlist:<peers>]<br /><br />[/syncfromflags:<source>]<br /><br />[/LocalClockDispersion:<seconds>]<br /><br />[/Reliable:（是和 #124; 無)]<br /><br />[/largephaseoffset:<milliseconds>]|電腦：<target> -調整設定的<target>。 如果您不指定，預設值是本機電腦。<br /><br />更新-通知時間服務設定已變更，導致，變更才會生效。<br /><br />manualpeerlist:<peers> -設定手動對等清單<peers>，即空格分隔 DNS 和/或 IP 位址的清單。 指定時多個對等，必須使用此選項住報價。<br /><br />syncfromflags:<source> -NTP client 應該同步哪些來源的設定。 <source> 應該是以逗號分隔的清單這些關鍵字（不區分大小寫）：<br /><br />手動-包含同儕手動對等清單。<br /><br />DOMHIER-同步網域階層網域控制站 (DC)。<br /><br />LocalClockDispersion:<seconds> -設定內部的正確性時鐘假設 W32Time 會當機的時間不能取得已設定來源。<br /><br />為了: (是和 #124; 無)-設定此電腦是否可靠的時間來源。<br /><br />只有有意義的網域控制站在這個設定。<br /><br />-這台電腦是可靠的時間服務。<br /><br />不需要-這部電腦不可靠的時間服務。<br /><br />largephaseoffset:<milliseconds> -設定本機之間的時差及網路 W32Time 會考慮增量的時間。|  
|w32tm /tz|顯示目前的時區設定。|  
|w32tm /dumpreg<br /><br />[/Subkey:<key>]<br /><br />[/computer:<target>]|顯示特定的登錄相關聯的值。<br /><br />預設金鑰是 HKLM\System\CurrentControlSet\Services\W32Time<br /><br />（根金鑰時間服務）。<br /><br />子：<key> -顯示子相關聯的值<key>的預設金鑰。<br /><br />電腦：<target> -查詢登錄設定電腦 <target>|  
|w32tm//query [/computer:<target>] {/source 與 #124;/configuration 和 #124;/Peers 與 #124;/status} [/verbose]|此參數是第一次進行提供 Windows 時間 client 版本的 Windows Vista 和 Windows Server 2008。<br /><br />顯示電腦的 Windows 時間服務的資訊。<br /><br />**電腦：<target>** -查詢資訊的**<target>**。 如果您不指定，預設值是本機電腦。<br /><br />**來源**-顯示的時間來源。<br /><br />**設定**-顯示的 [執行的階段和位置設定來自設定。 在 [詳細資訊模式，會顯示定義或未使用設定太。<br /><br />**同儕**-顯示等其狀態的清單。<br /><br />**狀態**-顯示 Windows 時間服務狀態。<br /><br />**詳細資訊**-設定的詳細資訊模式來顯示更多的資訊。|  
|w32tm//debug {/disable 與 #124;{/Enable /file:<name> /size:<bytes> /entries:<value> [/truncate]}}|此參數是第一次進行提供 Windows 時間 client 版本的 Windows Vista 和 Windows Server 2008。<br /><br />讓或停用 Windows 時間服務私人登入本機電腦。<br /><br />**停用**-停用私人登入。<br /><br />**讓**-讓私人登入。<br /><br />-   **檔案：<name>** -指定絕對檔案名稱。<br />-   **大小：<bytes>** -指定循環登入的大小上限。<br />-   **項目：<value>** -包含旗標指定號碼，以逗號分隔指定應該登入資訊的類型的清單。 若要 300 0 的有效的數字。 數字範圍無效，除了一個數字 0-100,103，例如 106。 0-300 值是登入的所有資訊。<br /><br />**截斷**-如果有的話截斷檔案。|  
  
如需有關**W32tm.exe**，查看協助及 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 的支援中心。  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Windows 時間服務登錄項目  
Windows 時間服務下列登錄無關。  
  
使用疑難排解或驗證需要的設定的套用做為參考提供這項資訊。 建議您不要不直接編輯登錄除非另有其他的替代方案。 變更登錄無法驗證再套用，如此一來，不正確的值可以儲存或 windows 作業系統。 這可能導致處於無法復原錯誤，系統中。  
  
可能的話，請使用群組原則」或其他 Windows 工具，例如 Microsoft Management Console (MMC)，以完成任務，而非編輯登錄直接。 如果您必須編輯登錄，小心謹慎。  
  
> [!WARNING]  
> 部分的群組原則物件 (GPO) 設定的不同對應的預設登錄項目中的系統管理範本檔案 (System.adm) 設定預設值。 如果您打算使用 GPO 任何 Windows 時間設定，請務必檢視的[Windows 時間服務群組原則設定預設值是不同的對應 Windows 時間服務登錄的項目在 Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066)。 這個問題，適用於 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。  
  
Windows 時間服務許多登錄的群組原則設定具有相同名稱的相同。 群組原則設定對應至登錄位在相同名稱的項目：  
  
>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
這個登錄位置，有數個登錄按鍵。 Windows 時間設定儲存所有的這些按鍵在值：
* [參數](#Parameters)
* [設定](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  
> [!NOTE]  
> 許多登錄 W32Time 一節中值用來 W32Time 來儲存資訊。 在任何時候應該不會手動變更下列值。 請勿修改本節中的設定，除非您熟悉的設定，並的特定新的值，將會如預期般運作。 在位於登錄下列項目：

>>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**  
  
參數一部分會儲存在時鐘刻度登錄中，某些位於秒。 從時鐘刻度轉秒鐘的時間：  
  
-   1 分鐘 = 60 秒  
  
-   1 秒 = 1000 ms  
  
-   1 ms 所述，= 10000 時鐘刻度在 Windows 系統中，[DateTime.Ticks 屬性](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx)。  
  
例如，5 分鐘就會成為 5 * 60\ * 1000\ * 10000 = 3000000000 時鐘刻度。 

所有版本都包含 Windows 7、Windows 8、Windows 10、Windows Server 2008 和 Windows Server 2008 R2、Windows Server 2012、Windows Server 2012R2、Windows Server 2016。  只有較新的 Windows 版本提供了一些項目。


#### <a name="Parameters"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|登錄項目|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|所有|此項目指示標準模式組合，允許對等裝置之間同步處理中。 網域成員的預設值為 1。 獨立戶端與伺服器的預設值為 1。|
|NtpServer|所有|此項目指定空格分隔對等裝置清單從的電腦取得戳記時間，包含一或多個 DNS 名稱或一行的 IP 位址。 必須唯一每個 DNS 名稱或列出的 IP 位址。 連接到網域的電腦必須更可靠的時間來源，例如官方美國時間時鐘與同步處理。  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 為對稱主動-有關更多此模式，請查看[Windows 時間伺服器：作業模式 3.3](https://go.microsoft.com/fwlink/?LinkId=208012)。</li><li>0x08 client</li></ul><br />還有網域成員此登錄項目的不預設值。 獨立戶端伺服器上的預設值是 time.windows.com，0x1。<br /><br />注意：的可用 NTP 伺服器的詳細資訊，請查看[Microsoft 知識庫文章 262680-在網際網路有簡單網路時間通訊協定 (SNTP) 時間伺服器的清單](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|所有|此項目是由 W32Time 維護。 它包含保留的資料所使用的 Windows 作業系統，這項設定的任何變更都可能造成無法預期的結果。 這個 DLL 網域成員並獨立戶端和伺服器上的預設位置為 %windir%\system32\w32time.dll。  |
|ServiceMain|所有|此項目是由 W32Time 維護。 它包含保留的資料所使用的 Windows 作業系統，這項設定的任何變更都可能造成無法預期的結果。 網域成員預設值為 SvchostEntry_W32Time。 獨立戶端伺服器上的預設值是 SvchostEntry_W32Time。  "|
|輸入|所有|表示對等接受從同步處理的此項目：  <ul><li>**非同步**。 其他來源的時間服務不會同步。</li><li>**NTP。** 從中指定的伺服器的時間服務會同步處理**NtpServer**。 登錄項目。</li><li>**NT5DS**。 時間服務會從網域層同步處理。  </li><li>**AllSync**。 時間服務使用的所有可用的同步處理機制。  </li></ul>網域成員預設值是**NT5DS**。 獨立戶端伺服器上的預設值是**NTP**。   |

#### <a name="Configuration"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config

|登錄項目|版本|描述|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|所有|此項目可控制是否這台電腦標示為可靠的時間伺服器。 除非它也會標示為時間伺服器的電腦不標示為可靠。<br /> -0x00 時間伺服器  <br /> -0x01 一律時間伺服器  <br /> -0x02 自動時間伺服器  <br /> -0x04 一律可靠的時間伺服器  <br /> -0x08 自動可靠的時間伺服器  <br />網域成員的預設值為 10。 獨立戶端與伺服器的預設值為 10。|
|EventLogFlags|所有|此項目控制登時間服務的活動。  <br />-時間捷徑：0x1  <br />-來源變更：0x2  <br />網域成員的預設值為 2。 獨立戶端伺服器上的預設值為 2。  |
|FrequencyCorrectRate|所有|此項目控制校正時鐘後的速度。 如果此值太小，時鐘變得不穩定和 overcorrects。 如果值太大，時鐘需要很長的時間來同步處理。 網域成員的預設值為 4。 獨立戶端伺服器上的預設值為 4。  <br /><br />請注意，0 不正確的值為 FrequencyCorrectRate 登錄項目。 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 電腦上，如果值設定為 0 Windows 時間服務會自動變更為 1。  |
|HoldPeriod|所有|此項目控制一段時間的增量偵測已停用，以快速讓同步處理本機電腦時鐘。 增量，而且時間範例指出當時超出數秒鐘，通常收到之後已一致傳回對的時機範例。 網域成員的預設值為 5。 獨立戶端伺服器上的預設值為 5。  |
|LargePhaseOffset|所有|此項目指定時間位移大於或這個 10 中的值為<sup>-7</sup>秒被視為突然增加。 網路流量大量例如干擾可能會造成突然增加。 除非長一段時間持續發生，將會忽略突然增加。 網域成員預設值為 50000000。 獨立戶端伺服器上的預設值是 50000000。  |
|LastClockRate|所有|此項目是由 W32Time 維護。 它包含保留的資料所使用的 Windows 作業系統，這項設定的任何變更都可能造成無法預期的結果。 網域成員預設值為 156250。 獨立戶端伺服器上的預設值是 156250。  |
|LocalClockDispersion|所有|此項目控制起始（以秒計），您必須假設時，才來源是建 CMOS 時鐘。 網域成員預設值為 10。 獨立戶端伺服器上的預設值為 10。|
|MaxAllowedPhaseOffset|所有|此項目指定 W32Time 嘗試使用時鐘速率調整電腦時鐘最大時差（以秒計）。 當時差超過速率這個時，W32Time 設定電腦時鐘直接。 網域成員的預設值為 300。 獨立戶端與伺服器的預設值為 1。  [如需詳細資訊查看下列](#MaxAllowedPhaseOffset)。|
|MaxClockRate|所有|此項目是由 W32Time 維護。 它包含保留的資料所使用的 Windows 作業系統，這項設定的任何變更都可能造成無法預期的結果。 網域成員預設值為 155860。 獨立戶端與伺服器的預設值是 155860。  |
|MaxNegPhaseCorrection|所有|此項目秒服務所做出中指定的最大負時間修正。 如果服務，不需要變更更長的時間，它登事件改為。 特殊案例：0xFFFFFFFF 表示一定要進行修正時間。 網域成員預設值為 0xFFFFFFFF。 獨立戶端與伺服器的預設值為 54000 (15 小時)。  |
|MaxPollInterval|所有|此項目指定最大的時間間隔，log2 秒允許系統輪詢間隔。 請注意，雖然系統必須輪詢依據排定的時間間隔，提供者可以拒絕製作範例，若要這樣做需要時才。 網域控制站的預設值為 10。 網域成員的預設值為 15。 獨立戶端與伺服器的預設值為 15。  |
|MaxPosPhaseCorrection|所有|此項目秒服務所做出中指定的最大正時間修正。 如果服務，不需要變更更長的時間，它登事件改為。 特殊案例：0xFFFFFFFF 表示一定要進行修正時間。 網域成員預設值為 0xFFFFFFFF。 獨立戶端與伺服器的預設值為 54000 (15 小時)。  |
|MinClockRate|所有|此項目是由 W32Time 維護。 它包含保留的資料所使用的 Windows 作業系統，這項設定的任何變更都可能造成無法預期的結果。 網域成員預設值為 155860。 獨立戶端與伺服器的預設值是 155860。  |
|MinPollInterval|所有|此項目指定最小的間隔 log2 秒鐘，以允許系統輪詢間隔。 請注意，雖然系統不會超過此要求範例，提供者可以產生範例時候以外排定的時間間隔。 網域控制站的預設值為 6。 網域成員的預設值為 10。 獨立戶端與伺服器的預設值為 10。  |
|PhaseCorrectRate|所有|此項目控制階段錯誤修正的速度。 指定小值能修正形同階段錯誤快速，但可能會造成時鐘變得不穩定。 如果值太大，花費較長的時間來更正錯誤階段。 <br /><br />網域成員預設值為 1。 獨立戶端伺服器上的預設值為 7。<br /><br />注意：0 會是無效的值為 PhaseCorrectRate 登錄項目。 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 電腦上，如果值設定為 0，Windows 時間服務會自動變更其 1。  |
|PollAdjustFactor|所有|此項目控制判斷来增加或減少系統輪詢間隔。 較大的值，少量錯誤，會導致將會減少輪詢間隔。 網域成員的預設值為 5。 獨立戶端伺服器上的預設值為 5。 |
|SpikeWatchPeriod|所有|此項目指定的時間，必須保存的可疑時差之前接受為正確（以秒計）。 網域成員的預設值為 900。 獨立戶端和工作站預設值為 900。  |
|TimeJumpAuditOffset|所有|簽署指出整數時間捷徑稽核臨界值，以秒。 如果時間服務以直接設定時鐘調整本機時鐘與的時間修正大於這個值，然後時間服務登稽核約會。|
|UpdateInterval|所有|此項目指定時鐘刻度階段修正調整之間的數字。 網域控制站的預設值為 100。 網域成員的預設值為 30000。 獨立戶端與伺服器的預設值是 360,000。  <br /><br />**注意**：零會是無效的值為 UpdateInterval 登錄項目。 在電腦上執行 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2，如果值設定為 0 Windows 時間服務會自動變更其 1。<br /><br />下列三種登錄並非 W32Time 預設設定，但新增登錄才能取得提升登入功能。 藉由變更 EventLogFlags 設定群組原則物件編輯器] 中的值可以修改系統事件登入來登入資訊。 根據預設，時間服務登入事件檢視器中每次建立它切換到新的時間來源。<br /><br />**警告**：預設值的群組原則物件 (GPO) 設定的不同適當的對應的設定中的系統管理範本檔案 (System.adm) 部分預設登錄項目。 如果您打算使用 GPO 任何 Windows 時間設定，請務必檢視的[Windows 時間服務群組原則設定預設值是不同的對應 Windows 時間服務登錄的項目在 Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066)。 這個問題，適用於 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。 |
|UtilizeSslTimeData|張貼 Windows 10 組建 1511|1 的項目表示 W32Time 將會使用多個 SSL 時間戳記到植時鐘大致不正確的。|

為了讓 W32Time 登入，必須先加入登錄下列項目：  

|登錄項目|版本|描述|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|所有|此項目控制量建立 Windows 次登入檔案中的項目。 預設值是不，這不登任何 Windows 階段的活動。 有效值 0 到 300。 這個值不會影響事件登入的項目通常會由 Windows 時間|
|FileLogName|所有|此項目控制位置和檔案名稱 Windows 次登入。 預設值為空白，以及應該不會變更除非**FileLogEntries**變更。 有效的值為完整路徑和檔案名稱 Windows 的時間用來建立檔案登入。 這個值不會影響事件登入的項目通常會由 Windows 的時間。  |
|FileLogSize|所有|此項目控制循環登入 Windows 階段登入檔案的行為。 當**FileLogEntries**和**FileLogName**所定義，此項目大小中, 定義位元組，以允許登入檔案，以到達之前，請先使用新的項目覆寫舊登入的項目。 任意正有效，且 3000000 建議。 這個值不會影響事件登入的項目通常會由 Windows 的時間。  |



#### <a name="NtpClient"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|登錄項目|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|所有|此項目指示標準模式組合，允許對等裝置之間同步處理中。 網域成員的預設值為 1。 獨立戶端與伺服器的預設值為 1。|
|CompatibilityFlags|所有|此項目指定下列相容性旗標和值： <br /><br />-DispersionInvalid: 0x00000001  <br />-IgnoreFutureRefTimeStamp: 0x00000002  <br /> -AutodetectWin2K: 0x80000000  <br />-AutodetectWin2KStage2: 0x40000000  <br /><br />網域成員預設值為 0x80000000。 獨立戶端與伺服器的預設值是 0x80000000。  |
|CrossSiteSyncFlags|所有|此項目判斷是否服務選擇同步夥伴以外的網域的電腦。 值與選項︰  <br /><br />-無：0  <br />-PdcOnly: 1  <br />-所有：2  <br /><br />如果未設定 NT5DS 值，忽略此值。 網域成員的預設值為 2。 獨立戶端與伺服器的預設值為 2。  |
|DllName|所有|此項目指定的時間提供 DLL 的位置。  <br /><br />這個 DLL 網域成員並獨立戶端和伺服器上的預設位置為 %windir%\system32\w32time.dll。|
|支援|所有|此項目會指出是否尚未在目前的時間服務 NtpClient 提供者。  <br /><ul><li>1 [是]</li><li>無 0</li></ul>網域成員預設值為 1。 獨立戶端伺服器上的預設值為 1。|
|EventLogFlags|所有|此項目指定登入 Windows 時間服務的活動。<ul><li>0x1 性變更</li><li>0x2 大型範例傾斜（這是適用於 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 只）</li></ul>網域成員預設值為 0x1。 獨立戶端伺服器上的預設值是 0x1。|
|InputProvider|所有|此項目會指出是否支援 NtpClient 提供者。  <ul><li>1 [是]  </li><li>無 0 </li></ul>網域成員預設值為 1。 獨立戶端伺服器上的預設值為 1。  |
|LargeSampleSkew|所有|此項目指定大型範例傾斜來登入秒。 為遵守用規格安全性和 Exchange 委員會（秒），這應會設定以三秒鐘。 事件將會登入此設定只 EventLogFlags 明確設定 0x2 大型範例傾斜時。 網域成員的預設值為 3。 獨立戶端伺服器上的預設值為 3。  |
|ResolvePeerBackOffMaxTimes|所有|此項目指定的時間按兩下時重複的等候時間間隔上限嘗試找出等與無法同步。 為零表示的等候時間間隔都最小值。 網域成員的預設值為 7。 獨立戶端伺服器上的預設值為 7。 |
|ResolvePeerBackoffMinutes|所有|此項目指定等待，分鐘，再嘗試找出等同步處理的初始間隔。 網域成員預設值為 15。 獨立戶端伺服器上的預設值為 15。  |
|SpecialPollInterval|所有|此項目指定特殊輪詢間隔（秒）手動對等裝置。 支援的 SpecialInterval 0x1 旗標是，當 W32Time 使用作業系統判斷而不是輪詢間隔此輪詢間隔。 網域成員預設值為 3600。 獨立戶端伺服器上的預設值為 604800。<br/><br/>新的組建 1702 SpecialPollInterval 包含 MinPollInterval 和 MaxPollInterval 組態登錄值。|
|SpecialPollTimeRemaining|所有|此項目是由 W32Time 維護。 它包含所使用的 Windows 作業系統保留的資料。 它秒再重新開機之後，將會重新同步處理 W32Time 中指定的時間。 這項設定的任何變更都可能造成無法預期的結果。 在這兩個網域成員和獨立戶端與伺服器上的預設值是空白的。  |

#### <a name="NtpServer"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|登錄項目|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|所有|此項目指示標準模式組合，允許之間戶端與伺服器同步處理中。 網域成員的預設值為 1。 獨立戶端與伺服器的預設值為 1。|
|DllName|所有|此項目指定的時間提供 DLL 的位置。<br /><br />這個 DLL 網域成員並獨立戶端和伺服器上的預設位置為 %windir%\system32\w32time.dll。  |
|支援|所有|此項目會指出是否尚未在目前的時間服務 NtpServer 提供者。 <ul><li>1 [是]</li><li>無 0</li></ul>網域成員預設值為 1。 獨立戶端伺服器上的預設值為 1。  |
|InputProvider|所有|此項目會指出是否支援 NtpServer 提供者。  <ul><li>1 [是]  </li><li>無 0 </li></ul>網域成員預設值為 1。 獨立戶端伺服器上的預設值為 1。  |

#### <a name="MaxAllowedPhaseOffset"></a>MaxAllowedPhaseOffset 資訊
為了讓來設定電腦時鐘逐漸 W32Time，時差必須小於 MaxAllowedPhaseOffset 值和符合下列方程式在同一時間：  
```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
測量 CurrentTimeOffset 時鐘刻度，其中 1ms = 10000 時鐘刻度在 Windows 系統中。  
  
也測量 SystemClockRate 與 PhaseCorrectRate 時鐘刻度中。 若要取得 SystemClockRate，您可以使用下列命令，並將它轉換秒到時鐘刻度使用公式秒的 * 1000\ * 10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate 是系統時鐘的速度。 使用 156000 秒做為範例，SystemclockRate 想要 = 0.0156000 * 1000 年 \ * 10000 = 156000 時鐘刻度。  
  
MaxAllowedPhaseOffset 也是秒。 若要將它轉換成時鐘刻度、乘 MaxAllowedPhaseOffset * 1000\ * 10000。  
  
下列兩個範例顯示如何在適用於  
  
**範例 1**：不同 4 分鐘的時間（例如，您的時間會上午 11:05 和時間範例收到等，並確認正確是上午 11:09）。  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
與是否符合上述方程式？ 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 100,000 < 78,000  
  
NO/FALSE  
```  
因此 W32tm 想設定時鐘回立即。  
  
> [!NOTE]  
> 在這種情形下，如果您想要設定時鐘回速度緩慢，您會需要調整 PhaseCorrectRate 或登錄 updateInterval，以及以確保方程式結果真正的值。  
  
**範例 2**：不同 3 分鐘的時間。  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
與是否符合上述方程式？
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
在這種情形下時鐘將回速度變慢。  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Windows 時間服務群組原則設定  
您可以設定最 W32Time 參數使用群組原則物件編輯器。 這包含設定的電腦將會可靠的時間來源 NTPServer 或 NTPClient 設定時間同步機制，以及設定的電腦。  
  
> [!NOTE]  
> Windows 時間服務群組原則設定可以設定 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 網域控制站，可在套用只提供給執行 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2。  
  
您可以找到群組原則設定用於 W32Time 設定群組原則物件編輯器嵌入式管理單元在下列位置：  
  
-   電腦設定 Templates\System\Windows 時間服務  
  
    設定**全球設定**在此。  
  
-   電腦設定 Templates\System\Windows 時間 Service\Time 提供者  
  
    設定**Windows NTP Client**設定。  
  
    讓**Windows NTP Client**在此。  
  
    讓**Windows NTP 伺服器**在此。  
  
> [!WARNING]  
> 部分的群組原則物件 (GPO) 設定的不同對應的預設登錄項目中的系統管理範本檔案 (System.adm) 設定預設值。 如果您打算使用 GPO 任何 Windows 時間設定，請務必檢視的[Windows 時間服務群組原則設定預設值是不同的對應 Windows 時間服務登錄的項目在 Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066)。 這個問題，適用於 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。  
  
下表列出 Windows 時間服務與每個設定的相關聯的預先設定的值相關聯的全域群組原則設定。 每個設定的相關詳細資訊，會看到在相對應登錄」[Windows 時間服務登錄項目](#w2k3tr_times_tools_uhlp)「先前此主題中。 下列設定會包含在單一 GPO 稱為**全球設定**。  
  
**Windows 時間相關聯的全域群組原則設定**  
  
|群組原則設定|預先設定的值。|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54000（15 時間）|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54000（15 時間）|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
下表列出的可用設定**Windows 設定 NTP Client** GPO 和預先設定與 Windows 時間服務相關聯的值。 每個設定的相關詳細資訊，會看到在相對應登錄」[Windows 時間服務登錄項目](#w2k3tr_times_tools_uhlp)「先前此主題中。  
  
**Windows 時間相關聯的 NTP Client 群組原則設定**  
  
|群組原則設定|預設值。|  
|------------------------|-----------------|  
|NtpServer|time.windows.com 0x1|  
|輸入|預設選項：<br /><br />-   **NTP。** 未加入網域的電腦上使用。<br />-   **NT5DS。** 加入網域的電腦上使用。|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>Windows 時間服務使用的網路連接埠  
Windows 時間遵循 NTP 規格，這需要 UDP 連接埠 123 的使用時間的所有同步處理通訊。 Windows 時間來保留此連接埠，並仍然保留隨時。 每當您的電腦同步處理的時鐘或提供時間至另一部電腦，在 UDP 連接埠 123 上執行的通訊。  
  
> [!NOTE]  
> 如果您的電腦有多個網路介面卡（也稱為多重主機電腦），您無法選擇讓 Windows 時間服務為基礎的網路介面卡。  
  
## <a name="w2k3tr_times_tools_qoep"></a>相關的資訊  
下列資源包含的此一節中相關的其他資訊。  
  
-   RFC *1305 年*IETF RFC 資料庫中  

