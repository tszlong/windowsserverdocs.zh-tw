---
title: 在 NPS 中使用規則運算式
description: 本主題說明使用規則運算式模式比對在 Windows Server 2016 中的 NPS。 您可以使用此語法來指定網路原則屬性與 RADIUS 領域的條件。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a985df9fea31e5ee180caef4e69899ae8468ff71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865259"
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 中使用規則運算式

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明使用規則運算式模式比對在 Windows Server 2016 中的 NPS。 您可以使用此語法來指定網路原則屬性與 RADIUS 領域的條件。

## <a name="pattern-matching-reference"></a>模式比對的參考

使用模式比對語法建立規則運算式時，您可以使用下表做為參照來源。

|字元|描述|範例|
|---------|-----------|-------|
|`\`  |將下一個字元標示為 比對的字元。 |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |比對輸入或行的開頭。 | &nbsp; |
|`$`  |比對輸入或行結尾。 | &nbsp; |
|`*`  |比對前置字元零或多次。 |`/zo*/ matches either "z" or "zoo."` |
|`+`  |比對前置字元一或多次。 |`/zo+/ matches "zoo" but not "z."` |
|`?`  |比對前置字元零或一次。 |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |比對新行字元以外的任何單一字元。  | &nbsp; |
|`(pattern)`  |符合 「 模式 」，並記住此比對。<br />比對常值字元`(`並`)`（括號），使用`\(`或`\)`。   | &nbsp;  |
|`x|y `  |比對 x 或 y。  |`/z|food?/ matches "zoo" or "food."` |
|`{n} `  |比對確切為 n 個多次\(n 是非\-負整數\)。  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{n,}`  |比對至少 n 次\(n 是非\-負整數\)。  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{n,m}`  |比對至少 n 個、 至多 m 多次\(m 和 n 都是非\-負整數\)。  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[xyz]`  |符合任何一個括住的字元\(字元集\)。  |`/[abc]/ matches the "a" in "plain."`  |
|`[^xyz]`  |比對不包含任何字元\(否定的字元集\)。  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |比對字邊界\(例如，空格\)。  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |比對非文字界限。  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |比對的數字字元\(等於 0 到 9 的數字\)。  | &nbsp; |
|`\D`  |比對非數字字元\(相當於`[^0-9]` \)。  | &nbsp; |
|`\f`  |比對換頁字元。  | &nbsp; |
|`\n`  |比對換行字元。  | &nbsp; |
|`\r`  |比對歸位字元。  | &nbsp; |
|`\s`  |比對任何空白字元包含空格、 定位及換頁字元\(相當於`[ \f\n\r\t\v]` \)。  | &nbsp; |
|`\S`  |比對任何非泛空白字元\(相當於`[^ \f\n\r\t\v]` \)。  | &nbsp; |
|`\t`  |比對定位字元。  | &nbsp; |
|`\v`  |比對垂直定位字元。  | &nbsp; |
|`\w`  |比對任何文字字元，包括底線\(相當於`[A-Za-z0-9_]` \)。  | &nbsp; |
|`\W`  |比對任何非\-文字字元，不包括底線\(相當於`[^A-Za-z0-9_]` \)。  | &nbsp; |
|`\num`  |參照記憶的比對\( `?num`num 所在正整數\)。  只有在使用此選項**取代**設定屬性操作時，文字方塊。| `\1` 會取代第一個記憶的比對中儲存的內容。  |
|`/n/ `  |允許的 ASCII 碼插入規則運算式\( `?n`，其中 n 是八進位、 十六進位或是十進位逸出值\)。  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>網路原則屬性的範例

下列範例說明使用模式比對語法指定網路原則屬性：

- 若要指定 899 區碼內的所有電話號碼，語法為：

     `899.*`

- 若要指定以 192.168.1 的 IP 位址範圍開頭，語法為：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>用於操作的使用者名稱屬性中的領域名稱的範例

下列範例說明使用模式比對語法操作領域名稱的使用者名稱屬性，位於**屬性**的連線要求原則內容 索引標籤。

**若要移除的使用者名稱屬性的領域部分**

在網際網路服務提供者中的委外撥號狀況\(ISP\)將連接要求路由到組織 NPS，ISP RADIUX proxy 可能需要領域名稱路由驗證要求。 不過，NPS 可能無法辨識的使用者名稱的領域名稱部分。 因此，領域名稱必須先移除由 ISP RADIUS proxy 將它轉寄到組織 NPS。

- 尋找： @microsoft \.com

- 將以下項目︰

**若要取代*user@example.microsoft.com*使用*example.microsoft.com\user***

- 尋找：`(.*)@(.*)`

- 取代：`$2\$1`



**若要取代*domain\user*使用*specific_domain\user***

- 尋找：`(.*)\\(.*)`

- 取代： *specific_domain*`\$2`



**若要取代*使用者*與 *user@specific_domain***

- 尋找：`$`

- 取代: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Proxy 伺服器轉寄 RADIUS 訊息的範例

您可以建立路由規則，當 NPS 做為 RADIUS proxy 轉送 RADIUS 訊息到 RADIUS 伺服器的一組指定的領域名稱。 以下是建議的語法，根據 領域名稱路由要求。

- **NetBIOS 名稱**: `WCOAST`
- **模式**:      `^wcoast\\`

在下列範例中，wcoast.microsoft.com 是 DNS 或 Active Directory 網域 wcoast.microsoft.com 的唯一使用者主體名稱 (UPN) 尾碼。 使用提供的模式，NPS proxy 可以將訊息路由根據網域 NetBIOS 名稱或 UPN 尾碼。

- **NetBIOS 名稱**: `WCOAST`
- **UPN 尾碼**:   `wcoast.microsoft.com`
- **模式**:      `^wcoast\\|@wcoast\.microsoft\.com$`


如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
