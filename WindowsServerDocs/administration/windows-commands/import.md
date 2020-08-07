---
title: 匯入 diskshadow
description: 匯入命令的參考文章，它會將可轉移的陰影複製從已載入的中繼資料檔案匯入到系統中。
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d0d76c9565904d6e24c41f4c728bf43061f5040
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888359"
---
# <a name="import-diskshadow"></a>匯入 (diskshadow) 

從載入的中繼資料檔案，將可轉移的陰影複製匯入到系統中。

> 重大您必須先使用 [[載入中繼資料] 命令](load-metadata.md)來載入 DiskShadow 中繼資料檔案，才能使用此命令。

## <a name="syntax"></a>語法

```
import
```

#### <a name="remarks"></a>備註

- 可轉移的陰影複製不會立即儲存在系統上。 其詳細資料會儲存在備份元件檔 XML 檔案中，而 DiskShadow 會自動要求並將其儲存在工作目錄中的 .cab 中繼資料檔案。 使用 [[設定中繼資料] 命令](set-metadata.md)來變更此 XML 檔案的路徑和名稱。

## <a name="examples"></a>範例

以下是範例 DiskShadow 腳本，示範如何使用匯**入**命令：

```
#Sample DiskShadow script demonstrating IMPORT
SET CONTEXT PERSISTENT
SET CONTEXT TRANSPORTABLE
SET METADATA transHWshadow_p.cab
#P: is the volume supported by the Hardware Shadow Copy provider
ADD VOLUME P:
CREATE
END BACKUP
#The (transportable) shadow copy is not in the system yet.
#You can reset or exit now if you wish.

LOAD METADATA transHWshadow_p.cab
IMPORT
#The shadow copy will now be loaded into the system.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskshadow 命令](diskshadow.md)