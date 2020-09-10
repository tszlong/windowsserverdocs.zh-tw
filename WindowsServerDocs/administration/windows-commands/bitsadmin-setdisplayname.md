---
title: bitsadmin setdisplayname
description: Bitsadmin setdisplayname 命令的參考文章，此命令會設定指定作業的顯示名稱。
ms.topic: reference
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 57a72d198b7d262f3a7958920e5d54955f8b6270
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630937"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

設定指定之作業的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /setdisplayname <job> <display_name>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| display_name | 用來當做特定工作之顯示名稱的文字。 |

## <a name="examples"></a>範例

若要將作業的顯示名稱設定為 *myDownloadJob*：

```
bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
