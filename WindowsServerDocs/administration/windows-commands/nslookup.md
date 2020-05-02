---
title: nslookup
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35f790a3a537959501afe7c3173317f22b934ec6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723494"
---
# <a name="nslookup"></a>nslookup

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示您可以用來診斷網域名稱系統（DNS）基礎結構的資訊。 使用此工具之前，您應該先熟悉 DNS 的運作方式。 只有當您已安裝 TCP/IP 通訊協定時，才能使用 nslookup 命令列工具。
## <a name="syntax"></a>語法

```
nslookup [<-SubCommand ...>] [{<computerTofind> | -<Server>}]
nslookup /exit
nslookup /finger [<UserName>] [{[>] <FileName>|[>>] <FileName>}]
nslookup /{help | ?}
nslookup /ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
nslookup /lserver <DNSDomain> 
nslookup /root 
nslookup /server <DNSDomain>
nslookup /set <KeyWord>[=<Value>]
nslookup /set all 
nslookup /set class=<Class>
nslookup /set [no]d2
nslookup /set [no]debug
nslookup /set [no]defname
nslookup /set domain=<DomainName>
nslookup /set [no]ignore
nslookup /set port=<Port>
nslookup /set querytype=<ResourceRecordtype>
nslookup /set [no]recurse
nslookup /set retry=<Number>
nslookup /set root=<RootServer>
nslookup /set [no]search
nslookup /set srchlist=<DomainName>[/...]
nslookup /set timeout=<Number>
nslookup /set type=<ResourceRecordtype>
nslookup /set [no]vc
nslookup /view <FileName>
```

### <a name="parameters"></a>參數

|                       參數                       |                                                                                                         描述                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [nslookup exit Command](nslookup-exit-command.md)   |                                                                                                     結束**nslookup**。                                                                                                     |
| [nslookup finger Command](nslookup-finger-command.md) |                                                                                  連接目前電腦上的 finger 伺服器。                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    顯示**nslookup**子命令的簡短摘要。                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             列出 DNS 網域的資訊。                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   將預設伺服器變更為指定的 DNS 網域。                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     將伺服器的預設伺服器變更為 DNS 功能變數名稱空間的根目錄。                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   將預設伺服器變更為指定的 DNS 網域。                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              變更會影響查閱功能的設定。                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  列印配置設定的目前值。                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     變更查詢類別。 類別會指定資訊的通訊協定群組。                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     開啟或關閉徹底的偵錯模式。 每個封包的所有欄位都會列印出來。                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               開啟或關閉偵錯模式。                                                                                               |
|                 nslookup/set defname                 |                                            將預設的 DNS 功能變數名稱附加至單一元件查詢要求。 單一元件是不包含句號的元件。                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 將預設的 DNS 功能變數名稱變更為指定的名稱。                                                                                  |
|                 nslookup/set 忽略                  |                                                                                              忽略封包截斷錯誤。                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          將預設的 TCP/UDP DNS 名稱伺服器埠變更為指定的值。                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       變更查詢的資源記錄類型。                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    告訴 DNS 名稱伺服器在沒有該資訊的情況之下，查詢其他伺服器。                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 設定重試次數。                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    變更用於查詢的根功能變數名稱稱。                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | 將 DNS 網域搜尋清單中的 DNS 功能變數名稱附加至要求，直到收到回應為止。 這適用于集合和查閱要求至少包含一個期間，但結尾不是尾端句點的情況。 |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    變更預設 DNS 功能變數名稱和搜尋清單。                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           變更等待回復要求的初始秒數。                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       變更查詢的資源記錄類型。                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     指定在將要求傳送至伺服器時，使用或不使用虛擬電路。                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          排序並列出前一個**ls**子命令或命令的輸出。                                                                          |

## <a name="remarks"></a>備註
- 如果*computerTofind*是 IP 位址，而查詢是針對 A 或 PTR 資源記錄類型，則會傳回電腦的名稱。 如果*computerTofind*是名稱，而且沒有尾端句點，則預設的 DNS 功能變數名稱會附加至名稱。 此行為取決於下列**set**子命令的狀態： **domain**、 **srchlist**、 **defname**和**search**。
- 如果您輸入連字號（-）而不是*computerTofind*，則命令提示字元會變更為**nslookup**互動模式。
- 命令列長度必須小於256個字元。
- **nslookup**有兩種模式：互動式和非互動式。
  如果您只需要查閱單一資料片段，請使用非互動式模式。 針對第一個參數，輸入您要查閱之電腦的名稱或 IP 位址。 針對第二個參數，輸入 DNS 名稱伺服器的名稱或 IP 位址。 如果您省略第二個引數， **nslookup**會使用預設的 DNS 名稱伺服器。
  如果您需要查閱一個以上的資料，則可以使用互動模式。 輸入第一個參數的連字號（-），以及第二個參數的 DNS 名稱伺服器的名稱或 IP 位址。 或者，省略參數和**nslookup**都會使用預設的 DNS 名稱伺服器。 以下是在互動模式中工作的一些秘訣：
  -   若要在任何時間中斷互動式命令，請按 CTRL + B。
  -   若要結束，請輸入**exit**。
  -   若要將內建命令視為電腦名稱稱，請在其前面加上 escape 字元（\\）。
  -   無法辨識的命令會被視為電腦名稱稱。
- 如果查閱要求失敗， **nslookup**會列印錯誤訊息。 下表列出可能的錯誤訊息。
  |**錯誤訊息**|**說明**|
  |-----------|----------|
  |`timed out`|伺服器在一段時間內未回應要求，而且有一定的重試次數。 您可以使用**set timeout**子命令來設定超時時間。 您可以使用**設定重試**子命令來設定重試次數。|
  |`No response from server`|伺服器電腦上沒有正在執行的 DNS 名稱伺服器。|
  |`No records`|DNS 名稱伺服器沒有電腦目前查詢類型的資源記錄，雖然電腦名稱稱是有效的。 查詢類型是使用**set querytype**命令來指定。|
  |`Nonexistent domain`|電腦或 DNS 功能變數名稱不存在。|
  |`Connection refused`<p>-或-<p>`Network is unreachable`|無法連接到 DNS 名稱伺服器或 finger 伺服器。 此錯誤通常發生于**ls**和**finger**要求。|
  |`Server failure`|DNS 名稱伺服器在其資料庫中發現內部不一致，因此無法傳回有效的答案。|
  |`Refused`|DNS 名稱伺服器已拒絕服務要求。|
  |`format error`|DNS 名稱伺服器發現要求封包的格式不正確。 這可能表示**nslookup**中發生錯誤。|
- 如需**nslookup**命令和 DNS 的詳細資訊，請參閱下列資源：
  - 先生，T.，Davies，J. 2000。 *Microsoft Windows 2000 Tcp/ip 通訊協定和服務技術參考*。 華盛頓州雷德蒙德： Microsoft 請按。
  - Albitz、P.、Loukides、M. 和 c. Liu。 2001。 *DNS 和 BIND，第四版*。 Sebastopol，加州： O'Reilly 和關聯，Inc。
  - Larson、M. 和 C. Liu。 2001。 *Windows 2000 上的 DNS*。 Sebastopol，加州： O'Reilly 和關聯，Inc。
    #### <a name="examples"></a>範例
    每個命令列選項都包含連字號（-），後面緊接著命令名稱，在某些情況下是等號（=）和值。 例如，若要將預設的查詢類型變更為 [主機（電腦）] 資訊，並將初始超時設定為10秒，請輸入： **nslookup-querytype = hinfo-timeout = 10**
    ## <a name="see-also"></a>另請參閱
    - [命令列語法關鍵](command-line-syntax-key.md)
