---
title: exec
description: Exec 命令的參考文章，它會在本機電腦上執行腳本檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4c9ff71b886bd84120bd3af7c1f8d8c143425da
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932313"
---
# <a name="exec"></a>exec

在本機電腦上執行腳本檔案。 此命令也會在備份或還原順序中複製或還原資料。 如果腳本失敗，則會傳回錯誤並結束 DiskShadow。

檔案可以是**cmd**腳本。

## <a name="syntax"></a>語法

```
exec <scriptfile.cmd>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<scriptfile.cmd>` | 指定要執行的腳本檔案。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskshadow 命令](diskshadow.md)
