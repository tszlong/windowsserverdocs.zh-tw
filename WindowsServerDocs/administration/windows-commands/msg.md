---
title: msg
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e44d754557f918c218e9e9d35149bffd983a0d54
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723859"
---
# <a name="msg"></a>msg

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將訊息傳送給遠端桌面工作階段主機（rd 工作階段主機）伺服器上的使用者。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

### <a name="parameters"></a>參數

|      參數       |                                                                                                                               描述                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  指定您想要接收訊息的使用者名稱。                                                                                                   |
|    <SessionName>     |                                                                                                 指定您想要接收訊息的會話名稱。                                                                                                 |
|     <SessionID>      |                                                                                            指定您想要接收訊息之使用者的會話數值識別碼。                                                                                            |
|     @<FileName>      |                                                                         識別檔案，其中包含您想要接收訊息的使用者名稱、會話名稱和會話識別碼的清單。                                                                         |
|          \*          |                                                                                                           將訊息傳送至系統上的所有使用者名稱。                                                                                                            |
| /server:<ServerName> |                                              指定您想要接收訊息之會話或使用者的 rd 工作階段主機伺服器。 如果未指定， **/server**會使用您目前登入的伺服器。                                              |
|   /time<Seconds>    | 指定您傳送的訊息在使用者畫面上顯示的時間量。 達到時間限制之後，訊息就會消失。 如果未設定時間限制，訊息會保留在使用者畫面上，直到使用者看到訊息並按一下 **[確定]** 為止。 |
|          /v          |                                                                                                         顯示正在執行之動作的相關資訊。                                                                                                         |
|          /W          |         等候來自使用者已收到訊息的確認。 請使用此參數搭配 **/time：**<*Seconds*>，以避免使用者不會立即回應時可能會很長的延遲。 搭配使用此參數與 **/v**也很有説明。          |
|      <Message>       |                  指定您想要傳送之訊息的文字。 如果未指定任何訊息，系統會提示您輸入訊息。 若要傳送包含在檔案中的訊息，請輸入小於（<）符號，後面加上檔案名。                  |
|          /?          |                                                                                                                  在命令提示字元顯示說明。                                                                                                                   |

## <a name="remarks"></a>備註
-   如果您未指定使用者或會話， **msg**會顯示錯誤訊息。 指定會話時，它必須是作用中的會話。
-   使用者必須具有訊息特殊存取權限，才能傳送訊息。

## <a name="examples"></a>範例
-   若要將「讓我們今天下午1點」的訊息傳送給 User1 的所有會話，請輸入：
    ```
    msg User1 Let's meet at 1PM today
    ```
-   若要將相同的訊息傳送至會話 modeM02，請輸入：
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   若要將訊息傳送至會話12，請輸入：
    ```
    msg 12 Let's meet at 1PM today
    ```
-   若要將訊息傳送至檔案 USERlist 中包含的所有會話，請輸入：
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   若要將訊息傳送給所有登入的使用者，請輸入：
    ```
    msg * Let's meet at 1PM today
    ```
-   若要將訊息傳送給所有使用者，並提供認可超時（例如10秒），請輸入：
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

## <a name="additional-references"></a>其他參考
-  - [命令列語法關鍵](command-line-syntax-key.md)
-  [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
