---
title: wbadmin get status
description: Wbadmin get status 的參考文章，會報告目前正在執行之備份或復原作業的狀態。
ms.topic: reference
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3245a6c92bf8a3ebde070f2f85e484dc9a317ce1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031986"
---
# <a name="wbadmin-get-status"></a>wbadmin get status



報告目前正在執行之備份或復原作業的狀態。

若要使用這個子命令，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中執行 **wbadmin** 。  (開啟提升許可權的命令提示字元，以滑鼠右鍵按一下 **命令提示**字元，然後按一下 [以 **系統管理員身分執行**]。 ) 

## <a name="syntax"></a>語法

```
wbadmin get status
```

### <a name="parameters"></a>參數

此子命令沒有任何參數。

## <a name="remarks"></a>備註

-   此子命令將不會停止，直到目前的備份或復原作業完成為止—即使您關閉命令視窗，子命令仍會繼續執行。
-   如果您想要停止目前的備份或復原操作，請使用 **wbadmin stop job** 子命令。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [Get-wbjob](/powershell/module/windowserverbackup/?view=winserver2012r2-ps) 指令 Cmdlet
