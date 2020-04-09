---
title: bitsadmin getclientcertificate
description: 適用于**bitsadmin getclientcertificate**的 Windows 命令主題，它會從作業中抓取用戶端憑證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c29d5c64fd172cfdd2d5d93c5ed22d519077806
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850761"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate

從作業抓取用戶端憑證。

## <a name="syntax"></a>語法

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的用戶端憑證。

```
C:\>bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)