---
title: bitsadmin setminretrydelay
description: Bitsadmin setminretrydelay 命令的參考文章，它會設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a28bdcc90fdeee4d5173272c8670f9d0bff3c0a0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927680"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| retrydelay | 在傳輸期間發生錯誤之後，BITS 等待的最短時間長度（以秒為單位）。 |

## <a name="examples"></a>範例

將名為*myDownloadJob*的作業設定為35秒的最小重試延遲：

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
