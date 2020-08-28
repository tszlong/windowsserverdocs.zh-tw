---
title: retain
description: 保留命令的參考文章，可準備現有的動態磁碟區以做為開機或系統磁碟區。
ms.topic: reference
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98c27c62ab7e0ac3320986dde6049be40d10db57
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036987"
---
# <a name="retain"></a>retain

準備現有的簡單動態磁碟區作為開機或系統磁碟區。 如果您使用主開機記錄 (MBR) 動態磁碟，此命令會在主開機記錄中建立磁碟分割專案。 如果您使用 GUID 磁碟分割表格 (GPT) 動態磁碟，此命令會在 GUID 磁碟分割資料表中建立磁碟分割專案。

## <a name="syntax"></a>語法

```
retain
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
