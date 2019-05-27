---
title: Dnscmd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39478e9b7dd8e8c69ed07f5d431486a7ed96b9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815499"
---
# <a name="dnscmd"></a>Dnscmd

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

管理 DNS 伺服器的命令列介面。 此公用程式可用於指令碼批次檔可協助自動化例行的 DNS 管理工作，或在您網路上執行簡單的自動的安裝和設定新的 DNS 伺服器。  
## <a name="syntax"></a>語法  
```  
dnscmd <ServerName> <command> [<command parameters>]  
```  
## <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|<ServerName>|遠端或本機 DNS 伺服器 IP 位址或主機名稱。|  
## <a name="commands"></a>命令  
|命令|描述|  
|------|--------|  
|[dnscmd /ageallrecords](#BKMK_1)|設定區域或節點中的所有時間戳記的目前時間。|  
|[dnscmd /clearcache](#BKMK_2)|清除 DNS 伺服器快取。|  
|[dnscmd /config](#BKMK_3)|重設 DNS 伺服器或內部網路區域設定。|  
|[dnscmd /createbuiltindirectorypartitions](#BKMK_4)|會建立內建的 DNS 應用程式目錄分割。|  
|[dnscmd /createdirectorypartition](#BKMK_5)|建立 DNS 應用程式目錄分割。|  
|[dnscmd /deletedirectorypartition](#BKMK_6)|刪除 DNS 應用程式目錄分割。|  
|[dnscmd /directorypartitioninfo](#BKMK_7)|列出的 DNS 應用程式目錄磁碟分割的相關資訊。|  
|[dnscmd /enlistdirectorypartition](#BKMK_8)|將複寫設定的 DNS 應用程式目錄分割中的 DNS 伺服器。|  
|[dnscmd /enumdirectorypartitions](#BKMK_9)|列出伺服器的 DNS 應用程式目錄分割。|  
|[dnscmd /enumrecords](#BKMK_10)|列出區域中的資源記錄。|  
|[dnscmd /enumzones](#BKMK_11)|列出指定的伺服器裝載的區域。|  
|[dnscmd /exportsettings](#BKMK_25a)|寫入文字檔案中的伺服器組態資訊。|  
|[dnscmd /info](#BKMK_12)|取得伺服器資訊。|  
|[dnscmd /ipvalidate](#BKMK_29a)|驗證的遠端 DNS 伺服器。|  
|[dnscmd /nodedelete](#BKMK_13)|刪除區域中的節點的所有記錄。|  
|[dnscmd /recordadd](#BKMK_14)|將資源記錄新增至區域。|  
|[dnscmd /recorddelete](#BKMK_15)|從區域移除資源記錄。|  
|[dnscmd /resetforwarders](#BKMK_16)|設定 DNS 伺服器來轉送遞迴查詢。|  
|[dnscmd /resetlistenaddresses](#BKMK_17)|設定為 DNS 要求提供服務的伺服器 IP 位址。|  
|[dnscmd /startscavenging](#BKMK_18)|起始伺服器清除功能。|  
|[dnscmd /statistics](#BKMK_19)|查詢，或清除伺服器統計資料。|  
|[dnscmd /unenlistdirectorypartition](#BKMK_20)|從 複寫設定的 DNS 應用程式目錄分割移除 DNS 伺服器。|  
|[dnscmd /writebackfiles](#BKMK_21)|所有區域或根提示將資料都儲存至檔案。|  
|[dnscmd /zoneadd](#BKMK_22)|DNS 伺服器上建立新的區域。|  
|[dnscmd /zonechangedirectorypartition](#BKMK_23)|變更區域所在的目錄磁碟分割。|  
|[dnscmd /zonedelete](#BKMK_24)|從 DNS 伺服器中刪除區域。|  
|[dnscmd /zoneexport](#BKMK_25)|寫入文字檔案中的區域的資源記錄。|  
|[dnscmd /zoneinfo](#BKMK_26)|顯示區域資訊。|  
|[dnscmd /zonepause](#BKMK_27)|暫停區域。|  
|[dnscmd /zoneprint](#BKMK_28)|在區域中，會顯示所有記錄。|  
|[dnscmd /zonerefresh](#BKMK_30)|強制重新整理從主要區域的次要區域。|  
|[dnscmd /zonereload](#BKMK_31)|重新載入區域，以從其資料庫。|  
|[dnscmd /zoneresetmasters](#BKMK_32)|變更主要伺服器提供次要區域的區域傳輸資訊。|  
|[dnscmd /zoneresetscavengeservers](#BKMK_33)|變更可以清除區域的伺服器。|  
|[dnscmd /zoneresetsecondaries](#BKMK_34)|重設為區域的次要資料庫資訊。|  
|[dnscmd /zoneresettype](#BKMK_29)|變更區域類型。|  
|[dnscmd /zoneresume](#BKMK_35)|繼續區域。|  
|[dnscmd /zoneupdatefromds](#BKMK_36)|使用從 active directory 網域服務 (AD DS) 中更新 active directory 整合的區域。|  
|[dnscmd /zonewriteback](#BKMK_37)|將區域資料儲存至檔案。|  
### <a name="BKMK_1"></a>dnscmd /ageallrecords  
設定在指定的區域或節點的 DNS 伺服器上的資源記錄的時間戳記的目前時間。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /ageallrecords <ZoneName>[<NodeName>] | [/tree]|[/f]  
```  
#### <a name="parameters"></a>參數  
<ServerName>  
指定管理計劃的系統管理員的 DNS 伺服器由 IP 位址、 完整的網域名稱 (FQDN) 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
<ZoneName>  
指定區域的 FQDN。  
<NodeName>  
在區域中指定特定節點或樹狀子目錄。 *NodeName*指定節點或樹狀子目錄中使用下列區域：  
-   @ 根區域或 FQDN  
-   節點 （以句點 （.） 結尾的名稱） 的 FQDN  
-   單一區域根目錄的相對名稱標籤  
/tree  
指定所有的子節點也會收到的時間戳記。  
/f  
執行命令，而不要求確認。  
#### <a name="remarks"></a>備註  
-   **Ageallrecords**命令適用於 DNS 的目前版本和舊版中的過時及清除功能不支援的 DNS 之間的回溯相容性。 將資源記錄沒有時間戳記的時間戳記的目前時間，並在沒有時間戳記的資源記錄上設定目前的時間。  
-   除非記錄時間戳記，否則不會發生記錄清除功能。 名稱伺服器 (NS) 資源記錄，啟動授權 (SOA) 資源記錄，以及 Windows 網際網路名稱服務 (WINS) 資源記錄不包含在清除過程中，而且它們不會加上時間戳記，即使**ageallrecords**命令會執行。  
-   除非清除已啟用 DNS 伺服器和區域，此命令會失敗。 如需有關如何啟用清除區域的資訊，請參閱 <<c0>  **過時**下方區域層級語法中的參數[config](#BKMK_3)命令。  
-   的 DNS 資源記錄的時間戳記使它們與非 Windows 2000，Windows XP 或 Windows Server 2003 作業系統執行的 DNS 伺服器不相容。 使用您加入的時間戳記**ageallrecords**命令便無法回復。  
-   如果未指定任何選擇性參數，此命令會傳回在指定的節點上的所有資源記錄。 如果至少一個選擇性的參數，指定的值**dnscmd**列舉只對應至或多個值的選擇性參數或參數中指定的資源記錄。  
#### <a name="example"></a>範例  
請參閱[範例 1:資源記錄的時間戳記會將目前時間](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_2"></a>dnscmd /clearcache  
清除指定的 DNS 伺服器上的資源記錄的 DNS 快取記憶體。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /clearcache  
```  
#### <a name="parameters"></a>參數  
<ServerName>  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
#### <a name="sample-usage"></a>使用範例  
`dnscmd dnssvr1.contoso.com /clearcache`  
### <a name="BKMK_3"></a>dnscmd /config  
變更登錄的 DNS 伺服器和個別的區域中的值。 接受伺服器層級設定和區域層級設定。  
> [!CAUTION]  
> 除非您別無選擇，否則請勿直接編輯登錄。 登錄編輯程式略過標準保護，讓設定可以使效能降低、 損害您的系統，或甚至要求您必須重新安裝 Windows。 您可以安全地改變大部分的登錄設定，使用 控制台 或 Microsoft Management Console (mmc) 中的程式。 如果您必須直接編輯登錄，請將它備份第一個。 讀取登錄編輯器說明，如需詳細資訊。  
#### <a name="server-level-syntax"></a>伺服器層級語法  
```  
dnscmd [<ServerName>] /config <Parameter>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
修改指定伺服器的組態。  
#### <a name="parameters"></a>參數  
<ServerName>  
指定想要管理的 DNS 伺服器以本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
<Parameter>  
指定的設定及選擇值。 參數值使用此語法：*Parameter* [*Value*]  
本節的其餘部分將說明下列參數值：  
-   **/addressanswerlimit**  
-   **/bindsecondaries**  
-   **/bootmethod**  
-   **/defaultagingstate**  
-   **/defaultnorefreshinterval**  
-   **/defaultrefreshinterval**  
-   **/disableautoreversezones**  
-   **/disablensrecordsautocreation**  
-   **/dspollinginterval**  
-   **/dstombstoneinterval**  
-   **/ednscachetimeout**  
-   **/enablednsprobes**  
-   **/enablednssec**  
-   **/enableglobalnamessupport**  
-   **/enableglobalqueryblocklist**  
-   **/eventloglevel**  
-   **/forwarddelegations**  
-   **/forwardingtimeout**  
-   **/globalnamesqueryorder**  
-   **/globalqueryblocklist**  
-   **/isslave**  
-   **/localnetpriority**  
-   **/logfilemaxsize**  
-   **/logfilepath**  
-   **/logipfilterlist**  
-   **/loglevel**  
-   **/maxcachesize**  
-   **/maxcachettl**  
-   **/namecheckflag**  
-   **/notcp**  
-   **/norecursion**  
-   **/recursionretry**  
-   **/recursiontimeout**  
-   **/roundrobin**  
-   **/rpcprotocol**  
-   **/scavenginginterval**  
-   **/secureresponses**  
-   **/sendport**  
-   **/strictfileparsing**  
-   **/updateoptions**  
-   **/writeauthorityns**  
-   **/xfrconnecttimeout**  
**/addressanswerlimit [0|5-28]**  
指定 DNS 伺服器可以傳送以回應查詢的主機記錄的數目上限。 值可以是零 (0)，或它可以是介於 5 到 28 的記錄。 預設值為零 (0)。  
**/bindsecondaries[0|1]**  
變更區域轉送的格式，使它可以達到最大的壓縮和效率。 不過，此格式不相容於舊版的柏克萊網際網路名稱網域 (BIND)。  
**0**  
使用最大的壓縮。 此格式會與繫結版本 4.9.4 相容和更新版本。  
**1**  
將一個資源記錄，每個訊息傳送到非 Microsoft DNS 伺服器。 此格式會比 4.9.4 與繫結版本相容。 此為預設設定。  
**/bootmethod[0|1|2|3]**  
決定的來源從中 DNS 伺服器取得其組態資訊。  
**0**  
清除組態資訊的來源。  
**1**  
從位於 DNS 目錄，也就是預設的 %systemroot%\System32\DNS 繫結檔案的載入。  
**2**  
從登錄載入。  
**3**  
從 AD DS 與登錄的負載。 此為預設設定。  
**/defaultagingstate[0|1]**  
決定是否在新建立的區域上的預設會啟用 DNS 的清除功能。  
**0**  
清除停用。 此為預設設定。  
**1**  
啟用清除功能。  
**/defaultnorefreshinterval[0x1-0xFFFFFFFF|0xA8]**  
設定一段時間沒有重新整理被動態更新的記錄。 在伺服器上的區域會自動繼承此值。 若要變更預設值，請輸入值的範圍內**0x1 0xFFFFFFFF**。 從伺服器的預設值是**0xA8**。  
**/defaultrefreshinterval [0x1-0xFFFFFFFF|0xA8]**  
設定一段時間所允許之 DNS 記錄的動態更新。 在伺服器上的區域會自動繼承此值。 若要變更預設值，請輸入值的範圍內**0x1 0xFFFFFFFF**。 從伺服器的預設值是**0xA8**。  
**/disableautoreversezones [0|1]**  
啟用或停用自動建立反向對應區域。 反向對應區域提供的網際網路通訊協定 (IP) 位址的 DNS 網域名稱的解析。  
**0**  
讓您自動建立反向對應區域。 此為預設設定。  
**1**  
停用自動建立反向對應區域。  
**/disablensrecordsautocreation {0|1}**  
指定是否在 DNS 伺服器會自動建立名稱伺服器 (NS) 資源記錄，其裝載的區域。  
**0**  
自動會建立名稱伺服器 (NS) 資源記錄的區域，DNS 伺服器所裝載。  
**1**  
不會自動建立的區域的名稱伺服器 (NS) 資源記錄，DNS 伺服器所裝載。  
**/dspollinginterval 0-30**  
指定 DNS 伺服器輪詢 AD DS 變更在 active directory 整合區域的頻率。  
**/dstombstoneinterval [1-30]**  
以秒為單位來保留已刪除的記錄，在 AD DS 中的時間量。  
**/ednscachetimeout [<seconds>]**  
指定擴充的 DNS (EDNS) 資訊會快取的秒數。 最小值為 3600，和最大值是 15,724,800。 預設值為 604,800 秒 （1 週）。  
**/enableednsprobes {0|1}**  
啟用或停用來探查其他伺服器，以判斷它們是否支援 EDNS 伺服器。  
**0**  
停用 active EDNS 探查支援。  
**1**  
啟用 active EDNS 探查支援。  
**/enablednssec {0|1}**  
啟用或停用 DNS 安全性延伸模組 (DNSSEC) 支援。  
**0**  
停用 DNSSEC。  
**1**  
可讓 DNSSEC。  
**/enableglobalnamessupport {0|1}**  
啟用或停用 GlobalNames 區域的支援。 GlobalNames 區域支援跨樹系的單一標籤 DNS 名稱的解析。  
**0**  
停用支援 GlobalNames 區域。 當您將設定值，此命令**0**，DNS 伺服器服務不會解析在 GlobalNames 區域中的單一標籤名稱。  
**1**  
啟用 GlobalNames 區域的支援。 當您將設定值，此命令**1**，DNS 伺服器服務解析在 GlobalNames 區域中的單一標籤名稱。  
**/enableglobalqueryblocklist {0|1}**  
啟用或停用全域查詢封鎖清單會封鎖清單中的名稱的名稱解析的支援。 DNS 伺服器服務會建立，並預設會啟用 「 全域查詢封鎖清單時，服務會啟動第一次。 若要檢視目前的全域查詢封鎖清單，請使用**dnscmd /info /globalqueryblocklist**命令。  
**0**  
停用支援的 「 全域查詢封鎖清單。 當您將設定值，此命令**0**，DNS 伺服器服務回應的查詢封鎖清單中的名稱。  
**1**  
啟用支援全域查詢封鎖清單。 當您將設定值，此命令**1**，DNS 伺服器服務未回應的查詢封鎖清單中的名稱。  
**/eventloglevel [0|1|2|4]**  
決定要在事件檢視器中的 DNS 伺服器記錄檔中記錄的事件。  
**0**  
不記錄任何事件。  
**1**  
只記錄錯誤。  
**2**  
只將錯誤和警告記錄。  
**4**  
記錄錯誤、 警告和資訊事件。 此為預設設定。  
**/forwarddelegations [0|1]**  
決定 DNS 伺服器在處理委派子區域的查詢。 這些查詢可以指在查詢中的子區域，或名為轉寄站清單傳送 DNS 伺服器。 只有當啟用轉送時，會使用在設定中的項目。  
**0**  
會自動傳送至適當的子區域的委派子區域是指的查詢。 此為預設設定。  
**1**  
轉送查詢參考到現有的轉寄站委派子區域。  
**/forwardingtimeout [<seconds>]**  
決定多少秒 (0x1 0xFFFFFFFF) 的 DNS 伺服器等待使用轉寄站，然後再嘗試另一個轉寄站回應。 預設值是**0x5**，這是 5 秒。  
**/globalneamesqueryorder** {**0|1**}  
指定是否將 DNS 伺服器服務會先查看在 GlobalNames 區域或區域的區域中解析名稱時。  
**0**  
DNS 伺服器服務會嘗試藉由查詢 GlobalNames 區域之前它會查詢它有權管理區域, 解析名稱。  
**1**  
DNS 伺服器服務會嘗試藉由查詢它的授權之前，它會查詢 GlobalNames 區域的區域解析名稱。  
**/globalqueryblocklist[[<name> [<name>]...]**  
一份您所指定的名稱，取代目前的全域查詢封鎖清單。 如果您未指定任何名稱，此命令會清除的區塊清單。 根據預設，「 全域查詢封鎖清單會包含下列項目：  
-   isatap  
-   wpad  
DNS 伺服器服務可以移除一個或兩個這些名稱時開始第一次，如果在現有的區域中找到這些名稱。  
**/isslave [0|1]**  
決定 DNS 伺服器轉寄的查詢會不收到任何回應時的回應。  
**0**  
指定 DNS 伺服器不是次級 (也稱為*從屬*)。 如果將轉寄站沒有回應，DNS 伺服器會嘗試解析查詢本身。 此為預設設定。  
**1**  
指定 DNS 伺服器從屬。 轉寄站沒有回應，如果 DNS 伺服器會終止搜尋，並將失敗訊息傳送給解析程式。  
**/localnetpriority [0|1]**  
決定當 DNS 伺服器具有相同名稱的多個主機記錄時，會傳回主應用程式記錄的順序。  
**0**  
在 DNS 資料庫中列出的順序傳回的記錄。  
**1**  
傳回的記錄，有類似的 IP 網路位址的第一次。 此為預設設定。  
**/logfilemaxsize [<size>]**  
以位元組為單位 (0x10000 0xFFFFFFFF) Dns.log 檔案的指定大小上限。 當檔案達到其大小上限時，DNS 會覆寫最舊的事件。 預設大小是 0x400000，這是 4 mb (MB)。  
**/logfilepath [<path+LogFileName>]**  
指定 Dns.log 檔案的路徑。 預設路徑是 %systemroot%\system32\dns\dns.log。 您可以使用的格式來指定不同的路徑*路徑 + LogFileName*。  
**/logipfilterlist <IPaddress> [,<IPaddress>...]**  
指定偵錯記錄檔中記錄的封包。 項目是 IP 位址的清單。 只有移至，然後從清單中的 IP 位址的封包會記錄。  
**/loglevel [<Eventtype>]**  
決定哪些類型的事件會記錄在 Dns.log 檔案。 每個事件類型被以十六進位數字。 若要讓多個事件記錄檔中的，使用十六進位加入要新增的值，然後輸入 總和。  
**0x0**  
DNS 伺服器不會建立記錄檔。 這是預設項目。  
**0x10**  
記錄查詢。  
**0x10**  
記錄通知。  
**0x20**  
記錄更新。  
**0xFE**  
記錄 nonquery 交易。  
**0x100**  
記錄問題的交易。  
**0x200**  
記錄的解答。  
**0x1000**  
記錄傳送封包。  
**0x2000**  
記錄接收封包。  
**0x4000**  
記錄使用者資料包通訊協定 (UDP) 封包。  
**0x8000**  
記錄檔傳輸控制通訊協定 (TCP) 封包。  
**0xFFFF**  
記錄所有封包。  
**0x10000**  
記錄 active directory 寫入交易。  
**0x20000**  
記錄 active directory 的更新交易。  
**0x1000000**  
記錄檔的完整的封包。  
**0x80000000**  
寫出交易記錄檔。  
**/maxcachesize**  
指定 DNS 伺服器的記憶體快取大小上限 (kb)。  
**/maxcachettl [<seconds>]**  
判斷記錄會儲存在快取秒數 (0x0 0xFFFFFFFF)。 如果**0x0**設定，DNS 伺服器不會快取的記錄。 預設值是**0x15180** （86,400 秒或 1 天）。  
**/maxnegativecachettl [<seconds>]**  
指定的秒數 (0x1 0xFFFFFFFF) 會記錄查詢負數答案的項目會儲存在 DNS 快取。 預設值是**0x384** （900 秒為單位）。  
**/namecheckflag [0|1|2|3]**  
指定檢查 DNS 名稱時使用哪些字元標準。  
**0**  
使用 ANSI 字元符合網際網路工程任務強制 (小組 IETF) 要求建議 (Rfc)。  
**1**  
使用 ANSI 字元不一定符合 IETF Rfc。  
**2**  
使用多位元組的 UCS 轉換格式 8 (utf-8) 個字元。 此為預設設定。  
**3**  
會使用所有的字元。  
**/norecursion [0|1]**  
決定 DNS 伺服器是否執行遞迴性名稱解析。  
**0**  
如果要求在查詢中，DNS 伺服器就會執行遞迴性名稱解析。 此為預設設定。  
**1**  
DNS 伺服器不會執行遞迴性名稱解析。  
**/notcp**  
這個參數已過時，並在目前版本的 Windows Server 中會有任何作用。  
**/recursionretry [<seconds>]**  
決定前一次嘗試連線的遠端伺服器的 DNS 伺服器等待的秒數 (0x1 0xFFFFFFFF) 數目。 預設值是 0x3 （3 秒）。 透過緩慢的廣域網路 (wan) 連結就會發生遞迴時，應增加這個值。  
**/recursiontimeout [<seconds>]**  
決定之前停用嘗試連絡遠端伺服器的 DNS 伺服器等待的秒數 (0x1 0xFFFFFFFF) 數目。 這些設定的範圍從**0x1**透過**0xFFFFFFFF**。 預設值是**0xF** （15 秒）。 當透過慢速 WAN 連結就會發生遞迴時，應增加這個值。  
**/roundrobin [0|1]**  
決定當伺服器具有相同名稱的多個主機記錄時，會傳回主應用程式記錄的順序。  
0  
DNS 伺服器不使用循環配置資源。 相反地，它會傳回第一筆記錄每一個查詢。  
**1**  
DNS 伺服器會在它傳回相符記錄清單的底部到頂端的記錄之間輪替。 此為預設設定。  
**/rpcprotocol [0x0|0x1|0x2|0x4|0xFFFFFFFF]**  
指定 DNS 伺服器的連線時，會使用遠端程序呼叫 (RPC) 通訊協定。  
**0x0**  
停用 DNS RPC。  
**0x1**  
使用 TCP/IP。  
**0x2**  
使用具名管道。  
**0x4**  
使用本機的程序呼叫 (LPC)。  
**0xFFFFFFFF**  
所有的通訊協定。 此為預設設定。  
**/scavenginginterval [<hours>]**  
判斷 DNS 伺服器的 「 清除 」 功能已啟用，並設定時 (0x0 0xFFFFFFFF) 之間清除循環數。 預設值是**0x0**，會停用 DNS 伺服器的清除功能。 設定大於**0x0**啟用清除的伺服器，並設定清除循環之間的小時數。  
**/secureresponses [0|1]**  
決定 DNS 是否篩選會儲存在快取的記錄。  
**0**  
將快取中的所有回應名稱查詢。 此為預設設定。  
**1**  
只有屬於相同的 DNS 樹狀子目錄，以快取的記錄，才會將儲存。  
**/sendport [<port>]**  
指定 DNS 會使用遞迴查詢傳送到其他 DNS 伺服器的連接埠號碼 (0x0 0xFFFFFFFF)。 預設值是**0x0**，這表示隨機選取的連接埠號碼。  
**/serverlevelplugindll[<Dllpath>]**  
指定的自訂外掛程式的路徑。 當*Dllpath*指定有效的 DNS 伺服器外掛程式，DNS 伺服器會呼叫函式在外掛程式中解析名稱查詢的範圍以外的所有在本機裝載區域的完整的路徑名稱。 查詢的名稱是否超出範圍的外掛程式，DNS 伺服器設定，就會執行使用轉送或遞迴時，名稱解析。 如果*Dllpath*未指定，如果先前已設定自訂外掛程式，請使用自訂外掛程式會停止的 DNS 伺服器。  
**/strictfileparsing [0|1]**  
載入區域時遇到錯誤的記錄時，請決定 DNS 伺服器的行為。  
**0**  
DNS 伺服器會繼續載入該區域，即使伺服器發生錯誤的記錄。 錯誤會記錄在 DNS 記錄。 此為預設設定。  
**1**  
DNS 伺服器停止載入該區域，並在 DNS 記錄檔中記錄錯誤。  
**/updateoptions <RecordValue>**  
禁止指定類型的記錄的動態的更新。 如果您想要設為禁止記錄檔中的多個記錄類型，使用十六進位加入来新增的值，然後輸入 總和。  
**0x0**  
不限制任何記錄的類型。  
**0x1**  
排除的授權 (SOA) 資源記錄的開始。  
**0x2**  
排除名稱伺服器 (NS) 資源記錄。  
**0x4**  
排除的名稱伺服器 (NS) 資源記錄的委派。  
**0x8**  
排除伺服器的主機記錄。  
**0x100**  
在安全的動態更新期間排除的授權 (SOA) 資源記錄的開始。  
**0x200**  
在安全的動態更新期間排除根名稱伺服器 (NS) 資源記錄。  
**0x30F**  
在標準的動態更新期間排除名稱伺服器 (NS) 資源記錄，起始授權 (SOA) 資源記錄，以及伺服器主機記錄。 在安全的動態更新期間排除根名稱伺服器 (NS) 資源記錄與授權 (SOA) 資源記錄的開始時間。 可讓委派和伺服器主機更新。  
**0x400**  
在安全的動態更新期間排除委派名稱伺服器 (NS) 資源記錄。  
**0x800**  
在安全的動態更新期間排除伺服器的主機記錄。  
**0x1000000**  
排除委派簽章人 (DS) 記錄。  
**0x80000000**  
停用 DNS 動態更新。  
**/writeauthorityns [0|1]**  
決定當 DNS 伺服器寫入回應的授權區段中的名稱伺服器 (NS) 資源記錄。  
**0**  
寫入轉介只授權單位 部分中的名稱伺服器 (NS) 資源記錄。 此設定符合 Rfc 1034，網域名稱的概念和功能，與 Rfc 2181，釐清 DNS 規格。 此為預設設定。  
**1**  
寫入的所有授權的成功回應的 [授權單位] 區段中的名稱伺服器 (NS) 資源記錄。  
**/xfrconnecttimeout [<seconds>]**  
決定等待秒的數 (0x0 0xFFFFFFFF) 的主要 DNS 伺服器從次要伺服器的傳輸回應。 預設值是**0x1E** （30 秒為單位）。 逾時值到期之後，會結束連接。  
#### <a name="zone-level-syntax"></a>區域層級語法  
```  
dnscmd /config <Parameters>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
修改指定區域的組態。  
#### <a name="parameters"></a>參數  
**<Parameters>**  
指定設定，而區域名稱和，選擇的值。 參數值使用此語法：*ZoneName Parameter* [*Value*]  
下列參數值記載於本節的其餘部分中：  
-   **/aging**  
-   **/allownsrecordsautocreation**  
-   **/allowupdate**  
-   **/forwarderslave**  
-   **/forwardertimeout**  
-   **/norefreshinterval**  
-   **/refreshinterval**  
-   **/securesecondaries**  
**/ 過時 <ZoneName>**  
啟用或停用在特定區域中清除。  
**/allownsrecordsautocreation  <ZoneName> [<Value>]**  
DNS 伺服器的名稱伺服器 (NS) 資源記錄自動建立設定會覆寫。 不會影響先前註冊為此區域的名稱伺服器 (NS) 資源記錄。 因此，您必須移除這些手動如果您不要它們。  
**/allowupdate <ZoneName>**  
判斷指定的區域是否接受動態更新。  
**/forwarderslave <ZoneName>**  
DNS 伺服器會覆寫 **/isslave**設定。  
**/forwardertimeout <ZoneName>**  
決定多少秒 DNS 區域等待使用轉寄站，然後再嘗試另一個轉寄站回應。 這個值會覆寫伺服器層級設定的值。  
**/norefreshinterval <ZoneName>**  
設定時間間隔期間沒有重新整理可以動態更新 DNS 記錄，在指定的區域中的區域。  
**/refreshinterval <ZoneName>**  
設定時間間隔期間重新整理可以動態更新 DNS 記錄，在指定的區域中的區域。  
**/securesecondaries <ZoneName>**  
判斷哪一個次要伺服器可以從這個區域的主要伺服器接收區域更新。  
#### <a name="remarks"></a>備註  
-   必須只能針對區域層級參數指定的區域名稱。  
### <a name="BKMK_4"></a>dnscmd /createbuiltindirectorypartitions  
建立 DNS 應用程式目錄分割。 安裝 DNS 時，服務應用程式目錄分割會建立樹系和網域層級。 若要建立 DNS 應用程式目錄分割已刪除，或永遠不會建立使用此命令。 不含參數，此命令會建立內建的 DNS 目錄分割的網域。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /createbuiltindirectorypartitions [/forest] [/alldomains]   
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**/forest**  
建立樹系的 DNS 目錄分割。  
**/alldomains**  
樹系中建立所有網域的 DNS 資料的分割。  
### <a name="BKMK_5"></a>dnscmd /createdirectorypartition  
建立 DNS 應用程式目錄分割。 安裝 DNS 時，服務應用程式目錄分割會建立樹系和網域層級。 此作業會建立額外的 DNS 應用程式目錄分割。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /createdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<PartitionFQDN>**  
將會建立 DNS 應用程式目錄分割的 FQDN。  
### <a name="BKMK_6"></a>dnscmd /deletedirectorypartition  
移除現有的 DNS 應用程式目錄分割。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /deletedirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<PartitionFQDN>**  
將會移除 DNS 應用程式目錄分割的 FQDN。  
### <a name="BKMK_7"></a>dnscmd /directorypartitioninfo  
列出指定的 DNS 應用程式目錄分割的相關資訊。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /directorypartitioninfo <PartitionFQDN> [/detail]   
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<PartitionFQDN>**  
DNS 應用程式目錄分割的 FQDN。  
**/detail**  
列出應用程式目錄分割的所有資訊。  
### <a name="BKMK_8"></a>dnscmd /enlistdirectorypartition  
將 DNS 伺服器加入至指定的目錄磁碟分割的複本集。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /enlistdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<PartitionFQDN>**  
DNS 應用程式目錄分割的 FQDN。  
### <a name="BKMK_9"></a>dnscmd /enumdirectorypartitions  
列出指定之伺服器的 DNS 應用程式目錄分割。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /enumdirectorypartitions [/custom]   
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**/custom**  
列出僅為使用者建立的目錄磁碟分割。  
### <a name="BKMK_10"></a>dnscmd /enumrecords  
列出 DNS 區域中的指定節點的資源的記錄。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /enumrecords <ZoneName> <NodeName> [/type <RRtype> <Rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<ChildName>] [/continue | /detail]   
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定想要管理的 DNS 伺服器由 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**/enumrecords**  
列出指定區域中的資源記錄。  
**<ZoneName>**  
指定的資源記錄所屬區域的名稱。  
**<NodeName>**  
指定的資源記錄中的節點名稱。  
**/type <RRtype> <Rrdata>**  
指定要列出的資源記錄類型和預期的資料類型：  
**<RRtype>**  
指定要列出的資源記錄類型。  
**<Rrdata>**  
指定是預期的資料錄的資料的類型。  
**/authority**  
包含授權的資料。  
**/glue**  
包含連接的資料。  
**/additional**  
包含所有列出的資源記錄相關的其他資訊。  
**{/node | /child | /startchild <ChildName>}**  
篩選，或將資訊新增至資源的記錄顯示：  
**/node**  
列出只在指定的節點的資源記錄。  
**/child**  
列出只指定的子網域的資源記錄。  
**/startchild <ChildName>**  
開始在指定的子網域的清單。  
**/continue | /detail**  
指定傳回的資料的顯示方式。  
**/continue**  
列出的資源記錄類型與資料。  
**/detail**  
列出的資源記錄的所有資訊。  
#### <a name="sample-usage"></a>使用範例  
`dnscmd /enumrecords test.contoso.com test /additional`  
### <a name="BKMK_11"></a>dnscmd /enumzones  
列出指定的 DNS 伺服器存在的區域。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <PartitionFQDN>]  
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**/primary | /secondary | /forwarder | /stub | /cache | /auto-created**  
篩選以顯示區域的類型：  
**/primary**  
列出所有標準主要區域或 active directory 整合區域的區域。  
**/secondary**  
列出所有標準的次要區域。  
**/forwarder**  
列出未解析的查詢轉寄至另一部 DNS 伺服器的區域。  
**/stub**  
列出所有的虛設常式區域。  
**/cache**  
列出載入至快取的區域。  
**/auto-created**  
列出 DNS 伺服器安裝期間自動建立的區域。  
**/ 正 |/ 反向 |/ds |/file**  
指定類型的顯示區域的其他篩選器：  
**/forward**  
清單正向對應區域。  
**/reverse**  
列出反向對應區域。  
**/ds**  
列出 active directory 整合區域。  
**/file**  
列出檔案所支援的區域。  
**/domaindirectorypartition**  
列出儲存在網域目錄磁碟分割中的區域。  
**/forestdirectorypartition**  
列出樹系 DNS 應用程式目錄分割中儲存的區域。  
**/customdirectorypartition**  
列出所有使用者定義的應用程式目錄分割中儲存的區域。  
**/legacydirectorypartition**  
列出儲存在網域目錄磁碟分割中的所有區域。  
**/directorypartition <PartitionFQDN>**  
列出儲存在指定的目錄磁碟分割中的所有區域。  
#### <a name="remarks"></a>備註  
-   **Enumzones**參數做為區域的清單上的篩選。 如果未不指定任何篩選，則會傳回區域的完整清單。 指定篩選條件時，只有符合該篩選條件的準則區域包含在傳回的清單中的區域。  
#### <a name="example"></a>範例  
請參閱[範例 2:DNS 伺服器上顯示區域的完整清單](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[範例 3:顯示 DNS 伺服器上的 自動建立區域的清單](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_25a"></a>dnscmd /exportsettings  
建立文字檔案，其中列出的 DNS 伺服器的設定詳細資料。 的文字檔案叫做 DnsSettings.txt。 它位於 %systemroot%\system32\dns 目錄中的伺服器。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /exportsettings   
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
#### <a name="remarks"></a>備註  
-   您可以在檔案中使用的資訊可**dnscmd /exportsettings**建立組態問題進行疑難排解，或以確保多部伺服器的設定都相同。  
### <a name="BKMK_12"></a>dnscmd /info  
從指定之伺服器的登錄的 [DNS] 區段的顯示設定：**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters**  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /info [<Setting>]  
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<Setting>**  
任何設定**資訊**命令傳回可以個別指定。 如果未指定的設定，則會傳回一般設定的報告。  
#### <a name="remarks"></a>備註  
-   此命令會顯示在 DNS 伺服器層級的登錄設定。 若要顯示區域層級的登錄設定，請使用[zoneinfo](#BKMK_26)命令。 若要查看可以使用此命令顯示的設定清單，請參閱[config](#BKMK_3)描述。  
#### <a name="example"></a>範例  
請參閱[範例 4:顯示從 DNS 伺服器的 IsSlave 設定](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[範例 5:顯示從 DNS 伺服器的 Recursiontimeout 設定](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_29a"></a>dnscmd /ipvalidate  
測試是否可運作的 DNS 伺服器，或是否在 DNS 伺服器可以做為轉寄站、 根提示伺服器或為特定區域的主要伺服器，識別 IP 位址。  
#### <a name="syntax"></a>語法  
```  
dnscmd [<ServerName>] /ipvalidate <Context> [<ZoneName>] [[<IPaddress>]]  
```  
#### <a name="parameters"></a>參數  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<Context>**  
指定要執行測試的類型。 您可以指定任何下列測試：  
-   **/dnsservers**測試具有您指定位址的電腦運作的 DNS 伺服器。  
-   **/forwarders**測試您指定的位址識別可做為轉寄站的 DNS 伺服器。  
-   **/roothints**測試您指定的位址識別可做為根提示名稱伺服器的 DNS 伺服器。  
-   **/zonemasters**測試您指定的位址識別是主要伺服器的 DNS 伺服器*ZoneName*。  
**<ZoneName>**  
識別區域。 使用此參數與 **/zonemasters**參數。  
**<IPaddress>**  
指定的命令會測試的 IP 位址。  
#### <a name="sample-usage"></a>使用範例  
<pre>dnscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2  
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2</pre>  
### <a name="BKMK_13"></a>dnscmd /nodedelete  
指定的主機，會刪除所有記錄。 # # # 語法```  
dnscmd [<ServerName>] /nodedelete <ZoneName> <NodeName> [/tree] [/f] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定區域的名稱。  
**<NodeName>**  
指定要刪除的節點的主機名稱。  
**/tree**  
刪除所有子記錄。  
**/f**  
執行命令，而不要求確認。 # # # 範例，請參閱[範例 6： 刪除節點的記錄](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_14"></a>dnscmd /recordadd  
將指定的區域中的 DNS 伺服器中的記錄。 # # # 語法```  
dnscmd [<ServerName>] /recordadd <ZoneName> <NodeName> <RRtype> <Rrdata> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定記錄所在的區域。  
**<NodeName>**  
指定特定節點的區域中。  
**<RRtype>**  
指定要加入的記錄類型。  
**<Rrdata>**  
指定預期的資料類型。  
> [!NOTE]  
當您新增一筆記錄時，請確定您使用正確的資料類型和資料格式。 如需資源記錄類型和適當的資料類型的清單，請參閱 <<c0> [ 資源記錄參考](https://technet.microsoft.com/library/cc758321(v=ws.10).aspx)。 # # # 範例使用方式
<pre>dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5  
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com</pre>  
### <a name="BKMK_15"></a>dnscmd /recorddelete  
從指定的區域中刪除的資源記錄。 # # # 語法```  
dnscmd <ServerName>/recorddelete <ZoneName> <NodeName> <RRtype> <Rrdata>[/f] ```  
#### Parameters  
**<ServerName>**  
> 指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定資源記錄所在的區域。  
**<NodeName>**  
指定的主機名稱。  
**<RRtype>**  
指定要刪除的資源記錄類型。  
**<Rrdata>**  
指定預期的資料類型。  
**/f**  
執行命令，而不要求確認:-因為節點可以擁有一個以上的資源記錄，此命令會要求您有非常明確的您想要刪除的資源記錄的類型。 -如果您指定的資料類型，而且您未指定的資源記錄的資料類型，會刪除與該特定資料型別，指定節點的所有記錄。 # # # 範例使用方式 `dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com`  
### <a name="BKMK_16"></a>dnscmd /resetforwarders  
選取或重設的 DNS 伺服器將轉寄 DNS 查詢時它無法在本機解析它們的 IP 位址。 # # # 語法```  
dnscmd [<ServerName>] /resetforwarders [<IPaddress> [，<IPaddress>]...][/ 逾時<timeOut>] [/ 從屬 | / /noslave] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<IPaddress>**  
列出的 IP 位址的 DNS 伺服器將轉寄無法解析的查詢。  
**/timeout <timeOut>**  
設定 DNS 伺服器轉寄站的回應所等待的秒數。 根據預設，這個值會是 5 秒。  
**/slave|/noslave**  
決定 DNS 伺服器是否會轉寄站來解析查詢時執行它自己的反覆查詢：  
**/slave**  
防止 DNS 伺服器轉寄站來解析查詢時執行它自己的反覆查詢。  
**/noslave**  
可讓 DNS 伺服器轉寄站來解析查詢時執行它自己的反覆查詢。 此為預設設定。 # # # 備註-根據預設，DNS 伺服器會執行反覆查詢時它無法解析查詢。 -使用設定的 IP 位址**resetforwarders**命令就會讓 DNS 伺服器，以執行遞迴查詢，以在指定的 IP 位址的 DNS 伺服器。 轉寄站無法解決查詢，如果 DNS 伺服器可以接著執行自己的反覆查詢。 -如果**從屬**參數時，DNS 伺服器不會執行它自己的反覆查詢。 這表示 DNS 伺服器將轉寄無法解析的查詢，才能在清單中，DNS 伺服器，而且它不會嘗試反覆查詢，如果轉寄站無法解決它們。 它會更有效率的 DNS 伺服器做為轉寄站設定一個 IP 位址。 您可以使用**resetforwarders**命令轉送給他們無法解析的查詢，一部 DNS 伺服器具有外部連線到網路中的內部伺服器。 -列出兩次的轉寄站 IP 位址會導致 DNS 伺服器嘗試兩次轉送到該伺服器。 # # # 範例使用方式
<pre>dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave  
dnscmd dnssvr1.contoso.com /resetforwarders /noslave</pre>  
### <a name="BKMK_17"></a>dnscmd /resetlistenaddresses  
指定的 IP 位址接聽 DNS 用戶端要求的伺服器上。 # # # 語法```  
dnscmd [<ServerName>] /resetlistenaddresses [<listenaddress>] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<listenaddress>**  
指定的 IP 位址接聽 DNS 用戶端要求的 DNS 伺服器上。 如果未不指定任何接聽位址在伺服器上的所有 IP 位址都接聽用戶端要求。 # # # 備註-根據預設，DNS 伺服器上的所有 IP 位址都接聽的用戶端 DNS 要求。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1`  
### <a name="BKMK_18"></a>dnscmd /startscavenging  
會告訴 DNS 伺服器嘗試在指定的 DNS 伺服器的過時資源記錄的立即搜尋。 # # # 語法```  
dnscmd [<ServerName>] /startscavenging ```  
#### Parameter  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。 # # # 備註-成功完成此命令會立即啟動 清理。 -雖然命令以啟動 清理似乎完全成功，清除不會啟動除非符合下列先決條件:-清除已啟用的伺服器和區域。 -區域會啟動。 -資源記錄都有時間戳記。 -如需有關如何啟用清除伺服器的資訊，請參閱**scavenginginterval**下方中的伺服器層級語法的參數[組態](#BKMK_3)一節。 -如需有關如何啟用清除區域的資訊，請參閱**過時**下方區域層級語法中的參數[config](#BKMK_3)一節。 -如需如何啟動已暫停的區域資訊，請參閱[zoneresume](#BKMK_35)一節。 -如需有關如何檢查時間戳記的資源記錄的資訊，請參閱[ageallrecords](#BKMK_1)一節。 -如果在 清理將失敗，不會出現警告訊息。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /startscavenging`  
### <a name="BKMK_19"></a>dnscmd /statistics  
顯示或清除指定的 DNS 伺服器的資料。 # # # 語法```  
dnscmd [<ServerName>] /statistics [<StatID>] [/ 清除] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<StatID>**  
指定的統計資料或要顯示的統計資料的組合。 識別碼用來識別統計資料。 如果指定無 statistic ID 編號，則會顯示所有統計資料。  
以下是您可以指定的數字和相對應顯示的統計資料的清單：  
**00000001**  
time  
**00000002**  
查詢  
**00000004**  
query2  
**00000008**  
Recurse  
**00000010**  
主檔  
**00000020**  
次要  
**00000040**  
WINS  
**00000100**  
Update  
**00000200**  
SkwanSec  
**00000400**  
ds  
**00010000**  
記憶體  
**00100000**  
PacketMem  
**00040000**  
Dbase  
**00080000**  
記錄  
**00200000**  
NbstatMem  
**/clear**  
指定的統計資料計數器重設為零。 # # # 備註-**統計資料**命令會顯示開始的計數器上的 DNS 伺服器啟動或繼續時。 # # # 範例，請參閱[範例 7:顯示時間統計資料的 DNS 伺服器](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[範例 8:顯示 NbstatMem 統計資料的 DNS 伺服器](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_20"></a>dnscmd /unenlistdirectorypartition  
從指定的目錄分割的複本集移除 DNS 伺服器。 # # # 語法```  
dnscmd [<ServerName>] /unenlistdirectorypartition <PartitionFQDN> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<PartitionFQDN>**  
將會移除 DNS 應用程式目錄分割的 FQDN。  
### <a name="BKMK_21"></a>dnscmd /writebackfiles  
檢查 DNS 伺服器記憶體的變更，並將事件寫入永續性儲存體。 # # # 語法```  
dnscmd [<ServerName>] /writebackfiles [<ZoneName>] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要更新的區域名稱。 # # # 備註- **writebackfiles**命令會更新所有已變更區域或指定的區域。 記憶體中尚未寫入至永續性儲存體的變更時，區域已經變更。 這是檢查所有區域的伺服器層級作業。 您可以指定一個區域，這項作業中，或者您可以使用[zonewriteback](#BKMK_37)作業。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /writebackfiles`  
### <a name="BKMK_22"></a>dnscmd /zoneadd  
新增一個區域的 DNS 伺服器。 # # # 語法```  
dnscmd [<ServerName>] /zoneadd <ZoneName> <Zonetype> [/dp <FQDN>|{/ 網域 | / 企業版 | / 舊版}] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定區域的名稱。  
**<Zonetype>**  
指定要建立的區域類型。 每個區域類型具有不同的必要的參數：  
**/dsprimary**  
建立 active directory 整合的區域。  
**/primary /file <FileName>**  
建立標準主要區域，並指定儲存區域資訊的檔案名稱。  
**/secondary <MasterIPaddress> [<MasterIPaddress>...]**  
建立標準的次要區域。  
**/stub <MasterIPaddress> [<MasterIPaddress>...] /file <FileName>**  
建立檔案備份的虛設常式區域。  
**/dsstub <MasterIPaddress> [<MasterIPaddress>...]**  
建立 active directory 整合式的虛設常式區域。  
**/轉寄站<MasterIPaddress>[<MasterIPaddress>].../ 檔案 <FileName>**  
指定建立的區域轉送到其他 DNS 伺服器無法解析的查詢。  
**/dsforwarder**  
指定建立的 active directory 整合的區域轉送到其他 DNS 伺服器無法解析的查詢。  
**/dp <FQDN> {/ 網域 | /enterprise | / 舊版}**  
指定用來儲存區域的目錄磁碟分割。  
**<FQDN>**  
指定的目錄磁碟分割的 FQDN。  
**/domain**  
會將區域儲存在網域目錄磁碟分割上。  
**/enterprise**  
會將區域儲存企業目錄磁碟分割上。  
**/legacy**  
會將區域儲存在舊版的目錄磁碟分割上。 # # # 備註-指定的區域類型 **/forwarder**或是 **/dsforwarder**建立執行條件式轉送的區域。 # # # 範例使用方式
<pre>dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary  
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_23"></a>dnscmd /zonechangedirectorypartition  
變更指定的區域所在的目錄磁碟分割。 # # # 語法```  
dnscmd [<ServerName>] /zonechangedirectorypartition <ZoneName>] {[<NewPartitionName>] |[<Zonetype>] } ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
目前區域所在的目錄磁碟分割的 FQDN。  
**<NewPartitionName>**  
區域將會移至目錄分割的 FQDN。  
**<Zonetype>**  
指定區域將會移至的目錄磁碟分割的類型。  
**/domain**  
將區域移至內建的網域目錄磁碟分割中。  
**/forest**  
將區域移至內建的樹系目錄分割中。  
**/legacy**  
將區域移至針對預先 active directory 網域控制站所建立的目錄分割。 這些目錄分割不是必要的原生模式的。  
### <a name="BKMK_24"></a>dnscmd /zonedelete  
刪除指定的區域。 # # # 語法```  
dnscmd [<ServerName>] /zonedelete <ZoneName> [/dsdel] [/f] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要刪除區域的名稱。  
**/dsdel**  
從 AD DS 中刪除該區域。  
**/f**  
執行命令，而不要求確認。 # # # 範例，請參閱[範例 9： 刪除區域的 DNS 伺服器從](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_25"></a>dnscmd /zoneexport  
建立文字檔案，其中列出指定區域的資源記錄。 # # # 語法 'dnscmd [<ServerName>] /zoneexport <ZoneName> <ZoneExportFile>' # # # 參數 **<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定區域的名稱。  
**<ZoneExportFile>**  
指定要建立的檔案名稱。 # # # 備註- **zoneexport**作業會建立 active directory 整合的區域以進行疑難排解的資源記錄的檔案。 根據預設，此命令會建立檔案位於預設為 %systemroot%/System32/Dns 目錄的 [DNS] 目錄中。 # # # 範例，請參閱[範例 10:區域資源記錄清單匯出至檔案](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_26"></a>dnscmd /zoneinfo  
會顯示一節的指定區域的登錄設定: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>** # # # 語法```  
dnscmd [<ServerName>] /zoneinfo <ZoneName> [<Setting>] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定區域的名稱。  
**<Setting>**  
您可以個別指定任何設定**zoneinfo**命令所傳回。 如果您未指定的設定，則會傳回所有設定。 # # # 備註- **zoneinfo**命令會顯示位於 DNS 區域在層級的登錄設定 **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>**。 -若要顯示伺服器層級的登錄設定，請使用[資訊](#BKMK_12)命令。 -若要查看您可以使用此命令顯示的設定清單，請參閱[config](#BKMK_3)命令。 # # # 範例，請參閱[範例 11:顯示 RefreshInterval 設定從登錄](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[範例 12:顯示過時設定從登錄](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_27"></a>dnscmd /zonepause  
暫停指定的區域，則會忽略查詢要求。 # # # 語法```  
dnscmd [<ServerName>] /zonepause <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要暫停區域的名稱。 # # # 註解-若要繼續區域並將它提供已暫停之後，請使用[zoneresume](#BKMK_35)命令。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zonepause test.contoso.com`  
### <a name="BKMK_28"></a>dnscmd /zoneprint  
列出區域中的記錄。 # # # 語法```  
dnscmd [<ServerName>] /zoneprint <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
識別要列出區域。  
### <a name="BKMK_30"></a>dnscmd /zonerefresh  
強制更新從主要區域的次要 DNS 區域。 # # # 語法```  
dnscmd <ServerName>/zonerefresh <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要重新整理區域的名稱。 # # # 備註- **zonerefresh**命令會強制進行檢查的主要伺服器的開頭的授權 (SOA) 資源記錄中的版本號碼。 如果主要伺服器上的版本號碼大於次要伺服器的版本號碼，進行區域轉送就會起始更新次要伺服器。 如果相同的版本號碼，不會發生區域轉送。 -強制的檢查就會發生預設每隔 15 分鐘。 若要變更預設值，請使用**dnscmd config refreshinterval**命令。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com`  
### <a name="BKMK_31"></a>dnscmd /zonereload  
複本區域從其來源的資訊。 # # # 語法```  
dnscmd <ServerName>/zonereload <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定重新載入區域的名稱。 # # # 備註-active directory 整合區域時，它會重新載入來自 AD DS。 -如果是標準檔案備份區域的區域，它會重新載入檔案。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zonereload test.contoso.com`  
### <a name="BKMK_32"></a>dnscmd /zoneresetmasters  
重設提供次要區域的區域傳輸資訊的主要伺服器的 IP 位址。 # # # 語法```  
dnscmd <ServerName>/zoneresetmasters <ZoneName> [/ 本機] [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定重新載入區域的名稱。  
**/local**  
設定本機的主要清單。 這個參數用於 active directory 整合區域。  
**<IPaddress>**  
次要區域的主要伺服器的 IP 位址。 # # # 備註-這個值最初設定時建立次要區域。 使用**zoneresetmasters**次要伺服器上的命令。 如果設定主要 DNS 伺服器上，這個值會有任何作用。 # # # 範例使用方式
<pre>dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1  
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local</pre>  
### <a name="BKMK_33"></a>dnscmd /zoneresetscavengeservers  
變更可以清除指定的區域伺服器的 IP 位址。 #### Syntax ```  
dnscmd [<ServerName>] /zoneresetscavengeservers <ZoneName> [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
識別要清除的區域。  
**<IPaddress>**  
列出可以執行 清除伺服器的 IP 位址。 如果省略這個參數，則裝載此區域的所有伺服器可以都清除它。 # # # 註解-根據預設，主控區域的所有伺服器都可以都清除該區域。 -如果區域裝載在多部 DNS 伺服器，您可以使用此命令，以減少清除的區域會啟動的次數。 -清除必須能夠在 DNS 伺服器與此命令會受到影響的區域。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2`  
### <a name="BKMK_34"></a>dnscmd /zoneresetsecondaries  
指定要為主要伺服器時的回應要求的區域轉送的次要伺服器的 IP 位址的清單。 # # # 語法```  
dnscmd [<ServerName>] /zoneresetsecondaries <ZoneName> {/ noxfr | / 不安全 | /securens | /securelist <SecurityIPaddresses>} {/ nonotify | / 通知 | /notifylist <NotifyIPaddresses>} ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略參數時，會使用本機伺服器。  
**<ZoneName>**  
指定將會有它的次要伺服器區域的名稱重設。  
**/noxfr | /nonsecure | /securens | /securelist <SecurityIPaddresses>**  
指定全部或僅部分要求更新的次要伺服器取得更新。  
**/noxfr**  
指定不允許任何區域轉送。  
**/nonsecure**  
指定被授與所有的區域轉送要求。  
**/securens**  
指定區域的名稱伺服器 (NS) 資源記錄中所列的伺服器，會授與傳輸。  
**/securelist**  
指定的區域傳輸僅會授與的伺服器清單。 這個參數必須緊接著的 IP 位址或主要伺服器使用的位址。  
**<SecurityIPaddresses>**  
列出從主要伺服器接收區域轉送的 IP 位址。 會使用這個參數只能搭配 **/securelist**參數。  
**/nonotify |通知 / |/notifylist <NotifyIPaddresses>**  
指定的變更通知只會傳送到特定的次要伺服器：  
**/nonotify**  
指定沒有變更通知會傳送到次要伺服器。  
**/notify**  
指定變更通知會傳送至所有的次要伺服器。  
**/notifylist**  
指定變更通知會傳送至伺服器的清單。 此命令必須緊接著的 IP 位址或主要伺服器使用的位址。  
**<NotifyIPaddresses>**  
指定的 IP 位址或位址的次要伺服器或伺服器通知會傳送哪些變更。 這份清單僅適用於 **/notifylist**參數。 # # # 備註-使用**zoneresetsecondaries**命令在主要伺服器，以指定如何回應區域轉送要求從次要伺服器上。 # # # 範例使用方式
<pre>dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify  
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2</pre>  
### <a name="BKMK_29"></a>dnscmd /zoneresettype  
變更區域類型。 # # # 語法```  
dnscmd [<ServerName>] /zoneresettype <ZoneName> <Zonetype> [/ overwrite_mem | /overwrite_ds] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示本機電腦的語法、 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
識別型別將會變更所在的區域。  
**<Zonetype>**  
指定要建立的區域類型。 每個類型具有不同的必要的參數：  
**/dsprimary**  
建立 active directory 整合的區域。  
**/primary /file <FileName>**  
建立標準主要區域。  
**/secondary <MasterIPaddress> [,<MasterIPaddress>...]**  
建立標準的次要區域。  
**/stub <MasterIPaddress>[,<MasterIPaddress>...] /file <FileName>**  
建立檔案備份的虛設常式區域。  
**/dsstub <MasterIPaddress>[,<MasterIPaddress>...]**  
建立 active directory 整合式的虛設常式區域。  
**/轉寄站<MasterIPaddress[,<MasterIPaddress>].../ 檔案<FileName>**  
指定建立的區域轉送到其他 DNS 伺服器無法解析的查詢。  
**/dsforwarder**  
指定建立的 active directory 整合的區域轉送到其他 DNS 伺服器無法解析的查詢。  
**/overwrite_mem | /overwrite_ds**  
指定覆寫現有資料的方式：  
**/overwrite_mem**  
覆寫 DNS 資料，從 AD DS 中的資料。  
**/overwrite_ds**  
覆寫 AD DS 中的現有資料。 # # # 備註-設定區域類型作為 **/dsforwarder**建立執行條件式轉送的區域。 # # # 範例使用方式
<pre>dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns  
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_35"></a>dnscmd /zoneresume  
啟動先前暫停的指定的區域。 # # # 語法```  
dnscmd <ServerName>/zoneresume <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要繼續的區域名稱。 # # # 備註-您可以使用這項作業，若要反轉[zonepause](#BKMK_27)作業。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com`  
### <a name="BKMK_36"></a>dnscmd /zoneupdatefromds  
更新來自 AD DS 指定 active directory 整合的區域。 # # # 語法```  
dnscmd <ServerName>/zoneupdatefromds <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要更新的區域名稱。 # # # 備註-active directory 整合的區域執行這項更新預設每隔五分鐘。 若要變更此參數，請使用**dnscmd config dspollinginterval**命令。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zoneupdatefromds`  
### <a name="BKMK_37"></a>dnscmd /zonewriteback  
檢查 DNS 伺服器記憶體的變更與指定的區域，並將事件寫入永續性儲存體。 # # # 語法```  
dnscmd <ServerName>/zonewriteback <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 伺服器來管理，以表示 IP 位址、 FQDN 或主機名稱。 如果省略這個參數，則會使用本機伺服器。  
**<ZoneName>**  
指定要更新的區域名稱。 # # # 註解-這是在區域層級作業。 您可以更新的 DNS 伺服器上的所有區域[writebackfiles](#BKMK_21)作業。 # # # 範例使用方式 `dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com`  
