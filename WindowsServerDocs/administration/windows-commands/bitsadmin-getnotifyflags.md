---
title: bitsadmin getnotifyflags
description: Bitsadmin getnotifyflags 命令的參考主題，它會抓取指定之作業的通知旗標。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36e4c3584b2e3be9c9985756aeaec08b40e74b0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717769"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

抓取指定之作業的通知旗標。

## <a name="syntax"></a>語法

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="remarks"></a>備註

作業可以包含下列一個或多個通知旗標：

| 旗標 | 描述 |
| ----- | ----- |
| 0x001 | 當作業中的所有檔案都已轉移時，產生事件。 |
| 0x002 | 發生錯誤時產生事件。 |
| 0x004 | 停用通知。 |
| 0x008 | 在修改作業或傳送進度時產生事件。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的通知旗標：

```
bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
