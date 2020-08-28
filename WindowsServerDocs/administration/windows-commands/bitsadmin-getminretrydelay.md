---
title: bitsadmin getminretrydelay
description: Bitsadmin getminretrydelay 命令的參考文章，這個命令會抓取服務在嘗試傳送檔案之前，遇到暫時性錯誤之後所等待的時間長度（以秒為單位）。
ms.topic: reference
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f5bc6d69e7dbc46bc7e0df3a34ac97f37fda252
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030306"
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
