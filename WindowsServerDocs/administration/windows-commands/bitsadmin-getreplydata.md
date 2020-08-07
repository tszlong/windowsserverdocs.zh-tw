---
title: bitsadmin getreplydata
description: Bitsadmin getreplydata 命令的參考文章，它會以十六進位格式抓取工作的伺服器上傳-回復資料。
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab9f774c7b31576e18de3ec5db9b5a72427ecdaa
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893900"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

針對作業，以十六進位格式抓取伺服器的上傳-回復資料。

> [!NOTE]
> BITS 1.2 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /getreplydata <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的上傳-回復資料：

```
bitsadmin /getreplydata myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
