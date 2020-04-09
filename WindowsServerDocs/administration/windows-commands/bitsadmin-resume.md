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
ms.openlocfilehash: 0a3f464ba00c5cc233c42a40c063372dc0d584e9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849751"
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

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會繼續名為*myDownloadJob*的作業。

```
C:\>bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)