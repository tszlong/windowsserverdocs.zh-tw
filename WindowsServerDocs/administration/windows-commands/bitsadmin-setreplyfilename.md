---
title: bitsadmin setreplyfilename
description: Bitsadmin setreplyfilename 命令的參考文章，其指定包含伺服器上傳-回復之檔案的路徑。
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd6254d0d3d642796930cbaaa95aa668e3886f3c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892949"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

指定包含伺服器上傳-回復之檔案的路徑。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /setreplyfilename <job> <file_path>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| file_path | 放置伺服器上傳-回復的位置。 |

## <a name="examples"></a>範例

若要設定名為*myDownloadJob*之作業的上傳-回復檔案名檔案路徑：

```
bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
