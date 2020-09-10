---
title: retain
description: 保留命令的參考文章，可準備現有的動態磁碟區以做為開機或系統磁碟區。
ms.topic: reference
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 201aa8b79f8ac0f89d44be7db3f84c9f3059e206
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626903"
---
# <a name="retain"></a>retain

準備現有的簡單動態磁碟區作為開機或系統磁碟區。 如果您使用主開機記錄 (MBR) 動態磁碟，此命令會在主開機記錄中建立磁碟分割專案。 如果您使用 GUID 磁碟分割表格 (GPT) 動態磁碟，此命令會在 GUID 磁碟分割資料表中建立磁碟分割專案。

## <a name="syntax"></a>語法

```
retain
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
