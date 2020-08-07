---
title: bitsadmin getbytestransferred
description: Bitsadmin getbytestransferred 命令的參考文章，它會抓取針對指定工作所傳輸的位元組數目。
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d6b8ebb8d03a2498796325de8878840f36c6b71
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894525"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

抓取指定作業所傳輸的位元組數目。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
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
