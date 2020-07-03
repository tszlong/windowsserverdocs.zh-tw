---
title: bitsadmin replaceremoteprefix
description: Bitsadmin replaceremoteprefix 命令的參考文章，它會視需要將作業中所有檔案的遠端 URL 從*oldprefix*變更為*newprefix*。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2453ac4c223baa049980578c81d9bc6539baac7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927970"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

視需要將作業中所有檔案的遠端 URL 從*oldprefix*變更為*newprefix*。

## <a name="syntax"></a>語法

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| oldprefix | 現有的 URL 前置詞。 |
| newprefix | 新的 URL 前置詞。 |

## <a name="examples"></a>範例

若要變更名為*myDownloadJob*之作業中所有檔案的遠端 URL，請從 *http://stageserver* 到 *http://prodserver* 。

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他資訊

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
