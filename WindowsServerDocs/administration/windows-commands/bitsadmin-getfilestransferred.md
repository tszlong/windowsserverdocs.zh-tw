---
title: bitsadmin getfilestransferred
description: Bitsadmin getfilestransferred 命令的參考文章，它會抓取針對指定工作所傳輸的檔案數目。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43257dcb8350974bfb258a9970c1a6fec787a226
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928255"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

抓取指定作業所傳輸的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業中傳輸的檔案數目：

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
