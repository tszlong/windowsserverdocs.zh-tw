---
title: bitsadmin geterror
description: Bitsadmin geterror 命令的參考文章，它會抓取所指定工作的詳細錯誤資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d729b946df4af33da3a55ff8051c59fbb5d7efe
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928305"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

抓取指定之作業的詳細錯誤資訊。

## <a name="syntax"></a>語法

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的錯誤資訊：

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
