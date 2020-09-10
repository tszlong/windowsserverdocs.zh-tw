---
title: bitsadmin replaceremoteprefix
description: Bitsadmin replaceremoteprefix 命令的參考文章，此命令會視需要將工作中所有檔案的遠端 URL，從 *oldprefix* 變更為 *newprefix*。
ms.topic: reference
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 83c517a9126a3b78dfc919af5663e939aceee186
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631126"
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
