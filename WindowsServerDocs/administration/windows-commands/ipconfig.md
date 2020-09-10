---
title: ipconfig
description: Ipconfig 命令的參考文章，此命令會顯示所有目前的 TCP/IP 網路設定值，並重新整理動態主機設定通訊協定 (DHCP) 和網域名稱系統 (DNS) 設定。
ms.topic: reference
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 20005fff04df421e5f3699600278d51b8711337d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639590"
---
# <a name="ipconfig"></a>ipconfig

顯示所有目前的 TCP/IP 網路設定值，並重新整理動態主機設定通訊協定 (DHCP) 和網域名稱系統 (DNS) 設定。 在不使用參數的情況下使用， **ipconfig** 會顯示第四版網際網路協定 (IPv4) 和 IPv6 位址、子網路遮罩，以及所有介面卡的預設閘道。

## <a name="syntax"></a>語法

```
ipconfig [/allcompartments] [/all] [/renew [<adapter>]] [/release [<adapter>]] [/renew6[<adapter>]] [/release6 [<adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <adapter>] [/setclassid <adapter> [<classID>]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /all | 顯示所有介面卡的完整 TCP/IP 設定。 介面卡可以是實體介面，例如已安裝的網路介面卡，亦可以是邏輯介面，例如撥號連線。 |
| /displaydns | 顯示 DNS 用戶端解析程式快取的內容，其中包括從本機主機檔案預先載入的專案，以及電腦所解析之名稱查詢的任何最近取得的資源記錄。 DNS 用戶端服務在查詢設定的 DNS 伺服器之前，會使用此資訊快速地解析經常查詢的名稱。 |
| /flushdns | 清除和重設 DNS 用戶端解析程式快取的內容。 在 DNS 疑難排解期間，您可以使用此程式來捨棄快取中的負快取專案，以及動態新增的任何其他專案。 |
| /registerdns | 針對在電腦上設定的 DNS 名稱和 IP 位址起始手動動態註冊。 您可以使用此參數來疑難排解失敗的 DNS 名稱註冊，或解決用戶端與 DNS 伺服器之間的動態更新問題，而不需要重新開機用戶端電腦。 TCP/IP 通訊協定 [advanced] 屬性中的 DNS 設定會決定要在 DNS 中註冊的名稱。 |
| /release `[<adapter>]` | 將 DHCPRELEASE 訊息傳送到 DHCP 伺服器以釋出目前的 DHCP 設定，並捨棄任何 (的 IP 位址設定（如果未指定介面卡，則為），如果未 *) 指定介面卡參數，* 則捨棄特定介面卡的 IP 位址設定。 此參數會針對設定為自動取得 IP 位址的介面卡停用 TCP/IP。 若要指定介面卡名稱，請輸入當您使用不含參數的 **ipconfig** 時所顯示的介面卡名稱。 |
| /release6`[<adapter>]` | 將 DHCPRELEASE 訊息傳送至 DHCPv6 伺服器以釋出目前的 DHCP 設定，如果未指定介面卡，則會捨棄任何介面卡 (的 IPv6 位址設定) 或如果包含 *適配* 卡參數，則為特定的介面卡。 此參數會針對設定為自動取得 IP 位址的介面卡停用 TCP/IP。 若要指定介面卡名稱，請輸入當您使用不含參數的 **ipconfig** 時所顯示的介面卡名稱。 |
| /renew `[<adapter>]` | 更新所有介面卡的 DHCP 設定 (如果未指定介面卡) ，或如果包含 *適配* 卡參數，則為特定的介面卡。 只有在已設定為自動取得 IP 位址的介面卡的電腦上，才能使用此參數。 若要指定介面卡名稱，請輸入當您使用不含參數的 **ipconfig** 時所顯示的介面卡名稱。 |
| /renew6 `[<adapter>]` | 更新所有介面卡的 DHCPv6 設定 (如果未指定介面卡) ，或如果包含 *適配* 卡參數，則為特定的介面卡。 此參數僅適用于已設定為自動取得 IPv6 位址的介面卡電腦。 若要指定介面卡名稱，請輸入當您使用不含參數的 **ipconfig** 時所顯示的介面卡名稱。 |
| /setclassid `<adapter>[<classID>]` | 設定指定之介面卡的 DHCP 類別識別碼。 若要設定所有介面卡的 DHCP 類別識別碼，請使用星號 (**&#42;**) 的萬用字元來取代 *介面卡*。 只有在已設定為自動取得 IP 位址的介面卡的電腦上，才能使用此參數。 如果未指定 DHCP 類別識別碼，則會移除目前的類別識別碼。 |
| /showclassid `<adapter>` | 顯示指定介面卡的 DHCP 類別識別碼。 若要查看所有介面卡的 DHCP 類別識別碼，請使用星號 (**&#42;**) 的萬用字元來取代 *介面卡*。 只有在已設定為自動取得 IP 位址的介面卡的電腦上，才能使用此參數。 |
| /? | 在命令提示字元顯示 [說明]。 |

#### <a name="remarks"></a>備註

- 此命令最適用于設定為自動取得 IP 位址的電腦。 這可讓使用者判斷 DHCP 已設定哪些 TCP/IP 設定值、 (APIPA) 的自動私人 IP 位址，或其他設定。

- 如果您為 *介面卡* 提供的名稱包含任何空格，請使用引號括住介面卡名稱 (例如「介面卡名稱」 ) 。

- 若為介面卡名稱， **ipconfig** 支援使用星號 ( * ) 萬用字元來指定名稱開頭為指定字串的介面卡，或名稱包含指定之字串的介面卡。 例如，會比對 `Local*` 以本機字串開頭的所有介面卡，並 `*Con*` 符合所有包含字串 Con 的介面卡。

### <a name="examples"></a>範例

若要顯示所有介面卡的基本 TCP/IP 設定，請輸入：

```
ipconfig
```

若要顯示所有介面卡的完整 TCP/IP 設定，請輸入：

```
ipconfig /all
```

若只要針對本機區域連接介面卡更新 DHCP 指派的 IP 位址設定，請輸入：

```
ipconfig /renew Local Area Connection
```

若要在疑難排解 DNS 名稱解析問題時排清 DNS 解析程式快取，請輸入：

```
ipconfig /flushdns
```

若要顯示所有名稱開頭為 Local 的介面卡的 DHCP 類別識別碼，請輸入：

```
ipconfig /showclassid Local*
```

若要設定本機區域連線介面卡的 DHCP 類別識別碼以進行測試，請輸入：

```
ipconfig /setclassid Local Area Connection TEST
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
