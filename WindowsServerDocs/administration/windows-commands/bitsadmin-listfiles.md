---
title: bitsadmin listfiles
description: Bitsadmin listfile 命令的參考文章，此命令會列出指定工作中的檔案。
ms.topic: reference
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 702c2d3d3370275373a91aef63aa82f7e84bcf14
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631517"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

列出指定工作中的檔案。

## <a name="syntax"></a>語法

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的檔案清單：

```
bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
