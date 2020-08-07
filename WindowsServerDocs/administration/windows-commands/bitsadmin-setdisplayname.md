---
title: bitsadmin setdisplayname
description: Bitsadmin setdisplayname 命令的參考文章，其會設定指定之作業的顯示名稱。
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac83fee33703555f1a8ba4b65ae8dc4d0f947916
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893164"
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
