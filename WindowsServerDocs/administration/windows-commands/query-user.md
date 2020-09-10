---
title: query user
description: 查詢使用者命令的參考文章，此命令會顯示遠端桌面工作階段主機伺服器上的使用者會話相關資訊。
ms.topic: reference
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bacc14f6945c9f1257763121b66a77d072b51803
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637412"
---
# <a name="query-user"></a>query user

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機伺服器上的使用者會話的相關資訊。 您可以使用此命令來找出特定使用者是否登入特定的遠端桌面工作階段主機伺服器。 此命令會傳回下列資訊：

- 使用者名稱

- 遠端桌面工作階段主機伺服器上的會話名稱

- 工作階段識別碼

- 會話的狀態 (作用中或已中斷連線) 

- 閒置時間 (在會話上一次按鍵或滑鼠移動之後的分鐘數) 

- 使用者登入的日期和時間

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
query user [<username> | <sessionname> | <sessionID>] [/server:<servername>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<username>` | 指定您想要查詢之使用者的登入名稱。 |
| `<sessionname>` | 指定您想要查詢之會話的名稱。 |
| `<sessionID>` | 指定您想要查詢之會話的識別碼。 |
| /server:`<servername>` | 指定您要查詢的遠端桌面工作階段主機伺服器。 否則，就會使用目前的遠端桌面工作階段主機伺服器。 只有當您在遠端伺服器上使用此命令時，才需要這個參數。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 若要使用此命令，您必須擁有 [完全控制] 許可權或 [特殊存取權限]。

- 如果您未使用 <使用者 *名稱*>、<*Sessionname*> 或 *sessionID* 參數指定使用者，則會傳回所有登入伺服器的使用者清單。 或者，您也可以使用 **查詢會話** 命令，顯示伺服器上所有會話的清單。

- 當 **查詢使用者** 傳回信息時，會在 `(>)` 目前的會話之前顯示大於符號的。

### <a name="examples"></a>範例

若要顯示系統上所有已登入使用者的相關資訊，請輸入：

```
query user
```

若要顯示 server *Server1*上使用者*USER1*的相關資訊，請輸入：

```
query user USER1 /server:Server1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [查詢命令](query.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
