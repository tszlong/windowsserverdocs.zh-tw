---
title: wbadmin disable backup
description: Wbadmin disable backup 命令的參考文章，此命令會停止執行現有的已排程每日備份。
ms.topic: reference
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f490ede3b6f48285291b1658458d8c8c176bb2c8
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156044"
---
# <a name="wbadmin-disable-backup"></a>wbadmin disable backup

停止執行現有的已排程每日備份。

若要使用此命令停用排程的每日備份，您必須是 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

## <a name="syntax"></a>語法

```
wbadmin disable backup [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin enable backup 命令](wbadmin-enable-backup.md)