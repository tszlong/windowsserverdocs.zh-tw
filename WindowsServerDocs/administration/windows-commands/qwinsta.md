---
title: qwinsta
description: Qwinsta 命令的參考文章，此命令會顯示遠端桌面工作階段主機伺服器上會話的相關資訊。
ms.topic: reference
ms.assetid: a793212a-7ecd-44cb-a77b-c5c2edb34979
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: def8a43822e1f7baf544b9f0da3ab84e42bbfb0f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637407"
---
# <a name="qwinsta"></a>qwinsta

適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端桌面工作階段主機伺服器上會話的相關資訊。 此清單不僅包含使用中會話的資訊，也包含伺服器執行的其他會話的相關資訊。

> [!NOTE]
> 此命令與 [查詢會話命令](query-session.md)相同。 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
qwinsta [<sessionname> | <username> | <sessionID>] [/server:<servername>] [/mode] [/flow] [/connect] [/counter]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<sessionname>` | 指定您想要查詢之會話的名稱。 |
| `<username>` | 指定您要查詢其會話的使用者名稱。 |
| `<sessionID>` | 指定您想要查詢之會話的識別碼。 |
| /server:`<servername>` | 識別要查詢的 rd 工作階段主機伺服器。 預設值為目前的伺服器。 |
| /mode | 顯示目前的行設定。 |
| /flow | 顯示目前的流程式控制制設定。 |
| /connect | 顯示目前的連接設定。 |
| /counter | 顯示目前的計數器資訊，包括已建立、已中斷連線和重新連線的會話總數。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 使用者一律可以查詢使用者目前登入的會話。 若要查詢其他會話，使用者必須具有特殊存取權限。

- 如果您未使用 <使用者 *名稱*>、<*Sessionname*> 或 *sessionID* 參數來指定會話，此查詢將會顯示系統中所有作用中會話的相關資訊。

- 當 **qwinsta** 傳回信息時，大於 `(>)` 符號會顯示在目前的會話之前。 例如：

    ```
    C:\>qwinsta
        SESSIONNAME     USERNAME        ID STATE    TYPE    DEVICE
        console         Administrator1  0 active    wdcon
        >rdp-tcp#1      User1           1 active    wdtshare
        rdp-tcp                         2 listen    wdtshare
                                        4 idle
                                        5 idle
    ```

    其中：
  - **SESSIONNAME** 指定指派給會話的名稱。
  - **USERNAME** 表示連接至會話之使用者的使用者名稱。
  - **狀態** 提供會話目前狀態的相關資訊。
  - **類型** 表示會話類型。
  - 主控台或網路連線會話不存在的**裝置**，是指派給會話的裝置名稱。
  - 初始狀態設定為 [已停用] 的任何會話，在啟用之前都不會顯示在 **qwinsta** 清單中。

### <a name="examples"></a>範例

若要顯示伺服器 *Server2*上所有作用中會話的相關資訊，請輸入：

```
qwinsta /server:Server2
```

若要顯示作用中會話 *modeM02*的相關資訊，請輸入：

```
qwinsta modeM02
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [查詢會話命令](query-session.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
