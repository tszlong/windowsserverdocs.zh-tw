---
title: bitsadmin getcreationtime
description: Bitsadmin getcreationtime 命令的參考文章，它會抓取指定工作的建立時間。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0efc3d2a0f0f2cf3e72a4ff634733b16ef80055d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923088"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

抓取指定之作業的建立時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的建立時間：

```
bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
