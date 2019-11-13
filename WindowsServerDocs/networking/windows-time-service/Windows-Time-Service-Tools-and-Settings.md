---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Windows 時間服務工具和設定
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 84d10c30d42e92d325475a1143c85acce7456a80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395679"
---
# <a name="windows-time-service-tools-and-settings"></a>Windows 時間服務工具和設定
>適用于： Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

在本主題中，您將瞭解 Windows Time 服務（W32Time）的工具和設定。 

如果您只想要同步處理加入網域的用戶端電腦的時間，請參閱[設定用戶端電腦以進行自動網域時間同步](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。 如需有關如何設定 Windows 時間服務的其他主題，請參閱[尋找 Windows 時間服務設定資訊的位置](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-time-service-top)。  

> [!CAUTION]  
> 您不應該使用 Net time 命令來設定或設定 Windows 時間服務執行的時間。  
>
> 此外，在執行 Windows XP 或更早版本的舊版電腦上，命令 Net time/querysntp 會顯示電腦設定為進行同步處理的網路時間通訊協定（NTP）伺服器名稱，但只有在電腦的時間用戶端設定為 NTP 或 AllSync 時，才會使用 NTP 伺服器。 該命令已被取代。  

大部分網域成員電腦的時間用戶端類型為 NT5DS，這表示它們會同步處理網域階層的時間。 唯一的例外是，網域控制站是做為樹系根域的主域控制站（PDC）模擬器操作主機，這通常是設定為與外部時間來源同步處理時間。 若要查看電腦的時間用戶端設定，請從 Windows Server 2008 和 Windows Vista 開始，從提升許可權的命令提示字元執行 W32tm/query/組態命令，並在命令輸出中讀取**類型**行。 如需詳細資訊，請參閱[Windows 時間服務的運作方式](https://docs.microsoft.com/windows-server/networking/windows-time-service/How-the-Windows-Time-Service-Works)。 您可以執行命令**reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** ，並在命令輸出中讀取 [ **NtpServer** ] 的值。  

> [!IMPORTANT]  
> 在 Windows Server 2016 之前，W32Time 服務的設計不是為了符合時間緊迫的應用程式需求。  不過，Windows Server 2016 的更新現在可讓您在網域中執行1毫秒精確度的解決方案。  如需詳細資訊，請參閱[windows 2016 精確時間](accurate-time.md)和[支援界限，為高準確度環境設定 windows 時間服務](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary)。  

## <a name="windows-time-service-tools"></a>Windows 時間服務工具  
下列工具會與 Windows 時間服務相關聯。  

#### <a name="w32tmexe-windows-time"></a>W32tm： Windows 時間  
**類別**  

此工具會安裝為 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 預設安裝的一部分。  

**版本相容性**  

此工具適用于 Windows XP、Windows Vista、Windows 7、Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 預設安裝。  

W32tm 是用來設定 Windows 時間服務設定。 它也可以用來診斷時間服務的問題。 W32tm 是慣用的命令列工具，可用於設定、監視或疑難排解 Windows 時間服務。  

下表描述搭配 W32tm 使用的參數。  

**W32tm 主要參數**  


|                                                                                                                                  參數                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                                                                                   W32tm/？                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      W32tm 命令列說明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                                               W32tm/register                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  註冊時間服務以服務的形式執行，並將預設設定新增至登錄。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                                              W32tm/unregister                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     取消註冊時間服務，並從登錄中移除所有設定資訊。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                 w32tm/監視<br /><br />[/domain：<domain name>][/computers：<name>[，<name>[，<name>...]]][/threads：<num>]                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                 網域-指定要監視的網域。 如果未指定功能變數名稱，或未指定網域或電腦選項，則會使用預設網域。 此選項可能會使用一次以上。<br /><br />電腦-監視指定的電腦清單。 電腦名稱稱會以逗號分隔，不含空格。 如果名稱前面加上 '\*'，則會將它視為 PDC。 此選項可能會使用一次以上。<br /><br />執行緒-指定要同時分析的電腦數目。 預設值為3。 允許的範圍為1-50。                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                                                                         w32tm/ntte <NT time epoch>                                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   將 NT 系統時間（10 ^-7） s 間隔中的 0h 1-Jan 1601 轉換成可讀取的格式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                                        w32tm/ntpte <NTP time epoch>                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      將 NTP 時間（2 ^-32） s 間隔中的 0h 1-Jan 1900 轉換成可讀取的格式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                                               w32tm/resync<br /><br />[/computer：<computer>]<br /><br />/nowait<br /><br />[/rediscover]<br /><br />[/soft]                                                                               |                                                                                                                                                                                                                                                                                                                                                                      告訴電腦應該儘快重新同步處理其時鐘，並擲出所有累積的錯誤統計資料。<br /><br />電腦：<computer>-指定應該重新同步處理的電腦。 如果未指定，本機電腦將會重新同步處理。<br /><br />nowait-不要等候重新同步處理發生;立即返回。 否則，請等候重新同步處理完成後再傳回。<br /><br />重新探索-再次設定網路設定，並再次探索網路來源，然後重新同步處理。<br /><br />使用現有的錯誤統計資料進行軟重新同步處理。 不實用，提供的相容性。                                                                                                                                                                                                                                                                                                                                                                      |
|                                                          w32tm/stripchart<br /><br />/computer：<target><br /><br />[/period：<refresh>]<br /><br />[/dataonly]<br /><br />[/samples：<count>]<br/><br/>[/rdtsc]<br/>                                                          |                            顯示此電腦與另一部電腦之間的位移的帶狀圖。<br /><br />電腦：<target>-測量其位移的電腦。<br /><br />period：<refresh>-樣本之間的時間（以秒為單位）。 預設值為2秒。<br /><br />dataonly-只顯示不含圖形的資料。<br /><br />範例：<count>-收集 <count> 範例，然後停止。 如果未指定，則會收集樣本，直到按下**Ctrl + C**為止。<br/><br/>rdtsc：對於每個範例，此選項會列印逗號分隔值，以及標頭 RdtscStart、RdtscEnd、FileTime、RoundtripDelay、NtpOffset 而不是文字圖形。<br/><ul><li>RdtscStart –在產生 NTP 要求之前收集的[RDTSC （讀取時間戳記計數器）](https://en.wikipedia.org/wiki/Time_Stamp_Counter)值。</li><li>RdtscEnd –在收到並處理 NTP 回應之後所收集的 RDTSC （讀取時間戳記計數器）值。</li><li>FileTime – NTP 要求中使用的本機 FILETIME 值。</li><li>RoundtripDelay –產生 NTP 要求和處理接收的 NTP 回應之間的時間（以秒為單位），計算方式為每 NTP 往返計算。</li><li>NTPOffset –本機電腦與 NTP 伺服器之間的時間位移（以秒為單位），計算方式為每個 NTP 位移計算。</li></ul>                             |
| w32tm/config<br /><br />[/computer：<target>]<br /><br />/update<br /><br />[/manualpeerlist：<peers>]<br /><br />[/syncfromflags：<source>]<br /><br />[/LocalClockDispersion：<seconds>]<br /><br />[/reliable：（是&#124;NO）]<br /><br />[/largephaseoffset：<milliseconds>] | 電腦：<target>-調整 <target>的設定。 如果未指定，則預設值為本機電腦。<br /><br />更新-通知時間服務設定已變更，使變更生效。<br /><br />manualpeerlist：<peers>-將手動對等清單設定為 <peers>，這是以空格分隔的 DNS 和/或 IP 位址清單。 指定多個對等時，此選項必須以引號括住。<br /><br />syncfromflags：<source>-設定 NTP 用戶端應從哪些來源進行同步處理。 <source> 應該是這些關鍵字的逗號分隔清單（不區分大小寫）：<br /><br />MANUAL-包含手動對等清單中的對等。<br /><br />DOMHIER-從網域階層中的網域控制站（DC）同步處理。<br /><br />LocalClockDispersion：<seconds>-設定 W32Time 在無法從其設定的來源取得時間時，將會假設的內部時鐘精確度。<br /><br />可靠：（是&#124;NO）-設定此電腦是否為可靠的時間來源。<br /><br />此設定只有在網域控制站上有意義。<br /><br />是-這台電腦是可靠的時間服務。<br /><br />否-這部電腦不是可靠的時間服務。<br /><br />largephaseoffset：<milliseconds>-設定 W32Time 將視為尖峰的時間差異。 |
|                                                                                                                                  w32tm/tz                                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              顯示目前的時區設定。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                                                                                  w32tm/dumpreg<br /><br />[/subkey：<key>]<br /><br />[/computer：<target>]                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                顯示與指定登錄機碼相關聯的值。<br /><br />預設金鑰為 HKLM\System\CurrentControlSet\Services\W32Time<br /><br />（時間服務的根機碼）。<br /><br />子機碼：<key>-顯示與預設金鑰的子機碼 <key> 相關聯的值。<br /><br />電腦：<target>-查詢電腦 <target> 的登錄設定                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                  w32tm/query [/computer：<target>] {/source &#124; /組態&#124; /peers &#124; /status} [/verbose]                                                                                   |                                                                                                                                                                                                                                                                                                                  第一次在 windows Vista 和 Windows Server 2008 的 Windows Time client 版本中提供此參數。<br /><br />顯示電腦的 Windows Time 服務資訊。<br /><br />**電腦：<target>** -查詢 **<target>** 的資訊。 如果未指定，則預設值為本機電腦。<br /><br />**來源**-顯示時間來源。<br /><br />設定 **-顯示**執行時間和設定來源的設定。 在 [詳細資訊] 模式中，也會顯示未定義或未使用的設定。<br /><br />**對等**-顯示對等的清單及其狀態。<br /><br />**狀態**-顯示 Windows 時間服務狀態。<br /><br />**verbose** -設定 verbose 模式以顯示詳細資訊。                                                                                                                                                                                                                                                                                                                   |
|                                                                                       w32tm/debug {/disable &#124; {/enable/file：<name>/size：<bytes>/entries：<value> [/truncate]}}                                                                                       |                                                                                                                                                                                                                                                                         第一次在 windows Vista 和 Windows Server 2008 的 Windows Time client 版本中提供此參數。<br /><br />啟用或停用本機電腦的 Windows Time 服務私人記錄檔。<br /><br />**停**用-停用私用記錄檔。<br /><br />**啟用**-啟用私用記錄檔。<br /><br />-   檔案 **：<name>** -指定絕對檔案名。<br />-   **大小：<bytes>** -指定迴圈記錄的大小上限。<br />-   **專案：<value>** -包含以數位指定的旗標清單，並以逗號分隔，以指定應記錄的資訊類型。 有效的數位為0到300。 除了單一數位（例如 0-100103106）之外，數位範圍也是有效的。 0-300 值是用來記錄所有資訊。<br /><br />**截斷**-截斷檔案（如果存在的話）。                                                                                                                                                                                                                                                                          |

---  
如需**W32tm**的詳細資訊，請參閱 windows XP、windows Vista、windows 7、windows server 2003、windows Server 2003 R2、windows server 2008 和 windows Server 2008 R2 中的說明及支援中心。  

## <a name="windows-time-service-registry-entries"></a>Windows 時間服務登錄專案
下列登錄專案會與 Windows 時間服務相關聯。  

這項資訊是以參考的形式提供，可用於疑難排解或驗證所需的設定。 除非沒有其他替代方法，否則建議您不要直接編輯登錄。 登錄編輯器或 Windows 不會先對登錄進行修改，也不會在應用程式中進行驗證，因此可能會儲存不正確的值。 這可能會導致系統中發生無法復原的錯誤。  

可能的話，請使用群組原則或其他 Windows 工具（例如 Microsoft Management Console （MMC））來完成工作，而不是直接編輯登錄。 如果您必須編輯登錄，必須非常小心。  

> [!WARNING]  
> 在群組原則物件（GPO）設定的系統管理範本檔案（System .adm）中設定的部分預設值，與對應的預設登錄專案不同。 如果您打算使用 GPO 設定任何 Windows 時間設定，請務必檢查[Windows time 服務的預設值群組原則設定與 Windows Server 2003 中對應的 Windows 時間服務登錄專案不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此問題適用于 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。  

Windows 時間服務的許多登錄專案與相同名稱的群組原則設定相同。 群組原則設定會對應至位於相同名稱的登錄專案：  

>**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\\**


此登錄位置中有數個登錄機碼。 Windows 時間設定會儲存在所有這些索引鍵的值中：

* [參數](#hklmsystemcurrentcontrolsetservicesw32timeparameters)
* [Web.config](#hklmsystemcurrentcontrolsetservicesw32timeconfig)
* [NtpClient](#hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient)
* [NtpServer](#hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver)

在登錄的 W32Time 區段中，有許多值都是由 W32Time 在內部用來儲存資訊。 這些值不應在任何時間手動變更。 除非您熟悉該設定，否則請不要修改本節中的任何設定，並確定新的值會如預期般運作。 下列登錄專案位於：

**HKLM\SYSTEM\CurrentControlSet\Services\W32Time**  

當您建立原則時，這些設定會在下列位置設定，其優先順序不會高於下一個位置：

**HKLM\SOFTWARE\Policies\Microsoft\Windows\W32time**

W32time 金鑰是使用原則所建立。  當您移除原則時，也會一併移除此金鑰。

其他預設位置：

**HKLM\SYSTEM\CurrentControlSet\Services\W32time**

某些參數會以時鐘的頻率刻度儲存在登錄中，有些則以秒為單位。 若要將時間從時鐘刻度轉換為秒：  

-   1分鐘 = 60 秒  

-   1秒 = 1000 毫秒  

-   1毫秒 = Windows 系統上的10000時鐘刻度，如[DateTime. 滴答屬性](https://docs.microsoft.com/dotnet/api/system.datetime.ticks?redirectedfrom=MSDN&view=netframework-4.7.2#System_DateTime_Ticks)中所述。  

例如，5分鐘會變成 5\*60\*1000\*10000 = 3000000000 頻率刻度。 

所有版本都包括 Windows 7、Windows 8、Windows 10、Windows Server 2008 和 Windows Server 2008 R2、Windows Server 2012、Windows Server 2012R2、Windows Server 2016。  某些專案僅適用于較新的 Windows 版本。

#### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|          登錄專案          | 版本 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AllowNonstandardModeCombinations |   全部   |                                                                                                                                                                                                                                                                                                                                                                                                              專案表示對等之間的同步處理允許非標準模式組合。 網域成員的預設值為1。 獨立用戶端和伺服器的預設值為1。                                                                                                                                                                                                                                                                                                                                                                                                              |
|            NtpServer             |   全部   | 專案會指定以空格分隔的對等清單，其中的電腦會從中取得時間戳記，其中包含一或多個 DNS 名稱或每行的 IP 位址。 列出的每個 DNS 名稱或 IP 位址都必須是唯一的。 連線到網域的電腦必須與更可靠的時間來源進行同步處理，例如美國官方時間時鐘。  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive-如需此模式的詳細資訊，請參閱[Windows 時間伺服器：3.3 模式的](https://go.microsoft.com/fwlink/?LinkId=208012)作業。</li><li>0x08 用戶端</li></ul><br />網域成員上沒有此登錄專案的預設值。 獨立用戶端和伺服器上的預設值為 time. windows .com，0x1。<br /><br />注意：如需可用 NTP 伺服器的詳細資訊，請參閱[Microsoft 知識庫文章 262680-可在網際網路上使用的簡易網路時間通訊協定（SNTP）時間伺服器清單](https://support.microsoft.com/help/262680/a-list-of-the-simple-network-time-protocol-sntp-time-servers-that-are) |
|            ServiceDll            |   全部   |                                                                                                                                                                                                                                                                                                                                                              專案是由 W32Time 維護。 它包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都會導致無法預期的結果。 網域成員和獨立用戶端和伺服器上此 DLL 的預設位置是%windir%\System32\W32Time.dll。                                                                                                                                                                                                                                                                                                                                                               |
|           ServiceMain            |   全部   |                                                                                                                                                                                                                                                                                                                                                       專案是由 W32Time 維護。 它包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都會導致無法預期的結果。 網域成員的預設值為 SvchostEntry_W32Time。 獨立用戶端和伺服器上的預設值為 SvchostEntry_W32Time。  "                                                                                                                                                                                                                                                                                                                                                       |
|               類型               |   全部   |                                                                                                                                                                                                                                  專案指出要接受同步處理的對等：  <ul><li>**NoSync**。 時間服務不會與其他來源同步處理。</li><li>**NTP.** 時間服務會從**NtpServer**中指定的伺服器進行同步處理。 登錄專案。</li><li>**NT5DS**。 時間服務會從網域階層進行同步處理。  </li><li>**AllSync**。 時間服務會使用所有可用的同步處理機制。  </li></ul>網域成員的預設值為**NT5DS**。 獨立用戶端和伺服器上的預設值為**NTP**。                                                                                                                                                                                                                                   |

---
#### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config

|    登錄專案     |          版本           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     AnnounceFlags     |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Entry 控制這部電腦是否標示為可靠的時間伺服器。 除非電腦也標示為時間伺服器，否則不會將它標示為可靠。<br /> -0x00 不是時間伺服器  <br /> -0x01 Always time 伺服器  <br /> -0x02 自動時間伺服器  <br /> -0x04 永遠可靠的時間伺服器  <br /> -0x08 自動可靠的時間伺服器  <br />網域成員的預設值為10。 獨立用戶端和伺服器的預設值為10。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|     EventLogFlags     |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          專案可控制時間服務所記錄的事件。  <br />-時間跳躍：0x1  <br />-來源變更：0x2  <br />網域成員的預設值為2。 獨立用戶端和伺服器上的預設值為2。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| FrequencyCorrectRate  |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    Entry 控制時鐘更正的速率。 如果這個值太小，則時鐘不穩定，而且 overcorrects。 如果值太大，則時鐘會花很長的時間來進行同步處理。 網域成員的預設值為4。 獨立用戶端和伺服器上的預設值為4。  <br /><br />請注意，0是 FrequencyCorrectRate 登錄專案的無效值。 在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 電腦上，如果值設定為0，Windows Time 服務就會自動將它變更為1。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|      HoldPeriod       |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   專案會控制停用尖峰偵測，以快速將本機時鐘帶入同步處理的時間長度。 尖峰是時間樣本，表示時間已關閉秒數，而且通常會在順利傳回正確的時間樣本之後收到。 網域成員的預設值為5。 獨立用戶端和伺服器上的預設值為5。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|   LargePhaseOffset    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        專案指定在 10<sup>-7</sup>秒內，大於或等於此值的時間位移會被視為尖峰。 網路中斷（例如大量流量）可能會導致尖峰。 除非長時間持續存在，否則會忽略尖峰。 網域成員的預設值是50000000。 獨立用戶端和伺服器上的預設值為50000000。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|     LastClockRate     |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           專案是由 W32Time 維護。 它包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都會導致無法預期的結果。 網域成員的預設值是156250。 獨立用戶端和伺服器上的預設值為156250。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| LocalClockDispersion  |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        專案可控制當唯一的時間來源為內建的 CMOS 時鐘時，您必須假設的散佈方式（以秒為單位）。 網域成員的預設值為10。 獨立用戶端和伺服器上的預設值為10。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| MaxAllowedPhaseOffset |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        專案指定 W32Time 嘗試使用頻率來調整電腦時鐘的最大位移（以秒為單位）。 當位移超過此速率時，W32Time 會直接設定電腦時鐘。 網域成員的預設值是300。 獨立用戶端和伺服器的預設值為1。  [如需詳細資訊，請參閱下文](#maxallowedphaseoffset-information)。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|     MaxClockRate      |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          專案是由 W32Time 維護。 它包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都會導致無法預期的結果。 網域成員的預設值是155860。 獨立用戶端和伺服器的預設值為155860。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| MaxNegPhaseCorrection |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              專案會指定服務所進行的最大負數時間更正（以秒為單位）。 如果服務判斷需要大於此的變更，則會改為記錄事件。 特殊案例：0xFFFFFFFF 表示一定要進行時間更正。 網域成員的預設值為0xFFFFFFFF。 獨立用戶端和伺服器的預設值為54000（15小時）。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|    MaxPollInterval    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     專案指定系統輪詢間隔中所允許的最大間隔（以 log2 秒為單位）。 請注意，當系統必須根據排定的間隔進行輪詢時，提供者可能會在要求時拒絕產生樣本。 網域控制站的預設值是10。 網域成員的預設值為15。 獨立用戶端和伺服器的預設值為15。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| MaxPosPhaseCorrection |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              專案會指定服務所進行的最大正值更正（以秒為單位）。 如果服務判斷需要大於此的變更，則會改為記錄事件。 特殊案例：0xFFFFFFFF 表示一定要進行時間更正。 網域成員的預設值為0xFFFFFFFF。 獨立用戶端和伺服器的預設值為54000（15小時）。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|     MinClockRate      |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          專案是由 W32Time 維護。 它包含 Windows 作業系統所使用的保留資料，而且對此設定所做的任何變更都會導致無法預期的結果。 網域成員的預設值是155860。 獨立用戶端和伺服器的預設值為155860。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|    MinPollInterval    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              專案指定系統輪詢間隔中所允許的最小間隔（以 log2 秒為單位）。 請注意，雖然系統不會要求比此更頻繁的範例，但是提供者可以在排程的間隔以外的時間產生樣本。 網域控制站的預設值為6。 網域成員的預設值為10。 獨立用戶端和伺服器的預設值為10。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|   PhaseCorrectRate    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Entry 控制更正階段錯誤的速率。 指定較小的值會快速更正階段錯誤，但可能會導致時鐘變得不穩定。 如果值太大，則需要較長的時間來更正階段錯誤。 <br /><br />網域成員的預設值為1。 獨立用戶端和伺服器上的預設值為7。<br /><br />注意：0是 PhaseCorrectRate 登錄專案的無效值。 在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 電腦上，如果此值設定為0，Windows Time 服務就會自動將它變更為1。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|   PollAdjustFactor    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       Entry 會控制要增加或減少系統輪詢間隔的決策。 值愈大，導致輪詢間隔減少的錯誤量就愈小。 網域成員的預設值為5。 獨立用戶端和伺服器上的預設值為5。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|   SpikeWatchPeriod    |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    專案會指定可疑的位移在接受為正確（以秒為單位）之前，必須保存的時間量。 網域成員的預設值是900。 獨立用戶端和工作站上的預設值為900。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|  TimeJumpAuditOffset  |            全部             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            不帶正負號的整數，表示時間跳躍審核閾值（以秒為單位）。 如果時間服務會直接設定時鐘來調整本機時鐘，而時間修正則大於此值，則時間服務會記錄一個 audit 事件。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|    UpdateInterval     |            全部             | 專案指定階段更正調整之間的時鐘刻度數目。 網域控制站的預設值是100。 網域成員的預設值是30000。 獨立用戶端和伺服器的預設值為360000。  <br /><br />**注意**：零是 UpdateInterval 登錄專案的無效值。 在執行 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 的電腦上，如果此值設定為0，則 Windows 時間服務會自動將其變更為1。<br /><br />下列三個登錄專案不是 W32Time 預設設定的一部分，但可以新增至登錄以取得更高的記錄功能。 您可以變更群組原則物件編輯器中 [EventLogFlags] 設定的值，以修改記錄到系統事件記錄檔的資訊。 根據預設，時間服務會在每次切換至新的時間來源時，于事件檢視器中建立記錄檔。<br /><br />**警告**：群組原則物件（GPO）設定的系統管理範本檔案（system. .adm）中設定的某些預設值，與對應的預設登錄專案不同。 如果您打算使用 GPO 設定任何 Windows 時間設定，請務必檢查[Windows time 服務的預設值群組原則設定與 Windows Server 2003 中對應的 Windows 時間服務登錄專案不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此問題適用于 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。 |
|  UtilizeSslTimeData   | 張貼 Windows 10 組建1511 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             專案1表示 W32Time 將使用多個 SSL 時間戳記來植入大致上不正確的時鐘。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

---
您必須新增下列登錄專案，才能啟用 W32Time 記錄：  

|登錄專案|版本|描述|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|全部|專案會控制在 Windows 時間記錄檔中建立的專案數量。 預設值為 [無]，這不會記錄任何 Windows 時間活動。 有效值為0到300。 此值不會影響 Windows 時間一般所建立的事件記錄檔專案|
|FileLogName|全部|Entry 控制 Windows 時間記錄檔的位置和檔案名。 預設值為空白，除非變更**FileLogEntries** ，否則不應變更。 有效的值是 Windows 時間將用來建立記錄檔的完整路徑和檔案名。 這個值不會影響 Windows 時間一般所建立的事件記錄檔專案。  |
|FileLogSize|全部|Entry 控制 Windows 時間記錄檔的迴圈記錄行為。 定義**FileLogEntries**和**FileLogName**時，Entry 會定義大小（以位元組為單位），以便在以新的專案覆寫最舊的記錄專案之前，允許記錄檔到達。 請針對此設定使用1000000或更大的值。 這個值不會影響 Windows 時間一般所建立的事件記錄檔專案。  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|登錄專案|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|全部|專案表示對等之間的同步處理允許非標準模式組合。 網域成員的預設值為1。 獨立用戶端和伺服器的預設值為1。|
|CompatibilityFlags|全部|專案會指定下列相容性旗標和值： <br /><br />-DispersionInvalid：0x00000001  <br />-IgnoreFutureRefTimeStamp：0x00000002  <br /> -AutodetectWin2K：0x80000000  <br />-AutodetectWin2KStage2：0x40000000  <br /><br />網域成員的預設值為0x80000000。 獨立用戶端和伺服器的預設值為0x80000000。  |
|CrossSiteSyncFlags|全部|專案決定服務是否選擇電腦網域以外的同步處理夥伴。 選項和值如下：  <br /><br />-None：0  <br />-PdcOnly：1  <br />-全部：2  <br /><br />如果未設定 NT5DS 值，則會忽略這個值。 網域成員的預設值為2。 獨立用戶端和伺服器的預設值為2。  |
|DllName|全部|專案指定時間提供者之 DLL 的位置。  <br /><br />網域成員和獨立用戶端和伺服器上此 DLL 的預設位置是%windir%\System32\W32Time.dll。|
|啟用|全部|專案指出是否已在目前的時間服務中啟用 NtpClient 提供者。  <br /><ul><li>是1</li><li>否0</li></ul>網域成員的預設值為1。 獨立用戶端和伺服器上的預設值為1。|
|EventLogFlags|全部|專案指定 Windows 時間服務所記錄的事件。<ul><li>0x1 可連線性變更</li><li>0x2 大型樣本誤差（僅適用于 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2）</li></ul>網域成員的預設值為0x1。 獨立用戶端和伺服器上的預設值為0x1。|
|InputProvider|全部|專案指出是否要啟用 NtpClient 作為 InputProvider，以從 NtpServer 取得時間資訊。 NtpServer 是一種時間伺服器，它會傳回可用於同步處理本機時鐘的時間樣本，以回應網路上的用戶端時間要求。 <ul><li>是 = 1  </li><li>否 = 0 </li></ul><p>網域成員和獨立用戶端的預設值：1  |
|LargeSampleSkew|全部|專案指定用於記錄的大型樣本誤差（以秒為單位）。 為了遵守安全性和交換委員會（秒）的規格，這應該設定為三秒。 只有當 EventLogFlags 已明確設定為0x2 大的樣本扭曲時，才會記錄此設定的事件。 網域成員的預設值為3。 獨立用戶端和伺服器上的預設值為3。  |
|ResolvePeerBackOffMaxTimes|全部|專案指定重複嘗試找出要同步處理失敗的對等時，要加倍的等候間隔最大次數。 值為零表示等待間隔一律為最小值。 網域成員的預設值為7。 獨立用戶端和伺服器上的預設值為7。 |
|ResolvePeerBackoffMinutes|全部|專案指定要等候的初始間隔（以分鐘為單位），然後再嘗試找出與同步處理的對等。 網域成員的預設值為15。 獨立用戶端和伺服器上的預設值為15。  |
|SpecialPollInterval|全部|專案指定手動對等的特殊輪詢間隔（以秒為單位）。 啟用 SpecialInterval 0x1 旗標時，W32Time 會使用此輪詢間隔，而不是由作業系統決定的輪詢間隔。 網域成員的預設值是3600。 獨立用戶端和伺服器上的預設值為604800。<br/><br/>組建1702的新，SpecialPollInterval 包含在 MinPollInterval 和 MaxPollInterval Config 登錄值中。|
|SpecialPollTimeRemaining|全部|專案是由 W32Time 維護。 它包含 Windows 作業系統所使用的保留資料。 它會指定在電腦重新開機之後，W32Time 重新同步處理之前的時間（以秒為單位）。 對此設定進行的任何變更都會導致無法預期的結果。 網域成員和獨立用戶端和伺服器上的預設值會保留空白。  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|登錄專案|版本|描述|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|全部|專案表示用戶端與伺服器之間的同步處理允許非標準模式組合。 網域成員的預設值為1。 獨立用戶端和伺服器的預設值為1。|
|DllName|全部|專案指定時間提供者之 DLL 的位置。<br /><br />網域成員和獨立用戶端和伺服器上此 DLL 的預設位置是%windir%\System32\W32Time.dll。  |
|啟用|全部|專案指出是否已在目前的時間服務中啟用 NtpServer 提供者。 <ul><li>是1</li><li>否0</li></ul>網域成員的預設值為1。 獨立用戶端和伺服器上的預設值為1。  |
|InputProvider|全部|專案指出是否要啟用 NtpClient 作為 InputProvider，以從 NtpServer 取得時間資訊。 NtpServer 是一種時間伺服器，它會傳回可用於同步處理本機時鐘的時間樣本，以回應網路上的用戶端時間要求。 <ul><li>是 = 1  </li><li>否 = 0 </li></ul><p>網域成員和獨立用戶端的預設值：1  |

---

#### <a name="maxallowedphaseoffset-information"></a>MaxAllowedPhaseOffset 資訊
為了讓 W32Time 逐漸設定電腦時鐘，位移必須小於**MaxAllowedPhaseOffset**值，並同時滿足下列方程式：  

* Windows Server 2016 和更新版本：
   ```  
    |CurrentTimeOffset| / (16*PhaseCorrectRate*pollIntervalInSeconds) <= SystemClockRate / 2  
   ``` 
* Windows Server 2012 R2 和更早版本：
   ```  
   |CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) <= SystemClockRate / 2  
   ``` 
**CurrentTimeOffset**值會以時鐘刻度來測量，其中1毫秒 = 10000 時鐘刻度在 Windows 系統上。  

**SystemClockRate**和**PhaseCorrectRate**也會以時鐘刻度來測量。 若要取得**SystemClockRate**值，您可以使用下列命令，並使用秒數 * 1000\*10000，將它從秒轉換成時鐘刻度：  

```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  

**SystemclockRate**是指系統上的時脈速率。 使用156000秒做為範例， **SystemclockRate**值會是 = 0.0156000 \* 1000 \* 10000 = 156000 頻率刻度。  

**MaxAllowedPhaseOffset**也會以秒為單位。 若要將它轉換成時鐘刻度，請將**MaxAllowedPhaseOffset**\*1000\*10000。  

下列範例示範當您使用 Windows Server 2012 R2 或更早版本時，如何套用這些計算。

**範例 1**：時間與4分鐘不同（例如，您的時間是11:05，而您從對等體收到的時間樣本，相信是11:09）。
  
```
phasecorrectRate = 1  

UpdateInterval = 30000 (clock ticks)  

systemclockRate = 156000 (clock ticks)  

MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  

|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  

Is CurrentTimeOffset <= MaxAllowedPhaseOffset?  

2400000000 <= 6000000000 = TRUE  
```

它是否符合上述方程式？ 

```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) <= SystemClockRate / 2)  

Is 2,400,000,000 / (30000*1) <= 156000/2  

Is 80,000 <= 78,000  

NO/FALSE  
```  

因此，W32tm 會立即重新設定時鐘。  

> [!NOTE]  
> 在此情況下，如果您想要讓時鐘恢復速度變慢，您也必須調整登錄中**PhaseCorrectRate**或**updateInterval**的值，以確保方程式的結果為**TRUE**。  

**範例 2**：時間不同于3分鐘。 
 
```  
phasecorrectRate = 1  

UpdateInterval = 30000 (clock ticks)  

systemclockRate = 156000 (clock ticks)  

MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  

currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  

Is CurrentTimeOffset <= MaxAllowedPhaseOffset?  

1800000000 <= 6000000000 = TRUE  
```  

它是否符合上述方程式？

```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) <= SystemClockRate / 2)  

Is 3 mins (1,800,000,000) / (30000*1) <= 156000/2  

Is 60,000 <= 78,000  

YES/TRUE  
```  

在此情況下，時鐘的設定會變慢。  

## <a name="windows-time-service-group-policy-settings"></a>Windows Time 服務群組原則設定  
您可以使用群組原則物件編輯器來設定大部分的 W32Time 參數。 這包括將電腦設定為 NTPServer 或 NTPClient、設定時間同步處理機制，以及將電腦設定為可靠的時間來源。  

> [!NOTE]  
> Windows 時間服務的群組原則設定可以在 Windows Server 2003、Windows Server 2003 R2、Windows Server 2008 和 Windows Server 2008 R2 網域控制站上設定，而且只能套用至執行 Windows Server 2003、Windows Server 的電腦2003 r2、Windows Server 2008 和 Windows Server 2008 R2。  

您可以在下列位置的 [群組原則物件編輯器] 嵌入式管理單元中，找到用來設定 W32Time 的群組原則設定：  

-   電腦建構管理的 Templates\System\Windows 時間服務  

    在這裡設定**全域設定**。  

-   電腦建構管理的 Templates\System\Windows 時間 Service\Time 提供者  

    在這裡設定**WINDOWS NTP 用戶端**設定。  

    在這裡啟用**WINDOWS NTP 用戶端**。  

    在這裡啟用**WINDOWS NTP 伺服器**。  

> [!WARNING]  
> 在群組原則物件（GPO）設定的系統管理範本檔案（System .adm）中設定的部分預設值，與對應的預設登錄專案不同。 如果您打算使用 GPO 設定任何 Windows 時間設定，請務必檢查[Windows time 服務的預設值群組原則設定與 Windows Server 2003 中對應的 Windows 時間服務登錄專案不同](https://go.microsoft.com/fwlink/?LinkId=186066)。 此問題適用于 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 和 Windows Server 2003。  

下表列出與 Windows 時間服務相關聯的全域群組原則設定，以及與每個設定相關聯的預先設定值。 如需有關每個設定的詳細資訊，請參閱本主題稍早的[Windows Time 服務登錄專案](#windows-time-service-registry-entries)中對應的登錄專案。 下列設定包含在名為 [**全域設定**] 的單一 GPO 中。  

**與 Windows 時間相關聯的全域群組原則設定**  

|群組原則設定|預先設定的值|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54000（15小時）|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54000（15小時）|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  

下表列出設定**WINDOWS NTP 用戶端**GPO 和與 Windows Time 服務相關聯之預先設定值的可用設定。 如需有關每個設定的詳細資訊，請參閱本主題稍早的[Windows Time 服務登錄專案](#windows-time-service-registry-entries)中對應的登錄專案。  

**與 Windows 時間相關聯的 NTP 用戶端群組原則設定**  

|群組原則設定|預設值|  
|------------------------|-----------------|  
|NtpServer|時間。 windows .com，0x1|  
|類型|預設選項：<br /><br />-   **NTP。** 在未加入網域的電腦上使用。<br />-   **NT5DS。** 在已加入網域的電腦上使用。|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  

## <a name="network-ports-used-by-the-windows-time-service"></a>Windows 時間服務所使用的網路埠  
Windows 時間遵循 NTP 規格，這需要使用 UDP 埠123來進行所有時間同步處理通訊。 此埠是由 Windows 時間保留，而且隨時保持保留。 每當電腦同步處理其時鐘或提供時間給另一部電腦時，就會在 UDP 埠123上執行通訊。  

> [!NOTE]  
> 如果您的電腦具有多個網路介面卡（也稱為多重主目錄電腦），則無法根據網路介面卡選擇性地啟用 Windows 時間服務。  

## <a name="related-information"></a>相關資訊  
下列資源包含與本節相關的其他資訊。  

-   IETF RFC 資料庫中的 RFC *1305*  

