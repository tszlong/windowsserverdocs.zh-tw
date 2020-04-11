---
title: bitsadmin setminretrydelay
description: 適用于**bitsadmin setminretrydelay**的 Windows 命令主題，會設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddae9a62a49ca07bb03649f131a0a1ebad8ee3fe
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122887"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

設定在嘗試傳輸檔案之前，在遇到暫時性錯誤之後，BITS 等待的最短時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| retrydelay | 在傳輸期間發生錯誤之後，BITS 等待的最短時間長度（以秒為單位）。 |

## <a name="examples"></a>範例

下列範例會將名為*myDownloadJob*之作業的最小重試延遲設定為35秒。

```
C:\>bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)