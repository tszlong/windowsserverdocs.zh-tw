---
title: bitsadmin getbytestotal
description: Bitsadmin getbytestotal 命令的參考文章，它會抓取指定工作的大小。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc153ae373152461ed127dde76c934da86be8d6b
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923134"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

抓取指定之作業的大小。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出名為*myDownloadJob*的作業大小：

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
