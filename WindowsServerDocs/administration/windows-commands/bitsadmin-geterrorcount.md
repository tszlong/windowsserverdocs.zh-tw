---
title: bitsadmin geterrorcount
description: Bitsadmin geterrorcount 命令的參考文章，此命令會抓取指定作業產生暫時性錯誤的次數計數。
ms.topic: reference
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7266af9244218cf4a6434c838390eac8149c80ab
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632112"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

抓取指定作業產生暫時性錯誤的次數計數。

## <a name="syntax"></a>語法

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的錯誤計數資訊：

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
