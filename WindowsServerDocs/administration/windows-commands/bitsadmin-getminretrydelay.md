---
title: bitsadmin getminretrydelay
description: Bitsadmin getminretrydelay 命令的參考文章，這個命令會抓取服務在嘗試傳送檔案之前，遇到暫時性錯誤之後所等待的時間長度（以秒為單位）。
ms.topic: reference
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: be71dc355e966b4a6ac627045f1ec5ceaf68f2d3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631946"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

以秒為單位，抓取服務在嘗試傳送檔案之前，遇到暫時性錯誤之後會等待的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的最小重試延遲：

```
bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
