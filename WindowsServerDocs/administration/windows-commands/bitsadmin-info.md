---
title: bitsadmin info
description: '**Bitsadmin info**的 Windows 命令主題，它會顯示指定作業的摘要資訊。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20b8358caba3e0c07b0c985cb24e8f7bde43b06c
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123112"
---
# <a name="bitsadmin-info"></a>bitsadmin info

顯示指定之作業的摘要資訊。

## <a name="syntax"></a>語法

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| /verbose | 選擇性。 提供有關每項作業的詳細資訊。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會捕獲名為*myDownloadJob*之作業的相關資訊。

```
C:\>bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)