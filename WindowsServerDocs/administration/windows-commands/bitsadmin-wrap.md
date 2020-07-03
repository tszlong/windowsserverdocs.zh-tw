---
title: bitsadmin wrap
description: Bitsadmin wrap 命令的參考文章，它會將任何行的輸出文字包裝到命令視窗的最右邊邊緣以外的下一行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47f5e38555eb2464d3febf5f958ce5a6af20452
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927256"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將延伸到命令視窗最右邊邊緣以外的任何一行輸出文字包裝到下一行。 您必須在任何其他參數之前指定此參數。

根據預設， [bitsadmin 監視器](bitsadmin-monitor.md)參數以外的所有參數都會包裝輸出文字。

## <a name="syntax"></a>語法

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ---------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的資訊，並將輸出文字包裝起來：

```
bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
