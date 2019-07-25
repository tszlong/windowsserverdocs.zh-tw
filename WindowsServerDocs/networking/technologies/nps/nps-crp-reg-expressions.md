---
title: 在 NPS 中使用規則運算式
description: 本主題說明如何在 Windows Server 2016 的 NPS 中使用正則運算式來進行模式比對。 您可以使用此語法來指定網路原則屬性和 RADIUS 領域的條件。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3182ce1d0e856b06b143719c488864e9a58fbc0a
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476579"
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 中使用規則運算式

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何在 Windows Server 2016 的 NPS 中使用正則運算式來進行模式比對。 您可以使用此語法來指定網路原則屬性和 RADIUS 領域的條件。

## <a name="pattern-matching-reference"></a>模式比對參考

建立具有模式比對語法的正則運算式時, 您可以使用下表做為參考來源。


|  字母  |                                                                                 描述                                                                                  |                                                                 範例                                                                 |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
|     `\`|                                                             將下一個字元標示為符合的字元。                                                               |                      `/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`                       |
|     `^`     |                                                                 符合輸入或行的開頭。                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    符合輸入或行的結尾。                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             比對前一個字元零次或多次。                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              比對前一個字元一或多次。                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              比對前一個字元零或一次。                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           符合分行符號以外的任何單一字元。                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         符合「模式」並記住相符專案。<br />若要比對常`(`值`)`字元和 (括弧) `\(` , `\)`請使用或。                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               符合 x 或 y。                                                          |
|   `{n} `    |                                                          精確比對 n \(次 n 是非\-負整數\)。                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          至少比對 n 次\(n 為非\-負整數\)。                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                至少比對 n 和最多 m 乘以\(m, n 為非\-負整數\)。                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       將任何一個括住的字元\(與一個\)字元集相符。                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  符合未括住\(負\)字元集的任何字元。                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              符合字邊界\((例如空格\))。                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         符合文字界限。                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       比對等於 0 \(到 9\)之數位的數位字元。                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           符合相當於\( `[^0-9]`的nondigit字元\)。                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        符合表單換頁字元。                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        符合換行字元。                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     符合換行字元。                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   比對\( `[ \f\n\r\t\v]`任何空白字元,包括空格、索引標籤和換頁的對應專案。\)                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  比對\( `[^ \f\n\r\t\v]`任何與相等的非空白字元。\)                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           符合 tab 字元。                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      符合垂直定位字元。                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              比對\( `[A-Za-z0-9_]`任何文字字元,包括與相等的底線。\)                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           \-比對\(任何非\)文字字元, 而不含與相等的底線。 `[^A-Za-z0-9_]`                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | 指的是已\(記住的相符`?num`專案, 其中\)num 是正整數。  此選項只能在設定屬性操作時, 于 [**取代**] 文字方塊中使用。 |                                       `\1`取代第一個記住的相符項中儲存的內容。                                       |
|   `/n/ `    |                      允許將 ASCII 碼插入正則運算式\( `?n`中, 其中 n 是八進位、十六進位或十進位的轉義值\)。                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>網路原則屬性的範例

下列範例說明如何使用模式比對語法來指定網路原則屬性:

- 若要指定899區碼中的所有電話號碼, 語法如下:

     `899.*`

- 若要指定以 192.168.1. 開頭的 IP 位址範圍, 語法為:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>在 [使用者名稱] 屬性中操作領域名稱的範例

下列範例說明如何使用模式比對語法來操作 [使用者名稱] 屬性的領域名稱, 此名稱位於連線要求原則屬性的 [**屬性**] 索引標籤上。

**若要移除使用者名稱屬性的領域部分**

在網際網路服務提供者\(isp\)將連線要求路由傳送至組織 NPS 的外包撥號案例中, isp RADIUS proxy 可能需要領域名稱來路由驗證要求。 不過, NPS 可能無法辨識使用者名稱的領域名稱部分。 因此, ISP RADIUS proxy 必須先移除領域名稱, 才能將它轉送到組織 NPS。

- 尋找: @microsoft \.com

- 將以下項目︰

**取代<em>user@example.microsoft.com</em>為*範例。 com\user***

- 尋找`(.*)@(.*)`

- 取代`$2\$1`



**以*specific_domain\user*取代*domain\user***

- 尋找`(.*)\\(.*)`

- Replace: *specific_domain*`\$2`



<strong>若要將*user*取代為 *user@specific_domain</strong>*

- 尋找`$`

- Replace: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Proxy 伺服器的 RADIUS 訊息轉送範例

當 NPS 做為 RADIUS proxy 使用時, 您可以建立路由規則, 將具有指定領域名稱的 RADIUS 訊息轉送到一組 RADIUS 伺服器。 以下是根據領域名稱來路由要求的建議語法。

- **NetBIOS 名稱**:`WCOAST`
- **模式**:`^wcoast\\`

在下列範例中, wcoast.microsoft.com 是 DNS 或 Active Directory 網域 wcoast.microsoft.com 的唯一使用者主要名稱 (UPN) 尾碼。 使用提供的模式, NPS proxy 可以根據網域 NetBIOS 名稱或 UPN 尾碼來路由傳送訊息。

- **NetBIOS 名稱**:`WCOAST`
- **UPN 尾碼**:`wcoast.microsoft.com`
- **模式**:`^wcoast\\|@wcoast\.microsoft\.com$`


如需管理 NPS 的詳細資訊, 請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊, 請參閱[網路原則伺服器 (NPS)](nps-top.md)。
