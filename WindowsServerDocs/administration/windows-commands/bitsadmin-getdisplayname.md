---
title: bitsadmin getdisplayname
description: Bitsadmin getdisplayname 命令的參考主題，它會抓取指定之作業的顯示名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d47a70d34dbff0249a9d69f1db1deaa879c76c8
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437063"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

抓取指定之作業的顯示名稱。

## <a name="syntax"></a>語法

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的顯示名稱：

```
bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
