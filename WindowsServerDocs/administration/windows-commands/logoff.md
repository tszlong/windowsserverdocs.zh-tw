---
title: 登出
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3daecbc2f4034070a5b805c75a6b647ba168e2c5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820538"
---
# <a name="logoff"></a>登出

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將使用者從遠端桌面工作階段主機（rd 工作階段主機）伺服器上的會話登出，並從伺服器刪除會話。


> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
### <a name="parameters"></a>參數

|      參數       |                                                                             說明                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  指定會話的名稱。                                                                  |
|     <SessionID>      |                                                 指定識別伺服器會話的數值識別碼。                                                 |
| /server:<ServerName> | 指定包含您要登出其使用者之會話的 rd 工作階段主機伺服器。 如果未指定，則會使用目前作用中的伺服器。 |
|          /v          |                                                       顯示正在執行之動作的相關資訊。                                                        |
|          /?          |                                                                 在命令提示字元顯示說明。                                                                 |

## <a name="remarks"></a>備註
- 您一律可以從目前登入的會話登出。 不過，您必須擁有 [完全控制] 許可權，才能登出其他會話的使用者。
- 將使用者從會話登出而不發出警告，可能會導致使用者的會話遺失資料。 在採取此動作之前，您應該使用**msg**命令將訊息傳送給使用者，以警告使用者。
- 如果未指定 <*SessionID*> 或 <*SessionName*>，**登出**就會登出使用者的目前會話。 如果您指定 <*SessionName*>，它必須是作用中的一個。
- 當您登出使用者時，所有處理常式都會結束，而且會話會從伺服器中刪除。
- 您無法從主控台會話登出使用者。
  ## <a name="examples"></a>範例
- 若要從目前的會話登出使用者，請輸入：
  ```
  logoff
  ```
- 若要使用會話的識別碼（例如會話12）從會話登出使用者，請輸入：
  ```
  logoff 12
  ```
- 若要使用會話和伺服器的名稱從會話登出使用者，例如 Server1 上的 session TERM04，請輸入：
  ```
  logoff TERM04 /server:Server1
  ```

## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
-   [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
