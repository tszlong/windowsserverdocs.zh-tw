---
title: wbadmin delete catalog
description: Wbadmin delete catalog 命令的參考文章，此命令會刪除儲存在本機電腦上的備份類別目錄。
ms.topic: reference
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4a19d856b65fdcf17fb369d81607445530f77275
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156109"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin delete catalog

刪除儲存在本機電腦上的備份類別目錄。 當備份類別目錄已損毀，且您無法使用 [ [wbadmin restore catalog](wbadmin-restore-catalog.md) ] 命令來還原時，請使用此命令。

若要使用此命令刪除備份類別目錄，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

## <a name="syntax"></a>語法

```
wbadmin delete catalog [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -quiet | 在不提示使用者的情況下執行命令。 |

#### <a name="remarks"></a>備註

- 如果您刪除電腦的備份類別目錄，就無法再使用 Windows Server Backup 嵌入式管理單元來取得為該電腦建立的任何備份。 但是，如果您可以移至另一個備份位置，並執行 [wbadmin restore catalog](wbadmin-restore-catalog.md) 命令，您可以從該位置還原備份類別目錄。

- 我們強烈建議您在刪除備份類別目錄之後建立新的備份。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin restore catalog 命令](wbadmin-restore-catalog.md)

- [移除-WBCatalog](/powershell/module/windowsserverbackup/Remove-WBCatalog)
