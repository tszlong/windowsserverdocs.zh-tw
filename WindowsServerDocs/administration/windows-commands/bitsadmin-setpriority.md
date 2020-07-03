---
title: bitsadmin setpriority
description: Bitsadmin setpriority 命令的參考文章，其會設定指定之作業的優先順序。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07afd9c636a5dbcd4e70de71b3a6f515e7e02bae
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927592"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

設定指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| priority | 設定作業的優先順序，包括：<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>範例

若要將名為*myDownloadJob*之作業的優先順序設定為正常：

```
bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
