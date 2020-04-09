---
title: bitsadmin getcompletiontime
description: '**Bitsadmin getcompletiontime**的 Windows 命令主題，它會抓取作業完成傳輸資料的時間。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5408e7e8c35135601a4a0af0ab7e9c55cea4c8dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850751"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

抓取作業完成資料傳輸的時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*的作業完成傳輸資料的時間。

```
C:\>bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)