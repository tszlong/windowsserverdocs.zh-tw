---
title: bitsadmin getproxybypasslist
description: Bitsadmin getproxybypasslist 命令的參考文章，此命令會抓取指定作業的 proxy 略過清單。
ms.topic: reference
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5b9b9ffd3865ef70408c566bdd832005e74f6598
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631749"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

抓取指定作業的 proxy 略過清單。

## <a name="syntax"></a>語法

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

### <a name="remarks"></a>備註

略過清單中包含的主機名稱或 IP 位址，或兩者都不是透過 proxy 路由傳送。 清單可以包含 `<local>` 以參考相同 LAN 上的所有伺服器。 清單可以是分號 (; ) 或以空格分隔。

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的 proxy 略過清單：

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
