---
title: 在 NPS 中使用規則運算式
description: 本主題說明如何在 Windows Server 的 NPS 中使用正則運算式來進行模式比對。 您可以使用此語法來指定網路原則屬性和 RADIUS 領域的條件。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: jgerend
author: jasongerend
msdate: 08/16/2019
ms.openlocfilehash: 94bb9b54dba046c57c6f82e6a52a71adbcf4ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396382"
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 中使用規則運算式

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

本主題說明如何在 Windows Server 的 NPS 中使用正則運算式來進行模式比對。 您可以使用此語法來指定網路原則屬性和 RADIUS 領域的條件。

## <a name="pattern-matching-reference"></a>模式比對參考

建立具有模式比對語法的正則運算式時，您可以使用下表做為參考來源。 請注意，正則運算式模式通常是以正斜線（/）括住。

|  字母  |  描述  |   範例                                                                 |
| ----------- | ------------- | ------------------------------------------------------------------------  |
|     `\ `     | 表示後面的字元是特殊字元，或應該逐字解讀。  | `/n/ matches the character "n" while the sequence /\n/ matches a line feed or newline character.`  |
|     `^`     |                                                                 符合輸入或行的開頭。                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    符合輸入或行的結尾。                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             比對前一個字元零次或多次。                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              比對前一個字元一或多次。                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              比對前一個字元零或一次。                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           符合分行符號以外的任何單一字元。                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         符合「模式」並記住相符專案。<br />若要比對常值字元 `(` 和 `)` （括弧），請使用 `\(` 或 `\)`。                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               符合 x 或 y。                                                          |
|   `{n} `    |                                                          只比對 n 次 \(n 是非 @ no__t-1negative integer @ no__t-2。                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          至少比對 n 次 \(n 為非 @ no__t-1negative integer @ no__t-2。                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                至少比對 n 和最多 m 次，@no__t 0m，n 是非 @ no__t-1negative integer @ no__t-2。                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       符合任何一個括住的字元 \(a character set @ no__t-1。                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  比對未括住 \(a 負字元集 @ no__t-1 的任何字元。                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              符合字邊界 \(for 範例，空格 @ no__t-1。                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         符合文字界限。                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       比對數位字元 \(equivalent 與0到 9 @ no__t-1 之間的數位。                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           比對 nondigit 字元 \(equivalent 與 `[^0-9]` @ no__t-2。                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        符合表單換頁字元。                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        符合換行字元。                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     符合換行字元。                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   比對任何空白字元，包括空格、索引標籤和表單摘要 \(equivalent 至 `[ \f\n\r\t\v]` @ no__t-2。                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  比對任何非空白字元 \(equivalent 至 `[^ \f\n\r\t\v]` @ no__t-2。                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           符合 tab 字元。                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      符合垂直定位字元。                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              符合任何文字字元，包括底線 \(equivalent 至 `[A-Za-z0-9_]` @ no__t-2。                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           比對任何非 @ no__t-0word 字元，但不包括底線 \(equivalent 至 `[^A-Za-z0-9_]` @ no__t-3。                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | 表示 \( @ no__t-1 的記憶相符專案，其中 num 是正整數 @ no__t-2。  此選項只能在設定屬性操作時，于 [**取代**] 文字方塊中使用。 |                                       `\1` 會取代第一個記住的相符項中儲存的內容。                                       |
|   `/n/ `    |                      允許將 ASCII 碼插入正則運算式 \( @ no__t-1，其中 n 是八進位、十六進位或十進位的轉義值 @ no__t-2。                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>網路原則屬性的範例

下列範例說明如何使用模式比對語法來指定網路原則屬性：

- 若要指定899區碼中的所有電話號碼，語法如下：

     `899.*`

- 若要指定以 192.168.1. 開頭的 IP 位址範圍，語法為：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>在 [使用者名稱] 屬性中操作領域名稱的範例

下列範例說明如何使用模式比對語法來操作 [使用者名稱] 屬性的領域名稱，此名稱位於連線要求原則屬性的 [**屬性**] 索引標籤上。

**若要移除使用者名稱屬性的領域部分**

在網際網路服務提供者 \(ISP @ no__t-1 將連線要求路由傳送至組織 NPS 的外包撥號案例中，ISP RADIUS proxy 可能需要領域名稱來路由驗證要求。 不過，NPS 可能無法辨識使用者名稱的領域名稱部分。 因此，ISP RADIUS proxy 必須先移除領域名稱，才能將它轉送到組織 NPS。

- 尋找： @microsoft @ no__t-1com

- 將以下項目︰

**若要取代<em>user@example.microsoft.com</em> ，_例如 com\user_ 。**

- 尋找： `(.*)@(.*)`

- 取代： `$2\$1`



**以_specific_domain\user_取代_domain\user_**

- 尋找： `(.*)\\(.*)`

- Replace： *specific_domain*`\$2`



<strong>將*user*取代為 *user@specific_domain</strong>*

- 尋找： `$`

- Replace： @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Proxy 伺服器的 RADIUS 訊息轉送範例

當 NPS 做為 RADIUS proxy 使用時，您可以建立路由規則，將具有指定領域名稱的 RADIUS 訊息轉送到一組 RADIUS 伺服器。 以下是根據領域名稱來路由要求的建議語法。

- **NetBIOS 名稱**： `WCOAST`
- **模式**： `^wcoast\\`

在下列範例中，wcoast.microsoft.com 是 DNS 或 Active Directory 網域 wcoast.microsoft.com 的唯一使用者主要名稱（UPN）尾碼。 使用提供的模式，NPS proxy 可以根據網域 NetBIOS 名稱或 UPN 尾碼來路由傳送訊息。

- **NetBIOS 名稱**： `WCOAST`
- **UPN 尾碼**： `wcoast.microsoft.com`
- **模式**： `^wcoast\\|@wcoast\.microsoft\.com$`


如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。
