---
title: bitsadmin makecustomheaderswriteonly
description: 適用于**bitsadmin makecustomheaderswriteonly**的 Windows 命令主題，這會使作業的自訂 HTTP 標頭變成隻讀。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9183b1b5de51020c5c6d2efad2c0a788d158a183
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850241"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

將作業的自訂 HTTP 標頭設為僅限寫入。

> [!Important]
> 此動作無法復原。

## <a name="syntax"></a>語法

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，讓自訂 HTTP 標頭僅限寫入。

```
C:\>bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)