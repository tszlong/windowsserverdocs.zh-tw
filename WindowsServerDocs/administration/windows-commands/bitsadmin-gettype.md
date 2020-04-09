---
title: bitsadmin gettype
description: '**Bitsadmin gettype**的 Windows 命令主題，它會抓取所指定工作的工作類型。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66a1fc5b0478e1eec26557dc9a7f76d50abcb8b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850441"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

抓取指定之作業的工作類型。

## <a name="syntax"></a>語法

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="output"></a>輸出

輸出值包括：

| 類型 | 描述 |
| --------------- | ----------- |
| 下載 | 此作業為下載。 |
| 上傳 | 作業是上傳。 |
| 上傳-回復 | 作業是上傳-回復。 |
| 未知 | 作業的類型不明。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的工作類型。

```
C:\>bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)