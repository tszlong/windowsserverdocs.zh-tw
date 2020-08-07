---
title: bitsadmin getnotifycmdline
description: Bitsadmin getnotifycmdline 命令的參考文章，它會抓取作業完成傳送資料時所執行的命令列命令。
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5726147701e3ef09e40b3edf900a2e4d417f95cc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894107"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

抓取指定的作業完成資料傳輸之後，要執行的命令列命令。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

當名為*myDownloadJob*的作業完成時，取得服務所使用的命令列命令。

```
bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
