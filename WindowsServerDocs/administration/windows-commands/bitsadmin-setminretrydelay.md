---
title: bitsadmin setminretrydelay
description: Bitsadmin setminretrydelay 命令的參考文章，此命令會設定 BITS 在嘗試傳送檔案之前，在發生暫時性錯誤之前等候的最小時間長度（以秒為單位）。
ms.topic: reference
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ef8ed52e991ae9c2d1807ade604cc0cab066743
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031266"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定在嘗試傳送檔案之前，在發生暫時性錯誤之後，BITS 等候的最小時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| retrydelay | 傳輸期間發生錯誤時所等待的最短時間長度（以秒為單位）。 |

## <a name="examples"></a>範例

針對名為 *myDownloadJob*的作業，將重試延遲的最小值設定為35秒：

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
