---
title: bitsadmin getproxybypasslist
description: Bitsadmin getproxybypasslist 命令的參考文章，它會抓取指定工作的 proxy 略過清單。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67e6e4a54bb8c4929acbbaa47c78e33bab95a283
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926831"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

抓取指定之作業的 proxy 略過清單。

## <a name="syntax"></a>語法

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

### <a name="remarks"></a>備註

「略過清單」包含不會透過 proxy 路由傳送的主機名稱或 IP 位址（或兩者）。 清單可以包含 `<local>` 來參考相同 LAN 上的所有伺服器。 清單可以是分號（;)或以空格分隔。

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的 proxy 略過清單：

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
