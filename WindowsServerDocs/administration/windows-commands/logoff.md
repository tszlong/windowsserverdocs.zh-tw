---
title: 登出
description: 登出命令的參考文章，此命令會從遠端桌面工作階段主機伺服器上的會話登出使用者並刪除會話。
ms.topic: reference
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 249b63e5b98d17b67c6d00fe6d2f8a634d67de6f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030526"
---
# <a name="logoff"></a>登出

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從遠端桌面工作階段主機伺服器上的會話登出使用者，並刪除會話。

## <a name="syntax"></a>語法
```
logoff [<sessionname> | <sessionID>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<sessionname>` | 指定會話的名稱。 這必須是使用中的會話。|
| `<sessionID>` | 指定識別伺服器會話的數值識別碼。 |
| /server:`<servername>` | 指定包含您要登出其使用者之會話的遠端桌面工作階段主機伺服器。 如果未指定，則會使用您目前作用中的伺服器。 |
| /v | 顯示正在執行之動作的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您一律可以從目前登入的會話登出。 不過，您必須擁有 [ **完全控制** ] 許可權，才能登出其他會話的使用者。

- 從會話登出使用者而不發出警告，可能會導致使用者的會話遺失資料。 您應該使用 **msg** 命令將訊息傳送給使用者，然後再採取此動作。

- 如果 `<sessionID>` `<sessionname>` 指定或未指定， **登出** 會將使用者從目前的會話登出。

- 登出使用者之後，所有的處理常式都會結束，且會話會從伺服器中刪除。

- 您無法從主控台會話登出使用者。

### <a name="examples"></a>範例

若要從目前的會話登出使用者，請輸入：

```
logoff
```

若要使用會話的識別碼（例如 *會話 12*）將使用者登出會話，請輸入：

```
logoff 12
```

若要使用會話和伺服器的名稱（例如 session *TERM04* on *Server1*）從會話登出使用者，請輸入：

```
logoff TERM04 /server:Server1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
