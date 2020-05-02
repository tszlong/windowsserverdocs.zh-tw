---
title: change logon
description: '[變更登入] 命令的參考主題，可啟用或停用用戶端會話的登入，或顯示目前的登入狀態。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2ebe75f6efa8c3bcfc0018d1f4e6051bb9ebb7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716125"
---
# <a name="change-logon"></a>change logon

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用或停用來自用戶端會話的登入，或顯示目前的登入狀態。 這個公用程式適用于系統維護。 您必須是系統管理員才能執行此命令。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱[Windows Server 中遠端桌面服務的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11))。

## <a name="syntax"></a>語法

```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /query | 顯示目前的登入狀態，無論是已啟用或已停用。 |
| /enable | 允許來自用戶端會話的登入，但無法從主控台進行登入。 |
| /disable | 停用用戶端會話的後續登入，但不從主控台。 不會影響目前登入的使用者。 |
| /drain | 停用來自新用戶端會話的登入，但允許重新附加至現有的會話。 |
| /drainuntilrestart | 停用來自新用戶端會話的登入，直到電腦重新開機，但允許重新附加至現有的會話。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 當您重新開機系統時，會重新啟用登入。

- 如果您是從用戶端會話連線到遠端桌面工作階段主機伺服器，然後在重新啟用登入之前停用登入並登出，您將無法重新連線到您的會話。 若要從用戶端會話重新啟用登入，請在主控台登入。

### <a name="examples"></a>範例

- 若要顯示目前的登入狀態，請輸入：
  
  ```
  change logon /query
  ```

- 若要啟用用戶端會話的登入，請輸入：

  ```
  change logon /enable
  ```

- 若要停用用戶端登入，請輸入：

  ```
  change logon /disable
  ```
  
## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [變更命令](change.md)

- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
