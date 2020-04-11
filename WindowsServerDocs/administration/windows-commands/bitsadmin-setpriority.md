---
title: bitsadmin setpriority
description: 適用于**bitsadmin setpriority**的 Windows 命令主題，其會設定指定之作業的優先順序。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9348680a61649b938267b3277de9aa5aa521361f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122761"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

設定指定之作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| 優先順序 | 設定作業的優先順序，包括：<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>範例

下列範例會將名為*myDownloadJob*之作業的優先順序設定為 normal。

```
C:\>bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)