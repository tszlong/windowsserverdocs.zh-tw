---
title: bitsadmin getminretrydelay
description: Bitsadmin getminretrydelay 命令的參考文章，它會抓取服務在嘗試傳輸檔案之前，會等待暫時性錯誤的時間長度（以秒為單位）。
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89e925dd8db3932e273552a82a497d99fd3bdb95
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894197"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

抓取服務在嘗試傳輸檔案之前，遇到暫時性錯誤之後，會等待的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
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
