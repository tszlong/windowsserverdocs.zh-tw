---
title: bitsadmin getowner
description: Bitsadmin getowner 命令的參考文章，此命令會抓取指定作業的擁有者。
ms.topic: reference
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2839936113e304e9c57308042f5c8cf66e887d36
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631877"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

顯示指定作業之擁有者的顯示名稱或 GUID。

## <a name="syntax"></a>語法

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要顯示名為 *myDownloadJob*之作業的擁有者：

```
bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
