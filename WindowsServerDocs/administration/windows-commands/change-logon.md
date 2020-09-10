---
title: change logon
description: 變更登入命令的參考文章，此命令會啟用或停用用戶端會話的登入，或顯示目前的登入狀態。
ms.topic: reference
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 55732dc5803f4ac783828293f5da839cca364b40
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629907"
---
# <a name="change-logon"></a>change logon

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用或停用用戶端會話的登入，或顯示目前的登入狀態。 此公用程式適用于系統維護。 您必須是系統管理員才能執行此命令。

> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 [Windows Server 遠端桌面服務中的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /query | 顯示目前的登入狀態，無論啟用或停用。 |
| /enable | 允許來自用戶端會話的登入，但無法從主控台進行登入。 |
| /disable | 停用來自用戶端會話的後續登入，而不是從主控台。 不會影響目前登入的使用者。 |
| /drain | 停用新用戶端會話的登入，但允許重新加入現有的會話。 |
| /drainuntilrestart | 停用新用戶端會話的登入，直到電腦重新開機為止，但允許重新加入現有的會話。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當您重新開機系統時，會重新啟用登入。

- 如果您是從用戶端會話連線到遠端桌面工作階段主機伺服器，然後在重新啟用登入之前停用登入和登出，您將無法重新連線到您的會話。 若要重新啟用來自用戶端會話的登入，請在主控台登入。

### <a name="examples"></a>範例

- 若要顯示目前的登入狀態，請輸入：

  ```
  change logon /query
  ```

- 若要從用戶端會話啟用登入，請輸入：

  ```
  change logon /enable
  ```

- 若要停用用戶端登入，請輸入：

  ```
  change logon /disable
  ```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [變更命令](change.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
