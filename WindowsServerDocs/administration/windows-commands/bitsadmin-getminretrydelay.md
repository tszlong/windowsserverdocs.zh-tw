---
title: bitsadmin getminretrydelay
description: Bitsadmin getminretrydelay 命令的參考文章，它會抓取服務在嘗試傳輸檔案之前，會等待暫時性錯誤的時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 066eb9a2c967d9d5e92aa8dbad2001a65a682796
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927010"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

抓取服務在嘗試傳輸檔案之前，遇到暫時性錯誤之後，會等待的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的最小重試延遲：

```
bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
