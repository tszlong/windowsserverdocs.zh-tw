---
title: dnscmd
description: Dnscmd 命令的參考文章，此命令是用來管理 DNS 伺服器的命令列介面。
ms.topic: reference
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a804965efaa135d366b62fe2379ad7d9c3d17e5c
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696916"
---
# <a name="dnscmd"></a>Dnscmd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理 DNS 伺服器的命令列介面。 此公用程式可用於撰寫批次檔的腳本，以協助自動化例行的 DNS 管理工作，或在您的網路上執行簡單的自動安裝和設定新的 DNS 伺服器。

## <a name="syntax"></a>語法

```
dnscmd <servername> <command> [<command parameters>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<servername>` | 遠端或本機 DNS 伺服器的 IP 位址或主機名稱。 |

## <a name="dnscmd-ageallrecords-command"></a>dnscmd/ageallrecords 命令

在 DNS 伺服器上指定的區域或節點上的資源記錄上，設定時間戳記的目前時間。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /ageallrecords <zonename>[<nodename>] | [/tree]|[/f]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定系統管理員打算管理的 DNS 伺服器，以 IP 位址、完整功能變數名稱 (FQDN) 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定區域的 FQDN。 |
| `<nodename>` | 使用下列內容指定區域中的特定節點或子樹：<ul><li>**@** 針對根區域或 FQDN</li><li>節點的 FQDN (具有句點 ( 的名稱。 ) 在結尾) </li><li>相對於區域根目錄之名稱的單一標籤。</li></ul> |
| /tree | 指定所有的子節點也會收到時間戳記。 |
| /f | 執行命令，而不要求確認。 |

##### <a name="remarks"></a>備註

- **Ageallrecords** 命令適用于目前的 dns 版本與舊版的 dns 之間的回溯相容性，在此版本中不支援過時和清除。 它會將目前時間的時間戳記新增至沒有時間戳記的資源記錄，並在具有時間戳記的資源記錄上設定目前時間。

- 除非記錄有時間戳記，否則不會進行記錄清除。 名稱伺服器 (NS) 資源記錄、授權單位啟動 (SOA) 資源記錄，以及 Windows 網際網路名稱服務 (WINS) 資源記錄不包含在清除程式中，而且即使在 **ageallrecords** 命令執行時也不會加上時間戳記。

- 除非已針對 DNS 伺服器和區域啟用清除，否則此命令會失敗。 如需有關如何啟用區域清除的詳細資訊，請參閱本文的命令語法中的 **過時** 參數 `dnscmd /config` 。

- 將時間戳記新增至 DNS 資源記錄，會使其與在 Windows Server 以外的作業系統上執行的 DNS 伺服器不相容。 使用 **ageallrecords** 命令新增的時間戳記無法反轉。

- 如果未指定任何選擇性參數，此命令會傳回指定節點上的所有資源記錄。 如果至少有一個選擇性參數指定了值，則 **dnscmd** 只會列舉對應至選擇性參數或參數中所指定值的資源記錄。

### <a name="examples"></a>範例

[範例1：將時間戳記的目前時間設定為資源記錄](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-1-set-the-current-time-on-a-time-stamp-to-resource-records)

## <a name="dnscmd-clearcache-command"></a>dnscmd/clearcache 命令

清除指定之 DNS 伺服器上資源記錄的 DNS 快取記憶體。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /clearcache
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |

### <a name="example"></a>範例

```
dnscmd dnssvr1.contoso.com /clearcache
```

## <a name="dnscmd-config-command"></a>dnscmd/config 命令

針對 DNS 伺服器和個別區域變更登錄中的值。 此命令也會修改指定之伺服器的設定。 接受伺服器層級和區域層級的設定。

> [!CAUTION]
> 除非您沒有替代方案，否則請勿直接編輯登錄。 登錄編輯程式會略過標準的保護，允許可能降低效能、損毀系統或甚至需要重新安裝 Windows 的設定。 您可以使用主控台或 Microsoft Management Console (mmc) 中的程式，安全地改變大部分的登錄設定。 如果您必須直接編輯登錄，請先備份。 如需詳細資訊，請參閱登錄編輯程式說明。

### <a name="server-level-syntax"></a>伺服器層級語法

```
dnscmd [<servername>] /config <parameter>
```

#### <a name="parameters"></a>參數

> [!NOTE]
> 本文包含「從屬」一詞的參考，Microsoft 已不再使用該字詞。 從軟體中移除該字詞時，我們也會將其從本文中移除。

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定您打算管理的 DNS 伺服器，以本機電腦語法、IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<parameter>` | 指定設定和值做為選項。 參數值使用此語法： *parameter* [*value*]。 |
| /addressanswerlimit`[0|5-28]` | 指定 DNS 伺服器可傳送以回應查詢的主機記錄數目上限。 值可以是零 (0) ，也可以是5到28筆記錄的範圍。 預設值為零 (0)。 |
| /bindsecondaries`[0|1]` | 變更區域傳輸的格式，讓它可以達到最大的壓縮和效率。 接受下列值：<ul><li>**0** -使用最大壓縮，且僅與系結版本4.9.4 和更新版本相容</li><li>**1** -每個訊息只會傳送一筆資源記錄至非 Microsoft DNS 伺服器，而且與4.9.4 之前的系結版本相容。 這是預設值。</li></ul> |
| /bootmethod`[0|1|2|3]` | 決定 DNS 伺服器取得其設定資訊的來源。 接受下列值：<ul><li>**0** -清除設定資訊的來源。</li><li>**1** -從位於 DNS 目錄的系結檔案（預設為）載入 `%systemroot%\System32\DNS` 。</li><li>**2** -從登錄載入。</li><li>**3** -從 AD DS 和登錄載入。 這是預設值。</li></ul> |
| /defaultagingstate`[0|1]` | 判斷新建立的區域是否預設啟用 DNS 清除功能。 接受下列值：<ul><li>**0** -停用清除。 這是預設值。</li><li>**1** -啟用清除。</li></ul> |
| /defaultnorefreshinterval`[0x1-0xFFFFFFFF|0xA8]` | 設定動態更新的記錄不會接受任何重新整理的一段時間。 伺服器上的區域會自動繼承此值。<p>若要變更預設值，請輸入 **0x1-0xffffffff** 範圍內的值。 伺服器的預設值為 **0xA8**。 |
| /defaultrefreshinterval `[0x1-0xFFFFFFFF|0xA8]` | 設定允許對 DNS 記錄進行動態更新的時段。 伺服器上的區域會自動繼承此值。<p>若要變更預設值，請輸入 **0x1-0xffffffff** 範圍內的值。 伺服器的預設值為 **0xA8**。 |
| /disableautoreversezones `[0|1]` | 啟用或停用反向對應區域的自動建立。 反向對應區域提供網際網路通訊協定的解析， (IP) 位址到 DNS 功能變數名稱。 接受下列值：<ul><li>**0** -啟用反向對應區域的自動建立。 這是預設值。</li><li>**1** -停用自動建立反向對應區域。</li></ul> |
| /disablensrecordsautocreation `[0|1]` | 指定 DNS 伺服器是否會自動為它所裝載的區域建立名稱伺服器 (NS) 資源記錄。 接受下列值：<ul><li>**0** -自動為 DNS 伺服器所裝載的區域建立名稱伺服器 (NS) 資源記錄。</li><li>**1** -不會自動為 DNS 伺服器所裝載的區域建立名稱伺服器 (NS) 資源記錄。</li></ul> |
| /dspollinginterval `[0-30]` | 指定 DNS 伺服器輪詢 active directory 整合區域變更 AD DS 的頻率。 |
| /dstombstoneinterval `[1-30]` |在 AD DS 中保留已刪除記錄的時間長度（以秒為單位）。 |
| /ednscachetimeout `[3600-15724800]` | 指定快取擴充 DNS (EDNS) 資訊快取的秒數。 最小值是 **3600**，而最大值是 **15724800**。 預設值為 **604800** 秒 (一周) 。 |
| /enableednsprobes `[0|1]` | 啟用或停用伺服器來探查其他伺服器，以判斷它們是否支援 EDNS。 接受下列值：<ul><li>**0** -停用對 EDNS 探查的主動支援。</li><li>**1** -啟用對 EDNS 探查的主動支援。</li></ul> |
| /enablednssec `[0|1]` | 啟用或停用 (DNSSEC) 的 DNS 安全性延伸模組支援。 接受下列值：<ul><li>**0** -停用 DNSSEC。</li><li>**1** -啟用 DNSSEC。</li></ul> |
| /enableglobalnamessupport `[0|1]` | 啟用或停用 GlobalNames 區域的支援。 GlobalNames 區域支援跨樹系解析單一標籤 DNS 名稱。 接受下列值：<ul><li>**0** -停用 GlobalNames 區域的支援。 當您將此命令的值設定為0時，DNS 伺服器服務不會解析 GlobalNames 區域中的單一標籤名稱。</li><li>**1** -啟用 GlobalNames 區域的支援。 當您將此命令的值設定為1時，DNS 伺服器服務會解析 GlobalNames 區域中的單一標籤名稱。</li></ul> |
| /enableglobalqueryblocklist `[0|1]` | 啟用或停用全域查詢封鎖清單的支援，以封鎖清單中名稱解析的名稱。 當服務第一次啟動時，DNS 伺服器服務預設會建立並啟用全域查詢封鎖清單。 若要查看目前的全域查詢封鎖清單，請使用 dnscmd/info **/globalqueryblocklist** 命令。 接受下列值：<ul><li>**0** -停用全域查詢封鎖清單的支援。 當您將此命令的值設定為0時，DNS 伺服器服務會回應封鎖清單中名稱的查詢。</li><li>**1** -啟用全域查詢封鎖清單的支援。 當您將此命令的值設定為1時，DNS 伺服器服務不會回應封鎖清單中名稱的查詢。</li></ul> |
| /eventloglevel `[0|1|2|4]` | 決定事件檢視器中記錄在 DNS 伺服器記錄檔中的事件。 接受下列值：<ul><li>**0** -不記錄任何事件。</li><li>**1** -只記錄錯誤。</li><li>**2** -只記錄錯誤和警告。</li><li>**4** -記錄錯誤、警告和資訊事件。 這是預設值。</li></ul> |
| /forwarddelegations `[0|1]` | 決定 DNS 伺服器如何處理委派子領域的查詢。 您可以將這些查詢傳送至查詢中所參考的子領域，或傳送到針對 DNS 伺服器命名的轉寄站清單。 只有在啟用轉送時，才會使用設定中的專案。 接受下列值：<ul><li>**0** -自動將參考委派 subzones 的查詢傳送至適當的子領域。 這是預設值。</li><li>**1** -將參考委派子領域的查詢轉送至現有的轉寄站。</li></ul> |
| /forwardingtimeout `[<seconds>]` | 判斷 (**0x1-0xffffffff**) DNS 伺服器在嘗試其他轉寄站之前等候轉寄站回應的秒數。 預設值是 **0x5**，也就是5秒。 |
| /globalneamesqueryorder `[0|1]` | 指定在解析名稱時，DNS 伺服器服務是否先在 GlobalNames 區域或本機區域中尋找。 接受下列值：<ul><li>**0** -DNS 伺服器服務會嘗試透過查詢 GlobalNames 區域來解析名稱，然後才能查詢其具有授權的區域。</li><li>**1** -DNS 伺服器服務會在查詢 GlobalNames 區域之前，藉由查詢已授權的區域來嘗試解析名稱。</li></ul> |
| /globalqueryblocklist`[[<name> [<name>]...]` | 以您指定的名稱清單取代目前的全域查詢封鎖清單。 如果您沒有指定任何名稱，此命令會清除封鎖清單。 根據預設，全域查詢封鎖清單會包含下列專案：<ul><li>isatap</li><li>wpad</li></ul>DNS 伺服器服務可在第一次啟動時，移除其中一個或兩個名稱（如果在現有的區域中找到這些名稱的話）。 |
| /isslave `[0|1]` | 決定當查詢轉送的查詢未收到回應時，DNS 伺服器的回應方式。 接受下列值：<ul><li>**0** -指定 DNS 伺服器不是從屬。 如果轉寄站沒有回應，DNS 伺服器會嘗試解析查詢本身。 這是預設值。</li><li>**1** -指定 DNS 伺服器為從屬。 如果轉寄站沒有回應，DNS 伺服器就會終止搜尋，並將失敗訊息傳送給解析程式。</li></ul> |
| /localnetpriority `[0|1]` | 決定當 DNS 伺服器有多個相同名稱的主機記錄時，傳回主機記錄的順序。 接受下列值：<ul><li>**0** -依記錄列在 DNS 資料庫中的順序傳回記錄。</li><li>**1** -傳回先有類似 IP 網路位址的記錄。 這是預設值。</li></ul> |
| /logfilemaxsize `[<size>]` | 指定 Dns .log 檔案 (**0x10000-0xffffffff**) 的大小上限（以位元組為單位）。 當檔案到達大小上限時，DNS 會覆寫最舊的事件。 預設大小是 **0x400000**，也就是 4 MB (MB) 。 |
| /logfilepath `[<path+logfilename>]` | 指定 Dns .log 檔的路徑。 預設路徑為 `%systemroot%\System32\Dns\Dns.log`。 您可以使用格式來指定不同的路徑 `path+logfilename` 。 |
| /logipfilterlist `<IPaddress> [,<IPaddress>...]` | 指定要記錄在 debug 記錄檔中的封包。 這些專案是 IP 位址的清單。 只會記錄進出清單中 IP 位址的封包。 |
| /loglevel `[<eventtype>]` | 決定要記錄在 Dns .log 檔中的事件種類。 每個事件種類都是以十六進位數位表示。 如果您想要在記錄檔中有一個以上的事件，請使用十六進位加法來新增值，然後輸入 sum。 接受下列值：<ul><li>**0x0** -DNS 伺服器不會建立記錄。 這是預設專案。</li><li>**0x10** -記錄查詢和通知。</li><li>**0x20** -記錄更新。</li><li>**0xFE** -記錄 nonquery 的交易。</li><li>**0x100** -記錄問題交易。</li><li>**0x200** -記錄解答。</li><li>**0x1000** -記錄傳送封包。</li><li>**0x2000** -記錄接收封包。</li><li>**0x4000** -記錄使用者資料包協定 (UDP) 封包。</li><li>**0x8000** -記錄傳輸控制通訊協定 (TCP) 封包。</li><li>**0xffff** -記錄所有封包。</li><li>**0x10000** -記錄 active directory 寫入交易。</li><li>**0x20000** -記錄 active directory 更新交易。</li><li>**0x1000000** -記錄完整的封包。</li><li>**0x80000000** -記錄寫入交易。</li><li></ul> |
| /maxcachesize | 指定 DNS 伺服器記憶體快取的大小上限（以 kb 為單位），以 (kb 為單位) 。 |
| /maxcachettl `[<seconds>]` | 判斷 (**0x0-0xffffffff**) a 記錄儲存在快取中的秒數。 如果使用了 **0x0** 設定，DNS 伺服器就不會快取記錄。 預設設定為 **0x15180** (86400 秒或1天) 。 |
| /maxnegativecachettl `[<seconds>]` | 指定 (**0x1-0xffffffff** 的秒數) 會將查詢的負面答案記錄到查詢中的專案仍會儲存在 DNS 快取中。 預設設定為 **0x384** (900 秒) 。 |
| /namecheckflag `[0|1|2|3]` | 指定檢查 DNS 名稱時使用的字元標準。 接受下列值：<ul><li>**0** -使用符合網際網路工程任務推動力的 ANSI 字元 (IETF) 要求 (rfc) 的評論。</li><li>**1** -使用不一定符合 IETF RFC 的 ANSI 字元。</li><li>**2** -使用多位元組 UCS 轉換格式 8 (utf-8) 字元。 這是預設值。</li><li>**3** -使用所有字元。</li></ul> |
| /norecursion `[0|1]` | 判斷 DNS 伺服器是否執行遞迴名稱解析。 接受下列值：<ul><li>**0** -DNS 伺服器會在查詢中要求時執行遞迴名稱解析。 這是預設值。</li><li>**1** -DNS 伺服器不會執行遞迴名稱解析。</li></ul> |
| /notcp | 此參數已淘汰，且在目前的 Windows Server 版本中不會有任何作用。 |
| /recursionretry `[<seconds>]` | 決定 DNS 伺服器在再次嘗試連線到遠端伺服器之前等候的秒數 (**0x1-0xffffffff**) 。 預設設定為 **0x3** (三秒) 。 當遞迴透過慢速廣域網路 (WAN) 連結時，應該增加這個值。 |
| /recursiontimeout `[<seconds>]` | 決定 DNS 伺服器在停止嘗試連線到遠端伺服器之前等候的秒數 (**0x1-0xffffffff**) 。 設定的範圍是從 **0x1** 到 **0xffffffff**。 預設設定為 **0xF** (15 秒) 。 當遞迴透過慢速 WAN 連結進行時，應該增加這個值。 |
| /roundrobin `[0|1]` | 決定當伺服器有多個相同名稱的主機記錄時，傳回主機記錄的順序。 接受下列值：<ul><li>**0** -DNS 伺服器不會使用迴圈配置資源。 相反地，它會將第一個記錄傳回給每個查詢。</li><li>**1** -DNS 伺服器會在其從頂端到相符記錄清單底部的記錄之間旋轉。 這是預設值。</li></ul> |
| /rpcprotocol `[0x0|0x1|0x2|0x4|0xFFFFFFFF]` | 指定遠端程序呼叫 (RPC) 在從 DNS 伺服器建立連線時使用的通訊協定。 接受下列值：<ul><li>**0x0** -停用 DNS 的 RPC。</li><li>**0x01** -使用 tcp/ip</li><li>**0x2** -使用具名管道。</li><li>**0x4** -使用 (LPC) 的本機程序呼叫。</li><li>**0xffffffff** -所有通訊協定。 這是預設值。</li></ul> |
| /scavenginginterval `[<hours>]` | 判斷是否已啟用 DNS 伺服器的清除功能，並設定在清除迴圈之間)  (**0x0-0xffffffff** 的小時數。 預設值為 **0x0**，此設定會停用 DNS 伺服器的清除。 大於 **0x0** 的設定可讓您清除伺服器，並設定清除週期之間的時數。 |
| /secureresponses `[0|1]` | 判斷 DNS 是否篩選儲存在快取中的記錄。 接受下列值：<ul><li>**0** -將所有對名稱查詢的回應儲存至快取。 這是預設值。</li><li>**1** -只將屬於相同 DNS 子樹的記錄儲存至快取。</li></ul> |
| /sendport `[<port>]` | 指定 DNS 用來將遞迴查詢傳送至其他 DNS 伺服器的埠號碼 (**0x0-0xffffffff**) 。 預設設定為 **0x0**，表示會隨機選取埠號碼。 |
| /serverlevelplugindll`[<dllpath>]` | 指定自訂外掛程式的路徑。 當 Dllpath 指定有效 DNS 伺服器外掛程式的完整路徑名稱時，DNS 伺服器會呼叫外掛程式中的函式，以解析位於所有本機裝載區域範圍之外的名稱查詢。 如果查詢的名稱超出外掛程式的範圍，則 DNS 伺服器會使用轉送或遞迴來執行名稱解析（如已設定）。 如果未指定 Dllpath，如果先前已設定自訂外掛程式，DNS 伺服器就會停止使用自訂外掛程式。 |
| /strictfileparsing `[0|1]` | 判斷 DNS 伺服器在載入區域時遇到錯誤記錄時的行為。 接受下列值：<ul><li>**0** -DNS 伺服器會繼續載入區域，即使伺服器遇到錯誤的記錄。 此錯誤會記錄在 DNS 記錄檔中。 這是預設值。</li><li>**1** -dns 伺服器會停止載入區域，並將錯誤記錄在 DNS 記錄檔中。</li></ul> |
| /updateoptions `<RecordValue>` | 禁止動態更新指定的記錄類型。 如果您想要在記錄檔中禁止超過一種記錄類型，請使用十六進位加法來新增值，然後輸入 sum。 接受下列值：<ul><li>**0x0** -不限制任何記錄類型。</li><li>**0x1** -排除 (SOA) 資源記錄的授權啟動。</li><li>**0x2** -排除名稱伺服器 (NS) 資源記錄。</li><li>**0x4** -排除名稱伺服器 (NS) 資源記錄的委派。</li><li>**0x8** -排除伺服器主機記錄。</li><li>**0x100** -在安全動態更新期間，不會 (SOA) 資源記錄中排除授權啟動。</li><li>**0x200** -在安全動態更新期間，排除根名稱伺服器 (NS) 資源記錄。</li><li>**0x30F** -在標準動態更新期間，排除名稱伺服器 (NS) 資源記錄、授權 (SOA) 資源記錄和伺服器主機記錄。 在安全動態更新期間，會將根名稱伺服器排除 (NS) 資源記錄，並 (SOA) 資源記錄來啟動授權單位。 允許委派和伺服器主機更新。</li><li>**0x400** -在安全動態更新期間，會將委派名稱伺服器排除 (NS) 資源記錄。</li><li>**0x800** -在安全動態更新期間，排除伺服器主機記錄。</li><li>**0x1000000** -排除委派簽署者 (DS) 記錄。</li><li>**0x80000000** -停用 DNS 動態更新。</li></ul> |
| /writeauthorityns `[0|1]` | 判斷 DNS 伺服器在回應的授權單位區段中，將名稱伺服器寫入 (NS) 資源記錄。 接受下列值：<ul><li>**0** -只將名稱伺服器 (NS) 資源記錄寫入至參考的授權區段。 這種設定符合 Rfc 1034、功能變數名稱概念和設備，以及 Rfc 2181 的說明，請查閱 DNS 規格。 這是預設值。</li><li>**1** -在所有成功的授權回應的授權單位區段中，寫入名稱伺服器 (NS) 資源記錄。</li></ul> |
| /xfrconnecttimeout `[<seconds>]` | 判斷 (**0x0-0xffffffff**) 主要 DNS 伺服器等候來自其次要伺服器之傳輸回應的秒數。 預設值為 **0x1E** (30 秒) 。 當超時值過期之後，連接就會終止。 |

### <a name="zone-level-syntax"></a>區域層級語法

修改指定區域的設定。 區功能變數名稱稱必須僅針對區域層級參數指定。

```
dnscmd /config <parameters>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<parameter>` | 指定設定、區功能變數名稱稱，以及指定值的選項。 參數值使用此語法： `zonename parameter [value]` 。 |
| /aging `<zonename>`| 啟用或停用特定區域中的清除。 |
| /allownsrecordsautocreation `<zonename>``[value]` |  (NS) 資源記錄 autocreation 設定覆寫 DNS 伺服器的名稱伺服器。 名稱伺服器 (NS) 先前為此區域註冊的資源記錄不會受到影響。 因此，如果您不想要的話，必須手動將它們移除。 |
| /allowupdate `<zonename>` | 判斷指定的區域是否接受動態更新。 |
| /forwarderslave `<zonename>` | 覆寫 DNS 伺服器 **/isslave** 設定。 |
| /forwardertimeout `<zonename>` | 決定 DNS 區域等候轉寄站回應的秒數，然後再嘗試另一個轉寄站。 這個值會覆寫伺服器層級所設定的值。 |
| /norefreshinterval `<zonename>` | 設定區域的時間間隔，在這段期間內，不會重新整理可動態更新指定區域中的 DNS 記錄。 |
| /refreshinterval `<zonename>` | 設定區域的時間間隔，在這段期間，重新整理可以動態更新指定區域中的 DNS 記錄。 |
| /securesecondaries `<zonename>` | 決定哪些次要伺服器可以從主伺服器接收此區域的區域更新。 |

## <a name="dnscmd-createbuiltindirectorypartitions-command"></a>dnscmd/createbuiltindirectorypartitions 命令

建立 DNS 應用程式目錄分割。 安裝 DNS 時，會在樹系和網域層級建立服務的應用程式目錄分割。 您可以使用此命令來建立已刪除或從未建立的 DNS 應用程式目錄分割。 如果沒有參數，此命令會建立網域的內建 DNS 目錄分割。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /createbuiltindirectorypartitions [/forest] [/alldomains]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| /forest | 建立樹系的 DNS 目錄分割。 |
| /alldomains | 為樹系中的所有網域建立 DNS 磁碟分割。 |

## <a name="dnscmd-createdirectorypartition-command"></a>dnscmd/createdirectorypartition 命令

建立 DNS 應用程式目錄分割。 安裝 DNS 時，會在樹系和網域層級建立服務的應用程式目錄分割。 此操作會建立額外的 DNS 應用程式目錄分割。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /createdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<partitionFQDN>` | 將建立的 DNS 應用程式目錄分割的 FQDN。 |

## <a name="dnscmd-deletedirectorypartition-command"></a>dnscmd/deletedirectorypartition 命令

移除現有的 DNS 應用程式目錄分割。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /deletedirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<partitionFQDN>` | 將移除的 DNS 應用程式目錄分割的 FQDN。 |

## <a name="dnscmd-directorypartitioninfo-command"></a>dnscmd/directorypartitioninfo 命令

列出指定的 DNS 應用程式目錄分割的相關資訊。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /directorypartitioninfo <partitionFQDN> [/detail]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<partitionFQDN>` | DNS 應用程式目錄分割的 FQDN。 |
| /detail | 列出應用程式目錄分割的所有資訊。 |

## <a name="dnscmd-enlistdirectorypartition-command"></a>dnscmd/enlistdirectorypartition 命令

將 DNS 伺服器加入至指定的目錄分割的複本集。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /enlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<partitionFQDN>` | DNS 應用程式目錄分割的 FQDN。 |

## <a name="dnscmd-enumdirectorypartitions-command"></a>dnscmd/enumdirectorypartitions 命令

列出指定之伺服器的 DNS 應用程式目錄分割。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /enumdirectorypartitions [/custom]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| /custom | 只列出使用者建立的目錄磁碟分割。 |

## <a name="dnscmd-enumrecords-command"></a>dnscmd/enumrecords 命令

列出 DNS 區域中指定節點的資源記錄。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /enumrecords <zonename> <nodename> [/type <rrtype> <rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<childname>] [/continue | /detail]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| /enumrecords | 列出指定區域中的資源記錄。 |
| `<zonename>` | 指定資源記錄所屬區域的名稱。 |
| `<nodename>` | 指定資源記錄節點的名稱。 |
| `[/type <rrtype> <rrdata>]` | 指定要列出的資源記錄類型和預期的資料類型。 接受下列值：<ul><li>`<rrtype>` -指定要列出的資源記錄類型。</li><li>`<rrdata>` -指定預期記錄的資料類型。</li></ul> |
| /authority | 包含授權資料。 |
| /glue | 包含粘附資料。 |
| /additional | 包含有關所列出資源記錄的所有其他資訊。 |
| /node | 只列出指定節點的資源記錄。 |
| /child | 只列出指定子域的資源記錄。 |
| /startchild`<childname>` | 在指定的子域開始清單。 |
| /continue | 只會列出其類型和資料的資源記錄。 |
| /detail | 列出資源記錄的所有資訊。 |

#### <a name="example"></a>範例

```
dnscmd /enumrecords test.contoso.com test /additional
```

## <a name="dnscmd-enumzones-command"></a>dnscmd/enumzones 命令

列出存在於指定之 DNS 伺服器上的區域。 **Enumzones** 參數可作為區域清單上的篩選準則。 如果未指定任何篩選準則，則會傳回完整的區域清單。 指定篩選準則時，只有符合該篩選準則的區域才會包含在傳回的區域清單中。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <partitionFQDN>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| /primary | 列出屬於標準主要區域或 active directory 整合式區域的所有區域。 |
| /secondary | 列出所有標準次要區域。 |
| /forwarder | 列出將未解析的查詢轉送到另一部 DNS 伺服器的區域。 |
| /stub | 列出所有存根區域。 |
| /cache | 只列出載入至快取中的區域。 |
| /auto-created] | 列出在 DNS 伺服器安裝期間自動建立的區域。 |
| /forward | 列出正向對應區域。 |
| /reverse | 列出反向對應區域。 |
| /ds | 列出 active directory 整合區域。 |
| /file | 列出檔案所支援的區域。 |
| /domaindirectorypartition | 列出儲存在網域目錄磁碟分割中的區域。 |
| /forestdirectorypartition | 列出儲存在樹系 DNS 應用程式目錄分割中的區域。 |
| /customdirectorypartition | 列出儲存在使用者定義的應用程式目錄分割中的所有區域。 |
| /legacydirectorypartition | 列出儲存在網域目錄磁碟分割中的所有區域。 |
| /directorypartition `<partitionFQDN>` | 列出儲存在指定目錄分割中的所有區域。 |

#### <a name="examples"></a>範例

- [範例2：顯示 DNS 伺服器上的完整區域清單](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-2-display-a-complete-list-of-zones-on-a-dns-server)) 

- [範例3：在 DNS 伺服器上顯示自動建立的區域清單](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-3-display-a-list-of-autocreated-zones-on-a-dns-server)

## <a name="dnscmd-exportsettings-command"></a>dnscmd/exportsettings 命令

建立一個文字檔，其中列出 DNS 伺服器的設定詳細資料。 文字檔的名稱為 *DnsSettings.txt*。 它位於 `%systemroot%\system32\dns` 伺服器的目錄中。 您可以使用 **dnscmd/exportsettings** 所建立的檔案中的資訊，針對設定問題進行疑難排解，或確定您已設定相同的多部伺服器。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /exportsettings
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |

## <a name="dnscmd-info-command"></a>dnscmd/info 命令

顯示指定伺服器登錄之 DNS 區段中的設定 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters` 。 若要顯示區域層級的登錄設定，請使用 `dnscmd zoneinfo` 命令。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /info [<settings>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<settings>` | **Info** 命令傳回的任何設定都可以個別指定。 如果未指定設定，則會傳回一般設定的報表。 |

#### <a name="example"></a>範例

- [範例4：顯示 DNS 伺服器的 IsSlave 設定](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-4-display-the-isslave-setting-from-a-dns-server)

- [範例5：顯示 DNS 伺服器的 RecursionTimeout 設定](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-5-display-the-recursiontimeout-setting-from-a-dns-server)

## <a name="dnscmd-ipvalidate-command"></a>dnscmd/ipvalidate 命令

測試 IP 位址是否識別出正常運作的 DNS 伺服器，或 DNS 伺服器是否可以作為轉寄站、根提示伺服器或特定區域的主伺服器。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /ipvalidate <context> [<zonename>] [[<IPaddress>]]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<context>` | 指定要執行的測試類型。 您可以指定下列任何一項測試：<ul><li>**/dnsservers** -測試具有您指定之位址的電腦是否為正常運作的 DNS 伺服器。</li><li>**/forwarders** -測試您指定的位址識別可作為轉寄站的 DNS 伺服器。</li><li>**/roothints** -測試您指定的位址識別可作為根提示名稱伺服器的 DNS 伺服器。</li><li>**/zonemasters** -測試您指定的位址，識別屬於 *zonename* 的主伺服器的 DNS 伺服器。 |
| `<zonename>` | 識別區域。 使用此參數搭配 **/zonemasters** 參數。 |
| `<IPaddress>` | 指定命令所測試的 IP 位址。 |

#### <a name="examples"></a>範例

```
nscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2
```

## <a name="dnscmd-nodedelete-command"></a>dnscmd/nodedelete 命令

刪除指定之主控制項的所有記錄。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /nodedelete <zonename> <nodename> [/tree] [/f]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定區域的名稱。 |
| `<nodename>` | 指定要刪除之節點的主機名稱。 |
| /tree | 刪除所有子記錄。 |
| /f | 執行命令，而不要求確認。 |

#### <a name="example"></a>範例

[範例6：從節點刪除記錄](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-6-delete-the-records-from-a-node)

## <a name="dnscmd-recordadd-command"></a>dnscmd/recordadd z 命令

將記錄新增至 DNS 伺服器中的指定區域。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /recordadd <zonename> <nodename> <rrtype> <rrdata>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定記錄所在的區域。 |
| `<nodename>` | 指定區域中的特定節點。 |
| `<rrtype>` | 指定要加入的記錄類型。 |
| `<rrdata>` | 指定預期的資料類型。 |

> [!NOTE]
> 加入記錄之後，請確定您使用正確的資料類型和資料格式。 如需資源記錄類型和適當資料類型的清單，請參閱 [Dnscmd 範例](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10))。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-recorddelete-command"></a>dnscmd/recorddelete 命令

刪除指定區域的資源記錄。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /recorddelete <zonename> <nodename> <rrtype> <rrdata> [/f]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定資源記錄所在的區域。 |
| `<nodename>` | 指定主機的名稱。 |
| `<rrtype>` | 指定要刪除的資源記錄類型。 |
| `<rrdata>` | 指定預期的資料類型。 |
| /f | 執行命令，而不要求確認。 由於節點可以有一個以上的資源記錄，因此，此命令會要求您非常明確地瞭解您想要刪除的資源記錄類型。 如果您指定資料類型，而且沒有指定資源記錄資料的類型，就會刪除具有指定節點之特定資料類型的所有記錄。 |

#### <a name="examples"></a>範例

```
dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-resetforwarders-command"></a>dnscmd/resetforwarders 命令

選取或重設 DNS 伺服器無法在本機解析 DNS 查詢時轉送的 IP 位址。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /resetforwarders <IPaddress> [,<IPaddress>]...][/timeout <timeout>] [/slave | /noslave]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<IPaddress>` | 列出 DNS 伺服器轉送未解析查詢的 IP 位址。 |
| /timeout `<timeout>` | 設定 DNS 伺服器等候轉寄站回應的秒數。 依預設，此值為5秒。 |
| /slave | 如果轉寄站無法解析查詢，則防止 DNS 伺服器執行自己的反復查詢。 |
| /noslave | 如果轉寄站無法解析查詢，可讓 DNS 伺服器執行自己的反復查詢。 這是預設值。 |
| /f | 執行命令，而不要求確認。 由於節點可以有一個以上的資源記錄，因此，此命令會要求您非常明確地瞭解您想要刪除的資源記錄類型。 如果您指定資料類型，而且沒有指定資源記錄資料的類型，就會刪除具有指定節點之特定資料類型的所有記錄。 |

##### <a name="remarks"></a>備註

- 根據預設，DNS 伺服器無法解析查詢時，會執行反復查詢。

- 使用 **resetforwarders** 命令設定 IP 位址，會導致 dns 伺服器在指定的 IP 位址上對 dns 伺服器執行遞迴查詢。 如果轉寄站無法解析查詢，則 DNS 伺服器可以執行自己的反復查詢。

- 如果使用 **/slave** 參數，DNS 伺服器就不會執行自己的反復查詢。 這表示 DNS 伺服器只會將未解析的查詢轉送到清單中的 DNS 伺服器，而且如果轉寄站無法解析這些查詢，它就不會嘗試反復查詢。 將一個 IP 位址設定為 DNS 伺服器的轉寄站會更有效率。 您可以針對網路中的內部伺服器使用 **resetforwarders** 命令，將無法解析的查詢轉寄到具有外部連線的一部 DNS 伺服器。

- 列出轉寄站的 IP 位址兩次會導致 DNS 伺服器嘗試轉送到該伺服器兩次。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave
dnscmd dnssvr1.contoso.com /resetforwarders /noslave
```

## <a name="dnscmd-resetlistenaddresses-command"></a>dnscmd/resetlistenaddresses 命令

指定伺服器上接聽 DNS 用戶端要求的 IP 位址。 根據預設，DNS 伺服器上的所有 IP 位址都會接聽用戶端 DNS 要求。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /resetlistenaddresses <listenaddress>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<listenaddress>` | 指定 DNS 伺服器上接聽 DNS 用戶端要求的 IP 位址。 如果未指定接聽位址，伺服器上的所有 IP 位址都會接聽用戶端要求。 |

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1
```

## <a name="dnscmd-startscavenging-command"></a>dnscmd/startscavenging 命令

告訴 DNS 伺服器在指定的 DNS 伺服器中嘗試立即搜尋過時的資源記錄。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /startscavenging
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |

##### <a name="remarks"></a>備註

- 此命令成功完成時，會立即開始清除。 如果清理失敗，則不會出現警告訊息。

- 雖然開始清除的命令似乎已順利完成，但除非符合下列先決條件，否則不會啟動清除：

    - 伺服器和區域都會啟用清除。

    - 區域已啟動。

    - 資源記錄具有時間戳記。

- 如需有關如何啟用伺服器清除的詳細資訊，請參閱 **/config** 區段中 **伺服器層級語法** 下的 **scavenginginterval** 參數。

- 如需有關如何啟用區域清除的詳細資訊，請參閱 **/config** 區段中 **區域層級語法** 下的 **過時** 參數。

- 如需有關如何重新開機已暫停區域的詳細資訊，請參閱本文中的 **zoneresume** 參數。

- 如需有關如何檢查時間戳記之資源記錄的詳細資訊，請參閱本文中的 **ageallrecords** 參數。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /startscavenging
```

## <a name="dnscmd-statistics-command"></a>dnscmd/statistics 命令

顯示或清除指定之 DNS 伺服器的資料。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /statistics [<statid>] [/clear]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<statid>` | 指定要顯示的統計資料或統計資料組合。 **Statistics** 命令會顯示啟動或繼續時，在 DNS 伺服器上啟動的計數器。 識別碼是用來識別統計資料。 如果未指定統計資料識別碼，則會顯示所有統計資料。 可以指定的數位，以及顯示的對應統計資料，可能包括：<ul><li>**00000001** 時間</li><li>**00000002** -查詢</li><li>**00000004** ->query2</li><li>**00000008** -遞迴</li><li>**00000010** -Master</li><li>**00000020** -次要</li><li>**00000040** -WINS</li><li>**00000100** -更新</li><li>**00000200** -SkwanSec</li><li>**00000400** -Ds</li><li>**00010000** -記憶體</li><li>**00100000** -PacketMem</li><li>**00040000** -Dbase</li><li>**00080000** -記錄</li><li>**00200000** -NbstatMem</li><li>**/clear** -將指定的統計資料計數器重設為零。</li></ul> |

#### <a name="examples"></a>範例

- [範例7： ](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-7-display-time-statistics-for-a-dns-server)

- [範例8：顯示 DNS 伺服器的 NbstatMem 統計資料](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-8-display-nbstatmem-statistics-for-a-dns-server)

## <a name="dnscmd-unenlistdirectorypartition-command"></a>dnscmd/unenlistdirectorypartition 命令

從指定的目錄分割的複本集移除 DNS 伺服器。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /unenlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<partitionFQDN>` | 將移除的 DNS 應用程式目錄分割的 FQDN。 |

## <a name="dnscmd-writebackfiles-command"></a>dnscmd/writebackfiles 命令

檢查 DNS 伺服器記憶體的變更，並將其寫入至持續性儲存體。 **Writebackfiles** 命令會更新所有的中途區域或指定的區域。 當記憶體中有變更尚未寫入持續性儲存體時，區域就會變更。 這是會檢查所有區域的伺服器層級作業。 您可以在這項作業中指定一個區域，也可以使用 **zonewriteback** 操作。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /writebackfiles <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要更新之區域的名稱。 |

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /writebackfiles
```

## <a name="dnscmd-zoneadd-command"></a>dnscmd/zoneadd 命令

將區域新增至 DNS 伺服器。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneadd <zonename> <zonetype> [/dp <FQDN> | {/domain | enterprise | legacy}]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定區域的名稱。 |
| `<zonetype>` | 指定要建立的區欄位型別。 指定 **/forwarder** 或 **/dsforwarder** 的區欄位型別會建立執行條件式轉送的區域。 每個區欄位型別都有不同的必要參數：<ul><li>**/dsprimary** -建立 active directory 整合式區域。</li><li>**/primary/file `<filename>`**-建立標準主要區域，並指定將用來儲存區域資訊的檔案名。</li><li>**/secondary `<masterIPaddress> [<masterIPaddress>...]`**-建立標準次要區域。</li><li>**/stub `<masterIPaddress> [<masterIPaddress>...]` /File `<filename>`** -建立檔案支援的存根區域。</li><li>**/dsstub `<masterIPaddress> [<masterIPaddress>...]`**-建立 active directory 整合式存根區域。</li><li>**/forwarder `<masterIPaddress> [<masterIPaddress>]` .../File `<filename>`** -指定建立的區域將無法解析的查詢轉送到另一部 DNS 伺服器。</li><li>**/dsforwarder** -指定建立的 active directory 整合式區域將無法解析的查詢轉送到另一部 DNS 伺服器。</li></ul> |
| `<FQDN>` | 指定目錄分割的 FQDN。 |
| /domain | 將區域儲存在網域目錄磁碟分割上。 |
| /enterprise | 將區域儲存在企業目錄磁碟分割上。 |
| /legacy | 將區域儲存在舊版目錄磁碟分割上。 |

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zonechangedirectorypartition-command"></a>dnscmd/zonechangedirectorypartition 命令

變更指定區域所在的目錄分割。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zonechangedirectorypartition <zonename> {[<newpartitionname>] | [<zonetype>]}
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 區域所在之目前目錄分割的 FQDN。 |
| `<newpartitionname>` | 將移至區域的目錄分割的 FQDN。 |
| `<zonetype>` | 指定將移至區域的目錄分割類型。 |
| /domain | 將區域移至內建網域目錄分割。 |
| /forest | 將區域移至內建樹系目錄分割。 |
| /legacy | 將區域移至為預先 active directory 網域控制站建立的目錄磁碟分割。 原生模式不需要這些目錄分割。 |

## <a name="dnscmd-zonedelete-command"></a>dnscmd/zonedelete 命令

刪除指定的區域。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zonedelete <zonename> [/dsdel] [/f]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要刪除之區域的名稱。 |
| /dsdel | 從 Azure Directory 網域服務刪除區域 (AD DS) 。 |
| /f | 執行命令，而不要求確認。 |

#### <a name="examples"></a>範例

- [範例9：從 DNS 伺服器刪除區域](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-9-delete-a-zone-from-a-dns-server)

## <a name="dnscmd-zoneexport-command"></a>dnscmd/zoneexport 命令

建立文字檔，其中列出指定區域的資源記錄。 **Zoneexport** 作業會為 active directory 整合區域建立資源記錄的檔案，以供疑難排解之用。 根據預設，這個命令所建立的檔案會放在 DNS 目錄中，也就是 `%systemroot%/System32/Dns` 目錄。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneexport <zonename> <zoneexportfile>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定區域的名稱。 |
| `<zoneexportfile>` | 指定要建立之檔案的名稱。 |

#### <a name="examples"></a>範例

- [範例10：將區域資源記錄清單匯出至檔案](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-10-export-zone-resource-records-list-to-a-file)

## <a name="dnscmd-zoneinfo"></a>dnscmd/zoneinfo z

顯示指定區域的登錄區段中的設定： `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\<zonename>`

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneinfo <zonename> [<setting>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定區域的名稱。 |
| `<setting>` | 您可以個別指定 **zoneinfo** 命令傳回的任何設定。 如果您未指定設定，則會傳回所有設定。 |

##### <a name="remarks"></a>備註

- 若要顯示伺服器層級的登錄設定，請使用 **/info** 命令。

- 若要查看您可以使用此命令顯示的設定清單，請參閱 **/config** 命令。

#### <a name="examples"></a>範例

- [範例11：從登錄顯示 RefreshInterval 設定](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-11-display-refreshinterval-setting-from-the-registry)

- [範例12：從登錄顯示過時設定](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-12-display-aging-setting-from-the-registry)

## <a name="dnscmd-zonepause-command"></a>dnscmd/zonepause 命令

暫停指定的區域，這會忽略查詢要求。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zonepause <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要暫停之區域的名稱。 |

##### <a name="remarks"></a>備註

- 若要在暫停區域後將它設為可供使用，請使用 **/zoneresume** 命令。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zonepause test.contoso.com
```

## <a name="dnscmd-zoneprint-command"></a>dnscmd/zoneprint 命令

列出區域中的記錄。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneprint <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要列出之區域的名稱。 |







## <a name="dnscmd-zonerefresh-command"></a>dnscmd/zonerefresh 命令

從主要區域強制更新次要 DNS 區域。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zonerefresh <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要重新整理之區域的名稱。 |

##### <a name="remarks"></a>備註

- **Zonerefresh** 命令會強制檢查主伺服器 s (SOA) 資源記錄中主伺服器的版本號碼。 如果主伺服器上的版本號碼高於次要伺服器的版本號碼，則會起始會更新次要伺服器的區域傳輸。 如果版本號碼相同，則不會進行任何區域傳輸。

- 強制檢查預設會每隔15分鐘進行一次。 若要變更預設值，請使用 `dnscmd config refreshinterval` 命令。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com
```

## <a name="dnscmd-zonereload-command"></a>dnscmd/zonereload 命令

從其來源複製區域資訊。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zonereload <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要重載之區域的名稱。 |

##### <a name="remarks"></a>備註

- 如果區域與 active directory 整合，則會從 Active Directory Domain Services (AD DS) 重載。

- 如果區域是標準的檔案備份區域，則會從檔案重載。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zonereload test.contoso.com
```

## <a name="dnscmd-zoneresetmasters-command"></a>dnscmd/zoneresetmasters 命令

重設主伺服器的 IP 位址，提供區域傳輸資訊給次要區域。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneresetmasters <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要重設之區域的名稱。 |
| /local | 設定本機主要清單。 此參數用於 active directory 整合式區域。 |
| `<IPaddress>` | 次要區域之主伺服器的 IP 位址。 |

##### <a name="remarks"></a>備註

- 此值最初是在次要區域建立時設定。 在次要伺服器上使用 **zoneresetmasters** 命令。 如果在主要 DNS 伺服器上設定此值，則此值沒有任何作用。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local
```

## <a name="dnscmd-zoneresetscavengeservers-command"></a>dnscmd/zoneresetscavengeservers 命令

變更可清除指定區域之伺服器的 IP 位址。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneresetscavengeservers <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要清除的區域。 |
| /local | 設定本機主要清單。 此參數用於 active directory 整合式區域。 |
| `<IPaddress>` | 列出可執行清理之伺服器的 IP 位址。 如果省略此參數，則裝載此區域的所有伺服器都可以進行清除。 |

##### <a name="remarks"></a>備註

- 依預設，裝載區域的所有伺服器都可以清除該區域。

- 如果區域裝載于多部 DNS 伺服器上，您可以使用此命令來減少清除區域的次數。

- 必須在受此命令影響的 DNS 伺服器和區域上啟用清除。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2
```

## <a name="dnscmd-zoneresetsecondaries-command"></a>dnscmd/zoneresetsecondaries 命令

指定當要求進列區域轉送時，主伺服器會回應之次要伺服器的 IP 位址清單。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneresetsecondaries <zonename> {/noxfr | /nonsecure | /securens | /securelist <securityIPaddresses>} {/nonotify | /notify | /notifylist <notifyIPaddresses>}
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定將重設其次要伺服器的區功能變數名稱稱。 |
| /local | 設定本機主要清單。 此參數用於 active directory 整合式區域。 |
| /noxfr | 指定不允許任何區域傳輸。 |
| /nonsecure | 指定授與所有區域轉送要求。 |
| /securens | 指定只有在名稱伺服器 (NS) 資源記錄中列出的伺服器才會被授與該區域的傳送。 |
| /securelist | 指定只授與伺服器清單的區域傳輸。 此參數後面必須接著主伺服器所使用的 IP 位址或位址。 |
| `<securityIPaddresses>` | 列出從主伺服器接收區域傳輸的 IP 位址。 這個參數只能搭配 **/securelist** 參數使用。 |
| /nonotify | 指定不將任何變更通知傳送到次要伺服器。 |
| /notify | 指定將變更通知傳送至所有次要伺服器。 |
| /notifylist | 指定只將變更通知傳送到伺服器清單。 此命令後面必須接著主伺服器所使用的 IP 位址或位址。 |
| `<notifyIPaddresses>` | 指定傳送變更通知的次要伺服器的 IP 位址或伺服器位址。 這份清單只適用于 **/notifylist** 參數。 |

##### <a name="remarks"></a>備註

- 您可以使用主伺服器上的 **zoneresetsecondaries** 命令，來指定它如何回應來自次要伺服器的區域轉送要求。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2
```

## <a name="dnscmd-zoneresettype-command"></a>dnscmd/zoneresettype 命令

變更區域的類型。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneresettype <zonename> <zonetype> [/overwrite_mem | /overwrite_ds]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 識別將變更類型的區域。 |
| `<zonetype>` | 指定要建立的區欄位型別。 每個類型都有不同的必要參數，包括：<ul><li>**/dsprimary** -建立 active directory 整合式區域。</li><li>**/primary/file `<filename>`**-建立標準的主要區域。</li><li>**/secondary `<masterIPaddress> [,<masterIPaddress>...]`**-建立標準次要區域。</li><li>**/stub `<masterIPaddress>[,<masterIPaddress>...]` /File `<filename>`** -建立檔案支援的存根區域。</li><li>**/dsstub `<masterIPaddress>[,<masterIPaddress>...]`**-建立 active directory 整合式存根區域。</li><li>**/forwarder `<masterIPaddress[,<masterIPaddress>]` .../File `<filename>`** -指定建立的區域將無法解析的查詢轉送到另一部 DNS 伺服器。</li><li>**/dsforwarder** -指定建立的 active directory 整合式區域將無法解析的查詢轉送到另一部 DNS 伺服器。</li></ul> |
| /overwrite_mem | 覆寫 AD DS 中資料的 DNS 資料。 |
| /overwrite_ds | 覆寫 AD DS 中的現有資料。 |

##### <a name="remarks"></a>備註

- 將區欄位型別設定為 **/dsforwarder** 時，會建立執行條件式轉送的區域。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zoneresume-command"></a>dnscmd/zoneresume 命令

啟動先前暫停的指定區域。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneresume <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要繼續之區域的名稱。 |

##### <a name="remarks"></a>備註

- 您可以使用此作業從 **/zonepause** 作業重新開機。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com
```

## <a name="dnscmd-zoneupdatefromds-command"></a>dnscmd/zoneupdatefromds 命令

從 AD DS 更新指定的 active directory 整合式區域。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zoneupdatefromds <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要更新之區域的名稱。 |

##### <a name="remarks"></a>備註

- Active directory 整合區域預設會每隔五分鐘執行一次此更新。 若要變更此參數，請使用 `dnscmd config dspollinginterval` 命令。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zoneupdatefromds
```

## <a name="dnscmd-zonewriteback-command"></a>dnscmd/zonewriteback 命令

檢查 DNS 伺服器記憶體是否有與指定區域相關的變更，並將其寫入至持續性儲存體。

### <a name="syntax"></a>語法

```
dnscmd [<servername>] /zonewriteback <zonename>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ----------- |
| `<servername>` | 指定要管理的 DNS 伺服器，以 IP 位址、FQDN 或主機名稱表示。 如果省略此參數，則會使用本機伺服器。 |
| `<zonename>` | 指定要更新之區域的名稱。 |

##### <a name="remarks"></a>備註

- 這是區域層級的作業。 您可以使用 **/writebackfiles** 操作來更新 DNS 伺服器上的所有區域。

#### <a name="examples"></a>範例

```
dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
