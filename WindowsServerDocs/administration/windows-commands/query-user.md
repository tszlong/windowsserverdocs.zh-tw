---
title: query user
description: 查詢使用者命令的參考文章，其會顯示遠端桌面工作階段主機伺服器上的使用者會話相關資訊。
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea760c32cc7955c96a363c994c2cb49227bceb2e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884400"
---
# <a name="query-user"></a>query user

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機伺服器上的使用者會話相關資訊。 您可以使用這個命令來找出特定使用者是否登入特定的遠端桌面工作階段主機伺服器。 此命令會傳回下列資訊：

- 使用者名稱

- 遠端桌面工作階段主機伺服器上的會話名稱

- 工作階段識別碼

- 會話的狀態 (作用中或已中斷連線) 

- [閒置時間] (在會話上一次按鍵或滑鼠移動之後的分鐘數) 

- 使用者登入的日期和時間

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱[Windows Server 中遠端桌面服務的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

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
| /server:`<servername>` | 指定您想要查詢的遠端桌面工作階段主機伺服器。 否則，會使用目前的遠端桌面工作階段主機伺服器。 只有當您在遠端伺服器上使用此命令時，才需要此參數。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 若要使用此命令，您必須擁有 [完全控制] 許可權或 [特殊存取權限]。

- 如果您未使用 <*username*>、<*Sessionname*> 或*sessionID*參數來指定使用者，則會傳回登入伺服器的所有使用者清單。 或者，您也可以使用 [**查詢會話**] 命令，顯示伺服器上所有會話的清單。

- 當**查詢使用者**傳回信息時，大於 `(>)` 符號會顯示在目前的會話之前。

### <a name="examples"></a>範例

若要顯示所有登入系統之使用者的相關資訊，請輸入：

```
query user
```

若要顯示伺服器*Server1*上使用者*USER1*的相關資訊，請輸入：

```
query user USER1 /server:Server1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [查詢命令](query.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
