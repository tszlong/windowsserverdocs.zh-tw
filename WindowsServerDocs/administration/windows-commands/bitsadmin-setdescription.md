---
title: bitsadmin setdescription
description: 適用于**bitsadmin setdescription**的 Windows 命令主題，其會設定指定之作業的描述。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b62e6b030c23c475418cd6f2c63f04edba1acff
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123011"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

設定指定之作業的描述。

## <a name="syntax"></a>語法

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| description | 用來描述作業的文字。 |

## <a name="examples"></a>範例

下列範例會抓取名為*myDownloadJob*之作業的描述。

```
C:\>bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)