---
title: bitsadmin setdisplayname
description: Bitsadmin setdisplayname 命令的參考主題，其會設定指定之作業的顯示名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 382cb2f20f0374c2d2787c4c3d88670b4f7260cd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719382"
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
| display_name | 用來做為特定工作之顯示名稱的文字。 |

## <a name="examples"></a>範例

若要將作業的顯示名稱設定為*myDownloadJob*：

```
bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
