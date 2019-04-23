---
title: 匯入
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddef3958bc431519e3cb89b658a58d1f4dba6938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835259"
---
# <a name="import"></a>匯入



從載入的中繼資料檔案中會傳送陰影複製匯入到系統。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
import
```

## <a name="remarks"></a>備註

-   傳送陰影複製，不會儲存在系統上立即。 它們的詳細資料會儲存在備份元件的文件 XML 檔案，DiskShadow 會自動要求並將儲存在工作目錄中的.cab 中繼資料檔案。 您可以變更這個檔案使用的名稱與路徑**集中繼資料**命令。
-   您可以使用之前**匯入**，您必須載入 DiskShadow 中繼資料檔案使用**載入中繼資料**命令。

## <a name="BKMK_examples"></a>範例

以下是示範如何使用範例 DiskShadow 指令碼**匯入**命令：
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

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)