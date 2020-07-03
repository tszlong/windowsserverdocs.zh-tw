---
title: bitsadmin getreplyprogress
description: Bitsadmin getreplyprogress 命令的參考文章，它會抓取伺服器上傳-回復的大小和進度。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 728a6ba9c63f31403da59dc392cff581ec0e5c83
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926718"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

抓取伺服器上傳-回復的大小和進度。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getreplyprogress <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的上傳-回復進度：

```
bitsadmin /getreplyprogress myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
