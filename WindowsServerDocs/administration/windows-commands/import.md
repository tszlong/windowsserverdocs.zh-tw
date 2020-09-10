---
title: 匯入 diskshadow
description: 匯入命令的參考文章，此命令會將可傳送的陰影複製從載入的中繼資料檔案匯入系統中。
ms.topic: reference
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bf86c069bf0f5de4d6fa773d319bdd75beeb93eb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634522"
---
# <a name="import-diskshadow"></a>匯入 (diskshadow) 

從載入的中繼資料檔案，將可轉移的陰影複製匯入系統中。

> 須知您必須使用 [load metadata 命令](load-metadata.md) 載入 DiskShadow 中繼資料檔案，才能使用此命令。

## <a name="syntax"></a>語法

```
import
```

#### <a name="remarks"></a>備註

- 可轉移的陰影複製不會立即儲存在系統上。 其詳細資料會儲存在備份元件檔 XML 檔中，而 DiskShadow 會自動要求並儲存在工作目錄中的 .cab 中繼資料檔案。 使用 [ [設定中繼資料] 命令](set-metadata.md) 來變更這個 XML 檔案的路徑和名稱。

## <a name="examples"></a>範例

以下是範例 DiskShadow 腳本，示範如何使用匯 **入** 命令：

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