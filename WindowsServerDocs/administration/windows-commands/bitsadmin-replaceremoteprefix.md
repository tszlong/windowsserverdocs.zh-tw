---
title: bitsadmin replaceremoteprefix
description: Bitsadmin replaceremoteprefix 命令的參考主題，視需要將作業中所有檔案的遠端 URL 從*oldprefix*變更為*newprefix*。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 745d026513413db799e86df3422d5ee19c89274f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717041"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

視需要將作業中所有檔案的遠端 URL 從*oldprefix*變更為*newprefix*。

## <a name="syntax"></a>語法

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| oldprefix | 現有的 URL 前置詞。 |
| newprefix | 新的 URL 前置詞。 |

## <a name="examples"></a>範例

若要變更名為*myDownloadJob*之作業中所有檔案的遠端 URL *http://stageserver* ， *http://prodserver*請從到。

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他資訊

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
