---
title: bitsadmin setdisplayname
description: 適用于**bitsadmin setdisplayname**的 Windows 命令主題，其會設定指定之作業的顯示名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b1086903dd130392800f325c451bb4750fbf8fa
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123008"
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
| 工作 | 作業的顯示名稱或 GUID。 |
| display_name | 用來做為特定工作之顯示名稱的文字。 |

## <a name="examples"></a>範例

下列範例會將作業的顯示名稱設定為*myDownloadJob*。

```
C:\>bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)