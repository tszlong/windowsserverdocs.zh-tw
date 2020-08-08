---
title: 在 NPS 中使用規則運算式
description: 本主題說明如何在 Windows Server 的 NPS 中使用正則運算式來進行模式比對。 您可以使用此語法指定網路原則屬性與 RADIUS 領域的條件。
manager: brianlic
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: jgerend
author: jasongerend
msdate: 08/16/2019
ms.openlocfilehash: b2df170153e2848239a8846e58a84981bc9ad12e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969385"
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 中使用規則運算式

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server (半年通道) 

本主題說明如何在 Windows Server 的 NPS 中使用正則運算式來進行模式比對。 您可以使用此語法指定網路原則屬性與 RADIUS 領域的條件。

## <a name="pattern-matching-reference"></a>模式比對參照

使用模式比對語法建立規則運算式時，可以使用下表做為參照來源。 請注意，正則運算式模式通常會以正斜線 (/) 括住。

|  字元  |  描述  |   範例                                                                 |
| ----------- | ------------- | ------------------------------------------------------------------------  |
|     `\ `     | 表示後面的字元是特殊字元，或應該逐字解讀。  | `/n/ matches the character "n" while the sequence /\n/ matches a line feed or newline character.`  |
|     `^`     |                                                                 比對輸入或行的開頭。                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    比對輸入或行的結尾。                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             不比對或比對前面多個字元。                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              比對前面的一或多個字元。                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              不比對或比對前面的一個字元。                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           比對新行字元之外的任何單一字元。                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         符合「模式」並記住相符專案。<br />若要比對常值字元 `(` 和 `)` (括弧) ，請使用 `\(` 或 `\)` 。                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               比對 x 或 y。                                                          |
|   `{n} `    |                                                          精確 \( 比對 n 次 n 是非 \- 負整數 \) 。                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          至少比對 n 次 \( n 為非 \- 負整數 \) 。                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                至少比對 n 和最多 m 乘以 m \( ，n 為非 \- 負整數 \) 。                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       將任何一個括住的字元與 \( 一個字元集相符 \) 。                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  符合未括住負字元集的任何字元 \( \) 。                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              符合字邊界 \( （例如空格） \) 。                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         比對非文字界限。                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       比對 \( 等於0到9之數位的數位字元 \) 。                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           符合 \( 相當於的 nondigit 字元 `[^0-9]` \) 。                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        比對換頁字元。                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        比對換行字元。                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     比對歸位字元。                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   比對任何空白字元，包括空格、索引標籤和換頁的 \( 對應專案 `[ \f\n\r\t\v]` \) 。                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  比對任何與相等的非空白字元 \( `[^ \f\n\r\t\v]` \) 。                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           比對定位字元。                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      比對垂直定位字元。                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              比對任何文字字元，包括與相等的底線 \( `[A-Za-z0-9_]` \) 。                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           比對任何非 \- 文字字元，而不含與相等的底線 \( `[^A-Za-z0-9_]` \) 。                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | 指的是已記住的相符專案 \( `?num` ，其中 num 是正整數 \) 。  此選項只能在設定屬性操作時，于 [**取代**] 文字方塊中使用。 |                                       `\1`取代第一個記住的相符項中儲存的內容。                                       |
|   `/n/ `    |                      允許將 ASCII 碼插入正則運算式中 \( `?n` ，其中 n 是八進位、十六進位或十進位的轉義值 \) 。                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>網路原則屬性的範例

下列範例描述如何使用模式比對語法指定網路原則屬性：

- 若要指定 899 區碼中的所有電話號碼，語法是：

     `899.*`

- 若要指定以 192.168.1. 開頭的 IP 位址範圍，語法為：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>在使用者名稱屬性中操作領域名稱的範例

下列範例說明如何使用模式比對語法來操作 [使用者名稱] 屬性的領域名稱，此名稱位於連線要求原則屬性的 [**屬性**] 索引標籤上。

**移除使用者名稱屬性的領域部分**

在網際網路服務提供者 isp 將連線 \( \) 要求路由傳送至組織 NPS 的外包撥號案例中，isp RADIUS proxy 可能需要領域名稱來路由驗證要求。 不過，NPS 可能無法辨識使用者名稱的領域名稱部分。 因此，ISP RADIUS proxy 必須先移除領域名稱，才能將它轉送到組織 NPS。

- 尋找： @microsoft \. com

- 將：

**取代為 <em>user@example.microsoft.com</em> _範例。 com\user_**

- 尋找`(.*)@(.*)`

- 取代`$2\$1`



**將 domain\user 取代為_specific_domain] \_ _使用者_**

- 尋找`(.*)\\(.*)`

- 取代： *specific_domain*`\$2`



<strong>若要將*user*取代為*user@specific_domain</strong>*

- 尋找`$`

- 取代： @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Proxy 伺服器轉寄 RADIUS 訊息的範例

您可以建立路由規則，在 NPS 做為 RADIUS Proxy 使用時，將具有指定領域名稱的 RADIUS 訊息轉寄到一組 RADIUS 伺服器。 下列是根據領域名稱路由要求的建議語法。

- **NetBIOS 名稱**：`WCOAST`
- **模式**：`^wcoast\\`

在下列範例中，wcoast.microsoft.com 是 DNS 或 Active Directory 網域 wcoast.microsoft.com (UPN) 尾碼的唯一使用者主體名稱。 使用提供的模式，NPS proxy 可以根據網域 NetBIOS 名稱或 UPN 尾碼來路由傳送訊息。

- **NetBIOS 名稱**：`WCOAST`
- **UPN 尾碼**：`wcoast.microsoft.com`
- **模式**：`^wcoast\\|@wcoast\.microsoft\.com$`


如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS) ](nps-top.md)。
