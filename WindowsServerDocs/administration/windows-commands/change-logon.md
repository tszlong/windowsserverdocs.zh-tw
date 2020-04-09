---
title: change logon
description: 用於變更登入的 Windows 命令主題，可啟用或停用用戶端會話的登入，或顯示目前的登入狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a101fc567981716536ecad8e754b81a43eb2c91
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848151"
---
# <a name="change-logon"></a>change logon

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用或停用來自用戶端會話的登入，或顯示目前的登入狀態。 這個公用程式適用于系統維護。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```
### <a name="parameters"></a>參數

|     參數      |                                                       描述                                                        |
|--------------------|--------------------------------------------------------------------------------------------------------------------------|
|       /query       |                             顯示目前的登入狀態，無論是已啟用或已停用。                              |
|      /enable       |                              允許來自用戶端會話的登入，但無法從主控台進行登入。                              |
|      /disable      |  停用用戶端會話的後續登入，但不從主控台。 不會影響目前登入的使用者。   |
|       /drain       |                 停用來自新用戶端會話的登入，但允許重新附加至現有的會話。                 |
| /drainuntilrestart | 停用來自新用戶端會話的登入，直到電腦重新開機，但允許重新附加至現有的會話。 |
|         /?         |                                           在命令提示字元顯示說明。                                           |

## <a name="remarks"></a>備註
- 只有系統管理員可以使用 [**變更登**入] 命令。
- 當您重新開機系統時，會重新啟用登入。 如果您是從用戶端會話連線到遠端桌面工作階段主機（rd 工作階段主機）伺服器並停用登入，然後在重新啟用登錄之前登出，您將無法重新連線到您的會話。 若要從用戶端會話重新啟用登入，請在主控台登入。

## <a name="examples"></a><a name=BKMK_examples></a>典型

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
  
## <a name="additional-references"></a>其他參考資料
- - [命令列語法關鍵](command-line-syntax-key.md)
- [change](change.md)
- [遠端桌面服務 (終端機服務) 命令參考資料](remote-desktop-services-terminal-services-command-reference.md)
