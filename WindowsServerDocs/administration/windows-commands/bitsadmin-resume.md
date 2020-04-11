---
title: bitsadmin resume
description: '**Bitsadmin resume**的 Windows 命令主題，它會在傳送佇列中啟用新的或已暫止的工作。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e81bd80232cd4ec8fbba70c86cd97bb9695680f8
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123073"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

在傳送佇列中啟用新的或已暫止的工作。

## <a name="syntax"></a>語法

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

下列範例會繼續名為*myDownloadJob*的作業。

```
C:\>bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)