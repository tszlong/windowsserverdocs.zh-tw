---
title: bitsadmin cache and setlimit
description: Bitsadmin cache 和 setlimit 命令的參考文章，其會設定快取大小限制。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de218990d9176336e779b551bfacc0897df5d114
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923213"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache and setlimit

設定快取大小限制。

## <a name="syntax"></a>語法

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| percent | 快取限制定義為總硬碟空間的百分比。 |

## <a name="examples"></a>範例

若要將快取大小限制設定為50%：

```
bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
