---
title: bitsadmin getfilestotal
description: Bitsadmin getfilestotal 命令的參考主題，它會抓取指定之作業中的檔案數目。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cf3b33c15bb18c8a141408f82fdd72a6e710817
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717980"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

抓取指定之作業中的檔案數目。

## <a name="syntax"></a>語法

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出包含在名為*myDownloadJob*之作業中的檔案數目：

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>另請參閱

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
