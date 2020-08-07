---
title: bitsadmin getfilestransferred
description: Bitsadmin getfilestransferred 命令的參考文章，它會抓取針對指定工作所傳輸的檔案數目。
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c089dc8a122f558a396fdffb77b173b7586ee900
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894300"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

抓取指定作業所傳輸的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
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
