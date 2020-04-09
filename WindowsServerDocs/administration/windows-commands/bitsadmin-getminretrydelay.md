---
title: bitsadmin getminretrydelay
description: 適用于**bitsadmin getminretrydelay**的 Windows 命令主題，它會抓取服務在嘗試傳輸檔案之前，在遇到暫時性錯誤之後所等待的時間長度（以秒為單位）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d79ffdf1f45b0198b4af535ed83154c3c2ec24f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850621"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

抓取服務在嘗試傳輸檔案之前，遇到暫時性錯誤之後，會等待的時間長度（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的最小重試延遲。

```
C:\>bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)