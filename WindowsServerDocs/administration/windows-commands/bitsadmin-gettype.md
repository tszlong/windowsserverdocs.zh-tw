---
title: bitsadmin gettype
description: Bitsadmin gettype 命令的參考文章，此命令會抓取指定作業的作業類型。
ms.topic: reference
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ccb938e1fe67164567bbe8d6c87d87423026db96
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631566"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

抓取指定作業的工作類型。

## <a name="syntax"></a>語法

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

傳回的輸出值可以是：

| 類型 | 描述 |
| --------------- | ----------- |
| 下載 | 作業是下載的。 |
| 上傳 | 作業是上傳。 |
| 上傳-回復 | 作業是上傳-回復。 |
| Unknown | 作業的類型未知。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的作業類型：

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
