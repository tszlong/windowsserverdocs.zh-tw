---
title: bitsadmin getcustomheaders
description: 適用于**bitsadmin getcustomheaders**的 Windows 命令主題，它會從作業中抓取自訂 HTTP 標頭。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80a3d1230fd541f0b986434ce373da4c8bb0c761
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850731"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

從作業中抓取自訂 HTTP 標頭。

## <a name="syntax"></a>語法

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會取得名為*myDownloadJob*之作業的自訂標頭。

```
C:\>bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)