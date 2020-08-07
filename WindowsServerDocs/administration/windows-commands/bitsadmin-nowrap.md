---
title: bitsadmin nowrap
description: Bitsadmin nowrap 命令的參考文章，它會截斷延伸至命令視窗最右邊邊緣以外的任何輸出文字行。
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72c50bdf7d19ea4603fc232dcf54acdfd97d3f17
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893642"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

截斷任何延伸到命令視窗最右邊邊緣外的輸出文字行。 根據預設，除了**監視器**參數之外，所有參數都會包裝輸出。 在其他參數前指定**nowrap**參數。

## <a name="syntax"></a>語法

```
bitsadmin /nowrap
```

## <a name="examples"></a>範例

若要在未包裝輸出的情況下，取得名為*myDownloadJob*之作業的狀態：

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
