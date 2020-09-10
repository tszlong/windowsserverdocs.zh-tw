---
title: bcdboot
description: Bcdboot 命令的參考文章，可快速設定系統磁碟分割，或修復位於系統磁碟分割上的開機環境。
ms.topic: reference
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7cc67c47093686882f0a7bfa1fcc47f8c444210
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632987"
---
# <a name="bcdboot"></a>bcdboot

可讓您快速設定系統磁碟分割，或修復位於系統磁碟分割上的開機環境。 系統磁碟分割是藉由將一組簡單的開機設定資料 (BCD) 檔複製到現有的空磁碟分割來設定。

## <a name="syntax"></a>語法

```
bcdboot <source> [/l] [/s]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| source | 指定要做為複製開機環境檔案來源的 Windows 目錄位置。 |
| /l | 指定地區設定。 預設的地區設定是美國英文。 |
| /s | 指定系統磁碟分割的磁片區信件。 預設值是由固件識別的系統磁碟分割。 |

## <a name="examples"></a>範例

如需有關如何找到 BCDboot 的詳細資訊，以及如何使用此命令的範例，請參閱 [BCDboot 命令列選項](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)) 主題。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
