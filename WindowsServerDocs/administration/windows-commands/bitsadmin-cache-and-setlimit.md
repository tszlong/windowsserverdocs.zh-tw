---
title: bitsadmin cache 和 setlimit
description: 適用于**bitsadmin cache 和 setlimit**的 Windows 命令主題，其會設定快取大小限制。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 746ee0b69da8f5bd22fec2ccbd432126cc25d94d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850871"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache 和 setlimit

設定快取大小限制。

## <a name="syntax"></a>語法

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| percent | 快取限制定義為總硬碟空間的百分比。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會將快取大小限制為50%。

```
C:\>bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)