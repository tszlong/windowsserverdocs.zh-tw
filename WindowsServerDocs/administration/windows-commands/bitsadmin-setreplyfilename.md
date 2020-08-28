---
title: bitsadmin setreplyfilename
description: Bitsadmin setreplyfilename 命令的參考文章，這個命令會指定包含伺服器上傳-回復的檔案路徑。
ms.topic: reference
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2991503ad558b318f52d07447af18d4f9e6178aa
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033416"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

指定包含伺服器上傳-回復之檔案的路徑。

> [!NOTE]
> BITS 1.2 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /setreplyfilename <job> <file_path>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| file_path | 要放置伺服器上傳-回復的位置。 |

## <a name="examples"></a>範例

若要針對名為 *myDownloadJob*的作業設定上傳-回復檔案名檔案路徑：

```
bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
