---
title: nslookup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3c20855befbe71413fa8fdb44925b8b634c0c11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841649"
---
# <a name="nslookup"></a>nslookup

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以用來診斷網域名稱系統 (DNS) 基礎結構的顯示資訊。 使用此工具之前，您應該熟悉 DNS 的運作方式。 只有當您已安裝 TCP/IP 通訊協定時，才使用 nslookup 命令列工具。
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
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[nslookup 結束命令](nslookup-exit-command.md)|結束**nslookup**。|
|[nslookup 手指命令](nslookup-finger-command.md)|與目前的電腦上的手指伺服器連線。|
|[nslookup help](nslookup-help.md)|顯示的簡短摘要**nslookup**子命令。|
|[nslookup ls](nslookup-ls.md)|列出 DNS 網域的資訊。|
|[nslookup lserver](nslookup-lserver.md)|變更指定的 DNS 網域的預設伺服器。|
|[nslookup root](nslookup-root.md)|變更 DNS 網域命名空間的根伺服器的預設伺服器。|
|[nslookup 伺服器](nslookup-server.md)|變更指定的 DNS 網域的預設伺服器。|
|[nslookup 組](nslookup-set.md)|變更組態設定會影響如何查閱函式。|
|[設定所有的 nslookup](nslookup-set-all.md)|列印目前的組態設定的值。|
|[nslookup set 類別](nslookup-set-class.md)|變更查詢類別。 此類別會指定通訊協定群組的資訊。|
|[nslookup set d2](nslookup-set-d2.md)|開啟或關閉，請開啟完整的偵錯模式。 列印每個封包的所有欄位。|
|[nslookup 設定偵錯](nslookup-set-debug.md)|開啟或關閉，請關閉偵錯模式。|
|nslookup /set defname|將預設的 DNS 網域名稱附加到單一元件的查閱要求。 單一元件是一種元件，不含句點。|
|[nslookup 設定網域](nslookup-set-domain.md)|變更預設的 DNS 網域名稱指定的名稱。|
|nslookup /set ignore|會忽略封包截斷錯誤。|
|[nslookup 設定連接埠](nslookup-set-port.md)|指定的值變更預設的 TCP/UDP DNS 名稱伺服器連接埠。|
|[nslookup set querytype](nslookup-set-querytype.md)|變更查詢的資源記錄類型。|
|[nslookup 設定 recurse](nslookup-set-recurse.md)|會告訴 DNS 名稱伺服器查詢其他伺服器，如果它沒有資訊。|
|[nslookup 設定重試](nslookup-set-retry.md)|設定重試次數。|
|[nslookup 設定根目錄](nslookup-set-root.md)|變更查詢所用的根伺服器的名稱。|
|[nslookup 設定搜尋](nslookup-set-search.md)|將 DNS 網域搜尋清單中的 DNS 網域名稱附加至要求，直到收到回應為止。 這適用於包含至少一個句號，設定及搜尋要求時，但不是以句點結尾。|
|[nslookup 設定 srchlist](nslookup-set-srchlist.md)|變更預設 DNS 網域名稱和搜尋清單。|
|[nslookup 設定逾時](nslookup-set-timeout.md)|變更初始的等候回覆要求的秒數。|
|[nslookup 設定類型](nslookup-set-type.md)|變更查詢的資源記錄類型。|
|[nslookup set vc](nslookup-set-vc.md)|指定要使用或不使用虛擬電路時傳送要求到伺服器。|
|[nslookup 檢視](nslookup-view.md)|排序和列出先前的輸出**ls**子命令。|
## <a name="remarks"></a>備註
-   如果*computerTofind*是 IP 位址和該查詢是針對 A 或 PTR 資源記錄傳回型別，電腦的名稱。 如果*computerTofind*是名稱，而沒有尾端期間，DNS 網域名稱會附加至名稱的預設值。 這個行為取決於下列的狀態**設定**子命令：**網域**， **srchlist**， **defname**，和**搜尋**。
-   如果您輸入連字號 （-），而不是*computerTofind*，命令提示字元會變成**nslookup**互動模式。
-   命令列的長度必須小於 256 個字元。
-   **nslookup**有兩種模式： 互動式和非互動式。
    如果您需要查閱單一部分的資料，請使用非互動模式。 第一個參數中，輸入名稱或您想要查閱之電腦的 IP 位址。 第二個引數，請輸入名稱或 IP 位址的 DNS 名稱伺服器。 如果您省略第二個引數**nslookup**會使用預設的 DNS 名稱伺服器。
    如果您需要查詢多個資料片段，您可以使用互動模式。 類型的第一個參數的名稱或 IP 位址的第二個參數的 DNS 名稱伺服器的連字號 （-）。 或者，您也可以省略這兩個參數並**nslookup**會使用預設的 DNS 名稱伺服器。 以下是有關在互動模式中的一些秘訣：
    -   若要隨時中斷互動式命令，請按 CTRL + B。
    -   若要結束，請輸入**結束**。
    -   若要將內建命令做為電腦名稱，前面加上逸出字元 (\\)。
    -   無法辨識的命令會解譯為電腦名稱。
-   如果查閱要求失敗， **nslookup**會印出錯誤訊息。 下表列出可能的錯誤訊息。
    |**錯誤訊息**|**描述**|
    |-----------|----------|
    |`timed out`|伺服器未在一段時間和重試次數之後要求回應。 您可以設定逾時期限**設定逾時**子命令。 您可以設定使用的重試次數**集重試**子命令。|
    |`No response from server`|沒有 DNS 名稱伺服器的伺服器電腦上執行。|
    |`No records`|雖然電腦名稱有效，則 DNS 名稱伺服器並沒有資源記錄的電腦，目前的查詢類型。 使用指定的查詢類型**設定 querytype**命令。|
    |`Nonexistent domain`|DNS 網域名稱的電腦不存在。|
    |`Connection refused`<br /><br />-或-<br /><br />`Network is unreachable`|無法建立與 DNS 名稱伺服器或手指伺服器連接。 通常會發生這個錯誤**ls**並**手指**要求。|
    |`Server failure`|DNS 名稱伺服器在其資料庫中發現內部不一致，而無法傳回有效的回應。|
    |`Refused`|DNS 名稱伺服器拒絕服務此要求。|
    |`format error`|DNS 名稱伺服器找到要求封包不是正確的格式。 它可能表示發生錯誤**nslookup**。|
-   如需詳細資訊**nslookup**命令和 DNS，請參閱下列資源：
    -   Lee, T., Davies, J.2000. *Microsoft Windows 2000 TCP/IP 通訊協定和服務的技術參考*。 Redmond，Washington:Microsoft Press.
    -   Albitz，P.，Loukides，m 和 c。 liu 在其中。 2001. *DNS 和繫結，第四版*。 Sebastopol 加州：O'Reilly 和 associates，Inc.
    -   Larson，m 和 c。 liu 在其中。 2001. *在 Windows 2000 上的 DNS*。 Sebastopol 加州：O'Reilly 和 associates，Inc.
#### <a name="examples"></a>範例
每個命令列選項是由連字號 （-），命令名稱，然後在某些情況下，等號 （=） 和值，後面緊接所組成。 例如，若要變更主機 （電腦） 資訊和 10 秒的初始逾時的預設查詢類型，請輸入： **nslookup-querytype = hinfo 逾時 = 10**
## <a name="see-also"></a>另請參閱
[命令列語法關鍵](command-line-syntax-key.md)
