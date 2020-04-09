---
title: 匯入
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07fdd03c73c454e92218a4c6983eac7f29b50883
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842171"
---
# <a name="import"></a>匯入



從載入的中繼資料檔案，將可轉移的陰影複製匯入到系統中。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
import
```

## <a name="remarks"></a>備註

-   可轉移的陰影複製不會立即儲存在系統上。 其詳細資料會儲存在備份元件檔 XML 檔案中，而 DiskShadow 會自動要求並將其儲存在工作目錄中的 .cab 中繼資料檔案。 您可以使用 [**設定中繼資料**] 命令來變更此檔案的路徑和名稱。
-   在您可以使用 [匯**入**] 之前，您必須使用 [**載入中繼資料**] 命令來載入 DiskShadow 中繼資料檔案。

## <a name="examples"></a><a name=BKMK_examples></a>典型

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