---
title: bitsadmin setminretrydelay
description: Bitsadmin setminretrydelay 命令的參考主題，其會設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fc54b4466d8f0bac12bd42ebf6c5e2c66087a15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720116"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| retrydelay | 在傳輸期間發生錯誤之後，BITS 等待的最短時間長度（以秒為單位）。 |

## <a name="examples"></a>範例

將名為*myDownloadJob*的作業設定為35秒的最小重試延遲：

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
