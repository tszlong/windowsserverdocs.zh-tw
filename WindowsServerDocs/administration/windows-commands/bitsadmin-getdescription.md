---
title: bitsadmin getdescription
description: Bitsadmin getdescription 命令的參考文章，此命令會抓取指定作業的描述。
ms.topic: reference
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ddc3ef5f5f8328182e91ed7ae3026e94464267b7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632148"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

抓取指定作業的描述。

## <a name="syntax"></a>語法

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的描述：

```
bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
