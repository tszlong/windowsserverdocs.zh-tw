---
title: msg
description: Msg 命令的參考文章，它會將訊息傳送給遠端桌面工作階段主機伺服器上的使用者
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 044d6c7e6dbf7c92cb0c947fcb60eb79ab1db05b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886191"
---
# <a name="msg"></a>msg

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將訊息傳送給遠端桌面工作階段主機伺服器上的使用者。

> [!NOTE]
> 您必須具有訊息特殊存取權限，才能傳送訊息。

## <a name="syntax"></a>語法

```
msg {<username> | <sessionname> | <sessionID>| @<filename> | *} [/server:<servername>] [/time:<seconds>] [/v] [/w] [<message>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<username>` | 指定您想要接收訊息的使用者名稱。 如果您未指定使用者或會話，此命令會顯示錯誤訊息。 指定會話時，它必須是作用中的會話。 |
| `<sessionname>` | 指定您想要接收訊息的會話名稱。 如果您未指定使用者或會話，此命令會顯示錯誤訊息。 指定會話時，它必須是作用中的會話。 |
| `<sessionID>` | 指定您想要接收訊息之使用者的會話數值識別碼。 |
| `@<filename>` | 識別檔案，其中包含您想要接收訊息的使用者名稱、會話名稱和會話識別碼的清單。 |
| * | 將訊息傳送至系統上的所有使用者名稱。 |
| /server:`<servername>` | 指定您要接收訊息之會話或使用者的遠端桌面工作階段主機伺服器。 如果未指定， **/server**會使用您目前登入的伺服器。 |
| /time`<seconds>` | 指定您傳送的訊息在使用者畫面上顯示的時間量。 達到時間限制之後，訊息就會消失。 如果未設定時間限制，訊息會保留在使用者畫面上，直到使用者看到訊息並按一下 **[確定]** 為止。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /W | 等候來自使用者已收到訊息的確認。 如果使用者沒有立即回應，請使用此參數搭配 `/time:<*seconds*>` 來避免可能的長時間延遲。 搭配使用此參數與 **/v**也很有説明。 |
| `<message>` | 指定您想要傳送之訊息的文字。 如果未指定任何訊息，系統會提示您輸入訊息。 若要傳送包含在檔案中的訊息，請輸入小於 ( # A0) 符號，後面接著檔案名。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要傳送有資格使用的訊息，*讓我們在今天的下午1點*到*User1*的所有會話，請輸入：

```
msg User1 Let's meet at 1PM today
```

若要將相同的訊息傳送至會話*modeM02*，請輸入：

```
msg modem02 Let's meet at 1PM today
```

若要將訊息傳送至檔案*userlist*中包含的所有會話，請輸入：

```
msg @userlist Let's meet at 1PM today
```

若要將訊息傳送給所有登入的使用者，請輸入：

```
msg * Let's meet at 1PM today
```

若要將訊息傳送給所有使用者，並使用確認超時 (例如10秒) ，請輸入：

```
msg * /time:10 Let's meet at 1PM today
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
