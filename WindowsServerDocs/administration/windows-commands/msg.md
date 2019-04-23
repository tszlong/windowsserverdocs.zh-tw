---
title: msg
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6c6193fc439140fa559643427067066e819c502
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879549"
---
# <a name="msg"></a>msg

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將訊息傳送至遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上的使用者。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<UserName>|指定您想要接收訊息之使用者的名稱。|
|<SessionName>|指定您想要接收訊息的工作階段的名稱。|
|<SessionID>|指定您想要收到其使用者工作階段的數字識別碼。|
|@<FileName>|識別包含的使用者名稱、 工作階段名稱和您想要接收訊息的工作階段識別碼清單的檔案。|
|*|將訊息傳送至所有系統上的使用者名稱。|
|/server:<ServerName>|其工作階段或您想要接收訊息的使用者，請指定 rd 工作階段主機伺服器。 如果未指定， **/server**會使用要您目前登入伺服器。|
|/time:<Seconds>|指定您所傳送的訊息會顯示在使用者畫面的時間量。 在達到時間限制之後，訊息就會消失。 如果設定沒有時間限制，訊息會保留使用者的畫面上，直到使用者會看到此訊息，然後按一下**確定**。|
|/v|顯示已執行之動作的相關資訊。|
|/w|等候來自使用者已收到訊息通知。 使用此參數與 **/時間：**<*秒*> 若要避免可能長時間的延遲，如果使用者立即回應。 使用此參數與 **/v**也很有用。|
|<Message>|指定您想要傳送之訊息的文字。 如果未不指定任何訊息，則系統會提示您輸入的訊息。 若要傳送的訊息，包含在檔案中，輸入的小於 (<) 符號，後面接著檔案名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   如果您未指定使用者或工作階段中， **msg**顯示錯誤訊息。 當指定的工作階段，它必須是作用中的一個。
-   使用者必須具有將訊息傳送的訊息特殊存取權限。

## <a name="BKMK_examples"></a>範例
-   若要傳送的訊息標題為 「 讓我們符合在今天下午 1 」 到所有工作階段 user1，請輸入：
    ```
    msg User1 Let's meet at 1PM today
    ```
-   若要將相同的訊息傳送至工作階段 modeM02 中，輸入：
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   若要將訊息傳送至 12 的工作階段中，輸入：
    ```
    msg 12 Let's meet at 1PM today
    ```
-   若要將訊息傳送至檔案 USERlist 中包含的所有工作階段中，輸入：
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   若要傳送訊息給所有使用者登入，請輸入：
    ```
    msg * Let's meet at 1PM today
    ```
-   若要將訊息傳送至所有使用者，以通知逾時 （例如，10 秒），請輸入：
    ```
    msg * /time:10 Let's meet at 1PM today
    ```
    
#### <a name="additional-references"></a>其他參考資料
-  [命令列語法關鍵](command-line-syntax-key.md)
-  [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
