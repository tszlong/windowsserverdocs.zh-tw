---
title: bitsadmin getbytestransferred
description: Bitsadmin getbytestransferred 命令的參考文章，它會抓取針對指定工作所傳輸的位元組數目。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ca8561a0c5eb92bb4bd716f7b20bd9f7ceaf606
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923116"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

抓取指定作業所傳輸的位元組數目。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業所傳輸的位元組數：

```
bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
