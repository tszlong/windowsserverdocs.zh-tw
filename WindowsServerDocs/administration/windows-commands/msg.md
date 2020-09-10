---
title: msg
description: Msg 命令的參考文章，此命令會將訊息傳送給遠端桌面工作階段主機伺服器上的使用者
ms.topic: reference
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 34b51bb82fdac6b847d69b4d59a345777054839f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639636"
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
| `<username>` | 指定您想要接收訊息之使用者的名稱。 如果您未指定使用者或會話，此命令會顯示錯誤訊息。 指定會話時，它必須是使用中的會話。 |
| `<sessionname>` | 指定您想要接收訊息的會話名稱。 如果您未指定使用者或會話，此命令會顯示錯誤訊息。 指定會話時，它必須是使用中的會話。 |
| `<sessionID>` | 指定要接收訊息的使用者之會話的數值識別碼。 |
| `@<filename>` | 識別檔案，其中包含您想要接收訊息之使用者名稱、會話名稱和會話識別碼的清單。 |
| * | 將訊息傳送至系統上的所有使用者名稱。 |
| /server:`<servername>` | 指定您想要接收訊息之會話或使用者的遠端桌面工作階段主機伺服器。 如果未指定，則 **/server** 會使用您目前登入的伺服器。 |
| /time`<seconds>` | 指定您傳送的訊息在使用者畫面上顯示的時間量。 到達時間限制之後，訊息就會消失。 如果未設定任何時間限制，則訊息會保留在使用者的畫面上，直到使用者看到訊息，然後按一下 **[確定]**。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /W | 等候使用者收到訊息的通知。 如果使用者沒有立即回應，請使用此參數搭配 `/time:<*seconds*>` 來避免可能的長時間延遲。 搭配使用此參數與 **/v** 也很有説明。 |
| `<message>` | 指定您想要傳送之訊息的文字。 如果未指定任何訊息，系統會提示您輸入訊息。 若要傳送包含在檔案中的訊息，請輸入小於 ( # A0) 符號，後面加上檔案名。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

為了傳送符合資格的訊息， *讓我們今天下午1點* 前往所有 *User1*的會話，請輸入：

```
msg User1 Let's meet at 1PM today
```

若要將相同的訊息傳送到會話 *modeM02*，請輸入：

```
msg modem02 Let's meet at 1PM today
```

若要將訊息傳送至檔案 *userlist*中包含的所有會話，請輸入：

```
msg @userlist Let's meet at 1PM today
```

若要將訊息傳送給所有已登入的使用者，請輸入：

```
msg * Let's meet at 1PM today
```

若要將訊息傳送給所有使用者，請確認超時 (例如10秒) ，請輸入：

```
msg * /time:10 Let's meet at 1PM today
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
