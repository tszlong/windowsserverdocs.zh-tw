---
title: quser
description: Quser 命令的參考文章，此命令會顯示遠端桌面工作階段主機伺服器上的使用者會話相關資訊。
ms.topic: reference
ms.assetid: 8056204f-ed11-4c91-bb1d-c799283a48a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0697fd6ef780f177f0905d2f2af5deb316c61037
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028076"
---
# <a name="quser"></a>quser

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機伺服器上的使用者會話的相關資訊。 您可以使用此命令來找出特定使用者是否登入特定的遠端桌面工作階段主機伺服器。 此命令會傳回下列資訊：

- 使用者名稱

- 遠端桌面工作階段主機伺服器上的會話名稱

- 工作階段識別碼

- 會話的狀態 (作用中或已中斷連線) 

- 閒置時間 (在會話上一次按鍵或滑鼠移動之後的分鐘數) 

- 使用者登入的日期和時間

> [!NOTE]
> 此命令與 [query user 命令](query-user.md)相同。 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
quser [<username> | <sessionname> | <sessionID>] [/server:<servername>]
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

- 當 **quser** 傳回信息時，大於 `(>)` 符號會顯示在目前的會話之前。

### <a name="examples"></a>範例

若要顯示系統上所有已登入使用者的相關資訊，請輸入：

```
quser
```

若要顯示 server *Server1*上使用者*USER1*的相關資訊，請輸入：

```
quser USER1 /server:Server1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [查詢使用者命令](query-user.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
