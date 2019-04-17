---
title: 使用規則運算式中 NPS
description: 本主題解釋如何使用運算式一般符合 NPS 在 Windows Server 2016 中的模式。 您可以使用下列語法指定的網路原則屬性與 RADIUS 領域的條件。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f84173f1f51be9fd44995dc41f759bbea4fb3539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-regular-expressions-in-nps"></a>使用規則運算式中 NPS

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題解釋如何使用運算式一般符合 NPS 在 Windows Server 2016 中的模式。 您可以使用下列語法指定的網路原則屬性與 RADIUS 領域的條件。

## <a name="pattern-matching-reference"></a>模式比參考資料

使用模式符合語法建立規則運算式時，您可以使用下表做為參考資料來源。

|一個字元|描述|範例|
|---------|-----------|-------|
|`\`  |標記的下一個字元符合字元。 |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |符合輸入或一行的開頭。 | &nbsp; |
|`$`  |符合輸入或一行的結尾。 | &nbsp; |
|`*`  |符合上述字元或多個時間。 |`/zo*/ matches either "z" or "zoo."` |
|`+`  |符合上述字元一或多個時間。 |`/zo+/ matches "zoo" but not "z."` |
|`?`  |比對前一個字元零或一次。 |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |符合新行字元以外的任何一個字元。  | &nbsp; |
|`( pattern )`  |符合「模式」，並會記住相符項目。   |`To match ( ) (parentheses), use "\(" or "\)".`  |
|' x | Y '  |符合 x 或 y。  |' /z|食物？/ 符合」動物園」或「食物。」` |
|`{ n } `  |符合完全 n 次 \（n 是 non\ 負 integer\）。  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{ n ,}`  |符合至少 n 時間 \（n 是 non\ 負 integer\）。  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{ n , m }`  |符合至少 n 和最 m 時間 \（m 和 n 是 non\ 負 integers\）。  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[ xyz ]`  |比對任何一個括號字元 \(a character set\)。  |`/[abc]/ matches the "a" in "plain."`  |
|`[^ xyz ]`  |比對任何不包含的字元 \ (負字元 set\)。  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |符合 word 邊界 \ (例如，space\)。  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |符合非邊界。  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |符合數字字元 \（相當於數字 0 到 9\）。  | &nbsp; |
|`\D`  |符合非數字字元 \ (相當於`[^0-9]`\)。  | &nbsp; |
|`\f`  |相符項目換字元。  | &nbsp; |
|`\n`  |相符項目換字元。  | &nbsp; |
|`\r`  |比對換字元。  | &nbsp; |
|`\s`  |符合空間，索引標籤，然後送紙包括任何空格字元 \ (相當於`[ \f\n\r\t\v]`\)。  | &nbsp; |
|`\S`  |比對任何非空格字元 \ (相當於`[^ \f\n\r\t\v]`\)。  | &nbsp; |
|`\t`  |符合] 索引標籤的字元。  | &nbsp; |
|`\v`  |符合垂直] 索引標籤的字元。  | &nbsp; |
|`\w`  |比對任何文字的字元，包括底線 \ (相當於`[A-Za-z0-9_]`\)。  | &nbsp; |
|`\W`  |比對任何 non\ 字字元，不含底線 \ (相當於`[^A-Za-z0-9_]`\)。  | &nbsp; |
|`\ num`  |指向記憶相符項目 \ (`?num`、num 位置是正 integer\)。  可以使用這個選項只在**取代**設定屬性操作時的文字方塊。| `\1` 將會取代項目儲存在第一次記憶相符項目。  |
|`/ n / `  |讓 ASCII 代碼插入規則運算式 \ (`?n`、n 是進位、十六進位或小數點 esc 鍵 value\)。  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>網路原則屬性範例

下列範例描述模式比語法指定的網路原則屬性使用：

- 若要指定的 899 區碼在所有的電話號碼，語法為：

     `899.*`

- 若要指定 192.168.1 開始 IP 位址，語法為：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>範例操作領域中的使用者名稱屬性名稱

下列範例描述模式比語法管理使用者名稱屬性，這位於領域名稱使用**屬性**索引標籤中連接要求原則的屬性。

**若要移除的領域部分的使用者名稱屬性**

在外部撥號案例中網際網路服務提供者 \(ISP\) 路徑連接要求公司 NPS 伺服器、ISP RADIUS proxy 可能需要路由傳送驗證要求領域名稱。 不過，NPS 伺服器可能無法辨識的領域名稱部分的使用者名稱。 因此，領域名稱必須先移除 ISP RADIUS proxy 轉送組織 NPS 伺服器。

- 尋找：@microsoft\.com

- 取代：

**將*user@example.microsoft.com*與*example.microsoft.com\user***

- 尋找：`(.*)@(.*)`

- 取代：`$2\$1`



**將*網域使用者*的*specific_domain\user***

- 尋找：`(.*)\\(.*)`

- 取代：*specific_domain*`\$2`



**將*使用者*有*user@specific_domain***

- 尋找：`$`

- 取代：@*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>範例 RADIUS 郵件轉寄 proxy 伺服器

您可以建立 NPS RADIUS proxy 為時，向前一組 RADIUS 伺服器的名稱指定的領域 RADIUS 訊息的路徑規則。 以下是建議的語法路由領域名稱為基礎的需求。

- **NetBIOS 名稱 **: `WCOAST`
- **模式 **:      `^wcoast\\`

下列範例中，在 wcoast.microsoft.com 是 DNS 或 Active Directory domain wcoast.microsoft.com 獨特的使用者主體名稱 (UPN) 尾碼。使用提供的模式，NPS proxy 可以傳送簡訊，根據網域 NetBIOS 名稱或 UPN 尾碼。

- **NetBIOS 名稱 **: `WCOAST`
- **UPN 尾碼 **:   `wcoast.microsoft.com`
- **模式 **:      `^wcoast\\|@wcoast\.microsoft\.com$`


如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
