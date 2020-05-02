---
title: bitsadmin gettype
description: Bitsadmin gettype 命令的參考主題，它會抓取指定之作業的工作類型。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 151f9b8e81229a666111ebcd20f060d84160445a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717481"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

抓取指定之作業的工作類型。

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

| 類型 | 說明 |
| --------------- | ----------- |
| 下載 | 此作業為下載。 |
| 上傳 | 作業是上傳。 |
| 上傳-回復 | 作業是上傳-回復。 |
| Unknown | 作業的類型不明。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的作業類型：

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
