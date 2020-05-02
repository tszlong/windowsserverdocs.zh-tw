---
title: bitsadmin getclientcertificate
description: Bitsadmin getclientcertificate 命令的參考主題，它會從作業中抓取用戶端憑證。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2582950dd02ca1880e4765fb974c83c423b22bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718129"
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
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的用戶端憑證：

```
bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
