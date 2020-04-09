---
title: bitsadmin gethelpertokensid
description: 適用于**bitsadmin gethelpertokensid**的 Windows 命令主題，它會傳回 BITS 傳送作業的協助程式權杖的 SID （如果有設定的話）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a2e26ff459b068595529fbd24e6165c130660570
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850641"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

傳回 BITS 傳送作業的協助程式 [權杖](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)SID （如果已設定的話）。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
