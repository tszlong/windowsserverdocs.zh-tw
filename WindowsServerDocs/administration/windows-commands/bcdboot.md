---
title: bcdboot
description: Bcdboot 命令的參考主題，可快速設定系統磁碟分割，或修復位於系統磁碟分割上的開機環境。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde91738f524350f72f0278495e4bd46a3960e6f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718704"
---
# <a name="bcdboot"></a>bcdboot

可讓您快速設定系統磁碟分割，或修復位於系統磁碟分割上的開機環境。 系統磁碟分割是藉由將一組簡單的開機設定資料（BCD）檔案複製到現有的空白磁碟分割來設定。

## <a name="syntax"></a>語法

```
bcdboot <source> [/l] [/s]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| source | 指定要做為複製開機環境檔案之來源的 Windows 目錄位置。 |
| /l | 指定地區設定。 預設地區設定為美式英文。 |
| /s | 指定系統磁碟分割的磁片區代號。 預設值是由固件識別的系統磁碟分割。 |

## <a name="examples"></a>範例

如需有關如何尋找 BCDboot 的詳細資訊，以及如何使用此命令的範例，請參閱[BCDboot 命令列選項](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)x)主題。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
