---
title: bitsadmin getproxybypasslist
description: Bitsadmin getproxybypasslist 命令的參考文章，它會抓取指定工作的 proxy 略過清單。
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16cca14a47f086be65764da5441d915d2d28d2db
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894001"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

抓取指定之作業的 proxy 略過清單。

## <a name="syntax"></a>語法

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

### <a name="remarks"></a>備註

「略過清單」包含不會透過 proxy 路由傳送的主機名稱或 IP 位址（或兩者）。 清單可以包含 `<local>` 來參考相同 LAN 上的所有伺服器。 清單可以是分號 (，) 或空格分隔。

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的 proxy 略過清單：

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
