---
title: bitsadmin getreplyfilename
description: Bitsadmin getreplyfilename 命令的參考文章，這個命令會取得包含作業之伺服器上傳-回復的檔案路徑。
ms.topic: reference
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d3dab8e7f2a50c00c1c5ccb72f4e6dea76df3090
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631715"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

取得包含作業之伺服器上傳-回復的檔案路徑。

> [!NOTE]
> BITS 1.2 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的上傳-回復檔案名：

```
bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
