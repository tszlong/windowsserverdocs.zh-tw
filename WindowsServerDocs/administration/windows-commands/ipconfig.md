---
title: ipconfig
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fff4e5088e3e08cf2e9e742d8fb6aa2187bb9e83
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438129"
---
# <a name="ipconfig"></a>ipconfig



顯示所有目前的 TCP/IP 網路設定值，並重新整理 動態主機設定通訊協定 (DHCP) 和網域名稱系統 (DNS) 設定。 不含參數， **ipconfig**顯示 Internet Protocol version 4 (IPv4) 和 IPv6 位址、 子網路遮罩和預設閘道的所有介面卡。

## <a name="syntax"></a>語法

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/all|顯示所有的配接器的完整 TCP/IP 設定。 介面卡可以是實體介面，例如已安裝的網路介面卡，亦可以是邏輯介面，例如撥號連線。|
|/allcompartments|顯示所有的區間的完整 TCP/IP 設定。|
|/displaydns|從本機 Hosts 檔案，以及任何預先載入的 DNS 用戶端解析程式快取的內容，其中包含兩個項目會顯示最近取得電腦解析的名稱查詢的資源記錄。 DNS 用戶端服務會使用此資訊來查詢其設定的 DNS 伺服器之前，快速解決經常查詢的名稱。|
|/flushdns|排清，並會重設 DNS 用戶端解析程式快取的內容。 在 DNS 疑難排解，您可以使用此程序捨棄負快取項目從快取，以及其他動態新增的項目。|
|/registerdns|會起始手動的動態註冊的 DNS 名稱和 IP 位址設定的電腦。 您可以使用這個參數來疑難排解失敗的 DNS 名稱註冊或解析不需要重新啟動用戶端電腦的用戶端和 DNS 伺服器之間的動態更新問題。 TCP/IP 通訊協定的進階屬性中的 DNS 設定會決定在 DNS 中註冊的名稱。|
|/ 版本 [\<配接器 >]|DHCPRELEASE 將訊息傳送至 DHCP 伺服器，以釋放目前的 DHCP 設定，並捨棄所有配接器的 IP 位址組態，（如果未指定配接器） 或特定的配接器若*配接器*包含參數。 此參數會設定為自動取得 IP 位址的介面卡，停用 TCP/IP。 若要指定配接器名稱，輸入 顯示當您使用的配接器名稱**ipconfig**不加任何參數。|
|/release6[\<Adapter>]|DHCPRELEASE 將訊息傳送到 DHCPv6 伺服器，以釋放目前的 DHCP 設定，並捨棄所有配接器的 IPv6 位址設定 （如果未指定配接器） 或特定的配接器若*配接器*包含參數。 此參數會設定為自動取得 IP 位址的介面卡，停用 TCP/IP。 若要指定配接器名稱，輸入 顯示當您使用的配接器名稱**ipconfig**不加任何參數。|
|/ 更新 [\<配接器 >]|更新所有介面卡 （如果未指定配接器） 或特定介面卡的 DHCP 設定，如果*配接器*參數是否包含。 只有在搭配設定為自動取得 IP 位址的介面卡的電腦上使用此參數。 若要指定配接器名稱，輸入 顯示當您使用的配接器名稱**ipconfig**不加任何參數。|
|/renew6 [\<Adapter>]|更新所有介面卡 （如果未指定配接器） 或特定介面卡的 DHCPv6 設定，如果*配接器*參數是否包含。 只有在搭配設定為自動取得 IPv6 位址的介面卡的電腦上使用此參數。 若要指定配接器名稱，輸入 顯示當您使用的配接器名稱**ipconfig**不加任何參數。|
|/setclassid \<Adapter>[ <ClassID>]|設定指定的介面卡的 DHCP 類別識別碼。 若要設定所有介面卡的 DHCP 類別識別碼，使用星號 ( **&#42;** ) 取代萬用字元*配接器*。 只有在搭配設定為自動取得 IP 位址的介面卡的電腦上使用此參數。 如果未指定 DHCP 類別識別碼，則會移除目前的類別識別碼。|
|/showclassid\<配接器 >|顯示指定的介面卡的 DHCP 類別 ID。 若要查看所有介面卡的 DHCP 類別識別碼，使用星號 ( **&#42;** ) 取代萬用字元*配接器*。 只有在搭配設定為自動取得 IP 位址的介面卡的電腦上使用此參數。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

- 此命令會設定為自動取得 IP 位址的電腦上最有用的。 這可讓使用者判斷哪一個 TCP/IP 組態值已設定 DHCP、 自動私人 IP 位址 (APIPA) 或其他設定。
- 如果您提供的名稱*配接器*包含任何空格，使用引號括住的配接器名稱 (範例： **「** <em>配接器名稱</em> **"** )。
- 如需配接器名稱， **ipconfig**支援使用星號 ( *) 萬用字元來指定任一個配接器，以指定的字串或配接器的名稱中包含指定的字串開頭的名稱。例如，**本機\\** * 符合本機字串開頭的所有介面卡和 **\*Con\\** * 比對包含的所有介面卡字串內容。

## <a name="examples"></a>範例

若要顯示所有的配接器的基本 TCP/IP 設定，請輸入：
```
ipconfig
```
若要顯示所有的配接器的完整 TCP/IP 設定，請輸入：
```
ipconfig /all
```
若要更新本機區域連線介面卡的 DHCP 指派 IP 位址組態，請輸入：
```
ipconfig /renew "Local Area Connection"
```
若要疑難排解 DNS 名稱解析問題時，排清 DNS 解析程式快取，請輸入：
```
ipconfig /flushdns
```
若要顯示的所有名稱開頭為本機介面卡的 DHCP 類別識別碼，請輸入：
```
ipconfig /showclassid Local*
```
若要設定測試的區域連線介面卡的 DHCP 類別識別碼，請輸入：
```
ipconfig /setclassid "Local Area Connection" TEST
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)