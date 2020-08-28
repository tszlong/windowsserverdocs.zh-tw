---
title: nslookup
description: Nslookup 命令的參考文章，顯示您可用來診斷網域名稱系統 (DNS) 基礎結構的資訊。
ms.topic: reference
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c06121384ec18a879eaccbb34c926ce68cff9f49
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032776"
---
# <a name="nslookup"></a>nslookup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示您可以用來診斷網域名稱系統 (DNS) 基礎結構的資訊。 使用此工具之前，您應該先熟悉 DNS 的運作方式。 只有當您已安裝 TCP/IP 通訊協定時，才能使用 nslookup 命令列工具。

Nslookup 命令列工具有兩種模式：互動式和非互動式。

如果您只需要查閱單一資料片段，我們建議使用非互動式模式。 針對第一個參數，輸入您想要查閱之電腦的名稱或 IP 位址。 針對第二個參數，輸入 DNS 名稱伺服器的名稱或 IP 位址。 如果您省略第二個引數， **nslookup** 會使用預設的 DNS 名稱伺服器。

如果您需要查閱一個以上的資料，您可以使用互動模式。 輸入第一個參數的連字號 (-) ，以及第二個參數的 DNS 名稱伺服器的名稱或 IP 位址。 如果您省略這兩個參數，此工具會使用預設的 DNS 名稱伺服器。 使用互動模式時，您可以：

- 您可以按下 CTRL + B，隨時中斷互動式命令。

- 退出，輸入 **exit**。

- 將內建命令視為電腦名稱稱，方法是在其前面加上 escape 字元 (\) 。 無法辨識的命令會被解釋為電腦名稱稱。

## <a name="syntax"></a>語法

```
nslookup [exit | finger | help | ls | lserver | root | server | set | view] [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [nslookup exit](nslookup-exit-command.md) | 離開 nslookup 命令列工具。 |
| [nslookup 手指](nslookup-finger-command.md) | 連接目前電腦上的手指伺服器。 |
| [nslookup help](nslookup-help.md) | 顯示子命令的簡短摘要。 |
| [nslookup ls](nslookup-ls.md) | 列出 DNS 網域的資訊。 |
| [nslookup lserver](nslookup-lserver.md) | 將預設伺服器變更為指定的 DNS 網域。 |
| [nslookup root](nslookup-root.md) | 將伺服器的預設伺服器變更為 DNS 功能變數名稱空間的根目錄。 |
| [nslookup server](nslookup-server.md) | 將預設伺服器變更為指定的 DNS 網域。 |
| [nslookup set](nslookup-set.md) | 變更影響查閱功能的設定。 |
| [nslookup set all](nslookup-set-all.md) | 列印 configuration 設定的目前值。 |
| [nslookup set class](nslookup-set-class.md) | 變更查詢類別。 類別會指定資訊的通訊協定群組。 |
| [nslookup set d2](nslookup-set-d2.md) | 開啟或關閉徹底的偵錯模式。 列印每個封包的所有欄位。 |
| [nslookup set debug](nslookup-set-debug.md) | 開啟或關閉偵錯模式。 |
| [nslookup set domain](nslookup-set-domain.md) | 將預設的 DNS 功能變數名稱變更為指定的名稱。 |
| [nslookup set port](nslookup-set-port.md) | 將預設的 TCP/UDP DNS 名稱伺服器埠變更為指定的值。 |
| [nslookup set querytype](nslookup-set-querytype.md) | 變更查詢的資源記錄類型。 |
| [nslookup set recurse](nslookup-set-recurse.md) | 告知 DNS 名稱伺服器，以查詢沒有資訊的其他伺服器。 |
| [nslookup set retry](nslookup-set-retry.md) | 設定重試次數。 |
| [nslookup set root](nslookup-set-root.md) | 變更用於查詢之根伺服器的名稱。 |
| [nslookup set search](nslookup-set-search.md) | 將 DNS 網域搜尋清單中的 DNS 功能變數名稱附加至要求，直到收到解答為止。 這適用于設定和查閱要求至少包含一段時間，但結尾不是句號的時候。 |
| [nslookup set srchlist](nslookup-set-srchlist.md) | 變更預設的 DNS 功能變數名稱和搜尋清單。 |
| [nslookup set timeout](nslookup-set-timeout.md) | 變更等候要求回復的初始秒數。 |
| [nslookup set type](nslookup-set-type.md) | 變更查詢的資源記錄類型。 |
| [nslookup set vc](nslookup-set-vc.md) | 指定將要求傳送到伺服器時，要使用或不使用虛擬電路。 |
| [nslookup view](nslookup-view.md) | 排序並列出先前的 **ls** 子命令或命令的輸出。 |

### <a name="remarks"></a>備註

- 如果 *computerTofind* 是 IP 位址，而且查詢是針對 **A** 或 **PTR** 資源記錄類型，則會傳回電腦的名稱。

- 如果 *computerTofind* 是名稱且沒有尾端句點，則預設的 DNS 功能變數名稱會附加至名稱。 此行為取決於下列 **set** 子命令的狀態： **domain**、 **srchlist**、 **defname**和 **search**。

- 如果您輸入連字號 (-) 而不是 *computerTofind*，命令提示字元會變更為 **nslookup** 互動模式。

- 如果查閱要求失敗，命令列工具會提供錯誤訊息，包括：

  | 錯誤訊息 | 描述 |
  | ------------- | ----------- |
  | 超時 |伺服器不會在一段時間內回應要求，以及特定的重試次數。 您可以使用 [nslookup set timeout](nslookup-set-timeout.md) 命令設定超時時間。 您可以使用 [nslookup set retry](nslookup-set-retry.md) 命令設定重試次數。 |
  | 伺服器沒有回應 | 伺服器電腦上沒有正在執行的 DNS 名稱伺服器。 |
  | 沒有記錄 | 雖然電腦名稱稱有效，但是 DNS 名稱伺服器沒有電腦目前查詢類型的資源記錄。 查詢類型是以 [nslookup set querytype](nslookup-set-querytype.md) 命令指定。 |
  | 不存在的網域 | 電腦或 DNS 功能變數名稱不存在。 |
  | 拒絕連線或無法連線到網路 | 無法連接到 DNS 名稱伺服器或手指伺服器。 此錯誤通常發生在 **ls** 和 **手指** 要求的情況下。 |
  | 伺服器失敗 | DNS 名稱伺服器在其資料庫中發現內部不一致，因此無法傳回有效的答案。 |
  | 拒絕 | DNS 名稱伺服器拒絕服務要求。 |
  | 格式錯誤 | DNS 名稱伺服器發現要求封包的格式不正確。 這可能表示 **nslookup**中發生錯誤。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
