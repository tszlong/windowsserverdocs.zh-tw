---
title: bitsadmin getproxylist-抓取指定之作業的 proxy 清單。
description: Bitsadmin getproxylist 命令的參考文章，它會抓取指定工作的 proxy 清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60c66db909b9b62d33ffda09e3b696362b6a65a0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926813"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

抓取要用於指定作業的 proxy 伺服器清單（以逗號分隔）。

## <a name="syntax"></a>語法

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的 proxy 清單：

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
