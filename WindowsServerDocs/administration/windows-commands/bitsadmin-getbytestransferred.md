---
title: bitsadmin getbytestransferred
description: 適用于**bitsadmin getbytestransferred**的 Windows 命令主題，它會抓取針對指定工作所傳輸的位元組數目。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 957b3e60bf8a5e41b3964f4d762633472606654d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850771"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

抓取指定作業所傳輸的位元組數目。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取所傳輸的位元組數目。

```
C:\>bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)