---
title: bitsadmin getcreationtime
description: 適用于**bitsadmin getcreationtime**的 Windows 命令主題，它會抓取指定工作的建立時間。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd8f718e53cc44dc5f4c6f5ff09c9a5c201e0564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850741"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

抓取指定之作業的建立時間。

## <a name="syntax"></a>語法

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的建立時間。

```
C:\>bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)