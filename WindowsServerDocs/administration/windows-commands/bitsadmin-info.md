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
ms.openlocfilehash: b9d8b840e39620db01c44001d9cb7d593be2be5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850341"
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