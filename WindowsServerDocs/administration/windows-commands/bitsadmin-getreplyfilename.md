---
title: bitsadmin getreplyfilename
description: Bitsadmin getreplyfilename 命令的參考文章，這個命令會取得包含作業之伺服器上傳-回復的檔案路徑。
ms.topic: reference
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c2e355bf0264c5dc995f0d241afe7b641d2a41b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034876"
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
