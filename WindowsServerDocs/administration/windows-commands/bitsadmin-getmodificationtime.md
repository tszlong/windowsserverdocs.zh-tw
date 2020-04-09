---
title: bitsadmin getmodificationtime
description: 適用于**bitsadmin getmodificationtime**的 Windows 命令主題，它會抓取上次修改作業的時間，或資料已成功傳輸。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ace0f64b1fbe7ba72174bb3df2bd4dd65e929769
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850611"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

抓取上次修改作業或資料傳輸成功的時間。

## <a name="syntax"></a>語法

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取上次修改時間。

```
C:\>bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)