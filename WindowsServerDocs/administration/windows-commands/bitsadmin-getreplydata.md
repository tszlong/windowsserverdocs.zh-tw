---
title: bitsadmin getreplydata
description: Bitsadmin getreplydata 命令的參考文章，此命令會以十六進位格式抓取工作的伺服器上傳-回復資料。
ms.topic: reference
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e00a17e4633c9b065d03684cb0c953d1f6db811
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031336"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

以十六進位格式抓取工作的伺服器上傳-回復資料。

> [!NOTE]
> BITS 1.2 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getreplydata <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的上傳-回復資料：

```
bitsadmin /getreplydata myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
