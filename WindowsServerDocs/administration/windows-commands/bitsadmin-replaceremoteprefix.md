---
title: bitsadmin replaceremoteprefix
description: Bitsadmin replaceremoteprefix 命令的參考文章，此命令會視需要將工作中所有檔案的遠端 URL，從 *oldprefix* 變更為 *newprefix*。
ms.topic: reference
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21c1ede5b05a80ba80b1bd470065e914fb353c3c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026326"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

視需要將工作中所有檔案的遠端 URL 從 *oldprefix* 變更為 *newprefix*。

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

若要將名為 *myDownloadJob*之工作中所有檔案的遠端 URL，從變更為 *http://stageserver* *http://prodserver* 。

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他資訊

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
