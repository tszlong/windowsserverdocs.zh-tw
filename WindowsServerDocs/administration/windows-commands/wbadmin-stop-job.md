---
title: wbadmin stop job
description: Wbadmin stop job 命令的參考文章，此命令會取消目前正在執行的備份或復原操作。
ms.topic: reference
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9952f28f674da6eae295dcffc0357cade19fdd1c
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524703"
---
# <a name="wbadmin-stop-job"></a>wbadmin stop job

取消目前正在執行的備份或復原操作。

> [!IMPORTANT]
> 無法重新開機已取消的作業。 您必須從一開始就執行取消的備份或復原操作。

若要使用此命令停止備份或復原作業，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

## <a name="syntax"></a>語法

```
wbadmin stop job [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)
