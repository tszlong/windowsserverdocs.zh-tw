---
title: bitsadmin gettype
description: Bitsadmin gettype 命令的參考文章，它會抓取指定工作的工作類型。
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f0fe66256e59526b874aaf2ec8ae0193846a1da
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893830"
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

| 類型 | 描述 |
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
